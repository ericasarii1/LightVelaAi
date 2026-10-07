# LightVelaAi — Hermes Config & Skills

Repositori berisi konfigurasi, skills, dan plugin **Hermes Agent** + integrasi **LightVela / LightClaw**.

## Struktur

| Path | Isi |
|---|---|
| `AGENTS.md` | Panduan agent yang bekerja di direktori home Hermes |
| `SOUL.md` | Persona / soul agent |
| `config.yaml` | Konfigurasi utama Hermes (model, gateway, compression, dll) |
| `plugins/lightclawbot/` | Plugin konektor LightClaw/LightVela (256 MCP tools) |
| `skills/` | Koleksi skills (productivity, research, software-development, dll) |
| `sessions/` `state/` `runtime/` | State gateway & sesi |
| `providers/` | Definisi provider model |

## ⚙️ Cara isi config (bot token, API key, dll)

Semua **secret tidak di-commit** ke repo ini — disimpan di file `.env` lokal:

```
~/.hermes/.env
```

Buka / edit:

```bash
nano ~/.hermes/.env
```

Contoh isinya (bagian Telegram):

```bash
# TELEGRAM INTEGRATION
# Token dari @BotFather (https://t.me/BotFather)
TELEGRAM_BOT_TOKEN=123456789:AA...  # <- ganti dengan token bot kamu
TELEGRAM_ALLOWED_USERS=123456789    # user ID Telegram yang diizinkan
TELEGRAM_HOME_CHANNEL=123456789     # chat ID default
TELEGRAM_HOME_CHANNEL_NAME=rrrr     # nama bot
```

Cara dapat bot token:
1. Chat [@BotFather](https://t.me/BotFather) di Telegram
2. Kirim `/newbot` → ikuti langkahnya → salin token
3. Paste ke `TELEGRAM_BOT_TOKEN=` di `~/.hermes/.env`
4. Restart gateway:

```bash
hermes gateway restart
hermes gateway status   # telegram harus "connected"
```

Variabel lain yang umum:

| Variabel | Fungsi |
|---|---|
| `CUSTOM_API_KEY` | API key provider model (LightVela: `https://token.lightvela.ai/v1`) |
| `CUSTOM_BASE_URL` | Base URL provider model |
| `LIGHTCLAW_SITE` | `cn` atau `intl` untuk plugin LightClaw |
| `TELEGRAM_BOT_TOKEN` | Token bot Telegram |
| `TELEGRAM_ALLOWED_USERS` | User ID yang boleh chat dengan bot |

> ⚠️ Jangan pernah commit `.env` — sudah di-ignore via `.gitignore`.
