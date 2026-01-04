## One-Time Setup

```bash
# Install yt-dlp
brew install yt-dlp

# Create dedicated folder
mkdir ~/transcripts
cd ~/transcripts
```

## Basic Commands

### Single Video

```bash
# Extract transcript only (recommended for AI analysis)
yt-dlp --write-auto-subs --sub-langs en --skip-download --sub-format txt "VIDEO_URL"

# With custom naming (cleaner file names)
yt-dlp --write-auto-subs --sub-langs en --skip-download --sub-format txt --output "%(title)s.%(ext)s" "VIDEO_URL"
```

### Bulk Processing

#### Entire Playlist

```bash
yt-dlp --write-auto-subs --sub-langs en --skip-download --sub-format txt "PLAYLIST_URL"
```

#### All Videos from Channel

```bash
yt-dlp --write-auto-subs --sub-langs en --skip-download --sub-format txt "https://youtube.com/@CHANNEL_NAME"
```

#### Multiple URLs from File

Create `urls.txt` with one URL per line, then:

```bash
yt-dlp --write-auto-subs --sub-langs en --skip-download --sub-format txt --batch-file urls.txt
```

#### Limited Channel Processing

```bash
# Only last 10 videos
yt-dlp --write-auto-subs --sub-langs en --skip-download --sub-format txt --playlist-end 10 "CHANNEL_URL"

# Videos from specific date onwards
yt-dlp --write-auto-subs --sub-langs en --skip-download --sub-format txt --dateafter 20240101 "CHANNEL_URL"
```

## Useful Options

### File Organization

```bash
# Create folders by channel
--output "%(uploader)s/%(title)s.%(ext)s"

# Simple title only
--output "%(title)s.%(ext)s"

# Just video ID (shortest)
--output "%(id)s.%(ext)s"
```

### Error Handling & Politeness

```bash
# Continue if some videos fail
--ignore-errors

# Be respectful to YouTube servers
--sleep-interval 2
```

## File Formats

- **TXT**: Clean text, perfect for AI analysis
- **VTT**: Includes timestamps, useful if you need to jump back to specific moments

## AI Analysis Workflow

### Step 1: Extract Transcript

Use any command above to get `.txt` file

### Step 2: AI Prompt Template

```
Analyze this YouTube transcript and provide:
1. 3 key insights
2. Main actionable takeaways  
3. Whether it's worth watching the full video (Yes/No + why)
4. Estimated time investment vs. value

[PASTE TRANSCRIPT HERE]
```

### Step 3: Save Results

Copy insights to Obsidian vault or preferred note-taking system

## Common File Naming Patterns

- Default: `Video Title [VIDEO_ID].en.txt`
- Custom: `Video Title.en.txt`
- Channel organized: `Channel Name/Video Title.en.txt`

## Quick Reference Commands

### Most Common (Single Video)

```bash
yt-dlp --write-auto-subs --sub-langs en --skip-download --sub-format txt "VIDEO_URL"
```

### Most Common (Bulk Processing)

```bash
yt-dlp --write-auto-subs --sub-langs en --skip-download --sub-format txt --batch-file urls.txt
```

### Production Ready (With Error Handling)

```bash
yt-dlp --write-auto-subs --sub-langs en --skip-download --sub-format txt --ignore-errors --sleep-interval 2 --output "%(title)s.%(ext)s" "VIDEO_URL"
```