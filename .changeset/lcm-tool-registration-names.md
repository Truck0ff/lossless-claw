---
"@martian-engineering/lossless-claw": patch
---

Pass `{ name: "..." }` as the second argument to each `api.registerTool(...)` call so OpenClaw's loader can disambiguate the four `lcm_*` tool factories.

Without this, the loader (which only captures `entry.names` from `opts.name`/`opts.names` or a static `tool.name` for object-style registrations) leaves all four factory entries with `entry.names = []` and they all inherit the manifest's `contracts.tools = ["lcm_grep", "lcm_describe", "lcm_expand", "lcm_expand_query"]` as their fallback name set. Once OpenClaw 5.x landed descriptor caching for plugin tools, the dispatcher's `find()` started returning the *first* registered factory for any of the four lookups — `lcm_grep` always won, and the other three threw `plugin tool runtime missing (lossless-claw): <name>` from the inner name-match.

This is a packaging/registration fix only; no behavioural change to any tool.
