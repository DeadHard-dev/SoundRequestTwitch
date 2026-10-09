# SoundRequestTwitch

**SoundRequestTwitch** is a Windows desktop app for Twitch music requests using Channel Points.

Developed by **DeadHardGaming**  
Twitch: [DeadHardGamingPOE](https://www.twitch.tv/DeadHardGamingPOE)

## Features

- Twitch OAuth login;
- automatic Channel Points reward setup;
- moderated or automatic request queue;
- automatic next-track playback;
- OBS Browser Source overlay;
- transparent and customizable widgets;
- color presets, animations, and audio-reactive pulse;
- selectable fonts and text size;
- cover art, title, duration, and progress display;
- clean native MP4/WebM player;
- persistent Twitch session after restart;
- portable Windows `.exe` — no installer required.

## Download

The portable application is created in `release/`:

```text
SoundRequestTwitch 1.0.0.exe
```

## First launch

1. Start `SoundRequestTwitch 1.0.0.exe`.
2. Open **Configure Twitch credentials**.
3. Create an application in the [Twitch Developer Console](https://dev.twitch.tv/console/apps).
4. Add this Redirect URL:

```text
http://localhost:3000/auth/twitch/callback
```

5. Enter the Client ID and Client Secret in the app.
6. Click **Sign in with Twitch**.
7. Authorize Channel Points access.

The Client Secret is encrypted and is never bundled into the executable.

## Channel Points setup

1. Open the **Moderator panel**.
2. Set the reward name and cost.
3. Select automatic or moderator approval.
4. Save the settings.

The app creates or updates the managed Twitch reward automatically.

## OBS setup

1. Open the **Moderator panel**.
2. Click **Copy link** in the OBS section.
3. Add a **Browser** source in OBS.
4. Paste the copied URL.
5. Set the size to `800 × 600`.
6. Enable **Control audio via OBS**.
7. Make sure the source is not muted in the OBS mixer.

After changing the URL or overlay settings, click **Refresh browser** in OBS.

## Overlay customization

Open **Overlay settings** to configure:

- widget style;
- font and text size;
- text, accent, and background colors;
- transparency;
- position and layout;
- pulse animation and pulse color;
- video, cover art, progress, and queue visibility.

The Overlay background can be disabled completely so that only the widgets remain visible.

YouTube links use the official YouTube source. YouTube branding, advertising, and provider-controlled elements cannot be removed from the official video source. The app hides the standard controls and displays its own widget around the source.

## Application data

The app creates this folder on the Desktop:

```text
TwitchSoundRequest-data
```

It stores encrypted Twitch credentials, queue data, and overlay settings. Do not distribute this folder with the executable.

## Exiting the app

Closing the window minimizes the app to the system tray. To stop it completely:

1. Right-click the SoundRequestTwitch icon near the clock.
2. Select **Exit**.

## Security

- Never publish a Twitch Client Secret.
- Never share the `TwitchSoundRequest-data` folder.
- Each local installation must authorize its own Twitch channel.
- The app does not download protected YouTube media.
