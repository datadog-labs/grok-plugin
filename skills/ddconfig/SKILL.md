---
name: ddconfig
description: Configures or troubleshoots the plugin's Datadog MCP server. Use when the user wants to change the Datadog domain, switch organizations, or when the server was previously configured but is not responding.
---

## Datadog MCP Server

You need to identify the plugin's Datadog MCP server as explained in [references/mcp-settings.md](references/mcp-settings.md). Always use this specific MCP server.

## Shared reference

Read [references/mcp-settings.md](references/mcp-settings.md) before proceeding. It contains the `datadog-server-state` check.

## Entry flow

Check the `datadog-server-state` (see `mcp-settings.md`). Use the `datadog://mcp/whoami` resource on the plugin's Datadog MCP server as the MCP call (do NOT use any other Datadog MCP server). Do not output anything until the `datadog-server-state` and resource content are available, and proceed based on the results:

- **datadog-server-state=working** and **valid content** — without any preamble, immediately show the user their current connection (from `whoami`): user name and email, organization name, and site (the `dd_site` value). Then let the user choose between [using a different Datadog MCP domain or site](#domain-flow) or [switching to a different Datadog organization](#organization-flow).
- **datadog-server-state=not-working** or **not valid content** — without any preamble, tell the user the server is configured but not working and go to the [Troubleshooting Flow](#troubleshooting-flow).

When communicating with the user below, describe the server state and actions in plain language. Do not reveal what was checked, what was found, or any implementation details like file contents or variable values.

## Troubleshooting Flow

The server is configured but not responding. Present the user with the likely causes — do not follow these sequentially, show them all and use judgment:

- **Domain issue.** Tell the user to follow the instructions in the `Configure the MCP Domain` section of [references/mcp-settings.md](references/mcp-settings.md).

- **Authentication.** The authentication may have expired or was never completed, and the user needs to follow these steps:

  1. Run the command `/plugins` and press `r` to reload all plugins.
  2. Run the command `/mcps`, select the Datadog MCP server, and press `i` to authenticate it (this step may not be needed if you are already signed in).

- **Network or access.** The user's network may be blocking the connection, or their Datadog account may not have API access, like not having the `MCP Read` permission.

If the domain looks wrong, suggest running the [Domain Flow](#domain-flow) to correct it.

## Domain Flow

Changes the Datadog MCP domain the server connects to.

1. Show the current domain information (from `whoami` → `dd_site` if available, or from the current domain in the registration file — see `mcp-settings.md` for the file format). Present it in plain language (e.g. "the plugin is currently connected to …") — follow the "Stay on script" rule in `mcp-settings.md`.

2. **Ask for the new domain.** Present the available sites and their MCP domains from `mcp-settings.md`, and ask which domain to switch to. The user may respond with an MCP domain directly, a site code, a URL, or something else — use the mapping rules in `mcp-settings.md` to resolve the answer. Ask for clarification if ambiguous.

   Follow the "Stay on script" rule in `mcp-settings.md`. In particular, do not preview the follow-up instructions from step 4 below (reload, re-authenticate, etc.) — that step emits them verbatim at the right moment.

3. Edit the domain in the registration file following the editing rule in `mcp-settings.md`.

4. Tell the user the domain has been changed and to follow these steps:

   1. Run the command `/plugins` and press `r` to reload all plugins.
   2. Run the command `/mcps`, select the Datadog MCP server, and press `i` to authenticate it (this step may not be needed if you are already signed in).

## Organization Flow

Switches to a different Datadog organization. The agent cannot do this automatically — the user must select the target organization in the browser.

Ask the user if they want to use an organization on the same domain or on a different domain.

- If on the same domain:
  - The user needs to reauthenticate and, during sign-in, choose the target organization in the browser, using the following steps:

    1. Run the command `/plugins` and press `r` to reload all plugins.
    2. Run the command `/mcps`, select the Datadog MCP server, and press `i` to authenticate it, choosing the target organization during sign-in.

- If on a different domain:
  - Run the [Domain Flow](#domain-flow) telling the user to choose the target organization in the browser during sign-in.
