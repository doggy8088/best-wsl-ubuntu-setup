# Claude Code for VS Code

## `keybindings.json`

```json
[
    {
      "key": "ctrl+shift+`",
      "command": "claude-code.runClaude.keyboard",
      "when": "claude-code.hasClaudeInPath"
    },
    {
      "key": "ctrl+escape",
      "command": "-claude-code.runClaude.keyboard",
      "when": "claude-code.hasClaudeInPath"
    },
    {
      "key": "ctrl+shift+`",
      "command": "-workbench.action.terminal.new",
      "when": "terminalProcessSupported || terminalWebExtensionContributedProfile"
    }
  ]
```
