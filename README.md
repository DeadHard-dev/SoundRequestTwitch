# SongRequest

A portable Windows desktop app for Twitch channel-point song requests. It uses PyQt6, Twitch Helix/EventSub, yt-dlp, VLC and a local OBS browser overlay.

## Features

- Twitch OAuth sign-in with encrypted local token storage.
- Automatic channel-point reward creation and editing.
- Automatic recovery when the configured reward is deleted in Twitch.
- Queue management with play/pause, skip, clear, volume, and YouTube thumbnails.
- YouTube and SoundCloud playback; Spotify track metadata resolved to a playable YouTube result.
- OBS Browser Source overlay with title, thumbnail, progress, and audio-reactive effects.
- Overlay effects: soft glow, in-card equalizer bars, card pulse, or disabled.
- Ten overlay presets, custom colors, effect strength, reset-effects button, and live updates.
- Portable JSON settings, one-file Windows executable, system-tray minimize behavior, and Russian/English UI.

## Requirements

- Windows 10/11 and Python 3.11 or newer (64-bit recommended).
- VLC 3.x installed for running from source. The packaged build bundles VLC's runtime DLLs and plugins.
- A Windows audio output device for audio-reactive overlay glow.
- A Twitch application Client ID. No Client Secret is embedded in the app.

## Twitch application setup

1. Create an application in the [Twitch Developer Console](https://dev.twitch.tv/console/apps).
2. Set the OAuth Redirect URL to exactly `http://localhost:8080/callback`.
3. Choose the application category appropriate for your use and copy its Client ID.
4. Keep local TCP ports `8080` (one-time OAuth callback) and `3000` (OBS overlay) available.

On first launch, enter the Client ID and choose **Sign in**. The app opens Twitch in the system browser and requests `channel:manage:redemptions` and `channel:read:redemptions`. The access token is encrypted with Windows DPAPI for the current Windows user and stored in `songrequest.json` next to the app. The Client ID and reward settings are also in that JSON file. Moving the app to another Windows user or computer requires signing in again.

The Twitch implicit OAuth flow does not issue a refresh token. If Twitch expires or revokes the access token, sign in again in the app.

After sign-in, enter the channel-point reward name and cost, then click **Create reward**. After creation, the button becomes **Save changes** and updates only that saved reward (including a renamed title or cost). If the reward is deleted in Twitch, the app detects that it is missing and returns to **Create reward** mode. Twitch reward names are limited to 45 characters. The app only handles redemptions for that configured reward.

Twitch's Helix API does not have a `REJECTED` redemption status. Invalid requests, playback failures, skips, and cleared queue items are set to Twitch's `CANCELED` status, which returns the viewer's points; successfully completed tracks are set to `FULFILLED`.

A redemption can contain a YouTube, Spotify, or SoundCloud track URL, or a track title. YouTube links and text searches use yt-dlp; individual SoundCloud track links are extracted and played directly through yt-dlp. Spotify track links are resolved through Spotify's public oEmbed metadata endpoint, then searched on YouTube for playable audio because Spotify does not expose a playback stream to this app. Spotify albums/playlists and SoundCloud playlists are not accepted as individual song requests. Audio URLs are extracted only when a request reaches the front of the queue, and the media itself is streamed rather than downloaded.

## OBS overlay

The app includes an in-app setup guide and an OBS overlay URL field with a **Copy URL** button. In OBS, add **Sources → Browser**, paste `http://localhost:3000/overlay`, and set the source dimensions to `800 × 600`. The player card is centered horizontally and vertically within the browser source and stays inside its bounds when you choose a wider card. The queue displays a YouTube thumbnail beside direct YouTube requests; the thumbnail is loaded off the GUI thread from YouTube's public image endpoint. The local overlay serves the current title, thumbnail and playback progress over a WebSocket. It binds only to loopback and is not accessible from other computers.

Open **Overlay studio** from the main window to choose from ten ready-made presets or customize the theme, colors, rhythmic visual style, glow color/intensity, card width, opacity, corner radius, thumbnail, title, and progress bar. Rhythmic visual styles include soft glow, equalizer bars inside the card, card pulse, or disabled. Use **Reset effects** only when you want to restore the default effect settings; otherwise your selected effect, strength, color, and visual style remain saved. Preset buttons use a restrained dark-glass palette; the selected accent is shown as a thin border instead of a full rainbow fill. Settings are saved in the portable JSON and pushed live to the OBS browser source. While a track plays, the app captures the Windows speaker output using loopback audio, measures loudness and low-frequency transients, and drives the glow from those live audio levels and detected beats. The glow stops when playback is paused or stopped. If Windows has no usable output device, playback continues and the app reports that audio-reactive capture is unavailable in its activity log.

The overlay hides its track card when the WebSocket disconnects and the app sends an empty track state before a normal shutdown, so OBS does not keep showing the last song after you close SongRequest.

Use the **РУ / EN** button in the app header to switch languages. The selected language is saved in `songrequest.json`. The Twitch Client ID is masked by default; use **Show** only when you need to verify it. The desktop UI uses a calm slate-blue/teal palette, and the main panel scrolls on smaller displays so queue controls remain accessible without overlapping the current-track label. Clicking the window close button hides the app in the system tray; use the tray icon's right-click menu → **Exit** to stop the app completely.

## Troubleshooting

### The reward button still says “Save changes”

Make sure the latest executable is running. Close an older tray instance with right-click → **Exit**, then start the current `dist\SongRequest.exe`. While authenticated, SongRequest checks the Twitch reward list every five seconds. A deleted reward changes the button back to **Create reward**.

### The overlay is blank or not centered

Keep SongRequest running, refresh the OBS Browser Source, and use `http://localhost:3000/overlay`. Recommended Browser Source dimensions are `800 × 600`. The source is local-only, so OBS and SongRequest must run on the same computer.

### Audio-reactive effects do not move

Select an effect in **Overlay studio**, set a non-zero effect strength, and make sure Windows has a default output device. The app captures the default speaker output through Windows loopback; playback itself continues if loopback capture is unavailable.

### Spotify requests

Only individual Spotify tracks are supported. Albums and playlists are rejected. Spotify does not provide a direct stream to this app, so the track is searched on YouTube after its metadata is read.

