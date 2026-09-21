# RescueTime agent plugin

This package connects Codex or Claude Code to RescueTime's remote MCP server. It adds guidance for interpreting tracked-time data and for explicitly requested control of focus sessions and timers. Connecting a client session requires RescueTime sign-in (OAuth) or an MCP API key; API keys provide read-only access. RescueTime reference documentation is public data available to any authenticated connection.

## Directory listing content

Canonical copy for the plugin directory forms (Claude directory, ChatGPT plugin directory).
Paste from here so every listing stays consistent.

### Description

> RescueTime is a time management and project tracking tool that helps you optimize your
> days and measure your results. It offers attention management features, day planning
> automations, and rich reporting. Connecting to an agent extends those automations across
> your whole work environment and grounds your project status reports, work-life balance
> check-ins, and productivity patterns in real tracked data.

### Example use cases

> Start a 45-minute focus session so I can finish this proposal without distractions.
>
> Build a summary of where my time went this week, broken down by project and category, and compare it with last week.
>
> Look at my last three weeks of tracked time and recommend specific changes to improve my work-life balance.
>
> Draft a status update for my client project from the time I logged against it this month.

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
