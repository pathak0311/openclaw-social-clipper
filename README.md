# 🦞 OpenClaw Social Clipper (`openclaw-social-clipper`)

Turn your **OpenClaw** agent (on Telegram, WhatsApp, Discord, or Slack) into a 1-tap Second Brain for **X (Twitter), Reddit, YouTube Shorts, and Instagram Reels**.

Stop saving posts and Reels to a "Watch Later" graveyard. Share any link directly from your phone's Share Sheet to your OpenClaw chat—your agent bypasses login walls, rips video audio/subtitles, extracts top comments, summarizes the core insights, replies in chat, and appends the structured note to your Markdown vault (`~/saved_notes.md`).

---

## ⚡ Why This Exists

Passing raw social links to standard LLM apps fails almost every time:
* **X (Twitter)** blocks unauthenticated scraper requests behind a login wall.
* **Reddit** mobile links redirect and serve bloated HTML.
* **Instagram Reels & YouTube Shorts** lock the actual value inside the **spoken audio**, not the caption—and downloading/transcribing video on-device inside iOS Shortcuts hits strict memory limits.

### How It Works
```text
[iPhone Share Sheet] ──► [Telegram / OpenClaw Bot]
                                │
                                ▼
                     ┌──────────────────────┐
                     │  extract_social.py   │
                     └──────────┬───────────┘
        ┌───────────────────────┼───────────────────────┐
        ▼                       ▼                       ▼
  [X / Twitter]             [Reddit]          [Reels / YT Shorts]
  api.fxtwitter           .json endpoint        yt-dlp + ffmpeg
 (Text + Quotes)       (Post + Top Comments)  (Auto-Subs + .mp3)
        │                       │                       │
        └───────────────────────┼───────────────────────┘
                                ▼
                  [Audio Transcription + LLM]
                                │
                                ▼
             [Telegram Summary + ~/saved_notes.md]
```

---

## 🚀 Installation (2 Ways)

### Method 1: The "Zero-SSH" Chat Install (Recommended)
You don't even need to open a terminal. Just copy and paste this prompt into your OpenClaw chat on Telegram/Discord:

> **Copy-paste to your OpenClaw agent:**
> ```text
> Install the OpenClaw Social Clipper skill from [https://github.com/](https://github.com/)<YOUR_USERNAME>/openclaw-social-clipper:
> 1. Ensure `yt-dlp` and `ffmpeg` are installed on the machine.
> 2. Clone the repo into `~/.openclaw/workspace/skills/social-clipper` (and copy `scripts/extract_social.py` to `~/extract_social.py` with `chmod +x`).
> 3. Read `SKILL.md` and register the rule in your permanent memory so every X, Reddit, YouTube Short, or Instagram Reel link I send is processed automatically.
> ```

### Method 2: Manual Server Install
If you have SSH access to your OpenClaw server:

```bash
# 1. Install dependencies (Ubuntu/Debian)
sudo apt update && sudo apt install -y ffmpeg
pip install -U yt-dlp

# 2. Clone directly into your OpenClaw skills directory
mkdir -p ~/.openclaw/workspace/skills
git clone [https://github.com/](https://github.com/)<YOUR_USERNAME>/openclaw-social-clipper.git \
  ~/.openclaw/workspace/skills/social-clipper

chmod +x ~/.openclaw/workspace/skills/social-clipper/scripts/extract_social.py
```

---

## 📱 Mobile Usage (iOS / Android)

### Direct Share (Zero Setup)
1. While viewing a Reel, Short, Tweet, or Reddit post, tap **Share**.
2. Select **Telegram** (or your connected OpenClaw messaging app) and tap your OpenClaw bot.
3. Done! Your agent replies with a bulleted breakdown a few seconds later.

### Optional: 1-Tap iOS Share Sheet Shortcut
Want a dedicated button in your iOS Share Sheet that strips tracking parameters (`?igsh=...`, `?utm_source=...`) automatically?
1. Open **Apple Shortcuts** -> tap **+** -> rename to **Save to OpenClaw**.
2. Enable **Show in Share Sheet** (Accepts `URLs and Text`).
3. Add **Get URLs from Input**.
4. Add **Replace Text** (Regex ON): Find `\?.*` and replace with blank.
5. Add Telegram's **Send Message** action -> send `[CLIP] [Updated Text]` to your OpenClaw bot with *"Show When Run"* toggled **OFF**.

---

## 🛠️ Troubleshooting

* **Instagram Reels Login Wall on Cloud IPs:** If your server runs on a datacenter IP (AWS EC2, DigitalOcean, Hetzner) and Instagram eventually rate-limits `yt-dlp`, export a browser cookie file (`cookies.txt`) from a throwaway Instagram account and add `"--cookies", "/path/to/cookies.txt"` to the `yt-dlp` command list in `scripts/extract_social.py`.

## 📄 License
MIT License — feel free to fork, extend, and plug into your own agent workflows!
