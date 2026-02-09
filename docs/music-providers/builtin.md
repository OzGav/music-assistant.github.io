# Builtin Provider  ![Preview image](../assets/icon.png){ width=70 align=right }

The Builtin provider supports manually adding track and radio station URLs to the database and it also generates various [Playlists](../usage.md/#playlists).

## Features

- You can add tracks accessible via URL or any online station. You can also add your locally created stations using, for example, [Icecast](https://icecast.org/).

## Settings

For each of the builtin playlists there is a toggle to enable or disable them. 

## Usage

1. In the main menu select 'Tracks' or 'Radio'
2. Then click on the icon in the top right corner

    ![screenshot](../assets/screenshots/url.png)

3. Add the full URL including http:// or https:// and optionally a custom name and image URL.

After completing step 3 the track or station will become available in the respective view.

## Known Issues / Notes

**Radio Stream Artist Artwork**. When playing radio streams, Music Assistant can display album or artist images instead of the station logo:

  - Requires "Artist - Title" format in the stream metadata (ICY or HLS)
  - Uses MusicBrainz to verify the artist + track combination for accurate matching
  - If a unique studio album is identified, album artwork is used (via TheAudioDB/Fanart.tv)
  - Falls back to artist artwork from TheAudioDB/Fanart.tv
  - Station logo is displayed when:
    - No artist/title metadata is available from the stream
    - The artist/track cannot be matched on MusicBrainz
    - No artwork is found on TheAudioDB or Fanart.tv
    - An advertisement is detected in the stream
  - Results are cached to minimize external API calls
