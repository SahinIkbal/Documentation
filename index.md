# Revit AI Automation MCP Service — Setup Guide

This guide explains how to connect **Claude Desktop** to the **Revit AI Automation MCP Service** after installing Revit AI.

## Prerequisites

- Windows 10/11
- **Autodesk Revit 2025 or 2026** (supported versions)
- **Claude Desktop** installed
- **Revit AI installer (.exe) download link** (will be shared via email)

## 0) Install Revit AI

1. Download the Revit AI installer (.exe) using the link provided in the email.
2. Run the installer and complete setup (default location only).
3. After installation, continue to the next step to locate the MCP Service executable.

## 1) Locate the MCP Service executable

Revit AI installs the MCP Service to the default path below:

`C:\Users\<YOUR_USERNAME>\AppData\Local\Programs\Revit AI\Revit AI Automation MCP Service.exe`

> Replace `<YOUR_USERNAME>` with your Windows account name.

## 2) Add the MCP server to Claude Desktop config

Claude Desktop reads MCP servers from its configuration file. Add an entry that points to the executable.

> Important: Ensure the path is correct and the file exists. If the path contains spaces, that’s OK—just pass it as the `command` value.

### Example config (recommended)

```jsonc
{
  // ...existing code...
  "mcpServers": {
    "revit-ai": {
      "command": "C:\\Users\\<YOUR_USERNAME>\\AppData\\Local\\Programs\\Revit AI\\Revit AI Automation MCP Service.exe",
      "args": []
    }
  }
  // ...existing code...
}
```

Notes:
- Use **double backslashes** in JSON (`\\`) or your JSON parser may treat `\U` etc. as escapes.
- Keep the server name stable (e.g., `revit-ai`) so prompts and tooling refer to a consistent identifier.

## 3) Restart Claude Desktop

After editing the config:
1. Fully quit Claude Desktop (ensure it’s not running in the tray)
2. Re-open Claude Desktop 


## 4) Verify it’s working

In Claude Desktop:
- Open MCP/tools view (if available) and confirm a server named `revit-ai` is connected
- If there’s a “refresh/reload tools” action, run it after restart

If the server starts successfully, Claude should be able to discover tools exposed by the service.

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