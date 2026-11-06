# YouTube Scraper

![YouTube Scraper](https://raw.githubusercontent.com/smoldesk/apify-scrapers/main/assets/youtube/banner.webp)

Scrape YouTube search results, channel videos, Shorts, playlists, channel profiles and comments. No API key, no quota, no login. Runs on [Apify](https://apify.com/smoldesk/youtube-scraper).

## What you get

- **Search**: keyword search with filters for upload date, duration, features (4K, subtitles, live), result type and country.
- **Videos**: title, views, likes, comment count, publish date, duration, description and its links, hashtags and chapters.
- **Channels**: videos, Shorts and live tabs of any `@handle`, plus a profile with subscribers, total views, join date, country, links and RSS URL.
- **Playlists**: every video in order, with its position.
- **Comments and replies**: text, likes, author, reply count, pinned and hearted flags.
- **New videos only**: `publishedAfter` stops reading a channel once it reaches older uploads.

## Run it

Open the [YouTube Scraper on Apify](https://apify.com/smoldesk/youtube-scraper), fill in the form and click Start. Or call it from code with your [Apify token](https://console.apify.com/settings/integrations):

```bash
curl -X POST "https://api.apify.com/v2/acts/smoldesk~youtube-scraper/run-sync-get-dataset-items?token=YOUR_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "searchQueries": ["home espresso setup"],
    "startUrls": [{"url": "https://www.youtube.com/@mkbhd"}],
    "maxResults": 50,
    "uploadDate": "month",
    "includeChannelInfo": true,
    "maxComments": 20
  }'
```

## Sample output

```json
{
  "type": "video",
  "id": "LKrwhjOI8Nc",
  "url": "https://www.youtube.com/watch?v=LKrwhjOI8Nc",
  "title": "There's never been a better time to go into math",
  "publishedDate": "2026-10-16",
  "durationSeconds": 2444,
  "viewCount": 2696714,
  "likeCount": 42758,
  "commentCount": 5300,
  "channelName": "3Blue1Brown",
  "channelHandle": "@3blue1brown",
  "channelSubscriberCount": 8740000,
  "chapters": [{"start": "0:00", "startSeconds": 0, "title": "Math student angst"}],
  "sourceType": "channel",
  "sourcePosition": 1
}
```

```json
{
  "type": "comment",
  "videoId": "dQw4w9WgXcQ",
  "text": "This song is nice when it stopped being spammed",
  "authorName": "@Kinutrix",
  "likeCount": 0,
  "replyCount": 0,
  "publishedAtApprox": "2026-11-03T17:12:13Z",
  "isPinned": false,
  "isReply": false
}
```

Video, channel and comment records always carry the same keys, so the CSV export opens cleanly in a spreadsheet. Pipe it into Google Sheets, Make, Zapier or n8n.

Only need comments or channel profiles? [YouTube Comments Scraper](https://github.com/smoldesk/youtube-comments-scraper) and [YouTube Channel Scraper](https://github.com/smoldesk/youtube-channel-scraper) do one job each with simpler input.

## Pricing

Pay per result. No monthly fee.

| Result | Per 1,000 |
|---|---|
| Video, Short or live stream | $1 |
| Channel profile | $1 |
| Comment or reply | $0.50 |

## Limits

- No transcripts and no video downloads.
- Tags, category and exact upload time are not returned. YouTube only shows them to its player.
- Shorts and live streams have `durationSeconds: null`.
- Subscriber and comment counts are rounded the way YouTube displays them. View and like counts are exact.

## FAQ

**Do I need a YouTube API key?**
No. There is no daily quota and no Google Cloud project to set up.

**How is this different from the YouTube Data API?**
The official API allows about 100 searches a day. This has no quota and returns chapters, description links and channel links in one run.

**How do I scrape all videos from a YouTube channel?**
Put the channel in `startUrls` and set `maxResults` to 0. Add `shorts` and `streams` to `channelTabs` for those tabs.

**How do I scrape YouTube comments?**
Set `maxComments` above 0. `commentsSortBy` picks top or newest, and `maxRepliesPerComment` adds replies.

**Can I get only the new videos of a channel?**
Yes. Set `publishedAfter` to `7 days` and schedule the run weekly.

## More scrapers

YouTube is one of 17 no-login scrapers by Smoldesk Studios. See the full list: [smoldesk/apify-scrapers](https://github.com/smoldesk/apify-scrapers).
