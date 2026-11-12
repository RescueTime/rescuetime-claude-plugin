---
name: rescuetime
description: Analyze a user's RescueTime data, report their timesheets or their managed teams' timesheets, explicitly control their current focus session or timer, or categorize their activities through RescueTime MCP. Use for recent tracked activity, time allocation, projects, tasks or clients, timesheet totals and status reports, team timesheet reports for managers, categories, productivity, focus state, daily or weekly patterns, bounded comparisons, requests to start or stop focus, or requests to fix Uncategorized activities or change an activity's category.
---

# RescueTime

Use the RescueTime MCP tools to answer questions about the user's tracked time. Treat the records as private behavioral data and report what was observed without judging whether the user's choices were good or bad.

## Choose the operation

- Use `reference_documentation` for taxonomy, productivity, overview, and default category definitions. Its content is public RescueTime data, available on any connection without extra scopes.
- Use `recent_activity` for a compact timeline. Request a `day` chunk for one calendar day at five-minute resolution, or a `week` chunk for seven days at hourly resolution. A week can follow the user's natural calendar week or use an explicit seven-day start/end range. Prefer a day unless the question needs a broader pattern.
- Use `timesheet_report` for timesheet questions: status reports, billing, utilization, and how much time went to a client, project or task. It returns totals already filtered and grouped, per day, week, month or the whole range. Use `project_time_history` only when the question needs the individual blocks in time order (when work happened, its sequence, gaps or overlaps) or a regrouping the report does not offer. Both need `mode`: `personal` or `team`. Use `list_projects`, `list_tasks` and `list_clients`, with the same `mode`, for ids and names.
- Use `taxonomy_rollup` for totals or calendar comparisons by activity, category, overview, or productivity.
- Use `get_focus_state` to determine whether a focus zone, focus session, or timer is active.
- Use `start_focus_session`, `start_timer`, and `stop_focus` only when the user explicitly asks to change focus state.
- Use `uncategorized_activities`, `list_categories`, and `categorization_context` to propose categories, and `categorize_uncategorized_activities` or `recategorize_activities` only to apply changes the user has approved.

If a user-data tool requests authentication, let the host complete the RescueTime OAuth flow. Never ask the user to paste an OAuth token or API key into the conversation. A user who deliberately manages credentials locally may configure a RescueTime MCP API key in the host's secure credential facility.

## Interpret results

- Preserve the returned time zone and state the actual start and end bounds.
- Distinguish automatically tracked activity from manually or automatically assigned project time.
- Explain that personal category and productivity overrides take precedence over RescueTime defaults.
- Treat productivity scores as the user's configured classification, not an objective assessment.
- When a response may be limited, state the limit and avoid implying that the result is complete.
- Aggregate before presenting private detail unless the user explicitly asks for the detail.
- Personal timesheet results are the user's own records, on their personal projects and on the team projects assigned to them. Team results are other members' time; say whose time each figure is.

## Work efficiently

- Resolve relative dates using the user's returned RescueTime time zone.
- Do not request a full year when a day or week answers the question.
- Fetch recent activity as a day or week chunk. Use `today` or `yesterday` for common day requests. For weeks, use `this week`, `last week`, `N weeks ago`, `week of DATE`, or an explicit seven-day `start_date` and `end_date`. Keep taxonomy rollups within 45 inclusive days and project history within 366 inclusive days.
- For period comparisons, make equivalent calls with matching interval lengths.
- Use public reference documentation only when its definitions materially improve the answer.

## Report timesheets in the right mode

Every timesheet tool takes a required `mode`:

- `personal`: the user's own time records, on their personal projects, tasks and clients and on the team ones assigned to them.
- `team`: time other members logged on team projects, tasks and clients across the teams the user manages. It needs a manager, administrator or owner role; a member gets a clear refusal. Members' time on their personal projects is never part of a team report.

Never add personal and team results into one total.

1. Pick the mode from what the user asks about: their own time is `personal`; their team's or a member's time is `team`. Ask when it is unclear.
2. Use `list_projects`, `list_tasks` and `list_clients` in that mode for ids and names. They never include the hidden placeholder task; time logged with no task is reported without one.
3. Start with `timesheet_report`; it answers most timesheet questions with totals per day, week, month or the whole range. Do not total `project_time_history` blocks yourself when the report can give the figure. Reach for `project_time_history` only for the order of work through the day or a breakdown the report cannot produce. In team mode every block names its user and team, and every row names the users and teams it is split by (`group_team_by`, per user by default).
4. Team mode returns reviewed time only. Pass `include_drafts: true` only when the user asks about unreviewed time, and keep it labelled. Personal mode always returns drafts separately as unreviewed.
5. Pass `team_ids` or `user_ids` only for teams and members the user manages; an id outside that is rejected by id. Treat a refusal as the answer, never retry in the other mode.
6. Billable hours and amounts appear only when RescueTime lets the user see rates; do not estimate them otherwise.
7. A `task_ids` filter on `timesheet_report` also counts time logged with no task, and a `client_ids` filter also counts time on projects with no client, as the web report does by default. Pass `include_no_task: false` or `include_no_client: false` when the user wants only the listed ones. The result says which applied (`no_task_time_included`, `no_client_time_included`); say so when you report the total.

Timesheet tools use normal time-data access in both modes; team mode is gated by the user's role, not by an extra scope.

## Control focus safely

1. Call `get_focus_state` before every focus-state change.
2. If a session or timer is already active, explain that state instead of starting another.
3. If starting a session will replace a focus zone, tell the user before acting.
4. Never start or stop focus merely because you recommended it. Require an explicit user request.
5. Pass the active state's `start_time_epoch` to `stop_focus` so a stale request cannot stop a newer session.
6. Report the focus state returned after the write. When stopping a timer, mention that linked project time may have been finalized.

Reading current focus state uses normal time-data access. Focus controls require RescueTime OAuth
with `focustime_data`; MCP API keys can inspect focus state but remain read-only.

## Categorize activities on request

1. Call `uncategorized_activities` for the last three weeks (its default) to find what RescueTime has not categorized.
2. Call `list_categories` for the categories the user can assign, and `categorization_context` for their occupation, job title, industry, and earlier categorizations.
3. Propose a table to the user: activity, suggested category, and a short reason drawn from the activity's details or the user's earlier choices. Mark guesses you are unsure of.
4. Call `categorize_uncategorized_activities` only after the user approves, with only the approved rows. Report what was updated, unchanged, or rejected.
5. Use `recategorize_activities` only for activities the user names, whatever their current category. Its results return each previous category: offer `reset_to_default` (when `previous_was_override` is false) or the previous category id as the undo.

Both write tools assign only the category; the activity then uses the category's productivity score. Mention this when the user had given that activity its own score. Reports update right away; precomputed daily summaries catch up in the background.

`categorization_context` is personal data: use it only to suggest categories, and do not repeat it back unless the user asks.

Categorization writes require RescueTime OAuth with `categorize_data` (Uncategorized activities only) or `recategorize_data` (any named activity). Listing Uncategorized activities and categories needs only `category_data`; MCP API keys cannot change categories.
