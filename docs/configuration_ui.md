---
title: UI Configuration
nav_order: 3
---

# UI Configuration

On the Integrations page, click to add a new Integration and search for Sky Q. Sky Q will only be visible if you have previously installed via one of the methods above and have restarted Home Assistant.

You will be asked to enter a host (which must be contactable from your HA server on your network using the host details you provide). The name defaults to Sky Q. Assuming the Sky Q box is switched on and the details are correct a device and entity will be created and be useable. You can then configure other items by clicking on Configure on the Sky Q Integration card. Details are below.

If you want to add a second Sky Q box, just follow the same process again and add a new instance of the integration.

## Configuration variables

| **Name**                            | **Default** | **Details** |
|-----------------------------------|:-----------:|-------------|
| Host                              |             | The IP of the SkyQ set top box, e.g., 192.168.0.10. |
| Name                              |             | The name you would like to give to the SkyQ set top box. |
| Show programme image            | True        | Allows you to disable returning images when watching recorded programmes. Useful if using a modified media player UI, where you don't want the background changing. |
| Show live TV details           | True        | Allows you to disable the retrieval of live TV programme information. Useful for people in those countries where the TV schedules are not available from current known sources. |
| Get Live Record status           | False       | Allows you to fetch the status of the currently playing item to see if it is a "Live Recording". |
| Entity to control volume of | _Empty_     | Specifies the entity for which volume control actions will be passed through to. No validation of the entity is done via the UI, warnings will show in the log if an invalid entity is used. Must be a media_player entity. e.g. media_player.braviatv|
| Advanced                         | False       | Show the advanced page of configuration items |
| TV device class   | True    | Sets device class to TV. Unticked sets it to Receiver. |
| Override Country | _Empty_     | Overrides the detected country from the SkyQ box. Currently supports "GBR", "ITA" and "DEU". In theory you shouldn't need to use this. |
| EPG Cache Length               | 20           |Allows you to configure the number of EPG channels to cache for the media browser. Larger numbers will cause slower initial load per day and consume more memory. |
| Custom Sources                    |  _Empty_    | List of channels or other commands that will appear in the source selection. |

## Sources

To configure sources, set as:

```
{"<YourChanneName>": "<button>,<button>,<button>", "<YourChanneName2>": "<button>,<button>,<button>"}.
```
Example
```
{"BBC1HD": "1,1,5", "BBC2": "1,0,2"}
```