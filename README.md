# Playlist File Importer

> **Import missing in Multiselect for YouTube 4.0?** This restores it.

A small browser extension that imports a saved list of YouTube video IDs into **Watch Later**, an **existing playlist**, or a **new playlist**, in seconds.

It was written after version 4.0 of *Multiselect for YouTube* dropped its playlist import option. It reads the same export files (`msfy_export_*.json`), but works with any file that contains video IDs or links.

## Install

There is no store listing, so you load it as an unpacked extension:

1. Download this repository (green **Code** button → **Download ZIP**) and unzip it.
2. Open `chrome://extensions` (or `brave://extensions`, `edge://extensions`).
3. Turn on **Developer mode**.
4. Click **Load unpacked** and choose the folder that contains `manifest.json`.
5. Open or reload a `www.youtube.com` tab.

Built for Chromium-based browsers. Tested in Chrome and Brave; Edge should work but is untested. Firefox is not supported.

## Use

1. On any YouTube page, click the **Import list** button in the bottom-right corner.
2. Choose your file.
3. Choose where to add it: Watch Later, an existing playlist (paste its link or ID), or a new private playlist (type a name).
4. Press **Start import** and wait until the log says it has finished, then refresh the page.

### Options

| Option | What it does |
|---|---|
| Add in reverse order | Watch Later puts new videos on top, so this keeps your original order. Switched on by default for Watch Later. |
| Skip videos already in the playlist | Avoids duplicates. If YouTube rejects this option, the tool retries without it. |
| Videos per request | How many videos are added in each request (default 50, maximum 200). If a batch is rejected, it is split in half and retried. |

Progress is saved in your browser. If the import stops or errors, run it again with the same file and settings and it resumes where it left off. Videos that can't be added (private or deleted) are listed in the log when it finishes.

## Supported file formats

- A JSON array of video IDs: `["dQw4w9WgXcQ", "5iOhzJdDawE"]`
- A JSON array of objects with a `videoId`, `id` or `url` field
- A plain text file with one video ID or YouTube link per line

Duplicate IDs in the file are removed automatically.

## How it works

The extension runs on `www.youtube.com` and calls the same internal endpoints that the YouTube website uses to edit playlists (`youtubei/v1/browse/edit_playlist` and `youtubei/v1/playlist/create`), using the session you are already signed in with. Because it doesn't use the official YouTube Data API, there is no Google Cloud setup and no daily quota.

## Privacy

- Your file is read locally in your browser.
- The only requests made are to `youtube.com`, as you, to add the videos.
- Nothing is sent to any other server, and the extension requests no special browser permissions.
- Progress is stored in `localStorage` for `youtube.com`.

## Limitations

- **It uses undocumented, internal YouTube endpoints.** YouTube can change them at any time without notice, which may break the extension. If it stops working, please open an issue.
- Automating YouTube this way may fall outside YouTube's Terms of Service. Use it on your own account, for your own playlists, at your own risk.
- Very large imports may be rate limited by YouTube. If you see an error about rate limiting, wait a few minutes and run it again to resume.
- Watch Later and playlists have a maximum size set by YouTube.

## Troubleshooting

- **No Import list button:** reload the YouTube tab after installing, and check that the extension is enabled with no errors on the extensions page. In the page console (F12), you should see `[Playlist File Importer] loaded`.
- **Errors about being signed in:** make sure you are signed in to YouTube in the same browser profile.
- **Wrong order in Watch Later:** toggle **Add in reverse order** and run it again on a fresh list.

## Disclaimer

This project is not affiliated with, endorsed by, or sponsored by YouTube, Google, or the developers of Multiselect for YouTube.

## Licence

MIT. See [LICENSE](LICENSE).
