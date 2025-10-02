# YouTube Translated Transcript Actor

An Apify Actor that extracts and translates YouTube video transcripts to your desired language. This API is listed on [apify](https://apify.com/pintostudio/youtube-translated-transcript)

## Overview

This Actor extracts transcripts from YouTube videos and provides translation capabilities. It returns both the original transcript and the translated version (if translation is needed), along with timing information for each subtitle segment.

## Features

- Extract transcripts from any YouTube video with available captions
- Translate transcripts to different languages using ISO 639-1 language codes
- Preserve timing information (start time and duration) for each subtitle segment
- Handle both automatic and manual captions
- Support for multiple languages

## Input Parameters

The Actor accepts the following input parameters:

| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| `videoUrl` | String | No | `https://www.youtube.com/watch?v=1WEAJ-DFkHE` | Full YouTube video URL |
| `targetLanguage` | String | No | `en` | Target language code in ISO 639-1 format (e.g., "en", "es", "fr", "de") |

### Input Example

```json
{
  "videoUrl": "https://www.youtube.com/watch?v=dQw4w9WgXcQ",
  "targetLanguage": "es"
}
```

## Output Format

The Actor returns a structured object containing:

- **videoId**: Extracted YouTube video ID
- **originalTranscript**: Original transcript with timing information
- **translatedTranscript**: Translated transcript (same as original if target language matches source)
- **message**: Status message about the translation process

### Output Example

```json
{
  "videoId": "1WEAJ-DFkHE",
  "originalTranscript": {
    "transcript": [
      {
        "start": "0.120",
        "dur": "1.650",
        "text": "- We're gonna fly on this jet"
      },
      {
        "start": "1.770",
        "dur": "2.370",
        "text": "that costs half a million\ndollars per flight"
      }
    ],
    "language": "en"
  },
  "translatedTranscript": {
    "transcript": [
      {
        "start": "0.120",
        "dur": "1.650",
        "text": "- Vamos a volar en este jet"
      },
      {
        "start": "1.770",
        "dur": "2.370",
        "text": "que cuesta medio millón\ndólares por vuelo"
      }
    ],
    "language": "es"
  },
  "message": "Translation completed successfully!"
}
```

## Transcript Object Structure

Each transcript object contains:

- **transcript**: Array of subtitle segments
  - **start**: Start time in seconds (string)
  - **dur**: Duration in seconds (string)
  - **text**: Subtitle text content
- **language**: Language code of the transcript

## Supported Languages

The Actor supports translation to any language using standard ISO 639-1 language codes:

- `en` - English
- `es` - Spanish
- `fr` - French
- `de` - German
- `it` - Italian
- `pt` - Portuguese
- `ru` - Russian
- `ja` - Japanese
- `ko` - Korean
- `zh` - Chinese
- And many more...

## Usage Examples

### Basic Usage

```javascript
import { ApifyClient } from 'apify-client';

const client = new ApifyClient({
    token: 'YOUR_API_TOKEN',
});

const input = {
    videoUrl: 'https://www.youtube.com/watch?v=VIDEO_ID',
    targetLanguage: 'es'
};

const run = await client.actor('your-actor-id').call(input);
const { items } = await client.dataset(run.defaultDatasetId).listItems();

console.log(items[0]);
```

### Batch Processing

```javascript
const videos = [
    'https://www.youtube.com/watch?v=VIDEO1',
    'https://www.youtube.com/watch?v=VIDEO2',
    'https://www.youtube.com/watch?v=VIDEO3'
];

for (const videoUrl of videos) {
    const input = { videoUrl, targetLanguage: 'fr' };
    await client.actor('the-actor-id').call(input);
}
```

## Error Handling

The Actor includes robust error handling for common issues:

- **Invalid URL format**: Validates YouTube URL structure
- **Invalid language code**: Ensures ISO 639-1 format compliance
- **Missing captions**: Handles videos without available transcripts
- **Translation errors**: Graceful fallback to original transcript

### Common Error Messages

- `Invalid target language code. Must be ISO 639-1 format (e.g., "en", "es")`
- `Video not found or has no available captions`
- `Translation service unavailable, returning original transcript`



## Limitations

- Requires videos to have available captions (automatic or manual)
- Translation quality depends on the underlying translation service
- Some languages may have limited support for certain video content



## Support and Troubleshooting

If you encounter issues:

1. Verify the YouTube URL is valid and accessible
2. Check that the target language code follows ISO 639-1 format
3. Ensure the video has available captions
4. Review Actor logs for detailed error information


---

## FAQs  

**Q1: Does this actor work for all YouTube videos?**  
A: No, it only works for public videos with transcripts enabled.  

**Q2: Can I scrape private videos?**  
A: No, private or restricted videos are not supported due to YouTube's privacy policies.  

**Q3: Is the actor compatible with playlists?**  
A: This actor processes a single video URL at a time. For playlist scraping, consider additional workflows.  

---

Start using the **YouTube Transcript Scraper** today to transform video content into actionable data! 🚀
