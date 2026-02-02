# youtube-5min

**NestJS backend that automatically generates 5-minute English lesson videos and publishes them to YouTube, TikTok, and Threads.**

## Tech Stack

| Layer | Technology |
|-------|-----------|
| Framework | NestJS 10 (TypeScript) |
| AI | OpenAI API (topic generation, vocabulary, dialogues) |
| TTS | Text-to-speech service (custom voice selection) |
| Video | FFmpeg (video compositing, audio mixing) |
| Queue | BullMQ + ioredis (job processing pipeline) |
| Scheduling | @nestjs/schedule (cron-based auto-generation) |
| Upload | YouTube Data API (googleapis), TikTok API, Threads API |
| HTTP | Axios |
| Validation | class-validator, class-transformer |
| Notifications | Slack webhooks |

## Architecture Overview

```
src/
├── main.ts                      # NestJS bootstrap
├── app.module.ts                # Root module (imports all feature modules)
├── lessons/
│   ├── lessons.controller.ts    # REST endpoints (create, status, simulate)
│   ├── lessons.service.ts       # Lesson lifecycle, word generation, voice selection
│   ├── lesson-scheduler.service.ts # Cron-based automatic lesson generation
│   ├── dto/create-lesson.dto.ts # Input validation (topic, words, voice preferences)
│   └── entities/lesson.entity.ts# Lesson data model (words, dialogue, status, progress)
├── openai/                      # OpenAI integration (topic, vocabulary, dialogue generation)
├── tts/                         # Text-to-speech audio generation
├── voice/                       # Voice catalog (gender, accent, education voices)
├── images/                      # Thumbnail and frame image generation
├── videos/                      # Video rendering and compositing
├── ffmpeg/                      # FFmpeg wrapper for audio/video processing
├── pipeline/
│   ├── pipeline.processor.ts    # Multi-step job processor (the core engine)
│   ├── pipeline.queue.ts        # BullMQ job queue management
│   ├── pipeline.types.ts        # Pipeline step type definitions
│   └── pipeline.module.ts       # Pipeline module wiring
├── youtube/                     # YouTube Data API upload and metadata
├── tiktok/                      # TikTok video upload
├── threads/                     # Threads post publishing
├── shorts/                      # YouTube Shorts generation
├── multiplatform/               # Cross-platform publishing orchestration
├── storage/                     # File system storage (lesson directories, assets)
├── slack/                       # Slack notification webhooks
├── error-history/               # Error tracking and history logging
├── dictionary/                  # Word dictionary data
├── assets/                      # Static assets (fonts, templates)
├── data/                        # Lesson data files
└── output/                      # Generated video output
```

## Key Features

- **AI-Powered Content** -- OpenAI generates topics, 15 vocabulary words (10 verbs + 5 nouns) with IPA, definitions in English/Vietnamese, and example sentences. Also generates contextual dialogues and trivia.
- **Smart Voice Selection** -- voice catalog with filtering by gender (male/female), accent (American/British), and random selection modes.
- **Multi-Step Pipeline** -- BullMQ job queue processes lessons through: topic generation, word generation, dialogue creation, TTS audio, image rendering, video compositing (FFmpeg), and upload.
- **Automated Scheduling** -- cron jobs automatically create and publish new lessons on a configured schedule.
- **Multi-Platform Publishing** -- uploads to YouTube (full videos), YouTube Shorts, TikTok, and Threads with platform-specific formatting.
- **Progress Tracking** -- real-time lesson status (pending, processing, done, error) with percentage progress via REST API.
- **Error Recovery** -- error history service logs failures per lesson; lessons can be re-simulated from saved JSON state.
- **Slack Notifications** -- posts status updates and alerts to a Slack channel.

## How It Works

1. A lesson is created via `POST /lessons` (or automatically by the scheduler). The DTO specifies an optional topic, word list, and voice preferences.
2. `LessonsService` generates a unique lesson ID, uses OpenAI to generate the topic and 15 vocabulary words (if not provided), validates the word structure, and selects a voice.
3. The lesson is enqueued into BullMQ via `PipelineQueue`.
4. `PipelineProcessor` processes the job in stages:
   - **OpenAI**: Generate dialogue script and trivia from the vocabulary
   - **TTS**: Convert dialogue and word pronunciations to audio files
   - **Images**: Render vocabulary cards and scene frames
   - **FFmpeg**: Composite frames + audio into a 5-minute video (and a Shorts version)
   - **Upload**: Publish to YouTube via googleapis, plus TikTok and Threads
5. Progress is updated in real-time; the final video path and YouTube URL are stored in the lesson entity.
6. `GET /lessons/:id/status` returns the current status, progress percentage, and output URLs.
7. All lesson data is persisted to `lesson.json` files in the storage directory for recovery and re-simulation.
