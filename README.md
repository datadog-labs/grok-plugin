# Datadog Grok Build Plugin

Query your Datadog data directly from Grok Build using natural language. Ask about logs, metrics, traces, dashboards, monitors, and more.

## What you need

- A [Datadog](https://www.datadoghq.com/) account
- [Grok Build](https://x.ai/cli) CLI (v1.0.5 or later, the latest version is recommended)

## Getting started

1. Install the plugin from the [Grok Build marketplace](https://github.com/xai-org/plugin-marketplace):

   ```
   /plugin install datadog
   ```

2. Type `/mcps` on the Grok Build command-line, select the **datadog** MCP server and authenticate. The agent will guide you through selecting the correct Datadog MCP domain. If you want to change the domain later, just tell the agent you want to change the Datadog domain (or run `/ddconfig`). The agent will guide you through it.

## Using the plugin

Once connected, just ask the agent anything about your Datadog data:

```
Show me error logs for the "checkout" service from the last hour
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

**Never connected before?** Type `/mcps` on the Grok Build command-line, select the **datadog** MCP server and authenticate. The agent will guide you through selecting the correct Datadog MCP domain.

**Was working before but stopped?** Run the `/ddconfig` command. It will check your site, authentication status, and network access to help diagnose the issue.

## Advanced usage

### Key authentication

Instead of OAuth, you can authenticate using a Datadog API key and application key. Set these environment variables before starting Grok Build:

```bash
DD_API_KEY=your-api-key \
DD_APPLICATION_KEY=your-application-key \
grok
```

### Datadog MCP Domain

Grok provides a UI for selecting the Datadog MCP Domain upon first use. This can be overridden by setting an environment variable.

- `DD_MCP_DOMAIN` — overrides the Datadog MCP domain that is manually selected.

Note that manual selection of the domain must be done at least one time to enable the plugin, but after that `DD_MCP_DOMAIN` will always override it.

## Good to know

- By default, authentication is handled via OAuth in your browser. Key authentication is also [supported](#key-authentication).
- No Datadog credentials are sent to the AI model provider.

## Support

- [Datadog MCP Server Documentation](https://docs.datadoghq.com/mcp_server/)

## Legal

See the [LICENSE](LICENSE) and [NOTICE](NOTICE) files included with this plugin.

For details on how Datadog handles your data, see the [Datadog Privacy Policy](https://www.datadoghq.com/legal/privacy).
