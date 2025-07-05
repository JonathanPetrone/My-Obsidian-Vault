# YouTube Transcript Processor - Installation & Usage Guide

A comprehensive tool for batch downloading and processing YouTube video transcripts with intelligent filename generation.

## Prerequisites

### Required Tools

1. **yt-dlp** - YouTube downloader
2. **jq** - JSON processor (for playlist handling)
3. **Standard Unix tools** - grep, sed, awk (included on macOS/Linux)

### Installation Commands

**macOS (using Homebrew):**

```bash
# Install Homebrew if you don't have it
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# Install required tools
brew install yt-dlp jq
```

**Ubuntu/Debian Linux:**

```bash
sudo apt update
sudo apt install yt-dlp jq
```

**Other Linux distributions:**

```bash
# Install yt-dlp via pip
pip install yt-dlp

# Install jq (varies by distro)
# Fedora: sudo dnf install jq
# Arch: sudo pacman -S jq
# CentOS: sudo yum install jq
```

## Installation

### Step 1: Download the Script

Save the batch processor script to a file called `youtube-transcripts.sh`:

```bash
# Create the script file
touch ~/youtube-transcripts.sh
chmod +x ~/youtube-transcripts.sh

# Copy the script content into the file using your preferred editor
# (The script content is provided in the previous artifact)
```

### Step 2: Add to Your Shell

**For Zsh (default on macOS):**

```bash
# Add to your .zshrc
echo "source ~/youtube-transcripts.sh" >> ~/.zshrc

# Reload your shell
source ~/.zshrc
```

**For Bash:**

```bash
# Add to your .bashrc or .bash_profile
echo "source ~/youtube-transcripts.sh" >> ~/.bashrc

# Reload your shell
source ~/.bashrc
```

### Step 3: Verify Installation

```bash
# Test that commands are available
batch-transcripts
# Should show usage information
```

## Basic Usage

### Single Video Processing

```bash
# Process one video
process-single-video "https://youtube.com/watch?v=dQw4w9WgXcQ"

# Specify output directory
process-single-video "https://youtube.com/watch?v=dQw4w9WgXcQ" my_transcripts
```

### Multiple Videos from File

```bash
# Create a URL file interactively
create-url-file my_videos.txt

# Or create manually:
cat > my_videos.txt << EOF
# YouTube URLs for batch processing
https://youtube.com/watch?v=video1
https://youtube.com/watch?v=video2
https://youtube.com/playlist?list=PLxxx
https://youtube.com/@channelname
EOF

# Process all URLs
batch-transcripts my_videos.txt transcripts
```

### Playlist Processing

```bash
# Process entire playlist (limited to 20 videos)
process-playlist "https://youtube.com/playlist?list=PLxxx" transcripts 20

# Process channel (limited to 15 videos)
process-playlist "https://youtube.com/@channelname" transcripts 15
```

### Recent Channel Videos

```bash
# Get transcripts from last 30 days
process-channel-recent "https://youtube.com/@channelname" 30 transcripts

# Last 7 days only
process-channel-recent "https://youtube.com/@channelname" 7 transcripts
```

## Advanced Usage Examples

### Example 1: Processing a Learning Playlist

```bash
# Create dedicated directory
mkdir -p ~/learning/programming-transcripts

# Process recent videos from a programming channel
process-channel-recent "https://youtube.com/@ThePrimeagen" 14 ~/learning/programming-transcripts

# Process specific Go programming playlist
process-playlist "https://youtube.com/playlist?list=PLrAXtmRdnEQy..." ~/learning/go-transcripts 25
```

### Example 2: Mixed Content Processing

```bash
# Create URL file with mixed content
create-url-file research_videos.txt

# Add to the file (manually or via editor):
# - Individual research videos
# - Conference playlists  
# - Educational channels

# Process everything
batch-transcripts research_videos.txt research_transcripts
```

### Example 3: Cleaning Existing VTT Files

```bash
# If you already have .vtt files
clean-vtt-auto subtitle.en.vtt
# Creates: subtitle.en.txt

# Or specify output name
clean-vtt-auto subtitle.en.vtt clean_transcript.txt
```

## File Organization

The tool creates organized output with descriptive filenames:

