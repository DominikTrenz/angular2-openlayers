# Foreword

Most of the following documentation is an adaptation of OpenLayers' own documentation: http://openlayers.org/en/latest/apidoc.
While trying to cover most important aspects in here, this documentation is by no way exhaustive, please refer to http://openlayers.org/en/latest/apidoc if in doubt.
Contributions welcome!

# Map Component 
The `MapComponent`(`aol-map`) is the root component of Angular2 Openlayers maps.

Available parameters are:

    - `width` (`string|undefined`): width of the enclosing `<div>`
        Defaults to "100%"
    - `height` (`string|undefined`): height of the enclosing `<div>`
        Defaults to "100%"
    - `pixelRatio` (`number|undefined`): physical pixels to device-independent pixels (dips) ratio. 
        Defaults to window.devicePixelRatio.
    - `keyboardEventTarget` (`Element|string|undefined`): element to listen to keyboard events on. 
        Defaults to enclosing `<div>`
    - `loadTilesWhileAnimating` (`boolean|undefined`): tiles loading policy while animating.
        Defaults to `false`.
    - `loadTilesWhileInteracting` (`boolean|undefined`): tiles loading policy while interacting.
        Defaults to `false`.
    - `logo` (`string|boolean|undefined`): map logo. Provide `true` to display the Openlayers logo or a string URL.
        Defaults to `undefined`.
    - `renderer` (`'canvas'|'webgl'|undefined`): map renderer.
        Defaults to canvas.

Exposed events are:

    - `click` (ol.MapBrowserEvent) - A click with no dragging. A double click will fire two of this.
    - `dblclick` (ol.MapBrowserEvent) - A true double click, with no dragging.
    - `moveend` (ol.MapEvent) - Triggered after the map is moved.
    - `pointerdrag` (ol.MapBrowserEvent) experimental - Triggered when a pointer is dragged.
    - `pointermove` (ol.MapBrowserEvent) - Triggered when a pointer is moved. Note that on touch devices this is triggered when the map is panned, so is not the same as mousemove.
    - `postcompose` (ol.render.Event) experimental
    - `postrender` (ol.MapEvent) experimental - Triggered after a map frame is rendered.
    - `precompose` (ol.render.Event) experimental
    - `propertychange` (ol.Object.Event) - Triggered when a property is changed.
    - `singleclick` (ol.MapBrowserEvent) - A true single click with no dragging and no double click. Note that this event is delayed by 250 ms to ensure that it is not a double click.

## Important note
A map component without a view won't fetch tiles, _i.e.__ it stays **blank**, provide an `<aol-view>` component to display
the map. Here is a simple example, based on OpenStreetMap tiles source:
 ```
<aol-map [width]="'500px'" [height]="'300'">
    <aol-view [zoom]="2">
        <aol-coordinate [x]="5.795122" [y]="45.210225" [srid]="'EPSG:4326'"></aol-coordinate>
    </aol-view>
    <aol-layer-tile>
        <aol-source-osm></aol-source-osm>
    </aol-layer-tile>
</aol-map>
 ```
 
# View Component
The `ViewComponent` (`<aol-view>`) describes which content to display. In most cases, the view specifies the center of the map, _i.e._ coordinates on which the map is centered, and a zoom level or extent.

Available parameters are:
    
    - `constrainRotation` (boolean|number|undefined) Rotation constraint. false means no constraint. true means no constraint, but snap to zero near zero. A number constrains the rotation to that number of values. For example, 4 will constrain the rotation to 0, 90, 180, and 270 degrees.
        Defaults to true.
    - `enableRotation` (boolean|undefined) Enable rotation. If false a rotation constraint that always sets the rotation to zero is used. The constrainRotation option has no effect if enableRotation is false.
        Defaults to true. 
    - `extent` (ol.Extent|undefined) extent that constrains the center, in other words, center cannot be set outside this extent.
        Defaults to undefined.
    - `maxResolution` (number|undefined) The maximum resolution used to determine the resolution constraint. It is used together with minResolution (or maxZoom) and zoomFactor. If unspecified it is calculated in such a way that the projection's validity extent fits in a 256x256 px tile. If the projection is Spherical Mercator (the default) then maxResolution defaults to 40075016.68557849 / 256 = 156543.03392804097.
    - `minResolution` (number|undefined) The minimum resolution used to determine the resolution constraint. It is used together with maxResolution (or minZoom) and zoomFactor. If unspecified it is calculated assuming 29 zoom levels (with a factor of 2). If the projection is Spherical Mercator (the default) then minResolution defaults to 40075016.68557849 / 256 / Math.pow(2, 28) = 0.0005831682455839253.
    - `maxZoom`	(number|undefined) The maximum zoom level used to determine the resolution constraint. It is used together with minZoom (or maxResolution) and zoomFactor. Default is 28. Note that if minResolution is also provided, it is given precedence over maxZoom.
    - `minZoom`	(number|undefined) The minimum zoom level used to determine the resolution constraint. It is used together with maxZoom (or minResolution) and zoomFactor. Default is 0. Note that if maxResolution is also provided, it is given precedence over minZoom.
    - `resolution`	(number|undefined) The initial resolution for the view. The units are projection units per pixel (e.g. meters per pixel). An alternative to setting this is to set zoom. Default is undefined, and layer sources will not be fetched if neither this nor zoom are defined.
    - `resolutions`	(number[]|undefined) Resolutions to determine the resolution constraint. If set the maxResolution, minResolution, minZoom, maxZoom, and zoomFactor options are ignored.
    - `rotation` (number|undefined)	 The initial rotation for the view in radians (positive rotation clockwise). 
        Defaults to 0.
    - `zoom` (number|undefined)	Only used if resolution is not defined. Zoom level used to calculate the initial resolution for the view. The initial resolution is determined using the ol.View#constrainResolution method.
    - `zoomFactor`(number|undefined) The zoom factor used to determine the resolution constraint. 
        Defaults to 2.

