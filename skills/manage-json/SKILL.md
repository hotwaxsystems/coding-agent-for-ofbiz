---
name: manage-json
description: |
  Read and write JSON in OFBiz Groovy and Java. Use when
  - Turning a map or list into JSON, or JSON into a map or list.
  - Calling an outside API that sends or returns JSON.
  - Writing or reading a JSON or JSONL file, small or large.
  - Deciding whether to store a JSON payload in the database.
---

## Goal
Read and write JSON the same way everywhere: with one shared Jackson mapper, streaming for large files, dates as text, and no full payloads in the database.

## Triggers
**ALWAYS** read this skill when:
- Code builds JSON, parses JSON, or calls an API that sends or returns JSON.
- Code writes or reads a `.json` or `.jsonl` file.
- A field or entity would hold a JSON payload.

## Rules

1. **Use one shared Jackson mapper, or OFBiz's `JSON` class.**
   - OFBiz trunk has `org.apache.ofbiz.base.lang.JSON`, which wraps one shared Jackson `ObjectMapper`. Use it for small JSON.
   - Where you need the mapper itself, keep one as a `static final` field for the whole component. Never create an `ObjectMapper` on each call.
   - Never change a shared mapper's settings in place. Make a `copy()` once, set it up, and keep it as its own `static final` field.
2. **Never use `JsonSlurper`, `JsonOutput`, `org.json`, or JSON built by joining strings.**
   - Hand-built strings break on quotes and special characters, and are hard to read.
   - `JsonSlurper` returns lazy maps by default, which behave badly when cached or passed to other code.
3. **Stream large files one record at a time, and close them with try-with-resources.**
   - Never collect every record in a list, then write the list.
   - Never read a whole large file into a string or a list.
   - `JSON.from(InputStream)` and `JSON.from(Reader)` read the whole input into memory. Use them for small JSON only.
   - Open every `Reader`, `Writer`, `JsonParser` and `JsonGenerator` in try-with-resources, so an error never leaves a file open.
4. **Write dates as text.**
   - By default Jackson writes a `java.sql.Timestamp` as a number of milliseconds. Turn that off and set a date format on your mapper copy.
5. **Never store full payloads in the database.**
   - Do not save a whole request, reply or product JSON in an entity field.
   - Keep the fields you need, a short cut-down copy, or a hash. Keep large files as files, not rows.

## Procedure

### Small JSON: a map to JSON and back

```groovy
import org.apache.ofbiz.base.lang.JSON

String body = JSON.from(requestMap).toString()
Map reply = JSON.from(responseText).toObject(Map.class)
```

```java
import org.apache.ofbiz.base.lang.JSON;

String body = JSON.from(requestMap).toString();
Map<String, Object> reply = UtilGenerics.cast(JSON.from(responseText).toObject(Map.class));
```

`JSON.from(Object)` and `toObject` throw `IOException`. Turn that into a service error with the reason, never swallow it.

### One shared mapper for a component

```groovy
import com.fasterxml.jackson.databind.ObjectMapper
import com.fasterxml.jackson.databind.SerializationFeature
import java.text.SimpleDateFormat

class JsonMappers {
    static final ObjectMapper MAPPER = new ObjectMapper()
    static final ObjectMapper FILE_MAPPER = MAPPER.copy()
            .disable(SerializationFeature.INDENT_OUTPUT)
            .disable(SerializationFeature.WRITE_DATES_AS_TIMESTAMPS)
            .setDateFormat(new SimpleDateFormat("yyyy-MM-dd HH:mm:ss"))
}

String body = JsonMappers.MAPPER.writeValueAsString(requestMap)
Map reply = JsonMappers.MAPPER.readValue(responseText, Map.class)
```

A configured `ObjectMapper` is safe to share between threads.

### Writing a large file: one record at a time

```groovy
import com.fasterxml.jackson.core.JsonGenerator

try (Writer writer = new OutputStreamWriter(new FileOutputStream(file), "UTF-8");
     JsonGenerator generator = JsonMappers.FILE_MAPPER.factory.createGenerator(writer)) {
    generator.writeStartArray()
    try (EntityListIterator records = from("Product").queryIterator()) {
        GenericValue product
        while ((product = records.next()) != null) {
            generator.writeObject([productId: product.productId, productName: product.productName])
        }
    }
    generator.writeEndArray()
}
```

Each record goes to the file as it is read. Memory stays flat, whatever the record count.

### Reading a large file: one record at a time

```groovy
import com.fasterxml.jackson.core.JsonParser
import com.fasterxml.jackson.core.JsonToken

try (JsonParser parser = JsonMappers.MAPPER.factory.createParser(file)) {
    if (parser.nextToken() != JsonToken.START_ARRAY) {
        return error("Expected a JSON array in ${file.name}")
    }
    while (parser.nextToken() == JsonToken.START_OBJECT) {
        Map record = JsonMappers.MAPPER.readValue(parser, Map.class)
        run service: "importProduct", with: record
    }
}
```

Only one record is in memory at a time.

### JSON array or JSONL

- A JSON array, `[{...},{...}]`, suits readers that expect one document.
- JSONL has one JSON object per line. It suits readers that go line by line, and files that grow by appending.
- Pick the format the reader expects. Write it with the same streaming pattern: for JSONL, write each object, then a newline.

### Storing a payload

- Keep the useful fields as real entity fields, so they can be searched.
- If a copy of the payload helps support, cut it to a size limit and say so in the row.
- Never keep secrets, tokens or images in the copy.

## Guardrails
- Never create a new `ObjectMapper` inside a loop or on each service call.
- Never call `configure`, `enable` or `disable` on a mapper someone else uses.
- Never use `JSON.from(InputStream)` or `JSON.from(Reader)` for a file that can be large.
- Never write JSON by joining strings, even for "just one field".
- Never leave a reader, writer, parser or generator open on the error path.
- Never put a full JSON payload in an entity field.

## Examples

**Calling an outside API**
```groovy
String body = JsonMappers.MAPPER.writeValueAsString([accountNumber: [value: accountNumber], trackingNumber: trackingNumber])
HttpResponse<String> reply = client.send(requestWith(body), HttpResponse.BodyHandlers.ofString())
Map replyMap = JsonMappers.MAPPER.readValue(reply.body(), Map.class)
```

**Wrong, then right**
```groovy
String body = '{"sku":"' + sku + '","qty":' + qty + '}'
```
```groovy
String body = JsonMappers.MAPPER.writeValueAsString([sku: sku, qty: qty])
```
The first breaks when `sku` holds a quote or a backslash. The second is always valid JSON.
