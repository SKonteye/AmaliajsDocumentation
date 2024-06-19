# AmaliajsDocumentation

## Overview

**Amalia.js** is an open-source library developed and maintained by the French National Audiovisual Institute (INA). It is a metadata-enriched HTML5 video player and visualization tool written in JavaScript. This guide will help C# developers integrate Amalia.js into an ASP.NET Core project.

## Key Features

- Metadata enriched HTML5 video player
- Metadata visualization tool
- Supports various plugins including:
  - Control bar
  - Timeline plugin
  - Caption plugin
  - Text synchronization plugin
  - Overlay plugin
  - Editor plugin (in progress)

## Installation

### Step 1: Add Amalia.js to Your Project

1. **Download the Amalia.js library**:
   - You can download the Amalia.js library from the [official repository](https://github.com/INA-Amalia/Amalia.js).

2. **Include the Amalia.js library in your project**:
   - Place the downloaded Amalia.js files in the `wwwroot` folder of your ASP.NET Core project.

3. **Add the Amalia.js script and styles in your HTML file**:
   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
       <meta charset="UTF-8">
       <meta name="viewport" content="width=device-width, initial-scale=1.0">
       <title>Amalia.js in ASP.NET Core</title>
       <link rel="stylesheet" href="~/wwwroot/css/amalia.css">
   </head>
   <body>
       <script src="~/wwwroot/js/amalia.js"></script>
   </body>
   </html>
   ```

### Step 2: Configure Amalia.js in Your Application

1. **Initialize Amalia.js**:
   - You can initialize Amalia.js in your JavaScript code as follows:
   ```javascript
   document.addEventListener("DOMContentLoaded", function() {
       var player = new AmaliaPlayer({
           data: "~/wwwroot/data/metadata.json",
           element: document.getElementById('videoPlayer')
       });
   });
   ```

2. **Create a div element to host the player**:
   ```html
   <div id="videoPlayer"></div>
   ```

## Metadata Format

### Generic Metadata Model

The generic metadata model for Amalia.js represents all metadata of a media stream, including audio, video, technical information, documentary notes, and results of automatic metadata extraction algorithms. The metadata is managed using a JSON format.

### Example Metadata Block

Here is an example of a simple metadata block:
```json
{
    "id": "amalia-simple01",
    "type": "simple",
    "algorithm": "demo-json-generator",
    "processor": "Ina Research Department - N. HERVE",
    "processed": 1418900533632,
    "version": 1,
    "localisation": [
        {
            "type": "simple",
            "tcin": "00:00:00.0000",
            "tcout": "00:01:00.0000",
            "tclevel": 0,
            "sublocalisations": {
                "localisation": [
                    {
                        "label": "A demo label !",
                        "tc": "00:00:30.0000",
                        "tclevel": 1
                    }
                ]
            }
        }
    ]
}
```

### Metadata Fields

- **id**: Unique identifier for the metadata block
- **type**: Type of the metadata
- **algorithm**: Name of the algorithm that generated the metadata
- **processor**: Information about the processor
- **processed**: Timestamp of when the metadata was processed
- **version**: Version of the metadata
- **localisation**: Temporal and spatial information about the metadata

### Temporal Metadata

- **tcin**: Start timecode (hh:mm:ss.SSSS)
- **tcout**: End timecode (hh:mm:ss.SSSS)
- **tc**: Specific timecode (hh:mm:ss.SSSS)
- **tclevel**: Hierarchical level of the metadata

### Spatial Metadata

- **type**: Type of spatial metadata (e.g., rectangle, ellipse)
- **coordinates**: Normalized coordinates (x, y, w, h) between 0 and 1
- **orientation**: Orientation value between -π and π

## Using Metadata in Amalia.js

### Example: Displaying Captions

1. **Create a metadata JSON file**:
   ```json
   {
       "id": "caption01",
       "type": "caption",
       "localisation": [
           {
               "tcin": "00:00:10.0000",
               "tcout": "00:00:15.0000",
               "label": "Hello, this is a caption example."
           }
       ]
   }
   ```

2. **Load the metadata in Amalia.js**:
   ```javascript
   document.addEventListener("DOMContentLoaded", function() {
       var player = new AmaliaPlayer({
           data: "~/wwwroot/data/caption_metadata.json",
           element: document.getElementById('videoPlayer'),
           plugins: {
               caption: {
                   active: true
               }
           }
       });
   });
   ```

## Binding Metadata to Plugins

Amalia.js allows static and dynamic binding of metadata blocks to visualization plugins. Here is a table of automatic bindings:

| Type                | Description                                       | Key                                                              |
|---------------------|---------------------------------------------------|------------------------------------------------------------------|
| DEFAULT             | Default binding                                   | fr.ina.amalia.player.PluginBindingManager.dataTypes.DEFAULT      |
| DETECTION           | Mapped with timeline plugin in mode cue point     | fr.ina.amalia.player.PluginBindingManager.dataTypes.DETECTION    |
| VISUAL_DETECTION    | Mapped with timeline plugin and overlay plugin    | fr.ina.amalia.player.PluginBindingManager.dataTypes.VISUAL_DETECTION |
| TRANSCRIPTION       | Mapped with Text synchronization and caption plugin | fr.ina.amalia.player.PluginBindingManager.dataTypes.TRANSCRIPTION |
| KEYFRAMES           | Mapped with timeline plugin in mode image         | fr.ina.amalia.player.PluginBindingManager.dataTypes.KEYFRAMES    |

### Example: Dynamic Binding

```javascript
var player = new AmaliaPlayer({
    data: "~/wwwroot/data/metadata.json",
    element: document.getElementById('videoPlayer'),
    plugins: {
        timeline: {
            active: true
        },
        caption: {
            active: true
        }
    }
});
```

## Conclusion

By following the steps and examples provided in this documentation, C# developers can effectively integrate and use Amalia.js in their ASP.NET Core projects. For more detailed information, refer to the official [Amalia.js documentation](https://github.com/INA-Amalia/Amalia.js) and explore additional examples and use cases.
