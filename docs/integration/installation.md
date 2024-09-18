## Installation of the Home Assistant Integration

!!! note
    Ensure the MA server is up and running properly with music and player providers added before trying to link it to HA via the integration

- Make sure that you have the [Home Assistant Community Store](https://hacs.xyz/) installed.
- Within HACS, search for `Music Assistant` and click the entry in the search results.
- Click the big (blue) button at the bottom for `Download`.
- Click the button again and in the dialog make sure `Show beta versions` is selected if you want to use those versions.
- Download the desired version
- Restart Home Assistant.
- The MA server will normally be discovered automatically by HA and you can just click on CONFIGURE.
- If for some reason you need to add the integration manually then go to HA Settings>>Devices & services>>Integrations and click the big `+ ADD INTEGRATION` button. Look for Music Assistant and click to add it. You will need to add the server IP and port (usually 8095). Look for the relevant line in the server logs. For example, `Starting server on 172.30.32.1:8095`. 
- Click SUBMIT and the Music Assistant integration is ready for use.

!!! note 
    The HA integration will create new media_player entities for those player types which are supported natively by MA. To see the names of those players go to `HA settings>>Devices & services>>Integrations>>Music Assistant` and view the entities. It is these players that need to be targeted in automations.

## Installation of the Home Assistant Player Provider

Once the HA Integration is installed it is possible to stream to HA media player entities. In order to do that the HA Player Provider needs to be installed.  However, first the Home Assistant Plug-in provider needs to be installed.

- Navigate to MA SETTINGS>>PROVIDERS and add the plug-in provider
- If using the Music Assistant add-on (i.e. HAOS), you wont need any server details, it should auto connect to the local HA instance
- If using the docker version of the MA server, you will be required to enter the URL to your HA instance and then authenticate

Next install the Home Assistant Player Provider

- You need the HA plug-in first before you can use/install this provider
- In the provider settings, you can select which players you want to utilise
- You can only use players that support "play_media", other players will be filtered out
- MA players will also be filtered out

!!! note
    Features are most likely limited with these players. Always prefer a native player provider if it exists in MA as that is optimised

## Service Calls

The integration adds four actions for use in scripts and automations. 

- [mass.play_media](../faq/massplaymedia.md)
- [mass.play_annnouncement](../faq/massannounce.md)
- [mass.search](../faq/masssearch.md)
- [mass.transfer_queue](../faq/masstransfer.md)