```
transcripts/
├── How_to_Learn_Programming_Faster_dQw4w9WgXcQ.txt
├── Advanced_System_Design_Patterns_abc123xyz.txt
├── Building_Scalable_Applications_def456ghi.txt
└── Go_Concurrency_Patterns_Tutorial_xyz789abc.txt
```

**Filename Format:** `{Title}_{VideoID}.txt`

- **Title:** Cleaned video title (max 50 chars, spaces replaced with underscores)
- **VideoID:** Unique YouTube video identifier
- **Extension:** Always `.txt` for easy processing

## Troubleshooting

### Common Issues

**1. "yt-dlp: command not found"**

```bash
# Check if yt-dlp is installed
which yt-dlp

# Install if missing (macOS)
brew install yt-dlp

# Or via pip
pip install yt-dlp
```

**2. "jq: command not found"**

```bash
# Install jq (macOS)
brew install jq

# Linux
sudo apt install jq  # Ubuntu/Debian
```

**3. "No subtitles found"**

- Not all videos have auto-generated subtitles
- Try different videos or channels
- Check if the video is too new (subtitles may not be ready)

**4. Permission denied**

```bash
# Make script executable
chmod +x ~/youtube-transcripts.sh

# Check file permissions
ls -la ~/youtube-transcripts.sh
```

**5. Functions not available after installation**

```bash
# Reload your shell configuration
source ~/.zshrc  # or ~/.bashrc

# Or restart your terminal
```

### Error Handling

The script includes built-in error handling:

- Skips videos without subtitles
- Continues processing even if individual videos fail
- Provides progress updates and status messages
- Cleans up temporary files automatically

## Configuration Options

### Customizing Limits

Edit the script to change default limits:

```bash
# In process-playlist function, change:
local max_videos="${3:-50}"  # Default max videos
# To your preferred limit:
local max_videos="${3:-100}"
```

### Changing Output Format

To modify the filename format, edit the `clean_filename` generation:

```bash
# Current format: Title_VideoID.txt
local clean_filename="${title}_${video_id}.txt"

# Alternative formats:
local clean_filename="${video_id}_${title}.txt"  # ID first
local clean_filename="$(date +%Y%m%d)_${title}_${video_id}.txt"  # With date
```

## Integration with Other Tools

### Obsidian Integration

```bash
# Process directly to Obsidian vault
process-playlist "https://youtube.com/@channelname" "~/Obsidian/Transcripts" 20

# Create index file
ls ~/Obsidian/Transcripts/*.txt > ~/Obsidian/transcript_index.md
```

### NotebookLM Integration

```bash
# Process to specific directory for NotebookLM
mkdir -p ~/Documents/NotebookLM/Sources
batch-transcripts video_urls.txt ~/Documents/NotebookLM/Sources
```

## Best Practices

### Organization Strategy

1. **Use descriptive directory names:**
    
    ```bash
    process-playlist "URL" programming_tutorials 20
    process-playlist "URL" design_patterns 15
    process-playlist "URL" ai_research 25
    ```
    
2. **Create topic-specific URL files:**
    
    ```bash
    create-url-file programming_channels.txt
    create-url-file ai_research.txt
    create-url-file design_tutorials.txt
    ```
    
3. **Regular processing schedule:**
    
    ```bash
    # Weekly script to get recent content
    #!/bin/bash
    process-channel-recent "https://youtube.com/@channel1" 7 weekly_updates
    process-channel-recent "https://youtube.com/@channel2" 7 weekly_updates
    ```
    

### Performance Tips

- **Limit playlist sizes** to avoid overwhelming downloads
- **Use recent filtering** for active channels to get only new content
- **Process during off-peak hours** to be respectful to YouTube's servers
- **Organize by topic** for easier reference and analysis

## Next Steps

After installation, you can:

1. **Start with single videos** to test the setup
2. **Create organized URL files** for your learning topics
3. **Set up regular processing** for channels you follow
4. **Integrate with your note-taking workflow** (Obsidian, NotebookLM, etc.)
5. **Build analysis workflows** using the structured transcript files

The transcripts are now ready for analysis, note-taking, or feeding into AI tools for summarization and insight extraction.