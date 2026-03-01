SubTales Video Automation Workflow

This project is a comprehensive automation within n8n that utilizes Artificial Intelligence to generate stories, synthesize narration via gTTS, render videos using FFmpeg, and prepare content for automated posting.
Workflow Overview

    Scheduling: Automated triggers initiate the process on business days at 08:00.

    Theme Generation: The system selects random themes, characters, and locations to ensure unique story outputs.

    Creative Writing (LLM): Integration with Groq (Llama 3.1) generates detailed scripts based on the defined themes.

    Audio Synthesis: Text is converted to speech using Python and gTTS.

    Subtitle Generation: The process generates synchronized .srt files.

    Video Rendering: FFmpeg processes the visual output, overlaying audio, burned-in subtitles, and resizing content to 1080x1920 (vertical format).

    Platform Distribution: Generation of metadata, titles, and SEO-optimized descriptions for TikTok and YouTube.

System Requirements

The environment hosting the n8n instance must support the following:

    Python 3: With the gTTS library installed (pip install gTTS).

    FFmpeg & FFprobe: Required for audio and video stream manipulation, analysis, and rendering.

    Chromium & Playwright: Required for the automated web-based upload modules.

    File System Paths:

        /tmp/subtales_audio/ for temporary audio chunks.

        /data/subtales_video/ for final rendered output.

Workflow Architecture
1. Context Generation

The Choose Theme & Universe node sets the creative foundation. The Create Story Prompt node translates these into machine-readable instructions, strictly enforcing the removal of unwanted characters, quotes, and sound cues to ensure clean output.
2. Audio Processing

The text is split into segments for processing. The Execute gTTS Python node handles synthesis. The FFprobe utility validates audio duration against the background video duration to ensure the final result maintains integrity.
3. Rendering Pipeline

The Create Final Video node executes the core FFmpeg command:

    Input: Background loop and generated audio.

    Filter: scale=1080:1920 ensures vertical consistency; subtitles filter burns the text directly into the frame.

    Encoding: libx264 codec with -preset ultrafast and -crf 28 for an efficient balance between speed and visual quality.

Configuration and Security

    Groq API: Ensure your API key is correctly configured in the Groq Chat Model node.

    Session Management: For automated uploads, the system expects a session_tiktok.json file located in the home directory of the n8n service user.

    Validation: The Pre-Post Validation node ensures that the story length and word count align with the specific constraints of the video type (short or long) before proceeding to rendering.
