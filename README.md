# Awesome OpenClaw JP 🇯🇵

[![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

> 日本語で学ぶ OpenClaw の実践ガイド・厳選スキル・セキュリティ対策

OpenClaw を日本語環境で使いこなすための実践的リソース集。セットアップから本番運用、セキュリティ強化まで体系的に整理しています。

## 目次

- [はじめに](#はじめに)
- [セットアップガイド](#セットアップガイド)
- [厳選スキル](#厳選スキル)
- [セキュリティ](#セキュリティ)
- [Telegram Bot 構築](#telegram-bot-構築)
- [運用ノウハウ](#運用ノウハウ)
- [おすすめ記事（Qiita / Zenn）](#おすすめ記事)
- [コミュニティ](#コミュニティ)
- [Contributing](#contributing)

---

## はじめに

OpenClaw は AI エージェントを Telegram・Discord・Slack などのメッセンジャーで動かすためのオープンソースプラットフォームです。

- **公式サイト**: [openclaw.ai](https://openclaw.ai)
- **GitHub**: [openclaw/openclaw](https://github.com/openclaw/openclaw) — ⭐ 337K+
- **ClawHub**: [clawhub.ai](https://clawhub.ai) — 13,000+ スキル
- **公式ドキュメント（日本語）**: [docs.openclaw.ai/ja-JP](https://docs.openclaw.ai/ja-JP)

### クイックスタート（5分で起動）

```bash
# 1. リポジトリをクローン
git clone https://github.com/openclaw/openclaw.git
cd openclaw

# 2. Docker で起動
docker compose up -d

# 3. 初期設定
docker exec -it openclaw-openclaw-gateway-1 openclaw onboard \
  --non-interactive --accept-risk --mode local

# 4. Telegram Bot を接続
# BotFather でトークンを取得し、openclaw.json の agents セクションに設定
```

---

## セットアップガイド

### Docker デプロイ（推奨）

| 項目 | 内容 |
|------|------|
| **最小要件** | 2GB RAM、10GB ストレージ、Docker + Docker Compose |
| **推奨環境** | Ubuntu 22.04+ / Debian 12+、4GB RAM |
| **対応プラットフォーム** | x86_64 Linux、Raspberry Pi（ARM64）、WSL2 |

```yaml
# docker-compose.yml の最小構成
services:
  openclaw-gateway:
    image: openclaw/openclaw:latest
    restart: unless-stopped
    ports:
      - "127.0.0.1:18789:18789"
    volumes:
      - ./config:/home/node/.openclaw
      - ./workspaces:/home/node/.openclaw/workspaces
    environment:
      - OPENCLAW_TELEGRAM_BOT_TOKEN=${TELEGRAM_BOT_TOKEN}
```

### NAS / ミニ PC での自宅サーバー構築

自宅の NAS やミニ PC で 24時間稼働の AI アシスタントを構築できます。

**必要なもの：**
- 常時起動 PC（NAS、ミニ PC、古いノートPC など）
- Ubuntu / Debian（推奨）
- Docker + Docker Compose
- Telegram アカウント + BotFather で作成した Bot トークン

**構築手順：**

1. **OS セットアップ** — Ubuntu Server / Desktop をインストール
2. **Docker インストール** — `curl -fsSL https://get.docker.com | sh`
3. **OpenClaw デプロイ** — `git clone` → `docker compose up -d`
4. **Telegram Bot 接続** — BotFather でトークン取得 → `openclaw.json` に設定
5. **セキュリティ強化** → [セキュリティ](#セキュリティ) セクション参照
6. **自動起動設定** — `restart: unless-stopped` で停電復帰も自動

### VPS デプロイ（Hetzner / ConoHa）

```bash
# VPS での構築例
ssh root@your-server

# 基本セットアップ
apt update && apt upgrade -y
adduser openclaw
usermod -aG docker openclaw

# Docker インストール
curl -fsSL https://get.docker.com | sh

# OpenClaw デプロイ
su - openclaw
git clone https://github.com/openclaw/openclaw.git
cd openclaw
docker compose up -d
```

---

## 厳選スキル

ClawHub には 13,000 以上のスキルがありますが、品質はまちまちです。以下は実際にテスト済みの推奨スキルです。

### 開発・コーディング

| スキル | 説明 | セキュリティ |
|--------|------|-------------|
| [self-improving-agent](https://clawhub.ai/pskoett/self-improving-agent) | 自己改善型エージェント | ✅ Benign |
| [agent-browser](https://clawhub.ai/TheSethRose/agent-browser) | ブラウザ自動操作 | ⚠️ 要確認 |

### 調査・リサーチ

| スキル | 説明 | セキュリティ |
|--------|------|-------------|
| [find-skills](https://clawhub.ai/JimLiuxinghai/find-skills) | ClawHub スキル検索 | ✅ Benign |
| [summarize](https://clawhub.ai/steipete/summarize) | コンテンツ要約 | ✅ Benign |

### セキュリティ

| スキル | 説明 | セキュリティ |
|--------|------|-------------|
| [skill-audit-framework](https://clawhub.ai/enawareness/skill-audit-framework) | スキルの安全性を監査するフレームワーク | ✅ Benign |
| [clawdbot-security-check](https://clawhub.ai/TheSethRose/clawdbot-security-check) | OC 自体のセキュリティチェック | ✅ Benign |

### 日本特化

| スキル | 説明 | セキュリティ |
|--------|------|-------------|
| [kakutei-shinkoku](https://clawhub.ai/enawareness/kakutei-shinkoku) | 確定申告サポート（フリーランス向け） | ✅ Benign |

### 健康・フィットネス

| スキル | 説明 | セキュリティ |
|--------|------|-------------|
| [run-coach](https://clawhub.ai/enawareness/run-coach) | Garmin 連携ランニングコーチ | ✅ Benign |

> ⚠️ **スキルをインストールする前に必ずセキュリティ確認を行ってください。** ClawHub の 13.4% のスキルにセキュリティ上の問題があることが報告されています（[Snyk ToxicSkills 調査](https://snyk.io/blog/toxicskills-malicious-ai-agent-skills-clawhub/)）。

---

## セキュリティ

### なぜセキュリティが重要か

- 2026年2月: **ClawHavoc 事件** — 341 個の悪意あるスキルが ClawHub で発見（[詳細](https://www.koi.ai/blog/clawhavoc-341-malicious-clawedbot-skills-found-by-the-bot-they-were-targeting)）
- Snyk 調査: ClawHub スキルの **13.4%** にクリティカルな問題（[レポート](https://snyk.io/blog/toxicskills-malicious-ai-agent-skills-clawhub/)）
- OC Gateway はデフォルトで全ポート公開 — 設定しないと危険

### 基本的なセキュリティ対策

```bash
# 1. SSH をキー認証に変更（パスワード認証を無効化）
sudo sed -i 's/#PasswordAuthentication yes/PasswordAuthentication no/' \
  /etc/ssh/sshd_config
sudo systemctl restart sshd

# 2. UFW ファイアウォール設定
sudo ufw default deny incoming
sudo ufw allow from 192.168.0.0/24  # LAN のみ許可（環境に合わせて変更）
sudo ufw enable

# 3. OC Gateway をローカルのみにバインド
# openclaw.json:
# "gateway": { "bind": "127.0.0.1:18789" }

# 4. セキュリティ監査を実行
docker exec <container-name> openclaw security audit
```

### Docker-UFW バイパス問題

**重要:** Docker はデフォルトで UFW のルールをバイパスします。`-p` で公開したポートは UFW の deny ルールを無視して外部からアクセス可能になります。

**対策:** `DOCKER-USER` iptables チェーンで制御し、systemd サービスで永続化。

```bash
#!/bin/bash
# docker-user-firewall.sh — Docker ポートのアクセス制御
iptables -F DOCKER-USER
iptables -I DOCKER-USER -j DROP
iptables -I DOCKER-USER -s 192.168.0.0/24 -j ACCEPT    # LAN（環境に合わせて変更）
iptables -I DOCKER-USER -s 172.16.0.0/12 -j ACCEPT      # Docker 内部通信
# Tailscale を使う場合:
# iptables -I DOCKER-USER -s 100.64.0.0/10 -j ACCEPT
```

### スキルのセキュリティ監査

スキルをインストールする前に、以下の 6 項目を確認：

1. **出所・身元確認** — GitHub リポジトリがあるか、作者の活動履歴があるか
2. **権限の適切性** — 要求する環境変数・API キーはスキルの目的に合っているか
3. **動作と説明の一致** — 説明にない動作（ネットワーク通信など）がないか
4. **認証情報の扱い** — API キーがログに出力されないか、適切に保管されているか
5. **永続化・副作用** — システムファイルを変更しないか、バックグラウンドプロセスを残さないか
6. **依存関係** — 不審なパッケージ、`curl | bash` パターンがないか

詳細なチェックリストは [skill-audit-framework](https://clawhub.ai/enawareness/skill-audit-framework) を参照。

### Tailscale でリモートアクセス

```bash
# Tailscale インストール（外出先から安全にアクセス）
curl -fsSL https://tailscale.com/install.sh | sh
sudo tailscale up

# UFW に Tailscale サブネットを追加
sudo ufw allow from 100.64.0.0/10
```

---

## Telegram Bot 構築

### マルチ Bot 構成

1 つの OpenClaw インスタンスで複数の Telegram Bot を運用できます。

```jsonc
// openclaw.json — マルチ Bot 構成例
{
  "agents": {
    "defaults": {
      "model": "anthropic/claude-sonnet-4-6",
      "channel": "telegram"
    },
    "main": {
      "model": "anthropic/claude-opus-4-6",
      "telegram": { "botToken": "YOUR_MAIN_BOT_TOKEN" }
    },
    "assistant": {
      "telegram": { "botToken": "YOUR_ASSISTANT_BOT_TOKEN" }
    }
  }
}
```

### 定時タスク（Heartbeat）

毎日決まった時間に自動メッセージを送る設定：

```jsonc
"heartbeat": {
  "every": "24h",
  "activeHours": {
    "start": "08:00",
    "end": "08:30",
    "timezone": "Asia/Tokyo"
  },
  "target": "telegram"
}
```

### 画像送信パイプライン

Playwright で HTML → スクリーンショット → Telegram 送信：

```bash
# 1. HTML を作成してスクリーンショット
node screenshot.mjs input.html output.png

# 2. Telegram に送信（sendPhoto → 失敗時 sendDocument にフォールバック）
node send-album.mjs output.png "キャプション" "$CHAT_ID" "$BOT_TOKEN"
```

---

## 運用ノウハウ

### コンテナ更新の注意点

OpenClaw のアップデート時に失われるもの：

| 項目 | 永続化方法 |
|------|-----------|
| `pip install` したパッケージ | Dockerfile に追加、または entrypoint スクリプトで自動インストール |
| `apt-get install` したフォント・ライブラリ | Dockerfile の RUN レイヤーに追加 |
| Playwright ブラウザ | Docker Volume で `/home/node/.cache` を永続化 |
| ワークスペースのデータ | Volume マウント（デフォルトで永続化） |
| `openclaw.json` 設定 | Volume マウント（デフォルトで永続化） |

### 日本語フォント（CJK）対応

コンテナ内で日本語が文字化けする場合：

```dockerfile
# Dockerfile に追加
RUN apt-get update -qq \
    && apt-get install -y --no-install-recommends \
      fonts-noto-cjk fonts-noto-color-emoji \
    && rm -rf /var/lib/apt/lists/*

COPY fontconfig.xml /etc/fonts/local.conf
RUN fc-cache -f
```

```xml
<!-- fontconfig.xml -->
<?xml version="1.0"?>
<!DOCTYPE fontconfig SYSTEM "fonts.dtd">
<fontconfig>
  <alias><family>sans-serif</family><prefer>
    <family>Noto Sans CJK JP</family>
    <family>Noto Color Emoji</family>
  </prefer></alias>
</fontconfig>
```

### Polling 障害の自動復旧

Telegram の polling が停止した場合に自動復旧する watchdog：

```bash
#!/bin/bash
# openclaw-watchdog.sh — Polling 停止時に自動復旧
CONTAINER="openclaw-openclaw-gateway-1"  # 環境に合わせて変更
COMPOSE_DIR="/path/to/openclaw"           # 環境に合わせて変更

LOG=$(docker logs "$CONTAINER" --since 3m 2>&1)

STALL=$(echo "$LOG" | grep -c 'Polling stall detected')
STARTED=$(echo "$LOG" | grep -c 'starting provider')

if [ "$STALL" -gt 0 ] && [ "$STARTED" -eq 0 ]; then
  echo "$(date): Polling stall — restarting" >> /var/log/openclaw-watchdog.log
  cd "$COMPOSE_DIR" && docker compose restart
fi
```

```bash
# crontab に追加（3分ごとに実行）
*/3 * * * * /usr/local/bin/openclaw-watchdog.sh
```

### トークン節約のコツ

- **defaults のモデルを Sonnet に設定** — 全 Bot が Opus を使うとクオータを早く消費する
- **メイン Bot だけ Opus** — 複雑な推理が必要な Bot のみ上位モデル
- **Cache hit 率を意識** — SOUL.md を安定させると cache 効率が上がる
- **画像の自動削除** — OC は履歴画像を自動クリアしてトークンを節約する仕組みがある

---

## おすすめ記事

### Qiita

- [#openclaw タグ](https://qiita.com/tags/openclaw) — 325+ 件の日本語記事
- セットアップ、セキュリティ、実務活用のカテゴリで検索推奨

### Zenn

- [OpenClaw トピック](https://zenn.dev/topics/openclaw) — 265+ 件の日本語記事
- 深い技術解説、Raspberry Pi デプロイ、Slack 連携など

> 💡 日本語の OpenClaw コンテンツは **590 件以上** 存在しますが、動画コンテンツ（YouTube）はほぼゼロです。

---

## コミュニティ

| プラットフォーム | リンク | 備考 |
|------------------|--------|------|
| **Discord（公式）** | [discord.gg/openclaw](https://discord.gg/openclaw) | 155K+ メンバー、英語中心 |
| **GitHub Discussions** | [openclaw/openclaw/discussions](https://github.com/openclaw/openclaw/discussions) | バグ報告・機能提案 |
| **Qiita** | [#openclaw](https://qiita.com/tags/openclaw) | 日本語記事 325+ |
| **Zenn** | [openclaw topic](https://zenn.dev/topics/openclaw) | 日本語記事 265+ |

---

## Contributing

プルリクエスト歓迎です。

- **スキルの追加**: 実際にインストール・テスト済みのスキルのみ
- **記事の追加**: Qiita/Zenn の高品質な記事へのリンク
- **ガイドの追加**: 実際の運用経験に基づいた内容

詳細は [CONTRIBUTING.md](CONTRIBUTING.md) を参照。

---

## License

MIT
