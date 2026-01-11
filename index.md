# Revit AI Automation MCP Service — Setup Guide

This guide explains how to connect **Claude Desktop** to the **Revit AI Automation MCP Service** after installing Revit AI.

## Prerequisites

- Windows 10/11
- **Autodesk Revit 2025 or 2026** (supported versions)
- **Claude Desktop** installed
- **Revit AI installer (.exe) download link** (will be shared via email)

## 1) Install Revit AI

1. Download the Revit AI installer (.exe) using the link provided in the email.
2. Run the installer and complete setup (default location only).
3. After installation, continue to the next step to locate the MCP Service executable.

## 2) Locate the MCP Service executable

Revit AI installs the MCP Service to the default path below:

`C:\Users\<YOUR_USERNAME>\AppData\Local\Programs\Revit AI\Revit AI Automation MCP Service.exe`

> Replace `<YOUR_USERNAME>` with your Windows account name.

## 3) Add the MCP server to Claude Desktop config

Claude Desktop reads MCP servers from its configuration file. Add an entry that points to the executable.

> Important: Replace `<YOUR_USERNAME>` with your Windows account name.

### Required: add ONLY this block

Paste the following **inside your existing** `"mcpServers": { ... }` object (add a trailing comma if your JSON requires it).

```jsonc
"revit-mcp-bridge": {
  "command": "C:\\Users\\<YOUR_USERNAME>\\AppData\\Local\\Programs\\Revit AI\\Revit AI Automation MCP Service.exe",
  "args": [],
  "env": {}
}
```

### Reference: full example config (do not overwrite unless intended)

```jsonc
{
  "mcpServers": {
    "revit-mcp-bridge": {
      "command": "C:\\Users\\<YOUR_USERNAME>\\AppData\\Local\\Programs\\Revit AI\\Revit AI Automation MCP Service.exe",
      "args": [],
      "env": {}
    }
  },
  "globalShortcut": "",
  "preferences": {
    "menuBarEnabled": false
  }
}
```

Notes:
- Use **double backslashes** in JSON (`\\`) or your JSON parser may treat `\U` etc. as escapes.
- Keep the server name stable (e.g., `revit-mcp-bridge`) so prompts and tooling refer to a consistent identifier.

## 4) Prepare Revit and Restart Claude Desktop

1. **Open Revit:** Launch Autodesk Revit and open your project.
   > **Important:** Ensure only **one instance** of Revit is running. Having multiple Revit windows open may cause connection issues.
2. **Quit Claude Desktop:** Fully quit Claude Desktop.
   > **Tip:** If Claude was previously open, check the Task Manager (`Ctrl + Shift + Esc`) to ensure no background processes remain. If necessary, end the task manually.
3. **Re-open Claude Desktop:** Launch the application again to load the new configuration.


## 5) Verify it's working

In Claude Desktop:
- Open MCP/tools view (if available) and confirm a server named `revit-mcp-bridge` is connected
- If there’s a “refresh/reload tools” action, run it after restart

If the server starts successfully, Claude should be able to discover tools exposed by the service.

## 6) Best Practice: Visual Context

For the best results when asking for changes or analysis:
- **Take a screenshot** of your current Revit view or the specific element in question.
- **Paste the image** into Claude along with your prompt.
> *Note:* Automatic screenshot capture is planned for a future update. For now, manually providing visual context helps the AI understand your model state better.

## Troubleshooting

### “Server failed to start” / no tools appear
- Confirm the executable exists at the default path above
- Try launching the executable manually (double-click) to see if Windows shows a missing dependency prompt
- Restart Claude Desktop after every config change

### Permission / security prompts
- If Windows SmartScreen or antivirus blocks the executable, allow it and retry
- Run Claude Desktop once as Administrator to test whether it’s a permission issue (then revert to normal)

## Quick checklist

- [ ] Claude Desktop installed
- [ ] Correct executable path copied
- [ ] Added to `mcpServers` as `command`
- [ ] Restarted Claude Desktop
- [ ] Tools/server shows as connected

---

## Note for Advanced Users

While this guide focuses on **Claude Desktop**, the Revit AI Tool follows the standard MCP protocol and can work with any MCP-compatible LLM client. If you're using another client, you should be able to configure it similarly by pointing to the executable path. Configuration details will vary by client.