## Usage example
```
<aol-view [zoom]="15">
    <aol-coordinate [x]="5" [y]="45" [srid]="'EPSG:4326'"></aol-coordinate>
</aol-view>
```
        
# Layer Components

`LayerComponents` (`<aol-layer-*>`) provide ways to displaying contents on the map.

Available parameters are: 

    - `opacity`	(`number|undefined`): layer's opacity, defaults to 1` (opaque).
    - `visible`	(`boolean|undefined`): layers visibility, defaults to `True`.
    - `extent`  (`ol.Extent`|undefined`): bounding extent for layer rendering. The layer will not be rendered outside of this extent.
    - `zIndex`	(`number|undefined`) experimental: layers are ordered, first by Z-index and then by position. The default Z-index is 0.
    - `minResolution` (`number|undefined`: 	minimum resolution (inclusive) at which the layer is visible.
    - `maxResolution` (`number|undefined`: 	maximum resolution (exclusive) at which the layer is visible.

# Feature component

The `FeatureComponent` (`aol-feature`) is a  vector object with a geometry and other attribute properties, similar to the features in vector file formats like GeoJSON.
Used in conjunction with a geometry (`<aol-geometry-*>`) and a style (`<aol-style>`), it allows displaying vector shapes on the map.

## Usage example
```
<aol-feature>
    <aol-geometry-point>
        <aol-coordinate [x]="5" [y]="45" [srid]="'EPSG:4326'"></aol-coordinate>
    </aol-geometry-point>
    <aol-style>
        <aol-style-circle [radius]="10">
            <aol-style-stroke [color]="'black'" [width]="2"></aol-style-stroke>
            <aol-style-fill [color]="'green'"></aol-style-fill>
        </aol-style-circle>
    </aol-style>
</aol-feature>
```


# Geometry components

The `GeometryComponents` (`aol-geometry-*`) allow defining geometrical shapes that, used in conjunction with a geometry (`<aol-geometry-*>`) and a style (`<aol-style>`),
display said geometrical shape to the map.

## Linestring component 

The `GeometryLinestringComponent` (`aol-geometry-linestring`) defines a collection of connected segments.

## Usage example
```
<aol-feature>
    <aol-geometry-linestring>
        <aol-collection-coordinates 
            [coordinates]="[[5.0, 45.01],[5.01, 45.03]]"
            [srid]="'EPSG:4326'">
        </aol-collection-coordinates>
    </aol-geometry-linestring>
    <aol-style>
        <aol-style-stroke [color]="'red'"></aol-style-stroke>
    </aol-style>
</aol-feature>
```


## Point component
The `GeometryPointComponent` (`aol-geometry-point`) defines a single point.

## Usage example
```
<aol-feature>
    <aol-geometry-point>
        <aol-coordinate [x]="5" [y]="45" [srid]="'EPSG:4326'"></aol-coordinate>
    </aol-geometry-point>
    <aol-style>
        <aol-style-circle [radius]="10">
            <aol-style-stroke [color]="'black'" [width]="width"></aol-style-stroke>
            <aol-style-fill [color]="'green'"></aol-style-fill>
        </aol-style-circle>
    </aol-style>
</aol-feature>
```

## Polygon component
The `GeometryPolygonComponent` (`aol-geometry-polygon`) defines a polygon.

## Usage example
```
<aol-feature>
    <aol-geometry-polygon>
        <aol-collection-coordinates 
            [coordinates]="[[5, 45],[5.05, 45.05],[5.05, 44.95],[4.95, 44.95]]"
            [srid]="'EPSG:4326'"
        >
        </aol-collection-coordinates>
    </aol-geometry-polygon>
    <aol-style>
        <aol-style-stroke [color]="'red'"></aol-style-stroke>
        <aol-style-fill [color]="[255,0,0,0.5]"></aol-style-fill>
    </aol-style>
</aol-feature>
```

# Style Components

`StyleComponents` (`<aol-style-*>`) provide ways to altering the look of vector features.

** WARNING ** : as of now, changes on style directives are not displayed. This issue lies in Openlayers: https://github.com/openlayers/ol3/issues/5775 (related).

`<aol-style-*>` must be encapsulated in a `<aol-style>` component. As of now, only `StyleCircleComponent` (`<aol-style-circle>`), `StyleFillComponent` (`<aol-style-fill>`),
`StyleIconComponent` (`<aol-style-icon`) and `StyleFillComponent` (`<aol-style-stroke>`) are implemented.

## `StyleCircleComponent` (`<aol-style-circle>`)

`StyleCircleComponent` (`<aol-style-circle>`) displays a circle.
Note that it can be further style using `<aol-style-fill>` and `<aol-style-stroke>`.
Available parameters are: 

    - `radius` (`number|undefined`): circle's radius, defaults to 10px.
    - `snapToPixel` (`boolean|undefined`): whether or not use sub-pixels, defaults to `True`.

## `StyleFillComponent` (`<aol-style-fill>`) 
Fill the host with a color, or gradient of colors.

Available parameters are: 
    - `color` (`Color|ColorLike|undefined`): color, gradient or pattern. See ol.color and ol.colorlike for possible formats. Defaults to black.

## `StyleIconComponent` (`<aol-style-icon`)
Displays an icon.

Available parameters are: 
    - `anchor` (`[number, number]`): image anchor. Defaults to [0.5, 0.5]: icon center.
    - `anchorXUnits` (`style.IconAnchorUnits`): X anchor unit: 'fraction' indicates a fraction of the icon, 'pixels' indicates a value in pixels. Defaults to 'fraction'.
    - `anchorYUnits` (`style.IconAnchorUnits`): Y anchor unit: 'fraction' indicates a fraction of the icon, 'pixels' indicates a value in pixels. Defaults to 'fraction'.
    - `anchorOrigin` (`style.IconOrigin`): origin of the anchor: bottom-left, bottom-right, top-left or top-right. Defaults to top-left.
    - `color` (`[number, number, number, number]`): tint of the icon. If not specified, the icon will be left as is.
    - `crossOrigin` (`style.IconOrigin`): crossOrigin attribute for loaded images.
    - `img` (`string`): image object for the icon.
    - `offset` (`[number, number]`): offset, which, together with the size and the offset origin, define the sub-rectangle to use from the original icon image. Defaults to [0, 0].
    - `offsetOrigin` (`style.IconOrigin`): origin of the offset: bottom-left, bottom-right, top-left or top-right. Defaults to top-left.
    - `opacity` (`number`): opacity of the icon. Default is 1.
    - `scale` (`number`): scale
    - `snapToPixel` (`boolean`): whether or not use sub-pixels, defaults to `True`.
    - `rotateWithView` (`boolean`): whether to rotate the icon with the view. Defaults to false.
    - `rotation` (`number`): rotation in radians (positive rotation clockwise). Defaults to 0.
    - `size` (`[number, number]`): icon size in pixel. Can be used together with offset to define the sub-rectangle to use from the origin (sprite) icon image.
    - `imgSize` (`[number, number]`): image size in pixels. Only required if img is set and src is not, and for SVG images in Internet Explorer 11. The provided imgSize needs to match the actual size of the image.
    - `src` (`string`): image source URI. Required.


## `StyleStrokeComponent` (`<aol-style-stroke>`) 
add a stroke around the host.

Available parameters are: 
    - `color`: (`Color|ColorLike|undefined`): color, gradient or pattern. See ol.color and ol.colorlike for possible formats. Defaults to black.
    - `lineCap`: (string|undefined): line cap style: butt, round, or square. Default to round.
    - `lineDash`: (number[]|undefined): line dash pattern. Defaults to undefined (no dash). No support inInternet Explorer 10 and lower.
    - `lineJoin`: (string|undefined): line join style: bevel, round, or miter. Defaults to round.
    - `miterLimit`: (number|undefined): miter limit. Defaults to 10.
    - `width`: (number|undefined): line width.