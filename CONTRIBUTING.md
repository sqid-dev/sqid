# Contributing to SQID

Thanks for your interest. SQID is in **early access**: we're opening the runtime and building the agent tools in public, so the most useful thing you can do right now is tell us what you'd build.

## Right now

- **Share a use case.** What would your agent, chat bot or app do with live visitor context? [Start a discussion](https://github.com/sqid-dev/sqid/discussions/categories/ideas).
- **Vote on the roadmap.** Tell us which framework you want tools for first (Vercel AI SDK, OpenAI Agents SDK, LangChain, Mastra, Claude Agent SDK).
- **Report a problem** with the docs or this README by [opening an issue](https://github.com/sqid-dev/sqid/issues/new/choose).

## When code lands

Each package will have its own setup notes. The general flow:

1. Open an issue or discussion first for anything bigger than a small fix, so we can agree on the approach.
2. Fork the repo and create a branch from `main`.
3. Keep pull requests focused: one change per PR, with a clear description of what and why.
4. Add or update tests and docs alongside the code.
5. Never commit secrets, API keys or real visitor data. Use the example `.env.example` values.

## Licensing of contributions

By contributing, you agree that your contribution is licensed under the license of the package you're changing: MIT for the SDK, MCP server, framework tools, connectors and examples, and AGPL-3.0 for the engine and gateway (see the [README](README.md#license)).

## Code of conduct

Everyone taking part is expected to follow our [Code of Conduct](CODE_OF_CONDUCT.md).
