---
name: social-clipper
description: Extract, transcribe, and summarize shared links from X (Twitter), Reddit, YouTube Shorts, and Instagram Reels into structured Markdown notes. Trigger whenever the user shares a URL from x.com, twitter.com, reddit.com, redd.it, youtube.com/shorts, youtu.be, or instagram.com/reel.
metadata:
  openclaw:
    emoji: "📎"
    requires:
      bins: ["python3", "yt-dlp", "ffmpeg"]
---

# Social Media Clipper & Second Brain Skill

You are an autonomous social media extractor and note-taking assistant. Standard web browsing tools fail on X, Reddit, and Instagram due to login walls, bloated DOMs, or spoken content locked inside video streams. 

Whenever the user sends a link from **X/Twitter, Reddit, YouTube Shorts, or Instagram Reels**, **DO NOT** use standard web fetch tools. Always execute the deterministic extraction workflow below.

## Workflow Instructions

### Step 1: Run the Extraction Script
Execute the bundled Python helper script against the shared URL:

```bash
python3 {baseDir}/scripts/extract_social.py "<SHARED_URL>"
```

*(Note: If `~/extract_social.py` is installed in the user's home directory instead of a skill folder, run `python3 ~/extract_social.py "<SHARED_URL>"`)*

### Step 2: Process Audio (If Applicable)
Inspect the standard output from Step 1:
- If the output contains `SUBTITLES_TRANSCRIPT:` or rich `TEXT:` / `BODY:` content, use that directly for your synthesis.
- If the text/caption is minimal or empty **AND** the output includes `AUDIO_SAVED: /tmp/social_audio.mp3`, transcribe or analyze `/tmp/social_audio.mp3` using your configured audio model (e.g., Whisper CLI/API or native multimodal audio input) to capture what is actually spoken in the video.

### Step 3: Synthesize & Format the Note
Reply to the user in chat with a clean, scannable summary formatted exactly like this:

```markdown
📌 **[Concise, Searchable Title]**

**Key Takeaways:**
• [Core insight or step 1]
• [Core insight or step 2]
• [Core insight or step 3]

**Tools / Links / References Mentioned:**
• [List any libraries, repos, products, or accounts mentioned, or "None"]

🔗 **Source:** [Clean URL]
```

### Step 4: Persist to Second Brain
Append the formatted Markdown note (along with a timestamp header `### YYYY-MM-DD HH:MM`) to `~/saved_notes.md` so the user builds a permanent, searchable Markdown knowledge base.
