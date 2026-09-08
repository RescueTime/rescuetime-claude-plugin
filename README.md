# RescueTime agent plugin

This package connects Codex or Claude Code to RescueTime's remote MCP server. It adds guidance for interpreting tracked-time data and for explicitly requested control of focus sessions and timers. Connecting a client session requires RescueTime sign-in (OAuth) or an MCP API key; API keys provide read-only access. RescueTime reference documentation is public data available to any authenticated connection.

## Install from a checkout

Replace `/path/to/rescuetime` with the repository root.

### Codex

```sh
codex plugin marketplace add /path/to/rescuetime
codex plugin add rescuetime@personal
```

Restart Codex after installation. On the first user-data request, complete the RescueTime sign-in and consent flow opened by the client.

Start a new Codex session after changing or reinstalling the plugin so updated skills and tools are loaded.

### Claude Code

```sh
claude plugin marketplace add /path/to/rescuetime
claude plugin install rescuetime@rescuetime
```

Restart Claude Code after installation. Use `/mcp` if Claude asks you to complete authentication before the first user-data request.

For direct development loading without installing:

```sh
claude --plugin-dir /path/to/rescuetime/plugins/rescuetime
```

Run `/reload-plugins` after local edits.

## MCP endpoint

The packaged plugin continues to connect to the production endpoint:

```text
https://www.rescuetime.com/mcp
```

The same endpoint can be used by any Streamable HTTP MCP client. For Claude on web or desktop, install the published [RescueTime connector from the Claude directory](https://claude.ai/directory/www-rescuetime-com) instead of adding the URL manually. For ChatGPT, install the published [RescueTime plugin from the ChatGPT plugin directory](https://chatgpt.com/plugins/plugin_asdk_app_6a9051cd5014819188709e4adad2839e). See the [RescueTime MCP guide](https://www.rescuetime.com/mcp-guide) for setup, permissions, limits, and example workflows.

## ChatGPT developer-mode registration

For normal ChatGPT use, install the published plugin from the directory link above — developer-mode
registration is only needed for developing and testing this package.

Register the production endpoint in ChatGPT developer mode using the full URL above. After registration, ChatGPT
assigns the connection a technical ID beginning with `asdk_app`. The distributable package records that ID
in `.app.json` and references it from `.codex-plugin/plugin.json`; `.app.json` does not contain the MCP endpoint
itself.

Keep the existing `mcpServers` entry when adding the ChatGPT app mapping. It remains the direct connection used by
Codex and Claude-compatible local installation.

## Developing against local Rails

Do not replace the production endpoint in the distributable package with a developer URL.

- A local Codex/Claude development package may use a separate `.mcp.json` that points directly to the local Rails
  MCP route.
- A local ChatGPT development package needs a separate developer-mode connection and a separate `.app.json`
  containing that connection's technical ID. Expose local Rails through OpenAI's secure MCP tunnel or RescueTime's
  internal development tunnel; hosted ChatGPT cannot connect directly to `localhost`.
- Never publish or commit a personal tunnel connection ID in the production `.app.json`.
