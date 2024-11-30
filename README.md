# Troubleshooting Guide: Claude Desktop MCP-GitHub Integration on Windows

This guide documents the process of setting up and troubleshooting MCP-GitHub integration for Claude Desktop on Windows platform.

## Prerequisites
- Windows operating system
- Node.js and npm installed
- Claude Desktop application
- GitHub account with access token

## Common Issues and Solutions

### 1. Installation of MCP-GitHub Server
First, install the MCP-GitHub server globally:
```bash
npm install -g @modelcontextprotocol/server-github
```

Verify the installation:
```bash
npx @modelcontextprotocol/server-github --version
```
Expected output: "GitHub MCP Server running on stdio"

### 2. GitHub Token Setup
1. Go to GitHub Settings -> Developer Settings -> Personal Access Tokens
2. Choose "Tokens (classic)" (not Fine-grained tokens)
3. Required permissions:
   - `repo` (minimum requirement)
   - Additional permissions based on your needs

### 3. Configuration File Location and Setup

#### Important Note About Installation Paths
The configuration file location may vary depending on your installation. Common paths include:
```
C:\Users\[YourUsername]\AppData\Roaming\Claude\claude_desktop_config.json
# or
C:\Users\[YourUsername]\AppData\Local\AnthropicClaude\claude_desktop_config.json
```

To find your correct path:
1. Check both locations above
2. Create the configuration file in the directory that exists on your system
3. If both exist, try configuring in each location until successful

#### Configuration File Content

##### Official Configuration (using npx)
```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": [
        "@modelcontextprotocol/server-github"
      ],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "your-github-token"
      }
    }
  }
}
```

##### Alternative Working Configuration (using node)
If the official configuration doesn't work, try this alternative configuration that uses the direct node path:
```json
{
  "mcpServers": {
    "github": {
      "command": "node",
      "args": [
        "C:/Users/[YourUsername]/AppData/Roaming/npm/node_modules/@modelcontextprotocol/server-github/dist/index.js",
        "--debug"
      ],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "your-github-token"
      }
    }
  }
}
```

Important notes:
- The file MUST be named `claude_desktop_config.json`
- Different installation methods may use different config file locations
- Use forward slashes (`/`) in the path, not backslashes (`\`)
- Ensure proper JSON formatting with double quotes
- Replace `[YourUsername]` with your actual Windows username
- Replace `your-github-token` with your actual GitHub token

### 4. Common Errors and Solutions

#### Configuration File Not Found
- Check both possible installation paths mentioned above
- Ensure you're creating the config file in the correct location
- Verify the file name is exactly `claude_desktop_config.json`

#### Invalid Character Error
If you see a Windows Script Host error about invalid characters:
- Check path separators (use `/` instead of `\`)
- Ensure no escape characters in the path
- Try switching between the npx and node configurations

#### JSON Syntax Error
If you see "Could not load app settings":
- Verify JSON syntax
- Check for proper double quotes
- Ensure no trailing commas
- Validate JSON format using a JSON validator
- Confirm the file is named exactly as `claude_desktop_config.json`

#### Connection Issues
If the server isn't connecting:
- Verify the token is properly set
- Check if the MCP server is running (`npx @modelcontextprotocol/server-github --version`)
- If the npx configuration doesn't work, try the node configuration with full path
- Ensure the path to the server module is correct
- Try creating the config file in the alternate location if one method doesn't work

## Verification Process
1. Close Claude Desktop completely
2. Reopen the application
3. Check for successful connection
4. If issues persist, add `--debug` flag in the args array for more detailed logs

## Additional Tips
- Always use proper JSON formatting
- Double-check paths and usernames
- Ensure all directories exist
- Keep your GitHub token secure
- Consider using environment variables for sensitive information
- If the official npx configuration doesn't work, try the alternative node configuration
- Pay attention to the installation path differences and try both possible locations

## Contributing
If you've encountered and solved other issues, please feel free to contribute to this guide.

## Support
If you need additional help:
- Check Claude Desktop documentation
- Visit the GitHub repository
- Contact support for persistent issues