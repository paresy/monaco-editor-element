# monaco-editor-element

A custom element wrapper for the Monaco editor by Microsoft.

Quick example:

```html
<monaco-editor></monaco-editor>
```

Customizable attributes: 

 - `namespace`: Where to find the `monaco-editor` source, default *node_modules/monaco-editor/min/vs*
 - `language`: Which language parser to use, default *javascript* ( Can't be changed after first render )
 - `value`: Content of the editor
 - `theme`: Will be shared across all editors ( see https://github.com/Microsoft/monaco-editor/issues/338 )
 - `read-only`
 - `no-line-numbers`
 - `no-minimap`: Disables the minimap
 - `no-drag-and-drop`: Disables the drag and drop


Integration:

Due to the fact that `monaco-editor` is only available through `npm`, that I couldn't find a public repository of the source code and that it heavily depends on `requirejs` some compromises have been made.
The editor needs its loader to work.
You can find that script under `dist/monaco-editor/vs`. You need to tell the element where the loader comes from so that `requirejs` can resolve the right modules. This is why the namespace attribute is here.
Furthermore, to be able to embed the editor inside a shadow root, a patch on the source doe must be applied. For everyone's convenience, I added a postinstall script that takes care of that.
Unfortunaletly it means that you will have to rely on the version inside the `dist` folder of this element.
If you want to host the `monaco-editor` somewhere else than the `dist` folder (I definitely will), you need to update these values.

Example:
Let's say you put the editor's files under a `vendor` folder.
Create a file to import the `monaco-editor` dependencies like so:

```html
<!-- monaco-editor-import.html -->
<!-- Requires the editor's code -->
<script src="/vendor/monaco-editor/min/vs/loader.js"></script>
<!-- Imports the custom element wrapper -->
<script src="./monaco-editor.js"></script>
```

Then In your app, you can simply:

```html
<link rel="import" href="./monaco-editor-import.html">

<!-- Make sure the editor knows where to find the source -->
<monaco-editor namespace="vendor/monaco-editor/min/vs"></monaco-editor>
```

When the first `monaco-editor` is created the dependencies are retrieved and you'll be notified by a `ready` event when it is... well... ready.

You can access the `editor` object to gain full control of the customization by calling `.getEditor()` on the custom elements 