---
name: youtube-summarizer
description: Summarizes YouTube videos by extracting transcripts and metadata using yt-dlp. Use this skill whenever the user shares a YouTube URL and wants a summary, overview, notes, or key takeaways from the video — even if they just paste a link and say "summarize this" or "what's this about". Also trigger for requests like "get the transcript", "what does this video cover", or "give me notes on this YouTube video".
---

# YouTube Video Summarizer

Summarize a YouTube video by extracting its transcript and metadata via yt-dlp, then producing a structured summary.

## Prerequisites

Requires `yt-dlp` installed on the system. It's available at `/opt/homebrew/bin/yt-dlp` on this machine.

## Step 1: Extract metadata

```bash
yt-dlp --skip-download --print "%(title)s|%(channel)s|%(duration_string)s|%(description)s" "<URL>"
```

Parse the output by splitting on `|` — fields are: title, channel, duration, description.

## Step 2: Download auto-generated subtitles

```bash
yt-dlp --write-auto-sub --sub-lang en --sub-format vtt --skip-download -o "/tmp/yt_transcript" "<URL>"
```

This saves the transcript to `/tmp/yt_transcript.en.vtt`.

If English subtitles aren't available, try `--sub-lang en,en-US,en-GB` or omit `--sub-lang` to get whatever is available, then note the language in your summary.

## Step 3: Parse the transcript

Use this Python snippet to extract clean, ordered transcript text:

```python
import re

with open('/tmp/yt_transcript.en.vtt', 'r') as f:
    content = f.read()

lines = content.split('\n')
text_lines = []
for line in lines:
    # Skip VTT metadata and timestamp lines
    if re.match(r'^\d{2}:\d{2}', line) or line.startswith('WEBVTT') or \
       line.startswith('Kind:') or line.startswith('Language:') or not line.strip():
        continue
    # Strip HTML tags (e.g. <c>, <00:00:01.000>)
    clean = re.sub(r'<[^>]+>', '', line).strip()
    if clean:
        text_lines.append(clean)

# Deduplicate consecutive identical lines (VTT often repeats lines)
deduped = []
prev = None
for line in text_lines:
    if line != prev:
        deduped.append(line)
        prev = line

transcript = ' '.join(deduped)
print(transcript)
```

Run via: `python3 -c "<inline script>"` or write to `/tmp/parse_vtt.py` and run it.

## Step 4: Produce the summary

Using the metadata and transcript, write a structured summary:

```
**[Title]** | [Channel] | [Duration]

## What it's about
One or two sentences on the video's core topic and purpose.

## Key topics covered
- Bullet points for each major section or concept, in order
- Be specific — include names, tools, terms that were actually discussed
- Aim for 5–10 bullets depending on video length

## Takeaways
The most actionable or memorable points a viewer should walk away with.
```

Adapt the structure to the content — a tutorial warrants more detail than a news clip. For long videos (>30 min), add a **Timestamps** section if the description included them.

## Error handling

- **No subtitles available**: Note this to the user. Offer to summarize based on the video description alone if that's available.
- **yt-dlp JS challenge warning**: Usually harmless — the subtitle download often succeeds despite the warning. Check whether the `.vtt` file was created.
- **Non-English video**: If no English subtitles exist, try downloading in the video's native language. You can still summarize if you can read the language; otherwise note the limitation.
- **Private/age-restricted video**: Inform the user the video isn't publicly accessible.
