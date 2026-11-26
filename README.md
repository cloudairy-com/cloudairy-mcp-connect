# Cloudairy MCP connector

This repository holds configuration and documentation only. The Cloudairy MCP server is a hosted service run by Cloudairy LLC; there is no server code here and nothing to install or run locally.

- Server URL: `https://mcp.cloudairy.com/mcp`
- Transport: streamable HTTP
- Sign-in: OAuth 2.1 with PKCE and dynamic client registration. No API key.
- MCP Registry name: `com.cloudairy/cloudairy` (version 1.0.0)
- Documentation: https://cloudairy.com/mcp

## What it does

Preview diagrams in chat and save them as editable Cloudairy boards; read and search your boards.

You need a Cloudairy account. The free plan works. Sign up at https://cloudairy.com.

## Tools

| Tool | What it does | Writes data |
|---|---|---|
| `list_workspaces` | Lists the workspaces you belong to | No |
| `list_artifacts` | Lists boards and other artifacts in a workspace | No |
| `search_artifacts` | Searches your artifacts by text | No |
| `get_artifact` | Reads one artifact | No |
| `get_artifact_guide` | Returns the diagram types and content rules Cloudairy accepts | No |
| `preview_diagram` | Shows a diagram as a preview in the chat. Saves nothing | No |
| `create_artifact` | Saves a new board in your workspace | Yes, creates a new board |
| `generate_artifact` | Cloudairy AI designs the diagram and saves it. Uses the workspace's AI credits | Yes, creates a new board |

The connector never edits or deletes existing boards.

## Permissions

When you connect, Cloudairy asks you to approve three permissions:

- Read your artifacts
- Create new artifacts
- Use Cloudairy AI

## Set-up

Every client below connects to the same URL. On first use the client opens a browser window where you sign in to Cloudairy and approve the permissions.

### Claude (web, desktop, mobile)

1. Open **Customize > Connectors**.
2. Click **+ Add**, then **Add custom connector**.
3. Name it `Cloudairy` and enter `https://mcp.cloudairy.com/mcp`.
4. Continue through the detected sign-in settings and click **Add**, then sign in to Cloudairy.

On Team and Enterprise plans an owner adds it first under **Organization settings > Connectors**.

### ChatGPT

1. Open https://chatgpt.com/plugins.
2. Select the plus button, then **Add custom MCP server**.
3. Enter the name `Cloudairy` and the server URL `https://mcp.cloudairy.com/mcp`.
4. Keep OAuth as the sign-in method, accept the notice, and create it. Then sign in to Cloudairy.

Availability depends on your ChatGPT plan and workspace policy.

### Claude Code

```bash
claude mcp add --transport http cloudairy https://mcp.cloudairy.com/mcp
```

Then run `/mcp` inside Claude Code, select `cloudairy`, and sign in.

### Cursor

Add to `~/.cursor/mcp.json` (all projects) or `.cursor/mcp.json` (one project):

```json
{
  "mcpServers": {
    "cloudairy": {
      "url": "https://mcp.cloudairy.com/mcp"
    }
  }
}
```

This repository is also a Cursor plugin (`.cursor-plugin/plugin.json` and `mcp.json`).

### VS Code

Add to `.vscode/mcp.json` in your workspace, or to the `mcp.json` of your user profile:

```json
{
  "servers": {
    "cloudairy": {
      "type": "http",
      "url": "https://mcp.cloudairy.com/mcp"
    }
  }
}
```

Or from a terminal:

```bash
code --add-mcp '{"name":"cloudairy","type":"http","url":"https://mcp.cloudairy.com/mcp"}'
```

### Gemini CLI

Install this repository as an extension, using this repository's GitHub URL:

```bash
gemini extensions install https://github.com/cloudairy-com/cloudairy-mcp-connect
```

Or add the server directly:

```bash
gemini mcp add --transport http cloudairy https://mcp.cloudairy.com/mcp
```

Or add to `~/.gemini/settings.json`:

```json
{
  "mcpServers": {
    "cloudairy": {
      "url": "https://mcp.cloudairy.com/mcp",
      "type": "http"
    }
  }
}
```

Then run `/mcp auth cloudairy` in Gemini CLI to sign in.

### Kiro

Add to `~/.kiro/settings/mcp.json` (all workspaces) or `.kiro/settings/mcp.json` (one workspace). The same file is in [`examples/kiro-mcp.json`](examples/kiro-mcp.json).

```json
{
  "mcpServers": {
    "cloudairy": {
      "url": "https://mcp.cloudairy.com/mcp"
    }
  }
}
```

Or use the install link:

[![Add to Kiro](https://kiro.dev/images/add-to-kiro.svg)](https://kiro.dev/launch/mcp/add?name=cloudairy&config=%7B%22url%22%3A%22https%3A%2F%2Fmcp.cloudairy.com%2Fmcp%22%7D)

### Cline

In the Cline panel open **Customize > MCP** and click **Add Remote Server** (older versions: **MCP Servers > Configure > Remote Servers**). Enter the name `cloudairy` and the URL `https://mcp.cloudairy.com/mcp`, choose **Streamable HTTP**, and add the server. Then click **Authenticate** on the `cloudairy` row and approve the sign-in in your browser.

Or add to `cline_mcp_settings.json` (**Customize > MCP > Edit Configuration**; the file is at `~/.cline/data/settings/cline_mcp_settings.json`):

```json
{
  "mcpServers": {
    "cloudairy": {
      "type": "streamableHttp",
      "url": "https://mcp.cloudairy.com/mcp",
      "disabled": false,
      "autoApprove": []
    }
  }
}
```

Agent-readable steps are in [`llms-install.md`](llms-install.md).

### Windsurf

Add to your `mcp_config.json`:

```json
{
  "mcpServers": {
    "cloudairy": {
      "url": "https://mcp.cloudairy.com/mcp",
      "transport": "http"
    }
  }
}
```

Then click **Authenticate** on the server and sign in.

### Other MCP clients

Any client that supports remote MCP servers over streamable HTTP with OAuth can connect to `https://mcp.cloudairy.com/mcp`. The registry entry is in [`server.json`](server.json).

## Example prompts

- "Using Cloudairy, show me a flowchart of an employee onboarding process."
- "Draw an AWS architecture for a web app with CloudFront, an Application Load Balancer, ECS Fargate and RDS, and save it to my workspace."
- "List my Cloudairy workspaces and my five most recent boards."
- "Find my Cloudairy board about the login flow and explain it."

## Disconnect

In Cloudairy, open **Profile settings > Connected apps** and remove the client. You can do this at any time. You can also remove the server from the client's own settings.

## Files in this repository

| File | Purpose |
|---|---|
| `gemini-extension.json` | Gemini CLI extension manifest |
| `.cursor-plugin/plugin.json`, `mcp.json` | Cursor plugin manifest and its MCP server entry |
| `llms-install.md` | Install steps for the Cline agent |
| `server.json` | Copy of the MCP Registry entry |
| `examples/kiro-mcp.json` | Kiro configuration example |
| `assets/` | Cloudairy logo files |

## Privacy, terms and support

- Privacy policy: https://cloudairy.com/privacy-policy#ai-connectors
- Terms: https://cloudairy.com/terms-and-conditions
- Trust center: https://cloudairy.com/trust-center
- Support: support@cloudairy.com

## Licence

The MIT licence in [`LICENSE`](LICENSE) covers the files in this repository only; use of the Cloudairy service is under the Cloudairy [Terms](https://cloudairy.com/terms-and-conditions).
