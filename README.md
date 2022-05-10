[![Validate with hassfest](https://github.com/RogerSelwyn/Home_Assistant_SkyQ_MediaPlayer/actions/workflows/hassfest.yaml/badge.svg)](https://github.com/RogerSelwyn/Home_Assistant_SkyQ_MediaPlayer/actions/workflows/hassfest.yaml) [![HACS Validate](https://github.com/RogerSelwyn/Home_Assistant_SkyQ_MediaPlayer/actions/workflows/hacs.yaml/badge.svg)](https://github.com/RogerSelwyn/Home_Assistant_SkyQ_MediaPlayer/actions/workflows/hacs.yaml) [![CodeFactor](https://www.codefactor.io/repository/github/rogerselwyn/home_assistant_skyq_mediaplayer/badge)](https://www.codefactor.io/repository/github/rogerselwyn/home_assistant_skyq_mediaplayer) [![Downloads for latest release](https://img.shields.io/github/downloads/RogerSelwyn/Home_Assistant_SkyQ_MediaPlayer/latest/total.svg)](https://github.com/RogerSelwyn/Home_Assistant_SkyQ_MediaPlayer/releases/latest)

![GitHub release](https://img.shields.io/github/v/release/RogerSelwyn/Home_Assistant_SkyQ_MediaPlayer) [![maintained](https://img.shields.io/maintenance/yes/2022.svg)](#) [![maintainer](https://img.shields.io/badge/maintainer-%20%40RogerSelwyn-blue.svg)](https://github.com/RogerSelwyn) [![hacs_badge](https://img.shields.io/badge/HACS-Default-41BDF5.svg)](https://github.com/hacs/integration) [![Community Forum](https://img.shields.io/badge/community-forum-brightgreen.svg)](https://community.home-assistant.io/t/custom-component-skyq-media-player/140306)


# Sky Q component for Home Assistant

The skyq platform allows you to control a Sky Q set top box.

**Note:** This integration does not support **Sky Glass** or internet only devices that are currently being sold outside the UK.

**Note:** Whilst it will pull back information for current channel and live programme, it will do this for a very limited set of countries (currently UK, and any countries that use the same EPG/images, plus Italy and Germany). If you are in an unsupported country, or don't want this information set 'live_tv' to False in your config.

If you are able to supply details on where to reliably retrieve EPG information and images from, please raise a Feature Request with the relevant details and I'll look to include.

There is currently support for the following device types within Home Assistant:

- [Media Player](#mediaplayer)
- [Sensor](#storagesensor) (UI config only)

### [Buy Me A ~~Coffee~~ Beer 🍻](https://buymeacoffee.com/rogtp)
I work on this integration because I like things to work well for myself and others, and for it to deliver as much as is achievable with the API. Please don't feel you are obligated to donate, but of course it is appreciated.

## <a name="mediaplayer">Media Player</a>

### Screenshots

_Component showing current TV with default media control_

<img src="https://github.com/RogerSelwyn/Home_Assistant_SkyQ_MediaPlayer/blob/master/screenshots/skyq_1.png">

_Component showing application with default media control_

<img src="https://github.com/RogerSelwyn/Home_Assistant_SkyQ_MediaPlayer/blob/master/screenshots/skyq_2.png">

_Component showing recording with [Mini Media Player](https://github.com/kalkih/mini-media-player)_

<img src="https://github.com/RogerSelwyn/Home_Assistant_SkyQ_MediaPlayer/blob/master/screenshots/skyq_3.png">

_Media Browser_

<img src="https://github.com/RogerSelwyn/Home_Assistant_SkyQ_MediaPlayer/blob/master/screenshots/skyq_4.png">

### Installation

You can either use HACS or install the component manually:

- Put the files from `/custom_components/skyq/` in your folder `<config directory>/custom_components/skyq/`

If you wish to add to the provided application images (such as Netflix) place '.png' images with names the same as the application in the following folder:

- `<config directory>/custom_components/skyq/static/`

For channels where there is no EPG, this can also be utilised to provide a channel image. Place an image file in the same directory with the same name as your channel source name (e.g. raiuno.png).

### Media Player Configuration

There are two methods of configuration, via the Home Assistant Integrations UI dialogue or via YAML. You cannot use both for the same Sky Q box, please use one or the other. Previous YAML configurations are not migrated to the UI method, please continue to use YAML, or delete YAML, reboot and add via UI.

#### Integrations UI (from v2.2.0)

On the Integrations page, click to add a new Integration and search for Sky Q. Sky Q will only be visible if you have previously installed via one of the methods above and have restarted Home Assistant.

You will be asked to enter a host (which must be contactable on your network and name). The name defaults to Sky Q. Assuming the Sky Q box is switched on and the details are correct a device and entity will be created and be useable. You can then configure other items by clicking on Configure on the Sky Q Integration card. Details are below.

If you want to add a second Sky Q box, just follow the same process again and add a new instance of the integration.

#### YAML

Add a skyq platform entry in your configuration.yaml as below.

**Example of basic configuration.yaml**

```
media_player:
 - platform:  skyq
   name: SkyQ Living Room
   host: 192.168.0.10
   live_tv: True
   sources:
      SkyOne: '1,0,6'
      SkyNews: '5,0,1'
```

#### Configuration variables

| **YAML**                                        | **UI**                            | **Default** | **Details** |
| ------------------------------------------------|-----------------------------------|:-----------:|-------------|
| platform<br>_(string)(Required)_                | n/a                               |             |Must be set to skyq |
| host<br>_(string)(Required)_                    | Host                              |             | The IP of the SkyQ set top box, e.g., 192.168.0.10. |
| name<br>_(string)(Required)_                    | Name                              |             | The name you would like to give to the SkyQ set top box. |
| sources<br>_(list)(Optional)_                   | Custom Sources                    |  _Empty_    | List of channels or other commands that will appear in the source selection. |
| output_programme_image<br>_(boolean)(Optional)_ |Show programme image            | True        | Allows you to disable returning images when watching recorded programmes. Useful if using a modified media player UI, where you don't want the background changing. |
| live_tv<br>_(boolean)(Optional)_                | Show live TV details           | True        | Allows you to disable the retrieval of live TV programme information. Useful for people in those countries where the TV schedules are not available from current known sources. |
| get_live_record<br>_(boolean)(Optional)_                | Get Live Record status           | False       | Allows you to fetch the status of the currently playing item to see if it is a "Live Recording". |
| tv_device_class                             | TV device class   | True    | Sets device class to TV. Unticked sets it to Receiver. |
| country<br>_(string)(Optional)_                 | Override Country | _Empty_     | Overrides the detected country from the SkyQ box. Currently supports "GBR", "ITA" and "DEU". In theory you shouldn't need to use this. |
| volume_entity<br>_(string)(Optional)_        | Entity to control volume of | _Empty_     | Specifies the entity for which volume control actions will be passed through to. No validation of the entity is done via the UI, warnings will show in the log if an invalid entity is used. Must be a media_player entity. e.g. media_player.braviatv|
| epg_cache_len<br>_(integer)(Optional)_          | EPG Cache Length               | 20           |Allows you to configure the number of EPG channels to cache for the media browser. Larger numbers will cause slower initial load per day and consume more memory. |

#### Sources

To configure sources, set as:

##### Via UI

```
{"<YourChanneName>": "<button>,<button>,<button>", "<YourChanneName2>": "<button>,<button>,<button>"}.
```
Example
```
{"BBC1HD": "1,1,5", "BBC2": "1,0,2"}
```

#### Via YAML

```
<YourChanneName> : ‘<button>,<button>,<button>’.
```
Example
```
  BBC1HD: "1,1,5"
  BBC2: "1,0,2"
```

#### Supported buttons

- sky, power, tvguide or home, boxoffice, search, sidebar, up, down, left, right, select, channelup, channeldown, i, dismiss, text, help,
- play, pause, rewind, fastforward, stop, record
- red, green, yellow, blue
- 0, 1, 2, 3, 4, 5, 6, 7, 8, 9

#### Next/Previous Buttons

The behaviour of the next and previous buttons is fastforward and rewind (multiple presses to increase speed, play to resume)

#### Sending Button Commands

If you are using [Mini Media Player](https://github.com/kalkih/mini-media-player) or some other player that supports sending 'play_media' commands, you can configure this in the front-end rather than having to configure a source and then assigning it to a button. For example, the below will send the 'channelup' button command to the Sky box:

```
shortcuts:
  buttons:
    - icon: 'mdi:chevron-up'
      id: channelup
      type: skyq
```
You can also send a sequence of commands as a service call such as the below ('backup' is included here because you may need to exit the Sky EPG, it is not required):

```
service: media_player.play_media
target:
  entity_id: media_player.sky_q
data:
  media_content_type: skyq
  media_content_id: 1,0,5
```

### Media Browser

Fetching data to support live programme information for the Media Browser is data intensive, so the information for upto 20 channels are cached to improve performance. The first media browser access per day will be slow (whilst the data is fetched), further accesses will utilise cached data. If the programme line up for a channel changes during the day, this will not be reflected in the media browser.

Images for the current live programme on each channel should show along with the title of each programme. These are identified via the channel number, so if you are using a 'custom' source with a Sky Q favourite it won't be able show the live programme. Applications can be included, but for an image to show the image file name in your SkyQ install directory needs to match the name of the custom source.

In custom sources, if you have 'backup' as your first command, then this will be ignored when identifying the channel number to lookup the live programme for. This will not impact the set of commands sent to the Sky Box when you select a channel.

### Switch Generation Helper

A utility function has been created to generate yaml configuration for SkyQ enabled media players to support easy usage with other home assistant integrations, e.g. google home

Usage based on Google home: _“turn on <source name / channel name> in the ”_

#### Configuration

| **YAML**                                        | **UI**                            | **Default** | **Details** |
| ------------------------------------------------|-----------------------------------|:-----------:|-------------|
| generate_switches_for_channels<br>_(boolean)(Optional)_ | Generate switches for channels | False | Generate switches for each item listed in source. The files will be generated in <config folder>/skyq<room>.yaml |
|room<br>_(string)(Optional)_                    | Room name                         | Default Room | The room where the SkyQ set top box is located. |

Avoid using [ ] in the name: or room: of your device. This field is required if you have more than one SkyQ box being configured with switches

**Example configuration.yaml with switch generation**

```
media_player:
 - platform:  skyq
   name: SkyQ Living Room
   host: 192.168.0.10
   sources:
      SkyOne: '1,0,6'
      SkyNews: '5,0,1'
   room: Living Room
   generate_switches_for_channels: true
```

To integrate these generated switch configuration files, add the generated yaml to your configuration.yaml. The following example configuration implements the generated switches from the generate_switches_for_channels function.

```
switch:
- platform: template
  switches: !include  skyq<room>.yaml
```

### Aliases

Because the name of the channel may not always be what you want to use to talk to Google Home, it is possible to place an alias file in the root of your Home Assistant configuration. This needs to be called `skyqswitchalias.yaml`. It is also possible to use this to rename some of the default switches, so you can change `play` to `engage` for example. The contents should be pairs of switchname and alias as below:

```
BBC One South: BBC South
ComedyCentHD: Comedy Central HD
SkySpMainEvHD: Sky Sports Main Event HD
SkySp PL HD: Sky Sports Premier League HD
SkyPremiereHD: Sky Premiere HD
SkyFamilyHD: Sky Family HD
play: engage
```

### Media Player Entity Attributes
The media player provides a main status, plus a range of attributes. The main attributes are common with other HA media players and provide detail on the currently playing item:
- source_list: BBC One South, BBC Two HD, ITV
- media_content_type: tvshow
- media_title: BBC Two HD
- media_series_title: Planet Earth II: A World of Wonder
- media_season: 2
- media_episode: 7
- media_channel: BBC Two HD
- source: BBC Two HD
- entity_picture_local: /api/media_player_proxy/media_player.sky_q_mini?token=6418e8bac0e0065a5276f3407e80d3f10157dbd6d7be93fc3d96582cbd6be80c&cache=450f5a402a3ce01c
- device_class: tv
- entity_picture: https://imageservice.sky.com/pd-image/1c54548b-4afa-4d8a-aab4-19e7201c26bb/16-9/456?territory=GB&provider=SKY&proposition=SKYQ
- icon: mdi:satellite-variant
- friendly_name: Sky Q Mini
- supported_features: 154547

The following items are specific to the SkyQ media player and are items requested by users of the integration:
- skyq_media_type - shows current media type being played
  - app - an application such as Netflix is being played
  - live - a live programme is being played (unknown if it has been paused previously or not)
  - liverecord - a live programme is being played as well as being recorded
  - pvr - a recording is being played
- skyq_transport_status
  - OK - standard status when all is OK, even in standby.
  - ERROR_PIN_REQUIRED - when the box is awaiting PIN entry. This can be used as a means of triggering automatic PIN entry.
  - Not present - if box is in lower power mode (overnight) or for an unsupported box (e.g. Sky Glass)
- skyq_channelno - the Sky Q channel being played. Not present if no channel has been identified (e.g. YouTube is being viewed)

 ### Homekit Integration

The integration will expose the relevant controls to Homekit as long as it is configured in the Homekit integration. The Sky Q media player should be exposed as an accesory. Normally you will want 'tv_device_class' to be enabled to provide the maximum functionality to Homekit, since this presents the box as a TV which can be managed by Apple Remote. If 'tv_device_class' is disabled, then the Sky Q box is presented as a Receiver to Homekit which then presents a set of switches in the Home app. Note that in the list of available channels that is presented in the Home app, the current channel is always available even if not configured as a source in HA.

## <a name="storagesensor">Storage Sensor</a>

One sensor is created per SkyQ device with it's value set to the current used space in GB. Max GB and percentage used are all provided as attributes of the sensor.
