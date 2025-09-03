# OpenAI Codex CLI 環境建置說明

## 打造容器開發環境說明

1. 安裝容器開發環境的相關工具

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

   > 注意: Codex CLI 有特別針對 `rg` 命令做優化，最小化安裝建議至少包含 `ripgrep` 套件。

2. 安裝 Codex CLI

   ```sh
   npm i -g @openai/codex

   codex --version
   ```

   💡 目前最新版為 `0.28.0`

3. 取得你的 OpenAI API Key

   ```sh
   codex login
   ```

   他會要求你登入 ChatGPT 或 OpenAI 帳號，只要登入成功就可以得到一把 API Key。

4. 測試 `codex` 是否可以正常運作

   ```sh
   codex 'hi'
   ```

以下是安裝的每個套件的詳細說明：

- **aggregate**

   一個網路流量分析工具，用於合併和分析網路流量資料。它可以將多個網路流量記錄合併成單一的統計資料，通常用於網路監控和流量分析。這個工具特別適合處理 netflow 或類似的網路流量資料，可以根據來源 IP、目的 IP、埠號等條件進行流量聚合。

- **ca-certificates**

   包含受信任的憑證授權機構 (Certificate Authority, CA) 的數位憑證集合。這些憑證用於驗證 HTTPS 連線和其他 SSL/TLS 加密連線的真實性。當您的系統嘗試建立安全連線時，它會使用這些憑證來驗證遠端伺服器的身分，確保連線的安全性和可信度。

- **curl**

   一個強大的命令列工具，用於傳輸資料到各種伺服器或從各種伺服器傳輸資料。支援多種協定包括 HTTP、HTTPS、FTP、SFTP 等。開發者經常使用它來測試 API、下載檔案、發送 POST 請求，或執行各種網路相關的操作。它提供豐富的選項來控制請求標頭、認證、代理設定等。

- **dnsutils**

   包含一系列 DNS (網域名稱系統) 診斷和查詢工具。主要包括 `dig` (域資訊查詢工具)、`nslookup` (名稱伺服器查詢)、`host` (DNS 查詢工具) 等命令。這些工具對於網路故障排除、DNS 設定驗證、網域資訊查詢等任務非常有用。

- **gh**

   GitHub 的官方命令列介面工具。允許您直接從終端機與 GitHub 互動，執行如建立儲存庫、管理 issues、提交 pull requests、檢視專案狀態等操作。它簡化了許多 GitHub 操作流程，讓開發者無需切換到網頁瀏覽器即可完成大部分 GitHub 相關任務。

- **git**

   世界上最流行的分散式版本控制系統。用於追蹤程式碼變更、協作開發、管理專案歷史記錄。Git 讓開發者能夠建立分支、合併程式碼、回復變更，並與其他開發者協作。它是現代軟體開發不可或缺的工具，廣泛用於開源專案和企業開發環境。

- **gnupg2**

   GNU Privacy Guard 的第二版，一個完整的 OpenPGP 標準實作。提供加密、數位簽章、金鑰管理等功能。開發者和系統管理員使用它來加密敏感檔案、驗證軟體套件的完整性、建立和管理數位身分。它支援對稱和非對稱加密，並提供強大的金鑰管理功能。

- **yq**

   一個輕量級且可攜的命令列 YAML 處理器。類似於 jq 但專門處理 YAML 格式的資料。它允許您解析、過濾、轉換 YAML 檔案，支援複雜的查詢操作和資料操作。在處理設定檔案、CI/CD 管道、Kubernetes 資源定義等場景中特別有用。

- **jq**

   一個強大的命令列 JSON 處理器。允許您解析、過濾、轉換和操作 JSON 資料。它提供類似於 SQL 的查詢語言，可以從複雜的 JSON 結構中提取特定資料、重新格式化資料、執行計算等操作。對於 API 開發、資料處理和自動化指令碼非常有價值。

