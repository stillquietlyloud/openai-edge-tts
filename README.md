# OpenAI-Compatible Edge-TTS API 🗣️

![GitHub stars](https://img.shields.io/github/stars/travisvn/openai-edge-tts?style=social)
![GitHub forks](https://img.shields.io/github/forks/travisvn/openai-edge-tts?style=social)
![GitHub repo size](https://img.shields.io/github/repo-size/travisvn/openai-edge-tts)
![GitHub top language](https://img.shields.io/github/languages/top/travisvn/openai-edge-tts)
![GitHub last commit](https://img.shields.io/github/last-commit/travisvn/openai-edge-tts?color=red)
[![Discord](https://img.shields.io/badge/Discord-Voice_AI_%26_TTS_Tools-blue?logo=discord&logoColor=white)](https://discord.gg/GkFbBCBqJ6)
[![LinkedIn](https://img.shields.io/badge/Connect_on_LinkedIn-%230077B5.svg?logo=linkedin&logoColor=white)](https://linkedin.com/in/travisvannimwegen)

This project provides a local, OpenAI-compatible text-to-speech (TTS) API using `edge-tts`. It emulates the OpenAI TTS endpoint (`/v1/audio/speech`), enabling users to generate speech from text with various voice options and playback speeds, just like the OpenAI API.

`edge-tts` uses Microsoft Edge's online text-to-speech service, so it is completely free.

[View this project on Docker Hub](https://hub.docker.com/r/travisvn/openai-edge-tts)

# Please ⭐️ star this repo if you find it helpful

## Features

- **OpenAI-Compatible Endpoint**: `/v1/audio/speech` with similar request structure and behavior.
- **SSE Streaming Support**: Real-time audio streaming via Server-Sent Events when `stream_format: "sse"` is specified.
- **Supported Voices**: Maps OpenAI voices (alloy, echo, fable, onyx, nova, shimmer) to `edge-tts` equivalents.
- **Flexible Formats**: Supports multiple audio formats (mp3, opus, aac, flac, wav, pcm).
- **Adjustable Speed**: Option to modify playback speed (0.25x to 4.0x).
- **Optional Direct Edge-TTS Voice Selection**: Use either OpenAI voice mappings or specify [any edge-tts voice](https://tts.travisvn.com) directly.

## ⚡️ Quick start

The simplest way to get started without having to configure anything is to run the command below

```bash
docker run -d -p 5050:5050 travisvn/openai-edge-tts:latest
```

This will run the service at port 5050 with all the default configs

_(Docker required, obviously)_

---

## ⚙️ Parameters & Voice Reference

### `/v1/audio/speech` — Request Parameters

| Parameter | Type | Required | Default | Description |
|---|---|---|---|---|
| `input` | string | ✅ | — | Text to synthesize (up to 4,096 characters) |
| `model` | string | ❌ | `tts-1` | TTS model identifier — see [Models](#models) |
| `voice` | string | ❌ | `en-US-AvaNeural` | OpenAI alias or direct edge-tts voice name — see [Voices](#voices) |
| `response_format` | string | ❌ | `mp3` | Audio output format — see [Audio Formats](#audio-formats) |
| `speed` | number | ❌ | `1.0` | Speech rate — `0.25` (slowest) to `4.0` (fastest) |
| `stream_format` | string | ❌ | `audio` | `"audio"` for raw binary stream, `"sse"` for Server-Sent Events |

---

### Models

| Model ID | Description |
|---|---|
| `tts-1` | Text-to-speech v1 (standard) |
| `tts-1-hd` | Text-to-speech v1 HD |
| `gpt-4o-mini-tts` | GPT-4o mini TTS |

> All three models use the same underlying Edge TTS engine. The model field is accepted for OpenAI API drop-in compatibility but does not change voice quality.

---

### Voices

#### OpenAI-Compatible Alias Voices

These short names are drop-in replacements for the standard OpenAI TTS voice names:

| Alias | Mapped Edge-TTS Voice | Gender | Locale |
|---|---|---|---|
| `alloy` | `en-US-JennyNeural` | Female | English (US) |
| `ash` | `en-US-AndrewNeural` | Male | English (US) |
| `ballad` | `en-GB-ThomasNeural` | Male | English (UK) |
| `coral` | `en-AU-NatashaNeural` | Female | English (AU) |
| `echo` | `en-US-GuyNeural` | Male | English (US) |
| `fable` | `en-GB-SoniaNeural` | Female | English (UK) |
| `nova` | `en-US-AriaNeural` | Female | English (US) |
| `onyx` | `en-US-EricNeural` | Male | English (US) |
| `sage` | `en-US-JennyNeural` | Female | English (US) |
| `shimmer` | `en-US-EmmaNeural` | Female | English (US) |
| `verse` | `en-US-BrianNeural` | Male | English (US) |

#### Direct Edge-TTS Voices

You can bypass the alias mapping and use any Edge-TTS voice directly by passing its full name as `voice`. Voice names follow the pattern **`{locale}-{Name}Neural`**:

```bash
# Japanese male voice
"voice": "ja-JP-KeitaNeural"

# Spanish (Mexico) male voice
"voice": "es-MX-JorgeNeural"

# French (France) female voice
"voice": "fr-FR-DeniseNeural"
```

Use the **`GET /v1/voices?language={locale}`** endpoint to retrieve voices for a specific locale, **`GET /v1/voices/all`** for the complete list, or browse and play samples at [tts.travisvn.com](https://tts.travisvn.com).

#### Notable English Voices

| Voice | Gender | Locale |
|---|---|---|
| `en-US-AvaNeural` *(server default)* | Female | English (US) |
| `en-US-AriaNeural` | Female | English (US) |
| `en-US-JennyNeural` | Female | English (US) |
| `en-US-EmmaNeural` | Female | English (US) |
| `en-US-GuyNeural` | Male | English (US) |
| `en-US-AndrewNeural` | Male | English (US) |
| `en-US-EricNeural` | Male | English (US) |
| `en-US-BrianNeural` | Male | English (US) |
| `en-US-ChristopherNeural` | Male | English (US) |
| `en-GB-SoniaNeural` | Female | English (UK) |
| `en-GB-LibbyNeural` | Female | English (UK) |
| `en-GB-RyanNeural` | Male | English (UK) |
| `en-AU-NatashaNeural` | Female | English (AU) |
| `en-AU-WilliamNeural` | Male | English (AU) |
| `en-CA-ClaraNeural` | Female | English (CA) |
| `en-CA-LiamNeural` | Male | English (CA) |
| `en-IN-NeerjaNeural` | Female | English (IN) |
| `en-IN-PrabhatNeural` | Male | English (IN) |

---

### Supported Languages & Locales

Edge TTS supports **100+ locales** across 50+ languages. Each locale includes at least one male and one female voice.

#### English

| Locale | Region |
|---|---|
| `en-US` | United States |
| `en-GB` | United Kingdom |
| `en-AU` | Australia |
| `en-CA` | Canada |
| `en-IN` | India |
| `en-IE` | Ireland |
| `en-NZ` | New Zealand |
| `en-SG` | Singapore |
| `en-HK` | Hong Kong SAR |
| `en-PH` | Philippines |
| `en-ZA` | South Africa |
| `en-NG` | Nigeria |
| `en-KE` | Kenya |
| `en-TZ` | Tanzania |

#### Spanish

| Locale | Region |
|---|---|
| `es-ES` | Spain |
| `es-MX` | Mexico |
| `es-US` | United States |
| `es-AR` | Argentina |
| `es-CO` | Colombia |
| `es-CL` | Chile |
| `es-PE` | Peru |
| `es-VE` | Venezuela |
| `es-BO` `es-CR` `es-CU` `es-DO` `es-EC` `es-GQ` `es-GT` `es-HN` `es-NI` `es-PA` `es-PR` `es-PY` `es-SV` `es-UY` | Other Latin America |

#### Major World Languages

| Locale | Language |
|---|---|
| `fr-FR` | French (France) |
| `fr-CA` | French (Canada) |
| `fr-BE` | French (Belgium) |
| `fr-CH` | French (Switzerland) |
| `de-DE` | German (Germany) |
| `de-AT` | German (Austria) |
| `de-CH` | German (Switzerland) |
| `it-IT` | Italian |
| `pt-BR` | Portuguese (Brazil) |
| `pt-PT` | Portuguese (Portugal) |
| `ja-JP` | Japanese |
| `ko-KR` | Korean |
| `zh-CN` | Chinese (Mandarin, Simplified) |
| `zh-TW` | Chinese (Taiwanese Mandarin, Traditional) |
| `zh-HK` | Chinese (Cantonese, Traditional) |
| `ru-RU` | Russian |
| `ar-AE` `ar-BH` `ar-DZ` `ar-EG` `ar-IQ` `ar-JO` `ar-KW` `ar-LB` `ar-LY` `ar-MA` `ar-OM` `ar-QA` `ar-SA` `ar-SY` `ar-TN` `ar-YE` | Arabic (16 regions) |
| `hi-IN` | Hindi (India) |
| `tr-TR` | Turkish |
| `pl-PL` | Polish |
| `nl-NL` | Dutch (Netherlands) |
| `nl-BE` | Dutch (Belgium) |
| `sv-SE` | Swedish |
| `nb-NO` | Norwegian Bokmål |
| `da-DK` | Danish |
| `fi-FI` | Finnish |
| `cs-CZ` | Czech |
| `sk-SK` | Slovak |
| `hu-HU` | Hungarian |
| `ro-RO` | Romanian |
| `uk-UA` | Ukrainian |
| `bg-BG` | Bulgarian |
| `hr-HR` | Croatian |
| `el-GR` | Greek |
| `he-IL` | Hebrew |
| `id-ID` | Indonesian |
| `ms-MY` | Malay |
| `th-TH` | Thai |
| `vi-VN` | Vietnamese |

#### Additional Languages

| Locale | Language |
|---|---|
| `af-ZA` | Afrikaans (South Africa) |
| `am-ET` | Amharic (Ethiopia) |
| `as-IN` | Assamese (India) |
| `az-AZ` | Azerbaijani (Azerbaijan) |
| `bn-BD` | Bangla (Bangladesh) |
| `bn-IN` | Bengali (India) |
| `bs-BA` | Bosnian (Bosnia & Herzegovina) |
| `ca-ES` | Catalan |
| `cy-GB` | Welsh (UK) |
| `et-EE` | Estonian |
| `eu-ES` | Basque |
| `fa-IR` | Persian (Iran) |
| `fil-PH` | Filipino (Philippines) |
| `ga-IE` | Irish (Ireland) |
| `gl-ES` | Galician |
| `gu-IN` | Gujarati (India) |
| `hy-AM` | Armenian |
| `is-IS` | Icelandic |
| `iu-Cans-CA` | Inuktitut (Syllabics, Canada) |
| `iu-Latn-CA` | Inuktitut (Latin, Canada) |
| `jv-ID` | Javanese (Indonesia) |
| `ka-GE` | Georgian |
| `kk-KZ` | Kazakh |
| `km-KH` | Khmer (Cambodia) |
| `kn-IN` | Kannada (India) |
| `lo-LA` | Lao (Laos) |
| `lt-LT` | Lithuanian |
| `lv-LV` | Latvian |
| `mk-MK` | Macedonian |
| `ml-IN` | Malayalam (India) |
| `mn-MN` | Mongolian |
| `mr-IN` | Marathi (India) |
| `mt-MT` | Maltese |
| `my-MM` | Burmese (Myanmar) |
| `ne-NP` | Nepali |
| `or-IN` | Odia (India) |
| `pa-IN` | Punjabi (India) |
| `ps-AF` | Pashto (Afghanistan) |
| `si-LK` | Sinhala (Sri Lanka) |
| `sl-SI` | Slovenian |
| `so-SO` | Somali |
| `sq-AL` | Albanian |
| `sr-RS` | Serbian (Cyrillic) |
| `sr-Latn-RS` | Serbian (Latin) |
| `su-ID` | Sundanese (Indonesia) |
| `sw-KE` | Kiswahili (Kenya) |
| `sw-TZ` | Kiswahili (Tanzania) |
| `ta-IN` `ta-LK` `ta-MY` `ta-SG` | Tamil (India, Sri Lanka, Malaysia, Singapore) |
| `te-IN` | Telugu (India) |
| `ur-IN` | Urdu (India) |
| `ur-PK` | Urdu (Pakistan) |
| `uz-UZ` | Uzbek |
| `wuu-CN` | Chinese (Wu, Simplified) |
| `yue-CN` | Chinese (Cantonese, Simplified) |
| `zh-CN-liaoning` | Chinese (Northeastern Mandarin) |
| `zh-CN-shaanxi` | Chinese (Zhongyuan Mandarin, Shaanxi) |
| `zh-CN-sichuan` | Chinese (Southwestern Mandarin) |
| `zu-ZA` | Zulu (South Africa) |

> For the complete, live list of voices for any locale, call `GET /v1/voices?language={locale}` or visit [tts.travisvn.com](https://tts.travisvn.com).

---

### Audio Formats

| Format | MIME Type | Notes |
|---|---|---|
| `mp3` *(default)* | `audio/mpeg` | Universally compatible — **no ffmpeg required** |
| `opus` | `audio/ogg; codec=opus` | Low-latency streaming — requires ffmpeg |
| `aac` | `audio/aac` | Good compression, widely supported — requires ffmpeg |
| `flac` | `audio/flac` | Lossless audio — requires ffmpeg |
| `wav` | `audio/wav` | Uncompressed PCM — requires ffmpeg |
| `pcm` | `audio/pcm` | Raw 16-bit PCM samples at 24 kHz — requires ffmpeg |

> Only `mp3` works without ffmpeg. For all other formats, use the `latest-ffmpeg` Docker image tag:
> ```bash
> docker run -d -p 5050:5050 travisvn/openai-edge-tts:latest-ffmpeg
> ```

---

### Speed

The `speed` parameter accepts a float from **`0.25`** to **`4.0`** (default `1.0`). It maps to the Edge TTS SSML `<prosody rate>` attribute:

| `speed` | SSML `rate` | Effect |
|---|---|---|
| `0.25` | `-75%` | Very slow |
| `0.5` | `-50%` | Half speed |
| `0.75` | `-25%` | Slightly slow |
| `1.0` | `+0%` | Normal (default) |
| `1.5` | `+50%` | Faster |
| `2.0` | `+100%` | Double speed |
| `4.0` | `+300%` | Maximum speed |

---

### Server Environment Variables

Set these in a `.env` file or pass them as container environment variables:

| Variable | Default | Description |
|---|---|---|
| `API_KEY` | `your_api_key_here` | Bearer token clients must send. Any string works — no real key needed |
| `PORT` | `5050` | Port the server listens on |
| `DEFAULT_VOICE` | `en-US-AvaNeural` | Voice used when the request omits `voice` |
| `DEFAULT_RESPONSE_FORMAT` | `mp3` | Format used when the request omits `response_format` |
| `DEFAULT_SPEED` | `1.0` | Speed used when the request omits `speed` |
| `DEFAULT_LANGUAGE` | `en-US` | Language hint for text pre-processing |
| `REQUIRE_API_KEY` | `True` | Set to `False` to disable Bearer token authentication |
| `REMOVE_FILTER` | `False` | Set to `True` to skip Markdown/emoji cleaning on the input text |
| `EXPAND_API` | `True` | Enable extra endpoint aliases (ElevenLabs & Azure AI Speech compatible routes) |
| `DETAILED_ERROR_LOGGING` | `True` | Include full stack traces in server error logs |

---

## Setup

### Prerequisites

- **Docker** (recommended): Docker and Docker Compose for containerized setup.
- **Python** (optional): For local development, install dependencies in `requirements.txt`.
- **ffmpeg** (optional): Required for audio format conversion. Optional if sticking to mp3.

### Installation

1. **Clone the Repository**:

```bash
git clone https://github.com/travisvn/openai-edge-tts.git
cd openai-edge-tts
```

2. **Environment Variables**: Create a `.env` file in the root directory with the following variables:

```
API_KEY=your_api_key_here
PORT=5050

DEFAULT_VOICE=en-US-AvaNeural
DEFAULT_RESPONSE_FORMAT=mp3
DEFAULT_SPEED=1.0

DEFAULT_LANGUAGE=en-US

REQUIRE_API_KEY=True
REMOVE_FILTER=False
EXPAND_API=True
DETAILED_ERROR_LOGGING=True
```

Or, copy the default `.env.example` with the following:

```bash
cp .env.example .env
```

3. **Run with Docker Compose** (recommended):

```bash
docker compose up --build
```

Run with `-d` to run docker compose in "detached mode", meaning it will run in the background and free up your terminal.

```bash
docker compose up -d
```

<details>
<summary>

#### Building Locally with FFmpeg using Docker Compose

</summary>

By default, `docker compose up --build` creates a minimal image _without_ `ffmpeg`. If you're building locally (after cloning this repository) and need `ffmpeg` for audio format conversions (beyond MP3), you can include it in the build.

This is controlled by the `INSTALL_FFMPEG_ARG` build argument. Set this environment variable to `true` in one of these ways:

1.  **Prefixing the command:**
    ```bash
    INSTALL_FFMPEG_ARG=true docker compose up --build
    ```
2.  **Adding to your `.env` file:**
    Add this line to the `.env` file in the project root:
    ```env
    INSTALL_FFMPEG_ARG=true
    ```
    Then, run `docker compose up --build`.
3.  **Exporting in your shell environment:**
    Add `export INSTALL_FFMPEG_ARG=true` to your shell configuration (e.g., `~/.zshrc`, `~/.bashrc`) and reload your shell. Then `docker compose up --build` will use it.

This is for local builds. For pre-built Docker Hub images, add the `latest-ffmpeg` tag to the version

```bash
docker run -d -p 5050:5050 -e API_KEY=your_api_key_here -e PORT=5050 travisvn/openai-edge-tts:latest-ffmpeg
```

---

</details>

Alternatively, **run directly with Docker**:

```bash
docker build -t openai-edge-tts .
docker run -p 5050:5050 --env-file .env openai-edge-tts
```

To run the container in the background, add `-d` after the `docker run` command:

```bash
docker run -d -p 5050:5050 --env-file .env openai-edge-tts
```

4. **Access the API**: Your server will be accessible at `http://localhost:5050`.

<details>
<summary>

## Running with Python

</summary>

If you prefer to run this project directly with Python, follow these steps to set up a virtual environment, install dependencies, and start the server.

### 1. Clone the Repository

```bash
git clone https://github.com/travisvn/openai-edge-tts.git
cd openai-edge-tts
```

### 2. Set Up a Virtual Environment

Create and activate a virtual environment to isolate dependencies:

```bash
# For macOS/Linux
python3 -m venv venv
source venv/bin/activate

# For Windows
python -m venv venv
venv\Scripts\activate
```

### 3. Install Dependencies

Use `pip` to install the required packages listed in `requirements.txt`:

```bash
pip install -r requirements.txt
```

### 4. Configure Environment Variables

Create a `.env` file in the root directory and set the following variables:

```plaintext
API_KEY=your_api_key_here
PORT=5050

DEFAULT_VOICE=en-US-AvaNeural
DEFAULT_RESPONSE_FORMAT=mp3
DEFAULT_SPEED=1.0

DEFAULT_LANGUAGE=en-US

REQUIRE_API_KEY=True
REMOVE_FILTER=False
EXPAND_API=True
DETAILED_ERROR_LOGGING=True
```

### 5. Run the Server

Once configured, start the server with:

```bash
python app/server.py
```

The server will start running at `http://localhost:5050`.

### 6. Test the API

You can now interact with the API at `http://localhost:5050/v1/audio/speech` and other available endpoints. See the [Usage](#usage) section for request examples.

</details>

<details>
<summary>

## Usage

</summary>

#### Endpoint: `/v1/audio/speech`

Generates audio from the input text. Available parameters:

**Required Parameter:**

- **input** (string): The text to be converted to audio (up to 4096 characters).

**Optional Parameters:**

- **model** (string): Set to "tts-1" or "tts-1-hd" (default: `"tts-1"`).
- **voice** (string): One of the OpenAI-compatible voices (alloy, echo, fable, onyx, nova, shimmer) or any valid `edge-tts` voice (default: `"en-US-AvaNeural"`).
- **response_format** (string): Audio format. Options: `mp3`, `opus`, `aac`, `flac`, `wav`, `pcm` (default: `mp3`).
- **speed** (number): Playback speed (0.25 to 4.0). Default is `1.0`.
- **stream_format** (string): Response format. Options: `"audio"` (raw audio data, default) or `"sse"` (Server-Sent Events streaming with JSON events).

**Note:** The API is fully compatible with OpenAI's TTS API specification. The `instructions` parameter (for fine-tuning voice characteristics) is not currently supported, but all other parameters work identically to OpenAI's implementation.

#### Standard Audio Generation

Example request with `curl` and saving the output to an mp3 file:

```bash
curl -X POST http://localhost:5050/v1/audio/speech \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer your_api_key_here" \
  -d '{
    "input": "Hello, I am your AI assistant! Just let me know how I can help bring your ideas to life.",
    "voice": "echo",
    "response_format": "mp3",
    "speed": 1.1
  }' \
  --output speech.mp3
```

#### Direct Audio Playback (like OpenAI)

You can pipe the audio directly to `ffplay` for immediate playback, just like OpenAI's API:

```bash
curl -X POST http://localhost:5050/v1/audio/speech \
  -H "Authorization: Bearer your_api_key_here" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "tts-1",
    "input": "Today is a wonderful day to build something people love!",
    "voice": "alloy",
    "response_format": "mp3"
  }' | ffplay -i -
```

Or for immediate playback without saving to file:

```bash
curl -X POST http://localhost:5050/v1/audio/speech \
  -H "Authorization: Bearer your_api_key_here" \
  -H "Content-Type: application/json" \
  -d '{
    "input": "This will play immediately without saving to disk!",
    "voice": "shimmer"
  }' | ffplay -autoexit -nodisp -i -
```

Or, to be in line with the OpenAI API endpoint parameters:

```bash
curl -X POST http://localhost:5050/v1/audio/speech \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer your_api_key_here" \
  -d '{
    "model": "tts-1",
    "input": "Hello, I am your AI assistant! Just let me know how I can help bring your ideas to life.",
    "voice": "alloy"
  }' \
  --output speech.mp3
```

#### Server-Sent Events (SSE) Streaming

For applications that need structured streaming events (like web applications), use SSE format:

```bash
curl -X POST http://localhost:5050/v1/audio/speech \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer your_api_key_here" \
  -d '{
    "model": "tts-1",
    "input": "This will stream as Server-Sent Events with JSON data containing base64-encoded audio chunks.",
    "voice": "alloy",
    "stream_format": "sse"
  }'
```

**SSE Response Format:**

```
data: {"type": "speech.audio.delta", "audio": "base64-encoded-audio-chunk"}

data: {"type": "speech.audio.delta", "audio": "base64-encoded-audio-chunk"}

data: {"type": "speech.audio.done", "usage": {"input_tokens": 12, "output_tokens": 0, "total_tokens": 12}}
```

#### JavaScript/Web Usage

Example using fetch API for SSE streaming:

```javascript
async function streamTTSWithSSE(text) {
  const response = await fetch('http://localhost:5050/v1/audio/speech', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
      Authorization: 'Bearer your_api_key_here',
    },
    body: JSON.stringify({
      input: text,
      voice: 'alloy',
      stream_format: 'sse',
    }),
  });

  const reader = response.body.getReader();
  const decoder = new TextDecoder();
  const audioChunks = [];

  while (true) {
    const { done, value } = await reader.read();
    if (done) break;

    const chunk = decoder.decode(value);
    const lines = chunk.split('\n');

    for (const line of lines) {
      if (line.startsWith('data: ')) {
        const data = JSON.parse(line.slice(6));

        if (data.type === 'speech.audio.delta') {
          // Decode base64 audio chunk
          const audioData = atob(data.audio);
          const audioArray = new Uint8Array(audioData.length);
          for (let i = 0; i < audioData.length; i++) {
            audioArray[i] = audioData.charCodeAt(i);
          }
          audioChunks.push(audioArray);
        } else if (data.type === 'speech.audio.done') {
          console.log('Speech synthesis complete:', data.usage);

          // Combine all chunks and play
          const totalLength = audioChunks.reduce(
            (sum, chunk) => sum + chunk.length,
            0
          );
          const combinedArray = new Uint8Array(totalLength);
          let offset = 0;
          for (const chunk of audioChunks) {
            combinedArray.set(chunk, offset);
            offset += chunk.length;
          }

          const audioBlob = new Blob([combinedArray], { type: 'audio/mpeg' });
          const audioUrl = URL.createObjectURL(audioBlob);
          const audio = new Audio(audioUrl);
          audio.play();
          return;
        }
      }
    }
  }
}

// Usage
streamTTSWithSSE('Hello from SSE streaming!');
```

#### International Language Example

And an example of a language other than English:

```bash
curl -X POST http://localhost:5050/v1/audio/speech \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer your_api_key_here" \
  -d '{
    "model": "tts-1",
    "input": "じゃあ、行く。電車の時間、調べておくよ。",
    "voice": "ja-JP-KeitaNeural"
  }' \
  --output speech.mp3
```

#### JavaScript/Web Usage

Example using fetch API for SSE streaming:

```javascript
async function streamTTSWithSSE(text) {
  const response = await fetch('http://localhost:5050/v1/audio/speech', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
      Authorization: 'Bearer your_api_key_here',
    },
    body: JSON.stringify({
      input: text,
      voice: 'alloy',
      stream_format: 'sse',
    }),
  });

  const reader = response.body.getReader();
  const decoder = new TextDecoder();
  const audioChunks = [];

  while (true) {
    const { done, value } = await reader.read();
    if (done) break;

    const chunk = decoder.decode(value);
    const lines = chunk.split('\n');

    for (const line of lines) {
      if (line.startsWith('data: ')) {
        const data = JSON.parse(line.slice(6));

        if (data.type === 'speech.audio.delta') {
          // Decode base64 audio chunk
          const audioData = atob(data.audio);
          const audioArray = new Uint8Array(audioData.length);
          for (let i = 0; i < audioData.length; i++) {
            audioArray[i] = audioData.charCodeAt(i);
          }
          audioChunks.push(audioArray);
        } else if (data.type === 'speech.audio.done') {
          console.log('Speech synthesis complete:', data.usage);

          // Combine all chunks and play
          const totalLength = audioChunks.reduce(
            (sum, chunk) => sum + chunk.length,
            0
          );
          const combinedArray = new Uint8Array(totalLength);
          let offset = 0;
          for (const chunk of audioChunks) {
            combinedArray.set(chunk, offset);
            offset += chunk.length;
          }

          const audioBlob = new Blob([combinedArray], { type: 'audio/mpeg' });
          const audioUrl = URL.createObjectURL(audioBlob);
          const audio = new Audio(audioUrl);
          audio.play();
          return;
        }
      }
    }
  }
}

// Usage
streamTTSWithSSE('Hello from SSE streaming!');
```

#### Additional Endpoints

- **POST/GET /v1/models**: Lists available TTS models.
- **POST/GET /v1/voices**: Lists `edge-tts` voices for a given language / locale.
- **POST/GET /v1/voices/all**: Lists all `edge-tts` voices, with language support information.

</details>

### Contributing

Contributions are welcome! Please fork the repository and create a pull request for any improvements.

### License

This project is licensed under GNU General Public License v3.0 (GPL-3.0), and the acceptable use-case is intended to be personal use. For enterprise or non-personal use of `openai-edge-tts`, contact me at tts@travisvn.com

---

## Example Use Case

> [!TIP]
> Swap `localhost` to your local IP (ex. `192.168.0.1`) if you have issues
>
> _It may be the case that, when accessing this endpoint on a different server / computer or when the call is made from another source (like Open WebUI), you need to change the URL from `localhost` to your local IP (something like `192.168.0.1` or similar)_

# Open WebUI

Open up the Admin Panel and go to Settings -> Audio

Below, you can see a screenshot of the correct configuration for using this project to substitute the OpenAI endpoint

![Screenshot of Open WebUI Admin Settings for Audio adding the correct endpoints for this project](https://utfs.io/f/MMMHiQ1TQaBo9GgL4WcUbjSRlqi86sV3TXh47KYBJCkdQ20M)

If you're running both Open WebUI and this project in Docker, the API endpoint URL is probably `http://host.docker.internal:5050/v1`

> [!NOTE]
> View the official docs for [Open WebUI integration with OpenAI Edge TTS](https://docs.openwebui.com/tutorials/text-to-speech/openai-edge-tts-integration)

# AnythingLLM

In version 1.6.8, AnythingLLM added support for "generic OpenAI TTS providers" — meaning we can use this project as the TTS provider in AnythingLLM

Open up settings and go to Voice & Speech (Under AI Providers)

Below, you can see a screenshot of the correct configuration for using this project to substitute the OpenAI endpoint

![Screenshot of AnythingLLM settings for Voice adding the correct endpoints for this project](https://utfs.io/f/MMMHiQ1TQaBoGx6WUTRDJUWPLqoMsXiNkajAdVOwgcxH6uv7)

---

## Quick Info

- `your_api_key_here` never needs to be replaced — No "real" API key is required. Use whichever string you'd like.
- The quickest way to get this up and running is to install docker and run the command below:

```bash
docker run -d -p 5050:5050 -e API_KEY=your_api_key_here -e PORT=5050 travisvn/openai-edge-tts:latest
```

---

# Voice Samples 🎙️

[Play voice samples and see all available Edge TTS voices](https://tts.travisvn.com/)
