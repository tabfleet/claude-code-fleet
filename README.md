# Tabfleet Fleet Pane for Claude Code

A Claude Code pane, status line and commands for the [Tabfleet](https://tabfleet.com) cloud browsers Claude launches:

- `/fleet` opens a pane listing your active browsers with minutes left, a live view link, and Close buttons.
- `/fleet-watch` shows the newest active browser's screen inside the pane, refreshed every couple of seconds.
- `/fleet-close-all` closes every active browser in the workspace.
- While browsers are running, the status line shows how many and the minutes left.
- When a Claude Code session ends, it closes the browsers that session launched, so unused minutes go back to your allowance. Browsers launched elsewhere stay open.

### What the fleet pane runs and sends

The pane calls Tabfleet tools itself, without the model asking, only through this plugin's Tabfleet server (`https://tabfleet.com/`):

| Tool | When |
| --- | --- |
| `list_sessions`, `get_usage` | When you run `/fleet`, `/fleet-watch` or `/fleet-close-all`, press Refresh, after any Tabfleet tool call, and every 20 seconds while a browser is running |
| `get_live_view` | After the model launches a browser, or when you press Live view; asks for a 15-minute view-only link |
| `browser_screenshot` | Every 1.5 seconds while you are watching a browser, as a PNG |
| `close_browser` | When you press Close or run `/fleet-close-all`, and at session end for browsers that session launched |

What it sends is limited to browser session IDs and those fixed arguments. It does not read or send your conversation, files, or anything else from your machine, it starts no programs, and it writes no files: screenshots are drawn in the pane from memory.

Its hooks see Claude Code's tool calls only to notice Tabfleet ones. Every call passes through unchanged. After `launch_browser` the pane remembers the new session and fetches its live view link. After any other Tabfleet tool it refreshes the list. Other tools are ignored.

## Install

```
/plugin marketplace add tabfleet/claude-plugin
/plugin install tabfleet-fleet@tabfleet
```

Then run `/mcp`, choose the plugin's Tabfleet server, and sign in. It works on its own, or alongside the [Tabfleet Browser](https://github.com/tabfleet/claude-plugin) plugin, which adds a skill that guides Claude through using the browser.

This plugin runs only in Claude Code. See the [Tabfleet documentation](https://tabfleet.com/docs), [terms](https://tabfleet.com/terms), and [privacy notice](https://tabfleet.com/privacy). For support or security reports, email [support@tabfleet.com](mailto:support@tabfleet.com).
