### Claude Code for Windows (PowerShell)

- Claude Code 的登入憑證與執行記錄

    `~/.claude/` 資料夾會包含大多 Claude Code 的設定檔和憑證，還有執行過程的記錄與待辦事項。

    `~/.claude/.credentials.json` 檔案包含了 Claude Code 的憑證資訊。

- Claude Code 的狀態資訊

    `~/.claude/.claude.json` 檔案包含了 Claude Code 的狀態資訊，包含每次執行 `claude` 的提示內容，都可以從這裡找到。這個檔案會在每次執行 Claude Code 時更新。

    從 Windows 複製 OAuth 認證資訊到 WSL 環境的指令：

    ```shell
    cp /mnt/c/Users/USERNAME/.claude/.credentials.json ~/.claude/.credentials.json
    ```

- Claude Code 透過 Claude 訂閱帳號登入的憑證檔


- 使用 `cld` 取代 `claude` 命令

    在 PowerShell 中，可以使用以下命令來設定別名，讓 `cld` 取代 `claude` 命令：

    ```powershell
    Set-Alias -Name cld -Value claude
    ```
