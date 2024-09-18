---
title: Installation
description: Installation guide for Music Assistant 
---

# Installing the Server

Music Assistant (MA) can be operated as a complete standalone product but it is actually designed to use side by side with Home Assistant. MA is built with automation in mind, hence the recommended installation method is to run the server as a Home assistant Add-on and then optionally [add the HA integration](https://music-assistant.io/integration/installation/). Install using one of the two methods below.


MA requires a 64bit Operating System and a minimum of 2GB of RAM on the physical device or the container (physical devices are recommended to have 4GB+ if they are running anything else)

## Primary installation method: Home Assistant Add-on

This is only available when running [HAOS](https://developers.home-assistant.io/docs/operating-system/). To install the Music Assistant Add-on:

1. Add the Music Assistant repository to your Home Assistant instance.

    [![Open your Home Assistant instance and show the add add-on repository dialog with a specific repository URL pre-filled.](https://my.home-assistant.io/badges/supervisor_add_addon_repository.svg)](https://my.home-assistant.io/redirect/supervisor_add_addon_repository/?repository_url=https%3A%2F%2Fgithub.com%2Fmusic-assistant%2Fhome-assistant-addon)

2. Install the Music Assistant add-on.

    [![Add Music Assistant as Add-on to Home Assistant.](https://my.home-assistant.io/badges/supervisor_addon.svg)](https://my.home-assistant.io/redirect/supervisor_addon/?addon=d5369777_music_assistant&repository_url=https%3A%2F%2Fgithub.com%2Fmusic-assistant%2Fhome-assistant-addon)
    
## Secondary installation method: Docker image

An alternative way to run the Music Assistant server is by running the docker image:

```
docker run --network host --privileged -v <dir>:/data ghcr.io/music-assistant/server
```

You must run the docker container with host network mode and in privileged mode. The data volume is `/data` - replace `<dir>` with a writable directory to ensure the data volume persists between updates.
If you want access to your local music files from within MA, make sure to also mount that, e.g. /media.
Note that accessing remote (SMB) shares can be done from within MA itself using the SMB File provider (but requires the privileged flag).

____________

## Server Notes

- Because the server heavily relies on multicast techniques like mDNS and uPnP to discover players in your network it MUST be run in the same Layer 2 network as your player devices.

- The server itself hosts a very simple webserver to stream audio to devices. This webinterface must be accessible via HTTP (no HTTPS) by IP-address from local players. See the server's logging at startup to see if the server has correctly auto-detected the local IP.

- You cannot run the slimproto integration in Home Assistant if the slimproto provider is enabled.

- The webinterface of the server can be reached on TCP port 8095 if enabled in the settings. By default, on HAOS based installations, the webserver is internal only and not exposed to the outside world. It is protected by HA authentication using the Ingress feature of Home Assistant (you also get the sidepanel shortcut for free!)
  
- You can access the frontend over https by following these steps:
  
    1. Expose the webserver via MA settings>>Core>>webserver
  
    2. To access the frontend behind a reverse proxy you will have to configure the reverse proxy to point at the 8095 port and expose it to whatever is desired (and add an ssl certificate). How that works differs for each implemention. 

!!! tip 
    You can keep the server more secure by NOT exposing the webserver and let the addon talk directly to the webserver on the internal docker network. In that case the internal dns name of the addon would be, for example, <http://<YOUR_HA_IP_ADDRESS>:8123/d5369777_music_assistant>

## Usage and Notes

- When running the Home Assistant add-on, you can access the webinterface from the add-on (a shortcut can be added to the sidebar) which is more secure than via the port (although the port can be exposed (but not changed) in the settings)
- If you are running Music Assistant in docker, you need to access the webinterface at http://youripaddress:8095. The port can be changed in the MA settings. If something else is using port 8095 then that must be shutdown until the MA port is changed.
- No providers are installed initially. You must navigate to the MA settings and add the providers (Music and Players) that you use
- Music from your music sources will be automatically loaded into the Music Assistant library. If you have multiple sources, they will be merged as one library
- In this first implementation the UI centres around the concept of the [Library](usage.md), so your artists, albums, tracks, playlists and radio stations. You can BROWSE the various providers to add aditional items to your Library. In a later release options will be provided to browse the recommendations of the various streaming providers
- Note that at the first startup it can take a while before data is available (first sync), the Music Assistant UI will notify you about tasks that are in progress. This can be seen by this symbol ![icon](assets/icons/sync-icon.png) next to the Music Provider entry in MA settings
- Music sources are synced at addon (re)start and every 3 hours (or other interval as selected in the settings)
- MA is designed to work on a Raspberry Pi which is also running Home Assistant. For this reason it does not make large demands on resources. Additionally, there are limits on the free API calls used for artwork and other metadata. The result of this is that initial syncs of large libraries can take a long time. Subsequent syncs should be noticeably faster.
- If a song is available on multiple providers (e.g. Spotify and a flac file on disk), the file/stream with the highest quality is always preferred when starting a stream. Highest quality is based on sample rate, bit depth and codec and local is preferred over cloud
- Music Assistant uses a custom stream port (TCP 8097 by default) to stream audio to players. Players must be able to reach the Home Assistant instance and this port. If you're running one of the recommended HAOS installation methods, this is all handled for you, otherwise you will have to make sure you're running MA in a container in privileged mode and HOST network mode. Note: If the default port 8097 is occupied, the next port will be tried, and so on

[repository-badge]: https://img.shields.io/badge/Add%20repository%20to%20my-Home%20Assistant-41BDF5?logo=home-assistant&style=for-the-badge
[repository-url]: https://my.home-assistant.io/redirect/supervisor_add_addon_repository/?repository_url=https%3A%2F%2Fgithub.com%2Fmusic-assistant%2Fhome-assistant-addon