- **less**

   一個終端機分頁器，用於檢視文字檔案內容。與傳統的 `more` 命令相比，less 提供更多功能，包括向前和向後捲動、搜尋、標記等。它是檢視長檔案、日誌檔案或命令輸出的首選工具。許多系統將 less 設定為 man 頁面的預設檢視器。

- **man-db**

   管理系統手冊頁面 (manual pages) 的資料庫和工具集。包含 `man` 命令和相關的索引資料庫。手冊頁面提供命令、函式、設定檔案等的詳細文件。man-db 負責維護這些頁面的索引，使搜尋和檢視文件變得快速高效。

- **procps**

   包含一系列監控和管理系統行程的工具。主要包括 `ps` (顯示行程)、`top` (即時系統監控)、`kill` (終止行程)、`pgrep` (搜尋行程) 等命令。這些工具對於系統監控、效能調優、故障排除等任務至關重要。

- **unzip**

   用於解壓縮 ZIP 格式壓縮檔案的工具。支援密碼保護的壓縮檔案、目錄結構保持、檔案屬性保留等功能。它是處理從各種來源下載的壓縮檔案的基本工具，特別是在處理軟體發布包、資料備份等場景中。

- **ripgrep**

   一個極其快速的文字搜尋工具，設計用來替代 grep。使用 Rust 語言開發，在大型程式碼庫中搜尋時效能優異。支援正規表示式、遞迴目錄搜尋、檔案類型過濾、Unicode 支援等功能。對於程式碼搜尋、日誌分析、文件處理等任務非常高效。

- **zsh**

   Z shell，一個功能豐富的互動式 shell，相容於 bash 但提供許多額外功能。包括強大的自動完成、主題支援、外掛系統、進階提示自訂等。許多開發者選擇 zsh 作為預設 shell，特別是配合 Oh My Zsh 等框架使用時，可以大幅提升命令列使用體驗。

- **zstd**

   Zstandard 壓縮演算法的實作，提供高壓縮比和快速壓縮/解壓縮速度。由 Facebook 開發，在壓縮率和速度之間取得良好平衡。廣泛用於資料儲存、網路傳輸、備份系統等場景。許多現代系統和應用程式採用 zstd 作為預設壓縮格式。

這個套件集合涵蓋了開發環境的基本需求，包括版本控制、網路工具、文字處理、系統監控、壓縮工具等，是建立完整開發環境的良好基礎。

## 如何設定 Azure OpenAI 金鑰給 codex 工具使用

> 💡 記得要先有 `AZURE_OPENAI_API_KEY` 環境變數，底下命令也要記得把 `YOUR-RESOURCE-NAME` 換成你的資源名稱。

```sh
mkdir -p ~/.codex

cat <<'EOF' | tee ~/.codex/config.toml > /dev/null
approval_policy = "on-failure"
sandbox_mode = "workspace-write"
model_reasoning_effort = "high"
model_reasoning_summary = "detailed"

model_provider = "azure"
model          = "codex-mini"

[model_providers.azure]
name         = "Azure OpenAI"
base_url     = "https://duotify-ai-coding-agent.openai.azure.com/openai"
env_key      = "AZURE_OPENAI_API_KEY"
wire_api     = "responses"
query_params = { api-version = "2025-04-01-preview" }

[profiles.azure_gpt5]
model_provider = "azure"
model = "gpt-5"
# Optional: prefer API key auth over ChatGPT login
preferred_auth_method = "apikey"
model_reasoning_effort = "high"
model_reasoning_summary = "detailed"

[model_providers.groq]
name         = "Groq"
base_url     = "https://api.groq.com/openai/v1"
env_key      = "GROQ_API_KEY"
wire_api     = "chat"
query_params = {}

# The Groq API doesn't support `prompt_cache_key` yet.
# https://console.groq.com/docs/responses-api#unsupported-features
#wire_api     = "responses"

[profiles.groq_gptoss]
model_provider = "groq"
model = "openai/gpt-oss-120b"
# Optional: prefer API key auth over ChatGPT login
preferred_auth_method = "apikey"
model_reasoning_effort = "high"
model_reasoning_summary = "detailed"
EOF
```

自訂 Profile 的使用方式：

