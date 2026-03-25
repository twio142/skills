---
name: media-transcript
description: Load a video or podcast episode into context by fetching its title, description, and transcript. Trigger when the user shares a YouTube, Bilibili, or podcast URL (e.g. pca.st, pocketcasts.com) and wants to discuss, summarize, or ask questions about it.
---

# Media Transcript Loader

Fetch the title, description, and transcript for a given URL and load them into context so the user can discuss, summarize, or ask questions about the content.

The URL argument is provided by the user. Detect the source from the URL:
- **YouTube**: `youtube.com`, `youtu.be`
- **Bilibili**: `bilibili.com`, `b23.tv`
- **Podcast**: anything else (e.g. `pca.st`, `pocketcasts.com`)

## API selection

Use `--api groq` by default. Switch to `--api gemini` if the audio duration exceeds **60 minutes**. Duration is available from the metadata for all sources (yt-dlp for YouTube/Bilibili, `Podcasts` tool for podcasts).

---

## YouTube

### Step 1: Metadata

```bash
yt-dlp --skip-download --print "%(title)s|%(uploader)s|%(duration_string)s|%(description)s" "<URL>"
```

Split on `|`: title, uploader, duration, description.

### Step 2: Subtitles

```bash
yt-dlp --write-auto-sub --skip-download --sub-format vtt -o "/tmp/media_transcript" "<URL>"
```

This writes a `.vtt` file. Find it with `ls /tmp/media_transcript*.vtt`.

### Step 3: Parse VTT

```python
import re, glob

path = glob.glob('/tmp/media_transcript*.vtt')[0]
with open(path) as f:
    content = f.read()

lines = content.split('\n')
text_lines = []
for line in lines:
    if re.match(r'^\d{2}:\d{2}', line) or line.startswith('WEBVTT') or \
       line.startswith('Kind:') or line.startswith('Language:') or not line.strip():
        continue
    clean = re.sub(r'<[^>]+>', '', line).strip()
    if clean:
        text_lines.append(clean)

deduped = []
prev = None
for line in text_lines:
    if line != prev:
        deduped.append(line)
        prev = line

print(' '.join(deduped))
```

Run inline: `python3 -c "<script>"` or write to `/tmp/parse_vtt.py`.

---

## Bilibili

### Step 1: Metadata

```bash
yt-dlp --skip-download --print "%(title)s|%(uploader)s|%(duration_string)s|%(description)s" "<URL>"
```

### Step 2: Download audio

```bash
yt-dlp -x --audio-format mp3 -o "/tmp/bili_audio" "<URL>"
```

This writes `/tmp/bili_audio.mp3`.

### Step 3: Transcribe

```bash
transcript-cli /tmp/bili_audio.mp3 --api groq --srt
```

---

## Podcast

### Step 1: Fetch episode info

```bash
trigger=episode_info Podcasts "<URL>"
```

This returns episode metadata including title, show name, description/shownotes, and a direct audio URL.

### Step 2: Download audio

Download the audio URL to `/tmp/podcast_audio.<ext>` using `curl -L -o`.

### Step 3: Transcribe

```bash
transcript-cli /tmp/podcast_audio.<ext> --api groq --srt
```

---

## Output

Hold the following structure in context. Do NOT write to any file at this stage:

```
# <Title>
**<Uploader / Show>** · <Duration>

## Description
<description or shownotes>

## Transcript
<transcript>
```

Confirm to the user that the content is loaded, then use the `AskUserQuestion` tool to ask:

- Question: "Would you like to save the transcript?"
- Header: "Save"
- Options:
    - "Save to current directory" — write as a `.md` file in cwd
    - "Save as Obsidian note" — create a note in the Obsidian vault
    - "Skip" — do not save

When saving, use this file format (with frontmatter):

```markdown
---
url: <URL>
tags:
  - <one or two relevant topic tags>
---
# <Title>
**<Uploader / Show>** · <Duration>

## Description
<description or shownotes>

## Transcript
<transcript>
```

**Option 1 — Save to cwd**: Write the file to `<title>.md` in the current working directory, replacing `:` and `/` with `-` in the filename.

**Option 2 — Save as Obsidian note**: Use the `custom-skills:obsidian` skill to create a note with the content above. Use the title as the note name, replacing `:` and `/` with `-`.

## Error handling

- **YouTube, no subtitles**: Note it to the user. Offer to download audio and transcribe instead using the Bilibili flow.
- **Bilibili audio too large for Gemini**: Warn the user if the file exceeds Gemini's limits.
- **Podcast tool fails**: Ask the user for the direct audio URL.
