# dragjs
## What is this?
In essence, this is **a library that allows for easy yet customisable dragging of objects on a `<canvas>`**
## Boring stuff
By Ojas Gulati (Github Ojas-Gulati) - released under the MIT license

::snore::
## Quickstart
  1. Put the script in the head tag
```html
<head>
  <script src="drag.js"></script>
</head>
```
  2. Create a canvas
```html
<body>
  <canvas id="canvas"></canvas>
</body>
``` 
  3. Make a new `dragCanvas` and add a draggable object
```javascript
var canvas = document.getElementById("canvas");
var ctx = canvas.getContext("2d");
var dragArea = new dragCanvas(canvas)
dragArea.add("rectangle", {
    "startX": 0,
    "startY": 10,
    "shape": function (x, y) {
        ctx.rect(x, y, 10, 10)
    },
    "boundingBox": {
        "width": 10,
        "height": 10,
        "center": false
    },
    "eventListeners": {
        "drag": {
            "shape": function (x, y) {
                ctx.rect(x - 2, y - 2, 14, 14)
            }
        } //
    }
})
```
## Functions + options
### Functions:
```javascript
new dragCanvas(canvas)         // Creates a new dragCanvas on the canvas specified.
dragArea.add(id, [options])    // Adds a draggable object
dragArea.disable([id])         // Disables clicking and dragging on the whole canvas or the name specified
dragArea.enable([id])          // Enables clicking and dragging on the whole canvas or the name specified
dragArea.disableLayer([id])    // Disables clicking and dragging on a certain layer if using layeredCanvas
dragArea.enableLayer([id])     // Enables clicking and dragging on a certain layer if using layeredCanvas
dragArea.remove(id)            // Removes an object
dragArea.redraw()              // Redraws the canvas i.e. will draw all objects attached to the dragArea
dragArea.objects               // Returns a list of object names
dragArea.getObjectData([id])   // Returns the internal data of a certain object or otherwise all objects
```

### Options:
* `x`: (**required**) The starting X coordinate of your object. If accessed later, it will contain the new coordinates.
* `y`: (**required**) The starting Y coordinate of your object. If accessed later, it will contain the new coordinates.
* `shape`: (**required**) When your shape is dragged, it will have new x and y coordinates. These will be passed into this function; draw the shape in the correct position. If you wish to modify any of the objects properties, return a JSON object containing only the values that you want to change e.g. x or y or shaped
* `isClicked` or `boundingBox`: (**required**) These both let the package know if the object is being pressed
    * `isClicked`: (higher priority than `boundingBox`) a function (mouseX, mouseY, currentX, currentY) that returns true if the mouse is over the shape. The shape's current position is given by currentX and currentY
    * `boundingBox`: This draws a rectangle around the area and assumes the element clicked on if the mouse location is inside the box. It is a JSON object with:
        * `width`: (**required**) The width of the bounding box. Can be a number or a function that returns a number. Width is calculated from the object's X coordinate.
        * `height`: (**required**) well what do you think it is
        * `center`: boolean - should be true if the box should be centered around the x and y coordinates of the object. Higher precedence than `centerV` and `centerH` Defaults to true.
        * `centerV`: boolean - should be true if the box should be centered vertically. Defaults to true.
        * `centerH`: boolean - should be true if the box should be centered horizontally. Defaults to true.
        * `offsetX`: the offset of the bounding box in the x direction
        * `offsetY`: the offset of the bounding box in the y direction
* `drawPriority`: Objects with higher draw priorities are drawn later i.e. "on top". If not present, defaults to `clickPriority`. If that is not present, defaults to 0.
* `clickPriority`: When objects overlap, objects with higher click priority are considered to be "on top" for dragging.  If not present, defaults to `drawPriority`. If that is not present, defaults to 0.
* `eventListeners`: A set of key-value pairs, each containing a modification JSON i.e. the changed key-value pairs from the original - see the example. The modification JSON can also contain an element `call`, which is a function to be called when the event is fired. The event listeners are (in precedence order):
    * `drag` - fired while the object is picked up, dragged or put down
    * `click` - fired when an object is clicked on
    * `hover` - fired when the mouse is hovering over the object
    * `pick` - fired when the element is picked up
    * `drop` - fired when the element is dropped
