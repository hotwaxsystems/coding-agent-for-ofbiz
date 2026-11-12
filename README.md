# Coding agent for Apache OFBiz

A coding agent for [Apache OFBiz](https://ofbiz.apache.org/) development.
It gives an AI coding agent the OFBiz knowledge it needs to write code that
fits the framework.

This is not an Apache Software Foundation project.
Apache OFBiz, OFBiz and Apache are trademarks of the Apache Software Foundation.

## What is inside

- `agents/ofbiz-developer.md` defines the OFBiz developer agent. It picks the
  right skill for each kind of work.
- `agents/ofbiz-business-analyst.md` defines the business analyst agent. It
  writes the business process story and checks each activity against OFBiz
  trunk as an overlap or a gap, before any design or code. It follows HEMP,
  the method by David E. Jones, who founded OFBiz.
- `skills/` holds 39 skills, one per area of OFBiz: entities, services,
  screens, forms, data, ECAs, Groovy, Java, security, tests and more.
- `SKILLS_SUMMARY.md` lists every skill and what it covers.

## Where the content comes from

The skills come from the `ai-agent-skills` plugin in
[apache/ofbiz-plugins](https://github.com/apache/ofbiz-plugins/tree/trunk/ai-agent-skills),
copied at commit `010cd7e1319340e335002fb897231fc63b575c3f`.
We develop them further here. Improvements go back to the Apache OFBiz
community as pull requests to apache/ofbiz-plugins.

## Install in Claude Code

```bash
claude plugin marketplace add hotwaxsystems/coding-agent-for-ofbiz
claude plugin install coding-agent-for-ofbiz@coding-agent-for-ofbiz
```

Other agents can read the `skills/` folder directly. Copy or link it into the
skills folder your agent reads, such as `.agents/skills/`.

## Contributing

Open a pull request. Keep each change small and on one topic.
Do not add customer names, customer data or private project material.

## License

Apache License 2.0. See [LICENSE](LICENSE) and [NOTICE](NOTICE).
