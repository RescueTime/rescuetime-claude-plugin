# RescueTime plugin for Claude

Connect Claude to RescueTime so it can analyze tracked activity, project and task time, timesheet totals, productivity patterns, and work-life balance. If you manage teams in a RescueTime organization, Claude can also report your teams' time on team projects, tasks and clients when you ask for a team report. Claude can check your current focus state and, when you explicitly ask, start or stop RescueTime focus sessions and timers, or help categorize your Uncategorized activities and change the category of activities you name.

For Claude on the web or desktop, install the [RescueTime connector from the Claude directory](https://claude.ai/directory/www-rescuetime-com). The plugin below adds the same production connection plus RescueTime-specific guidance to Claude Code.

## Install from the Claude plugin marketplace

Add this repository as a marketplace, then install the RescueTime plugin:

```sh
claude plugin marketplace add RescueTime/rescuetime-claude-plugin
claude plugin install rescuetime@rescuetime
```

Restart Claude Code after installation. Use `/mcp` if Claude asks you to complete RescueTime authentication before the first request involving your data.

For direct development loading, clone this repository and run:

```sh
claude --plugin-dir /path/to/rescuetime-claude-plugin
```

Run `/reload-plugins` after local edits.

## Example prompts

- Review last week's time by activity and project, then suggest balanced time blocks for this week.
- Draft a client-ready status update from my project time this month.
- Summarize my team's project time this month by member.
- Start a 30-minute focus session named Q3 Analysis.
- Find my Uncategorized activities from the last three weeks and suggest categories for them.

## Connection and permissions

The plugin connects only to RescueTime's production MCP endpoint:

```text
https://www.rescuetime.com/mcp
```

Connecting requires RescueTime sign-in. Reading tracked time and focus state is separate from the additional permissions used to start or stop focus sessions and timers, to categorize Uncategorized activities, and to change the category of activities you name. Write actions should occur only after an explicit request; Claude proposes category changes and applies them only after you approve them.

Timesheet tools work in a personal mode (your own time) or a team mode that Claude must ask for explicitly. Team mode needs a manager, administrator or owner role in your RescueTime organization, covers only team projects, tasks and clients across the teams you manage, and names whose time each record is.

The permission to categorize Uncategorized activities also shares your occupation, job title, industry, and company name (when set in RescueTime), along with examples of activities you have categorized yourself, so Claude can suggest fitting categories.

For setup details, data limits, permissions, and troubleshooting, see the [RescueTime MCP guide](https://www.rescuetime.com/mcp-guide). RescueTime's [privacy policy](https://www.rescuetime.com/privacy), [terms of service](https://www.rescuetime.com/tos), and [help center](https://help.rescuetime.com/) are also available online.

ChatGPT users can install the [RescueTime plugin from the ChatGPT plugin directory](https://chatgpt.com/plugins/plugin_asdk_app_6a9051cd5014819188709e4adad2839e).
