# Installing the Cloudairy MCP server (instructions for AI agents)

Cloudairy is a hosted remote MCP server. Do not clone this repository, install packages, build anything, or start a local process. There is no API key and no environment variable to set.

## Facts

- Server name: `cloudairy`
- URL: `https://mcp.cloudairy.com/mcp`
- Transport: streamable HTTP
- Authentication: OAuth 2.1 with PKCE and dynamic client registration. The user signs in to Cloudairy in a browser window. Never ask the user for a password, token or API key.
- Requirement: a Cloudairy account (the free plan works). Sign-up: https://cloudairy.com

## Steps for Cline

1. Open the Cline MCP settings file `cline_mcp_settings.json` (in the IDE: Customize > MCP > Edit Configuration, or in older versions MCP Servers > Configure > Configure MCP Servers; the file is at `~/.cline/data/settings/cline_mcp_settings.json`).
2. Add this entry under `mcpServers`. Keep every existing entry unchanged.

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

3. Save the file. The `type` field must be `streamableHttp`; without it Cline uses the legacy SSE transport.
4. Tell the user that a browser window opens for Cloudairy sign-in, and ask them to sign in and approve the permissions (read your artifacts, create new artifacts, use Cloudairy AI). If no window opens, ask them to start authorization for the `cloudairy` server from the MCP Servers panel.
5. Do not add tools to `autoApprove` unless the user asks. `create_artifact` and `generate_artifact` save new boards, and `generate_artifact` uses the workspace's AI credits.

## Check that it works

After sign-in the server lists 8 tools: `list_workspaces`, `list_artifacts`, `search_artifacts`, `get_artifact`, `get_artifact_guide`, `preview_diagram`, `create_artifact`, `generate_artifact`.

Call `list_workspaces`. A list of the user's workspaces means the set-up is complete.

## If it fails

- HTTP 401 before sign-in is expected. It starts the OAuth flow.
- If sign-in does not complete, remove the `cloudairy` entry, add it again, and repeat the sign-in.
- Help: https://cloudairy.com/mcp or support@cloudairy.com

## Remove

Delete the `cloudairy` entry from `cline_mcp_settings.json`. The user can also revoke access in Cloudairy under Profile settings > Connected apps.
