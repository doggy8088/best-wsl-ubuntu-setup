# OpenAI Codex CLI 環境建置說明

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

