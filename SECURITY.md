# Security policy

`clawtouch-skills` is a collection of markdown documentation. There
is no executable code in this repository, so the traditional
"security vulnerability" reporting flow does not really apply.

That said — if you find:

- A skill that gives **dangerously wrong step-by-step instructions**
  (a sequence that could delete user data, send unintended messages,
  or perform destructive actions when run by an LLM agent), or
- A skill that includes content violating the [contributing
  guidelines](CONTRIBUTING.md) (prohibited topics, problematic
  language),

please open a GitHub issue with the `bug` label, **or** if you
prefer private disclosure, email `support@tinqiao.com` with the
subject prefix `[clawtouch-skills security]`. We'll respond within
a few business days.

For security issues in the companion code repositories (the actual
MCP server / HID firmware), please use their security policies:

- [`clawtouch-mcp` SECURITY.md](https://github.com/tinqiao-oss/clawtouch-mcp/blob/master/SECURITY.md)
- [`clawtouch-hid` SECURITY.md](https://github.com/tinqiao-oss/clawtouch-hid/blob/master/SECURITY.md)

## A note on autonomous-agent risk

These skills are loaded by an LLM agent that then drives real keyboard
and mouse input, often by reading the screen — which is exactly where an
agent can be steered by untrusted on-screen content (prompt injection)
or simply act on a misunderstanding. That is a deployment risk to plan
for, distinct from a bug in any skill file or in the companion code. For
the risk disclosure and the operator mitigations (dedicated/least-
privilege host, human-in-the-loop, network isolation, panic stop,
treating screen content as untrusted), see the **Autonomy & safety**
section in the
[`clawtouch-mcp` README](https://github.com/tinqiao-oss/clawtouch-mcp/blob/master/README.md).
