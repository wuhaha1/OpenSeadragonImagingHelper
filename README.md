##OpenSeadragonImagingHelper

[See the Demo/Test Site Live Here](http://msalsbery.github.io/openseadragonimaginghelper/index.html)

OpenSeadragonImagingHelper is a plugin for [OpenSeadragon](https://github.com/openseadragon/openseadragon) 
which implements some properties and methods helpful in
imaging applications.

###Usage

[Download openseadragon-imaginghelper.js Here](http://msalsbery.github.io/openseadragonimaginghelper/scripts/openseadragon-imaginghelper.js)

To use the plugin, add **openseadragon-imaginghelper.js** after **openseadragon.js** to your site.
This adds the **ImagingHelper** class to the OpenSeadragon namespace.

An **ImagingHelper** object can be created and attached to an [OpenSeadragon.Viewer](http://openseadragon.github.io/docs/symbols/OpenSeadragon.Viewer.html) two ways:


1. Call the activateImagingHelper method on the viewer
2. Create a new ImagingHelper object, passing a viewer reference in the options parameter

Both methods return a new ImagingHelper object, and both methods also add the ImagingHelper
object reference to the viewer as a property called 'imagingHelper'.

```javascript
    // Example 1 - Use the Viewer.activateImagingHelper() method to create an ImagingHelper

    // create an OpenSeadragon viewer
    var viewer = OpenSeadragon({...});
    // add an ImagingHelper to the viewer
    var imagingHelper = viewer.activateImagingHelper({...});


    // Example 2 - Attach a new ImagingHelper to an existing OpenSeadragon.Viewer

    var imagingHelper = new OpenSeadragon.ImagingHelper({viewer: existingviewer});
```

###Details

The ImagingHelper class provides a simplified zoomFactor which is simply the ratio
of the displayed image pixel size to the image's native pixel size.

The ImagingHelper methods use three coordinate systems,
named as follows:


1. **physical:** Device pixel coordinates relative to the SeaDragon viewer
2. **logical:**  0.0 to 1.0 relative to the image's native dimensions
3. **data:**     Pixel coordinates relative to the image's native dimensions

Methods are provided to zoom and/or pan using these conventions, as well as to convert
individual horizontal/vertical values or point ({x,y}) objects between coordinate systems 
**(Note: methods that return a point object return new [OpenSeadragon.Point](http://openseadragon.github.io/docs/symbols/OpenSeadragon.Point.html)
objects)**

The ImagingHelper class extends the [OpenSeadragon.EventSource](http://openseadragon.github.io/docs/symbols/OpenSeadragon.EventHandler.html) class and raises
an event named **'image-view-changed'** whenever the viewer's zoom and/or pan position changes.

```javascript
    // Event Example 1 - Use the options 'viewChangedHandler' property to set a handler

    var viewer = OpenSeadragon({...});
    var imagingHelper = viewer.activateImagingHelper({viewChangedHandler: onImageViewChanged});

    function onImageViewChanged(event) {
        // event.viewportWidth == width of viewer viewport in logical coordinates relative to image native size
        // event.viewportHeight == height of viewer viewport in logical coordinates relative to image native size
        // event.viewportCenter == OpenSeadragon.Point, center of the viewer viewport in logical coordinates relative to image
        ...
    }


    // Event Example 2 - Add a handler to an existing ImagingHelper

    imagingHelper.addHandler('image-view-changed', function (event) {
        // event.viewportWidth == width of viewer viewport in logical coordinates relative to image native size
        // event.viewportHeight == height of viewer viewport in logical coordinates relative to image native size
        // event.viewportCenter == OpenSeadragon.Point, center of the viewer viewport in logical coordinates relative to image
        ...
    });
```

###Demo/Test Site

index.html and the folders 'data', 'images', and 'scripts' provide a demo/test site.
The page displays many OpenSeadragon and OpenSeadragonImagingHelper metrics, as well as the output of many OpenSeadragonImagingHelper methods,
all in real-time as the cursor moves and/or the image is zoomed/panned..

Additionally, there's an example of syncing an SVG overlay for annotation support.

All the sample code is in [scripts/viewmodel.js](http://msalsbery.github.io/openseadragonimaginghelper/scripts/viewmodel.js).  

###Notes

This initial code can certainly be optimized and trimmed, but was written before OpenSeadragon in C++ (then C#) for
the Silverlight MultiScaleImage control as well as several proprietary viewer controls on different platforms. 
I needed a standard way to store and/or pass coordinates between all these, and the MultiScaleImage class' simple viewportwidth and
viewportorigin properties in logical coordinates proved to be a good fit. This code was easily ported across languages and/or platforms
and/or viewers with only two properties/methods needing to be changed: setBitmapImageViewportWidth and setBitmapImageViewportOrigin
(for those familiar with DeepZoom on Silverlight, the source of inspiration should be obvious :)).

The [OpenSeadragon](https://github.com/openseadragon/openseadragon) viewer is much more robust than its Silverlight ancestor, and
I am happy to now be contributing to its source code.  So this is my first port of code I've been using all over the 
place for a long time in my medical imaging applications, made specifically for OpenSeadragon.

###In the works...


1. minZoom/maxZoom - fix to match OpenSeadragon viewer coordinate system
2. Better documentation

