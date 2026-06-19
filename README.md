# Datadog Grok Build Plugin

Query your Datadog data directly from Grok Build using natural language. Ask about logs, metrics, traces, dashboards, monitors, and more.

## What you need

- A [Datadog](https://www.datadoghq.com/) account
- [Grok Build](https://x.ai/cli) CLI

## Getting started

1. Install the plugin from the [Grok Build marketplace](https://github.com/xai-org/plugin-marketplace):

   ```
   /plugin install datadog
   ```

2. Run `/ddsetup` in the Grok Build chat to connect the plugin to your Datadog account. The agent will guide you through selecting the correct Datadog MCP domain.

> If you skipped setup, or want to change the domain later, run `/ddsetup` again. The agent will guide you through it.

## Using the plugin

Once connected, just ask the agent anything about your Datadog data:

```
Show me error logs from the last hour
```

```
What monitors are currently alerting?
```

```
Find traces for service "api-gateway" with latency > 500ms
```

```
List my dashboards
```

## Can't connect?

**Never connected before?** Run the `/ddsetup` command. It will help you provide the correct Datadog MCP domain and set up the MCP server.

**Was working before but stopped?** Run the `/ddconfig` command. It will check your site, authentication status, and network access to help diagnose the issue.

## Changing settings

The plugin provides a few commands you can run in the agent to manage configuration:

- `/ddconfig` — change your Datadog site or switch organizations
- `/ddtoolsets` — enable or disable groups of tools

## Advanced usage

### Key authentication

Instead of OAuth, you can authenticate using a Datadog API key and application key. Set all three environment variables before starting Grok Build:

```bash
DD_MCP_DOMAIN=your-mcp-domain \
DD_API_KEY=your-api-key \
DD_APPLICATION_KEY=your-application-key \
grok
```

The `DD_MCP_DOMAIN` value must be the MCP domain (e.g. `mcp.datadoghq.com`, `mcp.us3.datadoghq.com`, `mcp.datadoghq.eu`), not a URL — do not include `https://`. When using key authentication, `/ddsetup` is not required — the plugin connects directly.

### Environment variable overrides

The plugin stores its configuration as shell-style defaults in its registration file. You can override those defaults by setting the environment variables directly before starting Grok Build:

- `DD_MCP_DOMAIN` — overrides the Datadog MCP domain. If set, the plugin uses this value regardless of what `/ddsetup` or `/ddconfig` configured.
- `DD_MCP_TOOLSETS` — overrides the enabled toolsets (comma-separated). If set, the plugin uses this value regardless of what `/ddtoolsets` configured.

When these variables are set, `/ddsetup`, `/ddconfig`, and `/ddtoolsets` still edit the defaults in the registration file, but those defaults won't take effect until the variables are removed.

## Good to know

- By default, authentication is handled via OAuth in your browser. Key authentication is also [supported](#key-authentication).
- No Datadog credentials are sent to the AI model provider.

## Support

- [Datadog MCP Server Documentation](https://docs.datadoghq.com/mcp_server/)

## Legal

See the [LICENSE](LICENSE) and [NOTICE](NOTICE) files included with this plugin.

For details on how Datadog handles your data, see the [Datadog Privacy Policy](https://www.datadoghq.com/legal/privacy).
