# OpenAI Codex CLI V1 (Node.js) 環境建置說明

## 初始化容器內專用工具

1. 安裝 Codex CLI 必備工具

   ```sh
   sudo apt-get update && sudo apt-get install -y --no-install-recommends \
     aggregate \
     ca-certificates \
     curl \
     dnsutils \
     fzf \
     gh \
     git \
     gnupg2 \
     iproute2 \
     ipset \
     iptables \
     yq \
     jq \
     less \
     man-db \
     procps \
     unzip \
     ripgrep \
     zsh \
     zstd
   ```

2. 安裝 Codex CLI

   ```sh
   npm i -g @openai/codex

   codex --version
   ```

   💡 目前最新版為 `0.5.0`

3. 取得你的 OpenAI API Key

   ```sh
   codex login
   ```

   他會要求你登入 ChatGPT 或 OpenAI 帳號，只要登入成功就可以得到一把 API Key。

4. 測試 `codex` 是否可以正常運作

   ```sh
   codex 'hi'
   ```

## 從原始碼建置 codex (Node.js) 工具

```sh
# 設定 Git 配置，避免 Windows CRLF 換行符號問題
# 在跨平台開發時，Windows 使用 CRLF(\r\n)，而 Linux/Mac 使用 LF(\n)
# 設定為 input 可確保檔案在儲存時統一使用 LF 換行符號
echo "📝 設定 Git 換行符號處理方式..."
git config --global core.autocrlf input

# 切換到使用者主目錄並建立專案資料夾
# ~ 代表使用者的主目錄 (home directory)
echo "📁 建立專案目錄結構..."
cd ~/
mkdir projects; cd projects

# 從 GitHub 克隆 OpenAI Codex 儲存庫
# 這會下載完整的專案原始碼到本地端
echo "⬇️ 下載 OpenAI Codex 原始碼..."
git clone https://github.com/openai/codex.git
cd codex

# 鎖定版本在 Jul 12, 2025 的最新版 Commit 上
git checkout bfeb8c92a591e8f20ecabb2a1b5a22e1574e7951

cd codex-cli

# 獲取並套用社群貢獻的 Pull Request 修正
# 這些是 Will 保哥提交的重要修正
# https://github.com/openai/codex/pulls/doggy8088
echo "🔧 套用社群修正補丁..."
git fetch origin '+refs/pull/*/head:refs/remotes/origin/pr/*'
echo "   - 套用 PR #1004 修正...日誌檔名不應使用冒號字元"
git cherry-pick origin/pr/1004
echo "   - 套用 PR #1121 修正...codex -v <rollout> 無法正常運作"
git cherry-pick origin/pr/1121
echo "   - 套用 PR #1125 修正...如果使用者已明確標記環境已足夠鎖定，則允許在沒有沙盒的情況下執行。"
git cherry-pick origin/pr/1125
echo "   - 套用 PR #1130 修正...在文件中闡明專案文件發現與合併的邏輯"
git cherry-pick origin/pr/1130
echo "   - 套用 PR #1134 修正...為 codex 新增完整的 bash 自動完成功能"
git cherry-pick origin/pr/1134
echo "   - 套用 PR #1143 修正...修正 yq 不需要 -o=json 參數"
git cherry-pick origin/pr/1143

# 啟用 Node.js Corepack 套件管理器
# Corepack 允許使用不同的套件管理器 (如 pnpm, yarn) 而不需要全域安裝
echo "⚙️ 啟用 Node.js Corepack..."
corepack enable

# 設定 pnpm 套件管理器
echo "📦 設定 pnpm 套件管理器..."
yes | pnpm setup

# 套用 pnpm 套件管理器的環境變數
echo "📦 套用 pnpm 套件管理器的環境變數..."
source ~/.bashrc

# 安裝專案相依套件並進行建置
echo "📦 安裝專案相依套件..."
pnpm install

echo "🔨 編譯專案..."
pnpm build

# 僅限 Linux 系統：下載預建的沙盒執行檔案
# 這些檔案用於安全地執行程式碼，需要 gh (GitHub CLI) 和 zstd 工具
echo "🛡️ 安裝沙盒執行環境 (僅限 Linux) ..."
./scripts/install_native_deps.sh

# 顯示 CLI 工具的使用說明和選項
echo "📖 顯示使用說明..."
node ./dist/cli.js --help

# 直接執行本地建置的 CLI 工具
echo "🚀 測試執行 Codex CLI..."
node ./dist/cli.js

# 將 CLI 工具連結到全域環境，方便在任何地方使用
echo "🌐 安裝到全域環境..."
pnpm link --global

# 驗證安裝版本 (0.0.0-dev)
echo "✅ 驗證安裝版本... 注意: 自行建置的版本為 0.0.0-dev"
codex --version

# 設定 Bash 命令自動完成功能
# 這讓您可以使用 Tab 鍵自動完成 codex 命令
echo "⌨️ 設定命令自動完成..."
codex completion bash | sudo tee /etc/bash_completion.d/codex
source ~/.bashrc

echo "🎉 本地 OpenAI Codex CLI 安裝完成！"
```

## 如何設定 Azure OpenAI 金鑰給 codex (Node.js) 工具使用

由於 OpenAI 發佈到 npm registry 的 Codex CLI 一直都不支援 Azure OpenAI Service，每次都必須要從原始碼建置實在是太麻煩了，所以我自己發佈了一版沒問題的，安裝方式如下：

```sh
npm i -g @willh/codex
```

基本上，要支援 Azure OpenAI Service 只需要設定兩個環境變數到 `~/.profile` 即可：

```sh
cat <<'EOF' | tee -a ~/.profile > /dev/null
# Azure OpenAI Service
export AZURE_BASE_URL="https://YOUR-RESOURCE-NAME.openai.azure.com/openai"
export AZURE_OPENAI_API_KEY='YOUR-API-KEY'

export CODEX_UNSAFE_ALLOW_NO_SANDBOX=1
export CODEX_SANDBOX_NETWORK_DISABLED=0
EOF

source ~/.profile
```

接著直接執行就可以了：

```sh
codex -p azure -m codex-mini
```

> 💡 注意: 這裡的 `-m` 是 Azure OpenAI Service 的 Deployment name，並非是模型名稱，但建立 Deployment 的時候，模型一定要選擇 `o4-mini` 或 `codex-mini` 才可以！

如果常用的話，是可以調整 Codex CLI 設定檔 ( `~/.codex/config.json` ) 將 `azure` 提供者設定為預設值：

```json
cat <<'EOF' | tee ~/.codex/config.json > /dev/null
{
   "model": "codex-mini",
   "provider": "azure"
}
EOF
```

設定好之後，啟動時就比較簡單，預設就是使用 `azure` 提供者與 `codex-mini` 模型：

```sh
codex
```

💡 注意: 透過 [Azure OpenAI Responses API](https://learn.microsoft.com/en-us/azure/ai-foundry/openai/how-to/responses?WT.mc_id=DT-MVP-4015686&tabs=rest-api) 就只有 `o4-mini` 與 `codex-mini` 模型可以設定！

## 相關連結

- <https://github.com/openai/codex>
- [Securely Turbo‑Charge Your Software Delivery with Codex Coding Agent on Azure OpenAI | All things Azure](https://devblogs.microsoft.com/all-things-azure/securely-turbo%E2%80%91charge-your-software-delivery-with-the-codex-coding-agent-on-azure-openai/)
- [Developing inside a Container](https://code.visualstudio.com/docs/devcontainers/containers)
- [Development Containers](https://containers.dev/)

