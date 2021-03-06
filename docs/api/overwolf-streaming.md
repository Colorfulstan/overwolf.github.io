---
id: overwolf-streaming
title: overwolf.streaming API
sidebar_label: overwolf.streaming
---

This namespace contains all the functionality that allows the streaming of video/audio.

The term streaming might be a bit misleading – we regard saving a video to the local drive as streaming, as well as streaming the video to a streaming service such as Twitch.tv.

**Permissions required: Streaming**

## Methods Reference

* [overwolf.streaming.start()](#startsettings-callback)
* [overwolf.streaming.stop()](#stopstreamid-callback)
* [overwolf.streaming.changeVolume()](#changevolumestreamid-audiooptions-callback)
* [overwolf.streaming.setWatermarkSettings()](#setwatermarksettingssettings-callback)
* [overwolf.streaming.getWatermarkSettings()](#getwatermarksettingscallback)
* [overwolf.streaming.getWindowStreamingMode()](#getwindowstreamingmodewindowid-callback)
* [overwolf.streaming.setWindowStreamingMode()](#setwindowstreamingmodewindowid-streamingmode-callback)
* [overwolf.streaming.setWindowObsStreamingMode()](#setwindowobsstreamingmodewindowid-obsstreamingmode-callback)
* [overwolf.streaming.setBRBImage()](#setbrbimagestreamid-image-backgroundcolor-callback)
* [overwolf.streaming.getStreamEncoders()](#getstreamencoderscallback)
* [overwolf.streaming.getAudioDevices()](#getaudiodevicescallback)
* [overwolf.streaming.updateStreamingDesktopOptions()](#updatestreamingdesktopoptionsstreamid-newoptions-mousecursorstreamingmethod-callback)
* [overwolf.streaming.updateTobiiSetting()](#updatetobiisettingstreamid-param-callback)
* [overwolf.streaming.getRunningRecorders()](#getrunningrecorderscallback)

## Events Reference

* [overwolf.streaming.onStreamingSourceImageChanged](#onstreamingsourceimagechanged)
* [overwolf.streaming.onStopStreaming](#onstopstreaming)
* [overwolf.streaming.onStartStreaming](#onstartstreaming)
* [overwolf.streaming.onStreamingError](#onstreamingerror)
* [overwolf.streaming.onStreamingWarning](#onstreamingwarning)
* [overwolf.streaming.onVideoFileSplited](#onvideofilesplited)
* [overwolf.streaming.onRecordingEngineStateChanged](#onrecordingenginestatechanged)

## Types Reference

* [overwolf.streaming.StreamSettings](#streamsettings-object) Object
* [overwolf.streaming.enums.StreamingProvider](#overwolfstreamingenumsstreamingprovider-enum)
* [overwolf.streaming.StreamParams](#streamparams-object) Object
* [overwolf.streaming.StreamInfo](#streaminfo-object) Object
* [overwolf.streaming.StreamAuthParams](#streamauthparams-object) Object
* [overwolf.streaming.StreamVideoOptions](#streamvideooptions-object) Object
* [overwolf.streaming.StreamingVideoEncoderSettings](#streamingvideoencodersettings-object) Object
* [overwolf.streaming.enums.StreamEncoder](#overwolfstreamingenumsstreamencoder-enum)
* [overwolf.streaming.StreamingVideoEncoderNVIDIA_NVENCSettings](#streamingvideoencodernvidia_nvencsettings-object) Object
* [overwolf.streaming.enums.StreamEncoderPreset_NVIDIA](#overwolfstreamingenumsstreamencoderpreset_nvidia-enum)
* [overwolf.streaming.enums.StreamEncoderRateControl_NVIDIA](#overwolfstreamingenumsstreamencoderratecontrol-nvidia-enum)
* [overwolf.streaming.StreamingVideoEncoderIntelSettings](#streamingvideoencoderintelsettings-object) Object
* [overwolf.streaming.StreamingVideoEncoderx264Setting](#streamingvideoencoderx264settings-object) Object
* [overwolf.streaming.enums.StreamEncoderPreset_x264](#overwolfstreamingenumsstreamencoderpreset_x264-enum)
* [overwolf.streaming.enums.StreamEncoderRateControl_x264](#overwolfstreamingenumsstreamencoderratecontrol_x264-enum)
* [overwolf.streaming.StreamingVideoEncoderAMD_AMFSettings](#streamingvideoencoderamd_amfsettings-object) Object
* [overwolf.streaming.enums.StreamEncoderPreset_AMD_AMF](#overwolfstreamingenumsstreamencoderpreset_amd_amf-enum)
* [overwolf.streaming.enums.StreamEncoderRateControl_AMD_AMF](#overwolfstreamingenumsstreamencoderratecontrol_amd_amf-enum)
* [overwolf.streaming.StreamDesktopCaptureOptions](#streamdesktopcaptureoptions-object) Object
* [overwolf.streaming.StreamAudioOptions](#streamaudiooptions-object)
* [overwolf.streaming.StreamDeviceVolume](#streamdevicevolume-object) Object
* [overwolf.streaming.StreamPeripheralsCaptureOptions](#streamperipheralscaptureoptions-object) Object
* [overwolf.streaming.StreamPeripheralsCaptureOptions](#streamperipheralscaptureoptions-object) Object
* [overwolf.streaming.enums.StreamMouseCursor](#overwolfstreamingenumsstreammousecursor-enum)
* [overwolf.streaming.StreamIngestServer](#streamingestserver-object) Object
* [overwolf.streaming.ReplayType](#replaytype-enum)
* [overwolf.streaming.WatermarkSettings](#watermarksettings-object) Object
* [overwolf.streaming.WatermarkSettings](#watermarksettings-object) Object
* [overwolf.streaming.enums.StreamingMode](#overwolfstreamingenumsstreamingmode-enum)
* [overwolf.streaming.enums.ObsStreamingMode](#overwolfstreamingenumsobsstreamingmode-enum)

## The basic usage flow should be:

### 1. Register to the onStopStreaming

Register to the [onStopStreaming](#onstopstreaming) event to know when the streaming stopped:

```js
overwolf.streaming.onStopStreaming.addListener(function(result) {
  //result.stream_id
  //result.url (relevant to video only)
});
```

### 2. Register to the onStreamingError

Register to the [onStreamingError](#onstreamingerror) event, to know if a streaming error occurred which will also stop the streaming session:

```js
overwolf.streaming.onStreamingError.addListener(function(result) {
  //result.stream_id
  /*
    result.error = (one of):
      "Unknown"
      "Unauthorized"
      "Invalid_Server"
      "No_Internet"
      "Invalid_Video_Settings"
      "No_Playback_Device"
      "Not_InGame"
      "Internet_Congested"
      "Twitch_Dll_Load_Error"
      "Twitch_Init_Error"
      "MEDIA_PLAYER_DLL_ERROR"
      "No_Encoder"
      "Out_Of_Disk_Space"

      "CAPTURE_DEVICE_ERROR"
      "CAPTURE_DEVICE_NO_FRAMES"
      "GAME_UNSUPPORTED_FORMAT_ERROR"
      "KRAKEN_DEVICE"
      "PLAYBACK_DEVICE_FORMAT"
      "INPUT_DEVICE_FORMAT"
      "NVIDIA_UPDATE_DRIVER"
      "NVIDIA_ENCODER"
      "CPU_OVERLOADED"
      "FILE_OUTPUT_ERROR"
      "SWITCHABLE_GRAPHICS_SHT_UNSUPPORTED"
      "NO_CAPTURE_DEVICE"  
  */
});
```

NOTE: we only started with the UPPERCASE convention later on.

### 3. Register to the onStreamingWarning

 Register to the [onStreamingWarning](#onstreamingwarning) event to know when a warning occurred:

```js
overwolf.streaming.onStreamingWarning.addListener(function(result) {
  //result.stream_id
  //result.error = "HIGH_CPU_USAGE" // there will probably be frames lost
});
```

### 4. Register to onStreamingSourceImageChanged

 Optionally register to [onStreamingSourceImageChanged](#onstreamingsourceimagechanged), to get an event when the user switched between capturing his desktop and the game.

### 5. Call overwolf.streaming.getStreamEncoders

Call [getStreamEncoders()](#getstreamencoderscallback) and [getAudioDevices()](#getaudiodevicescallback).
This will give a list of all possible encoders and audio devices – you can then use this list to let the user select his preferred encoder/device.
In terms of encoder priorities – we recommend: NVIDIA > AMD > INTEL > x264.
As long as the "enabled" field = true, you can offer the user to use the encoder.

### 6. Call overwolf.streaming.start

Create a JSON object with all of the streaming details (See example on the top of the page) and call [start()](#startsettings-callback).

* For video recording you don’t need the `ingest_server`, `stream_info` and `auth` objects.
* For video recording we recommend using a `max_kbps` value of higher than 8000.
* For streaming we recommend using a `max_kbps` smaller than 3000.
* Once start succeeded, you’ll get a callback with `result.status == "success"` and a `stream_id` that can be used to stop the streaming session or change the volume of the stream.

### 7. Allow the user to change volume

Allow the user to change volume with [changeVolume()](#changevolumestreamid-audiooptions-callback) while streaming.

### 8. Call overwolf.streaming.stop

Call [stop()](#stopstreamid-callback) to stop the streaming session.

### Extras

* Use [setBRBImage()](#setbrbimagestreamid-image-backgroundcolor-callback) when streaming to Twitch.tv when `capture_desktop` is not enabled, this will allow you to customize the Be-Right-Back image the viewers will see.
* Use [setWindowStreamingMode](#setwindowstreamingmodewindowid-streamingmode-callback) for video recording and streaming – this method works on a per-overwolf-window basis – for each window you can decide if it is to be shown in the stream or not – regardless of whether the streamer is viewing it or not.
* Use [setWindowObsStreamingMode](#setwindowobsstreamingmodewindowid-obsstreamingmode-callback) when you aren’t streaming with Overwolf but want to leverage Overwolf’s APIs and stream with OBS.

## start(settings, callback)

#### Version added: 0.78

> Start a new stream.

Parameter | Type                                            | Description             |
--------- | ------------------------------------------------| ----------------------- |
settings  | [StreamSettings](#streamsettings-object) object | The stream settings     |
callback  | function                                        | Returns with the result |

#### Callback argument: Success

A callback function which will be called with the status of the request

```json
{ "status": "success", "stream_id": 1 }
```

#### Callback argument: Failure

A callback function which will be called with the status of the request

```json
{ "status": "error", "error": "something went wrong..." }
```

#### Usage Example

```javascript
overwolf.streaming.start(
    {
      "provider": overwolf.streaming.enums.StreamingProvider.Twitch,
      "settings": {
        "stream_info": {
          "url": "http://www.twitch.tv/{twitch_user_name}",
          "title": "title"
        },
        "auth": {
          "client_id": "{twitch_client_id}",
          "token": storage.get('login-token')
        },
        "audio": {
          "mic": {
            "volume": 100,
            "enabled": true
          },
          "game": {
            "volume": 75,
            "enabled": true
          }
        },
        "video": {
          "auto_calc_kbps": false,
          "fps": 30,
          "width": 1920,
          "height": 1080,
          "max_kbps": 1500,
          "notify_dropped_frames_ratio": 0.5,
          "test_drop_frames_interval": 5000,
          "encoder": {
            "name": overwolf.streaming.enums.StreamEncoder.X264,
            "config": {
              "preset": overwolf.streaming.enums.StreamEncoderPreset_x264.ULTRAFAST,
              "rate_control": overwolf.streaming.enums.StreamEncoderRateControl_x264.RC_CBR,
              "keyframe_interval": 2
            }
          }
        },
        "ingest_server": {
          "name": "US West: San Francisco, CA",
          "template_url": "rtmp://live.twitch.tv/app/{stream_key}"
        },
        "peripherals": {
          "capture_mouse_cursor": "both"
        }
      }
    },
    function(result) {
        if (result.status == "success")
        {
            console.debug(result.stream_id);
        }
    }
);
```

## stop(streamId, callback)

#### Version added: 0.78

> Stops the given stream.

Parameter | Type                                            | Description             |
--------- | ------------------------------------------------| ----------------------- |
streamId  |int | The id of the stream to stop     |
callback  | function                                        | Returns with the result |

#### Callback argument: Success

A callback function which will be called with the status of the request, and the  stream id if successful

```json
{ "status": "success", "stream_id": 1 }
```

#### Callback argument: Failure

A callback function which will be called with the status of the request

```json
{ "status": "error", "error": "something went wrong..." }
```

## changeVolume(streamId, audioOptions, callback)

#### Version added: 0.78

> Changes the volume of the stream.

Parameter     | Type                                                    | Description                                           |
------------- | --------------------------------------------------------| ----------------------------------------------------- |
streamId      | int                                                     | The id of the stream on which the volume is changed   |
audioOptions  | [StreamAudioOptions](#streamaudiooptions-object) Object | The new volumes encapsulated in an object             |
audioOptions  | function                                                | Returns with the result                               |

#### Callback argument: Success

A callback function which will be called with the status of the request

```json
{ "status": "success" }
```

#### Callback argument: Failure

A callback function which will be called with the status of the request

```json
{ "status": "error" }
```

## setWatermarkSettings(settings, callback)

#### Version added: 0.78

> Sets the watermark settings.

Parameter | Type                                                    | Description                                           |
----------| --------------------------------------------------------| ----------------------------------------------------- |
settings  | [WatermarkSettings](#watermarksettings-object) Object   | The new watermark settings                            |
callback  | function                                                | Returns with the result                               |

#### Callback argument: Success

A callback function which will be called with the status of the request when done setting the new watermark settings.

```json
{ "status": "success" }
```

#### Callback argument: Failure

A callback function which will be called with the status of the request

```json
{ "status": "error" }
```

#### Usage Example

```javascript
overwolf.streaming.setWatermarkSettings(
    { showWatermark : true },
    function (result) {
        console.log ("watermark settings changed.");
    }
);
```

## getWatermarkSettings(callback)

#### Version added: 0.78

> Gets the watermark settings.

Parameter | Type                                                    | Description                                           |
----------| --------------------------------------------------------| ----------------------------------------------------- |
settings  | [WatermarkSettings](#watermarksettings-object) Object   | The new watermark settings                            |
callback  | function                                                | Returns with the result                               |

#### Callback argument: Success

A function that will be called with a JSON containing the status and the watermark settings if successful

```json
{ "status": "success" , showWatermark: true }
```

#### Callback argument: Failure

A callback function which will be called with the status of the request

```json
{ "status": "error" }
```

## getWindowStreamingMode(windowId, callback)

#### Version added: 0.78

> Call the given callback function with the window’s streaming mode as a parameter.

Parameter | Type     | Description                                              |
----------| ---------| -------------------------------------------------------- |
windowId  | string   | The id of the window for which to get the streaming mode |
callback  | function | Returns with the result                                  |

#### Callback argument: Success

A callback function which will be called with the status of the request and the window’s streaming mode as a parameter

```json
{ "status": "success", streaming_mode: "WhenVisible" }
```

#### Callback argument: Failure

A callback function which will be called with the status of the request

```json
{ "status": "error" }
```

## setWindowStreamingMode(windowId, streamingMode, callback)

#### Version added: 0.78

> Call the given callback function with the window’s streaming mode as a parameter.

Parameter     | Type                                                                                     | Description                                              |
--------------| -----------------------------------------------------------------------------------------| ---------------------------------------------------------|
windowId      | string                                                                                   | The id of the window for which to set the streaming mode |
streamingMode | [overwolf.streaming.enums.StreamingMode](#overwolfstreamingenumsstreamingmode-enum) enum | The desired streaming mode                               |
callback      | function                                                                                 | Returns with the result  (Optional)                      |

#### Callback argument: Success

A callback function which will be called with the status of the request, after streaming mode was set

```json
{ "status": "success" }
```

#### Callback argument: Failure

A callback function which will be called with the status of the request

```json
{ "status": "error" }
```

## setWindowObsStreamingMode(windowId, obsStreamingMode, callback)

#### Version added: 0.78

> Sets the streaming mode for the window when using OBS.

Parameter     | Type                                                                                            | Description                                              |
--------------| ------------------------------------------------------------------------------------------------| ---------------------------------------------------------|
windowId      | string                                                                                          | The id of the window for which to set the streaming mode |
streamingMode | [ overwolf.streaming.enums.ObsStreamingMode](#overwolfstreamingenumsobsstreamingmode-enum) enum | The desired OBS streaming mode                           |
callback      | function                                                                                        | Returns with the result  (Optional)                      |

#### Callback argument: Success

A callback function which will be called with the status of the request, after streaming mode was set

```json
{ "status": "success" }
```

#### Callback argument: Failure

A callback function which will be called with the status of the request

```json
{ "status": "error" }
```

## setBRBImage(streamId, image, backgroundColor, callback)

#### Version added: 0.78

> Set a stream’s Be Right Back image.

Parameter       | Type      | Description                                                              |
----------------| ----------| -------------------------------------------------------------------------|
streamId        | int       | The id of the stream for which to set the Be Right Back image            |
image           | Object    | The image to set, as an IMG object or a URL                              |
backgroundColor | string    | The color to paint the last game frame with before overlaying the image  |
callback        | function  | Returns with the result  (Optional)                                      |

#### Callback argument: Success

A callback function which will be called with the status of the request

```json
{ "status": "success" }
```

#### Callback argument: Failure

A callback function which will be called with the status of the request

```json
{ "status": "error" }
```

## getStreamEncoders(callback)

#### Version added: 0.83

> Returns an array of supported streaming encoders, with extra metadata for each one.

Parameter       | Type      | Description                                                               |
----------------| ----------| --------------------------------------------------------------------------|
callback        | function  | A callback function to call with the array of encoders and their metadata |

#### Callback argument: Success

A callback function which will be called with the status of the request

```json
{
    "status": "success",
    "encoders": [
        {
            "name" : "INTEL",
            "display_name" : "Intel® Quick Sync (uses iGPU)",
            "enabled" : true,
            "presets" : ["LOW", "MEDIUM", "HIGH"]
        },
        ...
    ]
}
```

#### Callback argument: Failure

A callback function which will be called with the status of the request

```json
{ "status": "error" }
```

## getAudioDevices(callback)

#### Version added: 0.78

> Returns an array of all audio devices that can be used.

Parameter       | Type      | Description                                                                    |
----------------| ----------| -------------------------------------------------------------------------------|
callback        | function  | A callback function to call with the array of audio devices and their metadata |

#### Callback argument: Success

A callback function which will be called with the status of the request

```json
{
    "status": "success",
    "devices": [
        {
            "display_name" : "Speakers (USB Ear-Microphone)",
            "display_id" : "{0.0.0.00000000}.{ec2a6c4b-f750-4045-bb93-d745ecc76937}",
            "device_state" : "Active",
            "can_record" : false,
            "can_playback" : true
        },
        ...
    ],
    "default_recording_device_id": "{0.0.0.00000000}.{ec2a6c4b-f750-4045-bb93-d745ecc76937}",
    "default_playback_device_id": "{0.0.1.00000000}.{39da502b-2b96-4b76-83ae-9841f0b46570}"
}
```

#### Callback argument: Failure

A callback function which will be called with the status of the request

```json
{ "status": "error" }
```

## updateStreamingDesktopOptions(streamId, newOptions, mouseCursorStreamingMethod, callback)

#### Version added: 0.78

> Update stream desktop capture options.

Parameter                  | Type                                                                                              | Description                                                     |
---------------------------| --------------------------------------------------------------------------------------------------| ----------------------------------------------------------------|
streamId	               | int                                                                                               | The id of the stream                                            |
newOptions	               | [StreamDesktopCaptureOptions](#streamdesktopcaptureoptions-object) Object                         | The updated desktop capture streaming options                   |
mouseCursorStreamingMethod | [overwolf.streaming.enums.StreamMouseCursor](#overwolfstreamingenumsstreammousecursor-enum) enum  | The updated value of the mouse cursor streaming method          |
callback                   | function                                                                                          | A callback function to call with success or failure indicationa |

#### Callback argument: Success

A callback function which will be called with the status of the request

```json
{ "status": "success" }
```

#### Callback argument: Failure

A callback function which will be called with the status of the request

```json
{ "status": "error" }
```

## updateTobiiSetting(streamId, param, callback)

#### Version added: 0.97

> Update Tobii streaming layer.

Parameter | Type             | Description                                                     |
----------| -----------------| ----------------------------------------------------------------|
streamId  | int              | The id of the stream                                            |
param     | TobiiLayerParams | The Tobii layer visibility param                                |
callback  | function         | A callback function to call with success or failure indicationa |

#### Callback argument: Success

A callback function which will be called with the status of the request

```json
{ "status": "success" }
```

#### Callback argument: Failure

A callback function which will be called with the status of the request

```json
{ "status": "error" }
```

## getRunningRecorders(callback)

#### Version added: 0.127

> Returns a list of all running recording services (in a case that the app is using our capturing features).

Parameter | Type             | Description                                                                      |
----------| -----------------| ---------------------------------------------------------------------------------|
callback  | function         | A callback function to call with the array of recorders and their extensions ids |

#### Callback argument: Success

A callback function which will be called with the status of the request

```json
//example value:
{
  "0": {
    "extensions": [
      "nafihghfcpikebhfhdhljejkcifgbdahdhngepfb",
      "hffhbjnafafjnehejohpkfhjdenpifhihebpkhni"
    ]
  }
}
```

#### Callback argument: Failure

A callback function which will be called with the status of the request

```json
{ "status": "error" }
```

## onStreamingSourceImageChanged

#### Version added: 0.78

> Fired when the stream started streaming a new image source (desktop, game).

## onStopStreaming

#### Version added: 0.78

> Fired when the stream has stopped.

## onStartStreaming

#### Version added: 0.106

> Fired when the stream has started.

## onStreamingError

#### Version added: 0.78

> Fired upon an error with the stream.

#### Possible Error Codes

* Unknown
* Unauthorized
* Invalid_Server
* No_Internet
* Invalid_Video_Settings
* No_Playback_Device
* Not_InGame
* Internet_Congested
* Game_Quit_Mid_Stream
* Twitch_Dll_Load_Error
* Twitch_Init_Error
* No_Encoder
* Out_Of_Disk_Space
* Update_Driver

#### Event Data Example

```json
{
    "status": "error",
    "stream_id": 1,
    "error": "Internet_Congested"
}
```

## onStreamingWarning

#### Version added: 0.78

> Fired upon a warning with the stream.

## onVideoFileSplited

#### Version added: 0.103

> Fired upon video file splited.

## onRecordingEngineStateChanged

#### Version added: 0.127

> Fired when a replay stated\\stopped by any app.

## StreamSettings Object

#### Version added: 0.78

Stream settings container.

| Name      | Type                                                                | Description                              | Since |
|-----------| --------------------------------------------------------------------|------------------------------------------|------ |
| provider  | [overwolf.streaming.enums.StreamingProvider](#overwolfstreamingenumsstreamingprovider-enum) enum | The stream provider name                 | 0.78  |
| settings  | [StreamParams](#streamparams-object) object                                | The stream provider settings             | 0.78  |

#### Object data example

```json
"settings": {
	"video": { "buffer_length": 20000 },
	"audio": {
	    "mic": {
	        "volume": 100,
		"enabled": true
	    },
	    "game": {
	        "volume": 75,
		"enabled": true
	    }
         },
	    "peripherals": { "capture_mouse_cursor": "both" }
	 }
}
```

## overwolf.streaming.enums.StreamingProvider enum

#### Version added: 0.78

| Options       | Description                                     |
|---------------| ------------------------------------------------|
| Unknown       | The stream provider name                        |
| Twitch        | Stream to Twitch                                |
| VideoRecorder |                                                 |
| RTMP          | Stream to YouTube, Facebook, smashcast.tv, etc. |

## StreamParams Object

#### Version added: 0.78

Represents the settings required to start a stream.

| Name      | Type                                                                | Description                              | Since |
|-----------| --------------------------------------------------------------------|------------------------------------------|------ |
| stream_info  | [StreamInfo](#streaminfo-object) object | The basic stream information          | 0.78  |
| auth  | [StreamAuthParams](#streamauthparams-object) object     | Stream authorization data             | 0.78  |
| video  | [StreamVideoOptions](#streamvideooptions-object) Object       | Stream video options             | 0.78  |
| audio  | [StreamAudioOptions](#streamaudiooptions-object) Object  | Stream audio options             | 0.78  |
| peripherals  | [StreamPeripheralsCaptureOptions](#streamperipheralscaptureoptions-object) Object | Defines how peripherals (i.e. mouse cursor) are streamed. </br>**Permissions required: DesktopStreaming** | 0.78  |
| ingest_server  | [StreamIngestServer](##streamingestserver-object) Object   | Information on the server that is being streamed to       | 0.78  |
| replay_type  | [ReplayType](#replaytype-enum) enum           | The replay type to use       | 0.78  |
| gif_as_video  | bool               | Create gif as video (Gif Replay Type only)     | 0.78  |
| max_quota_gb  | double               | Max media folder size in GB     | 0.78  |

## StreamInfo Object

#### Version added: 0.78

The basic stream information.

| Name  | Type   | Description                             | Since |
|-------| -------|-----------------------------------------|------ |
| url   | string | The URL where the stream can be watched | 0.78  |
| title | string | The stream title                        | 0.78  |

## StreamAuthParams Object

#### Version added: 0.78

Stream authorization data.

| Name      | Type   | Description                                                                                             | Since |
|-----------| -------|---------------------------------------------------------------------------------------------------------|------ |
| client_id | string | The client id part of the authorization data. This part is usually constant for each application.       | 0.78  |
| token     | string | The token part of the authorization data. This part if usually user-specific, and received after login. | 0.78  |

## StreamVideoOptions Object

#### Version added: 0.78

Stream video options.

| Name      | Type   | Description                                                                                             | Since |
|-----------| -------|---------------------------------------------------------------------------------------------------------|------ |
| auto_detect | bool | Defines if to try to automatically detect the best settings for streaming       | 0.83  |
| auto_calc_kbps | bool | Defines if to try to automatically calculate the kbps. If set to true, then the max_kbps field is ignored | 0.83  |
| fps   | int | Defines the Frames Per Second for the stream | 0.78  |
| width | int | Defines the stream width in pixels  | 0.78  |
| height| int | Defines the stream height in pixels | 0.78  |
| max_kbps| int | Defines the maximum KB per second of the stream | 0.78  |
| buffer_length| int | Defines the length of the buffer to be recorded in millisenconds (max 40 seconds) | 0.83  |
| encoder|  [StreamingVideoEncoderSettings](#streamingvideoencodersettings-object) Object |Defines the video encoder settings to use | 0.83  |
| capture_desktop|  [StreamDesktopCaptureOptions](#streamdesktopcaptureoptions-object) Object |Defines the desktop streaming options.</br>Permissions required: DesktopStreaming | 0.83  |
| frame_interval| int | Interval between frames when creating gifs | 0.83  |
| test_drop_frames_interval | uint | The interval, in milliseconds, in which to test for dropped frames | 0.92  |
| notify_dropped_frames_ratio| double | The ratio of dropped to non-dropped frames for which to issue a notification | 0.92  |
| max_file_size_bytes| uint | Defines file maximum size. when video reach {max_file_size_bytes}, the recorder will flush the video file and stat a new video file. onFileSpilt event will be fired | 0.103  |
| sub_folder_name| string | Defines Sub folder for video file path destination (See note below this table) | 0.103  |
| override_overwolf_setting| bool | Do not use Overwolf capture setting. In case True you must provider all video setting (encoder..) | 0.103  |
| tobii| int | [TobiiLayerParams](overwolf-tobii) | 0.97  |

##### `sub_folder_name` note: 

* Defines Sub folder for video file path destination (Optional):  
`OverwolfVideoFolder\\AppName\\|sub_folder_name\\|file_name|`
* In case [folder_name] is empty:  
`OverwolfVideoFolder\\AppName\\|sub_folder_name|`

## StreamingVideoEncoderSettings Object

#### Version added: 0.78

Stream video options.

| Name   | Type                                                                                     | Description                                        | Since |
|--------| -----------------------------------------------------------------------------------------|----------------------------------------------------|------ |
| name   | [overwolf.streaming.enums.StreamEncoder](#overwolfstreamingenumsstreamencoder-enum) enum |  Defines which video encoder to use. Default: X264 | 0.83  |
| config | one of the [settings objects below](#see-also)                                                        |  Defines the settings of the specific encoder      | 0.83  |

#### See also:

* [StreamingVideoEncoderNVIDIA_NVENCSettings](#streamingvideoencodernvidia_nvencsettings-object)
* [StreamingVideoEncoderIntelSettings](#streamingvideoencoderintelsettings-object)
* [StreamingVideoEncoderx264Settings](#streamingvideoencoderx264settings-object)
* [StreamingVideoEncoderAMD_AMFSettings](#streamingvideoencodernvidia_nvencsettings-object)

## overwolf.streaming.enums.StreamEncoder enum

#### Version added: 0.78

| Options      | Description                                                            |
|--------------| -----------------------------------------------------------------------|
| INTEL        | Use the Intel Quick Sync encoder. Only available on supporting devices |
| X264         | Use the x264 encoder. This is the default encoder                      |
| NVIDIA_NVENC |    Uses the nVidia encoder                                             |
| AMD_AMF      | Uses the AMD AMF encoder                                               |

## StreamingVideoEncoderNVIDIA_NVENCSettings Object

#### Version added: 0.83

Defines the configuration for the NVIDIA NVENC encoder.

| Name              | Type                                                       | Description                                                   | Since |
|-------------------| -----------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------| ----- |
| preset            |  [overwolf.streaming.enums.StreamEncoderPreset_NVIDIA](#overwolfstreamingenumsstreamencoderpreset_nvidia-enum) enum  |  Defines which preset the encoder should use                    | 0.83  |
| rate_control      |  [overwolf.streaming.enums.StreamEncoderRateControl_NVIDIA](#overwolfstreamingenumsstreamencoderratecontrol_nvidia-enum) enum  |  Defines the rate control mode the encoder should use | 0.83  |
| keyframe_interval |  int                                                                                                |  Defines the time, in seconds, after which to send a keyframe | 0.83  |

## overwolf.streaming.enums.StreamEncoderPreset_NVIDIA enum

#### Version added: 0.83

| Options                      | Description    |
|------------------------------| ---------------|
| AUTOMATIC                    |                |
| DEFAULT                      |                |
| HIGH_QUALITY                 |                |
| HIGH_PERFORMANCE             |                |
| BLURAY_DISK                  |                |
| LOW_LATENCY                  |                |
| HIGH_PERFORMANCE_LOW_LATENCY |                |
| HIGH_QUALITY_LOW_LATENCY     |                |
| LOSSLESS                     |                |
| HIGH_PERFORMANCE_LOSSLESS    |                |

## overwolf.streaming.enums.StreamEncoderRateControl_NVIDIA enum

#### Version added: 0.83

| Options            | Description    |
|--------------------| ---------------|
| RC_CBR             |                |
| RC_CQP             |                |
| RC_VBR             |                |
| RC_VBR_MINQP       |                |
| RC_2_PASS_QUALITY  |                |

## StreamingVideoEncoderIntelSettings Object

#### Version added: 0.83

TBA

## StreamingVideoEncoderx264Settings Object

#### Version added: 0.83

Defines the configuration for an x264 encoder.

| Name              | Type                                                       | Description                                                   | Since |
|-------------------| -----------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------| ----- |
| preset            |  [overwolf.streaming.enums.StreamEncoderPreset_x264](#overwolfstreamingenumsstreamencoderratecontrol_x264-enum-enum) enum  |  Defines which preset the encoder should use                    | 0.83  |
| rate_control      |  [overwolf.streaming.enums.StreamEncoderRateControl_x264](#overwolfstreamingenumsstreamencoderratecontrol_x264-enum) enum  |  Defines the rate control mode the encoder should use | 0.83  |
| keyframe_interval |  int                                                                                                |  Defines the time, in seconds, after which to send a keyframe | 0.83  |

## overwolf.streaming.enums.StreamEncoderPreset_x264 enum

#### Version added: 0.83

| Options    | Description    |
|------------| ---------------|
| ULTRAFAST  |                |
| SUPERFAST  |                |
| VERYFAST   |                |
| FASTER     |                |
| FAST       |                |
| MEDIUM     |                |
| SLOW       |                |
| SLOWER     |                |
| VERYSLOW   |                |
| PLACEBO    |                |

## overwolf.streaming.enums.StreamEncoderRateControl_x264 enum

#### Version added: 0.83

| Options            | Description    |
|--------------------| ---------------|
| RC_CBR             |                |
| RC_CQP             |                |
| RC_VBR             |                |
| RC_VBR_MINQP       |                |
| RC_2_PASS_QUALITY  |                |

## StreamingVideoEncoderAMD_AMFSettings Object

#### Version added: 0.84

Defines the configuration for an x264 encoder.

| Name              | Type                                                       | Description                                                   | Since |
|-------------------| -----------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------| ----- |
| preset            |  [overwolf.streaming.enums.StreamEncoderPreset_AMD_AMF](#overwolfstreamingenumsstreamencoderpreset_amd_amf-enum) enum  |  Defines which preset the encoder should use                    | 0.84  |
| rate_control      |  [overwolf.streaming.enums.StreamEncoderRateControl_AMD_AMF](#overwolfstreamingenumsstreamencoderratecontrol_amd_amf-enum) enum  |  Defines the rate control mode the encoder should use | 0.84  |
| keyframe_interval |  int                                                                                                |  Defines the time, in seconds, after which to send a keyframe | 0.83  |

## overwolf.streaming.enums.StreamEncoderPreset_AMD_AMF enum

#### Version added: 0.84

| Options    | Description    |
|------------| ---------------|
| AUTOMATIC  |                |
| BALANCED  |                |
| SPEED   |                |
| QUALITY     |                |
| ULTRA_LOW_LATENCY       |                |
| LOW_LATENCY     |                |

## overwolf.streaming.enums.StreamEncoderRateControl_AMD_AMF enum

#### Version added: 0.84

| Options            | Description    |
|--------------------| ---------------|
| RC_CBR             |                |
| RC_CQP             |                |
| RC_VBR             |                |
| RC_VBR_MINQP       |                |

## StreamDesktopCaptureOptions Object

#### Version added: 0.78

Defines the configuration for an x264 encoder.

| Name          | Type  | Description                                                                    | Since |
|---------------| ------|--------------------------------------------------------------------------------| ----- |
| enable        | bool  |  Defines if to capture the desktop while game is not running or not in focus   | 0.78  |
| monitor_id    | uint  |  Defines which monitor to stream when streaming desktop                        | 0.78  |
| force_capture | bool  | Defines if to force desktop streaming even when a game is in foreground        | 0.78  |

## StreamAudioOptions Object

#### Version added: 0.83

Defines the configuration for an x264 encoder.

| Name        | Type                                              | Description                                                                    | Since |
|-------------| --------------------------------------------------|--------------------------------------------------------------------------------| ----- |
| mic         | [StreamDeviceVolume](#streamdevicevolume-object)  |  Defines the microphone volume as applied to the stream                        | 0.83  |
| mic_volume  | int                                               |  Defines the microphone volume as applied to the stream in a range of 0 to 100 | 0.83  |
| game        | [StreamDeviceVolume](#streamdevicevolume-object)  | Defines the game volume as applied to the stream                               | 0.83  |
| game_volume | int                                               | Defines the game volume as applied to the stream in a range of 0 to 100        | 0.83  |

## StreamDeviceVolume Object

#### Version added: 0.78

Defines a device volume and enablement settings.

| Name      | Type   | Description                                          | Since |
|-----------| ------ |------------------------------------------------------| ----- |
| enable    | bool   |  Defines if the device is enabled                    | 0.78  |
| volume    | int    |  Defines the device volume in the range of 0 to 100  | 0.78  |
| device_id | string | Defines the device ID to use                         | 0.78  |

## StreamPeripheralsCaptureOptions Object

#### Version added: 0.83

Stream capture options for peripheral devices.

| Name                 | Type                                                                                               | Description                                                     | Since |
|----------------------| -------------------------------------------------------------------------------------------------- |-----------------------------------------------------------------| ----- |
| capture_mouse_cursor | [overwolf.streaming.enums.StreamMouseCursor](#overwolfstreamingenumsstreammousecursor-enum) enum   |  Defines when to capture the mouse cursor while streaming is on | 0.83  |

## overwolf.streaming.enums.StreamMouseCursor enum

#### Version added: 0.83

| Options     | Description                                         |
|-------------| ----------------------------------------------------|
| both        | Always capture the mouse cursor                     |
| gameOnly    | Capture the mouse cursor only when a game is focued |
| desktopOnly | Capture the mouse cursor only when on the desktop   |

## StreamIngestServer Object

#### Version added: 0.78

Information on the server that is being streamed to.

| Name         | Type   | Description                                                                                | Since |
|--------------| ------ |--------------------------------------------------------------------------------------------| ----- |
| name         | string | The server name that is being streamed to                                                  | 0.78  |
| template_url | string | The server’s url template. Use the token {stream_key} to specify the stream key in the url | 0.78  |

## ReplayType enum

The replay type to use.

#### Version added: 0.83

| Options | Description     |
|---------| ----------------|
| video   |                 |
| gif     |                 |

## WatermarkSettings Object

#### Version added: 0.78

The basic stream information.

| Name           | Type   | Description                                                               | Since |
|----------------| -------|---------------------------------------------------------------------------|------ |
| showWatermark  | bool   | Determines whether or not to display the Overwolf watermark on the stream | 0.78  |

## overwolf.streaming.enums.StreamingMode enum

#### Version added: 0.78

| Options      | Description                                                                                                         |
|--------------| --------------------------------------------------------------------------------------------------------------------|
| WhenVisible  | Stream the window when it is visible. This is the default behavior. The viewers will see what you see               |
| Always       | Always stream the window, even when it is hidden or minimized. The viewers will continue to see it while you don’t  |
| Never        | Never stream the window. The viewers will not see the window even if you do see it.                                 |

## overwolf.streaming.enums.ObsStreamingMode enum

#### Version added: 0.78

| Options                     | Description                                                                                                                                             |
|-----------------------------| --------------------------------------------------------------------------------------------------------------------------------------------------------|
| OBSNoAwareness              | The default Overwolf window style                                                                                                                       |
| OBSAwareness                | Allows to capture the window using Overwolf OBS plugin and will not be visible ingame or by Overwolf capturing apps (will still be visible in desktop)  |
| OBSAwarenessHideFromDeskTop | Same as OBSAwareness but also not visible in desktop (hidden)                                                                                           |
