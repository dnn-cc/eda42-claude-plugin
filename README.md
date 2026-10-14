# EDA42 plugin for Claude

Design electronic schematics and PCBs in your [EDA42](https://eda42.cc/)
account from Claude. Claude reasons and calls EDA42's MCP tools; EDA42
owns the design, storage, checks and exports.

## Install in Claude Code

```sh
claude plugin marketplace add dnn-cc/eda42-claude-plugin
claude plugin install eda42@eda42
```

Or, inside Claude Code, run `/plugin marketplace add dnn-cc/eda42-claude-plugin`
and then `/plugin install eda42@eda42`.

Then run `/mcp`, choose the EDA42 server and sign in. The plugin adds:

- the hosted MCP server `https://eda42.cc/mcp`, and
- the `design-with-eda42` skill, which loads current design guidance from
  your authenticated connection.

## Use in Claude.ai or Claude Desktop

Add a custom connector with the URL `https://eda42.cc/mcp` and sign in.
Existing connections to `https://nomoreda.com/mcp` remain supported during the
domain transition; reconnect only when you choose to move that connection.

## Access and permissions

- Sign-in uses OAuth; Claude handles sign-in and token refresh. This
  repository contains no credentials, and no API key is needed.
- On the EDA42 consent page, choose **All my projects** or **Selected
  projects**, and **Read projects** (default) or **Read and edit projects**.
- Your existing project permissions still apply. Disconnect at any time in EDA42 under
  **Preferences → Connections**; reconnect to change access.
- Every project operation names an explicit project. Ask Claude to list your
  projects first.

Engineering results require your review. Design checks and native exports do
not constitute fabrication approval.

## Support

- Website: <https://eda42.cc/>
- Support: <https://eda42.cc/support/>
- Privacy: <https://eda42.cc/privacy/>
- Terms: <https://eda42.cc/terms/>

This repository is published automatically from the EDA42 source with each
release. Changes made here directly are overwritten; contact support instead
of opening pull requests.
