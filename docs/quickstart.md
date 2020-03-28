# Efe.js Quickstart

## Overall Philosophy

Efe.js is a declaritive development paradigm for interactive HTML5 applications. It handles resources, input, state management, and other features.

Most features of Efe.js follow a declaritive pattern as shown below, where the developer declares what something **is** rather than what it **does**.

~~~~
resource("<resourceName>", "<type>", ["http://www.example.com/ex.png"]);
~~~~

Once something is declared, Efe.js will handle the boilerplate code necessary to make that feature work, and expose functions to work with those features. 

## Hello World

The anatomy of a barebones Efe.js application looks like so:

~~~~
resource("<resourceName>", "image", ["http://www.example.com/ex.png"]);
resource("<resourceName>", "audio", ["http://www.example.com/ex.mp3"]);
surface("<canvasElementId>", {dpiScaling: 'live'});

scene("_loader", {
	init: function() {
		downloadResources();
	},
	update: function() {
		if (downloadDone) setScene("intro");
	}
});

scene("intro", {
	init: function() {},
	update: function() {},
	draw: function() {
        surfaces.<canvasElementId>.ctx.fillText("Hello world", 0, 0);
    },
    cleanup: function() {}
});

// Init must be called manually to start the library
function init() {
	startEngine(pipeline, "<canvasElementId>");
	requestAnimationFrame(pipeline);
}
~~~~

In this example, we are declaring several resources and downloading them in the `_loader` scene. Then, we are transitioning to the `intro` scene, which will contain the first state of our application.

## Scenes

A **scene** is a container for a particular state of an app. Scenes can be used to divide an application into pages or views, such as the title screen of a game, level selector, and the game itself. Or it could be used to divide an application into modes. For instance, a drawing app may have a seperate scene for each drawing tool, even though the view is the same. 

You have full control over what a scene means for your application.

Scenes are made up of several functions like so:

~~~~
scene("<sceneId>", {
	init: function() {},
	update: function() {},
	draw: function() {},
    cleanup: function() {}
});
~~~~

These functions are all optional.

When a scene is initiated, `init()` will automatically be called first. Then, `update()` and `draw()` will be called repeatedly using `Window.requestAnimationFrame()`. This is where most of the logic of a scene will live.

`update()` is guaranteed to be called before `draw()` when the scene is initiated. However, `draw()` may be called several times before `update()` is called again. The idea is that `update()` is called within somewhat regular intervals, while `draw()` is called whenever the browser needs a frame rendered.

To transition from one scene to another, use:

~~~~
setScene("<sceneId>");
~~~~

## Input

### Keyboard Input

To see which keys are pressed down, access the global `keys` object, like so:

~~~~
if (keys.ctrl && keys.a) 
~~~~

| Special Keys |
| ----- |
| keys.enter |
| keys.shift |
| keys.ctrl  |
| keys.alt   |
| keys.up    |
| keys.down  |
| keys.left  |
| keys.right |

| Numbers   |
| --------- |
| keys.num0 |
| keys.num1 |
| ...       |
| keys.num9 |

| Letters   |
| --------- |
| keys.a    |
| keys.b    |
| ...       |
| keys.z    |

### Mouse Input

Likewise, mouse input may be accessed using the global `mouse` object.

| Property     | Type |
|----------------|-----------| 
| `x`            | `number`  |
| `y`            | `number`  |
| `leftButton`   | `boolean` |
| `middleButton` | `boolean` |
| `rightButton`  | `boolean` |
| `pressedAt.x`  | `number`  |
| `pressedAt.y`  | `number`  |
| `pressedAt.t`  | `number`  |

**Note:** `mouse.pressedAt.t` is used to get the time that the last mouse button was pressed down at.

## Surfaces 

This section will be added later, as Surfaces are still a work in progress.