```sh
codex -p azure_gpt5
```

## 順利連接 Azure OpenAI Service 的技巧

目前的 Codex CLI 有兩個預先編譯好的 Linux 版本：

1. musl 版本

   若用 `npm install -g @openai/codex` 安裝，預設就是安裝這個。

   這個版本無法解析 Azure OpenAI Service 的 API 端點網址，所以永遠連不上。

   連不上 AOAI 的原因我有做過[深度分析](https://github.com/openai/codex/issues/1552)，問題出在 Codex CLI 預設的 musl 版本無法解析 `*.openai.azure.com` 域名造成的。

2. GNU 版本

   需從 [Releases](https://github.com/openai/codex/releases) 頁面手動下載。

   這個版本可以正確解析 Azure OpenAI Service 的 API 端點網址，唯有這個版本才能正常運作，但必須用 Ubuntu 24.04 LTS 以上版本才能跑。

   透過 Ubuntu 22.04 LTS 是無法執行的，因為 GLIBC 版本太久導致。

如果 Codex CLI 要使用 Azure OpenAI Services (AOAI) 的端點與金鑰，有以下兩種方法：

1. 參考下一個小節，從原始碼開始建置 `codex` 程式才能用。

2. 手動添加 DNS/IP 對應到 `/etc/hosts` 檔案中

    避免 DNS 解析也可以解決無法連線的問題，這是我研究好幾天才得出的應變措施！

    假設你的 AOAI 的資源名稱為 `duotify-coding-agent.openai.azure.com` 的話，就執行以下指令即可設定完成：

    ```sh
    DOMAIN="duotify-coding-agent.openai.azure.com"; sudo sed -i "/$DOMAIN/d" /etc/hosts && ip=$(dig +short $DOMAIN | grep -v '^$' | tail -n1) && [ -n "$ip" ] && echo "$ip $DOMAIN" | sudo tee -a /etc/hosts
    ```

這種安裝方法可以順利使用 Azure OpenAI Service 服務！

## 從原始碼建置 codex 工具

```sh
# 安裝必要的建置工具
sudo apt install -y build-essential pkg-config libssl-dev

# Clone the repository and navigate to the root of the Cargo workspace.
git clone https://github.com/openai/codex.git
cd codex/codex-rs

# Install the Rust toolchain, if necessary.
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh -s -- -y
source "$HOME/.cargo/env"
rustup component add rustfmt
rustup component add clippy

# Build Codex.
cargo build

# Launch the TUI with a sample prompt.
cargo run --bin codex -- "explain this codebase to me"

# After making changes, ensure the code is clean.
cargo fmt -- --config imports_granularity=Item
cargo clippy --tests

# Run the tests.
cargo test

# Install
cargo install --path cli
```

如果要更新到 Repo 的最新版，可以在 `git pull` 之後到 `codex-rs` 資料夾執行以下命令：

```sh
cargo install --path cli
```

你可以用以下命令列出已安裝的套件：

```sh
cargo install --list
```

你可以用以下命令移除已安裝的套件：

```sh
cargo uninstall codex-cli
```

## 使用 DotSlash 安裝 Codex CLI 版本

這種安裝方式必須先有 [DotSlash](https://dotslash-cli.com/) 才能執行程式：

```sh
# 安裝 Codex CLI
curl -sSL "https://github.com/openai/codex/releases/download/$(curl -s "https://api.github.com/repos/openai/codex/releases/latest" | jq -r .tag_name)/codex" -o ~/.local/bin/codex \
  && chmod +x ~/.local/bin/codex
```

## 相關連結

- <https://github.com/openai/codex>
- [Securely Turbo‑Charge Your Software Delivery with Codex Coding Agent on Azure OpenAI | All things Azure](https://devblogs.microsoft.com/all-things-azure/securely-turbo%E2%80%91charge-your-software-delivery-with-the-codex-coding-agent-on-azure-openai/)
- [Developing inside a Container](https://code.visualstudio.com/docs/devcontainers/containers)
- [Development Containers](https://containers.dev/)

