# Froala WYSIWYG Editor Svelte Wrapper

[![npm version](https://badge.fury.io/js/svelte-froala-wysiwyg.svg)](https://badge.fury.io/js/svelte-froala-wysiwyg)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

The official Svelte wrapper for [Froala WYSIWYG HTML Editor](https://froala.com/wysiwyg-editor/).

## Features
- **Two-way Data Binding:** Seamlessly sync editor content with Svelte variables.
- **Custom Configuration:** Full access to all Froala options and events.
- **Special Tag Support:** Initialize the editor on `<img>`, `<button>`, `<input>`, and `<a>` tags.
- **Manual Control:** Programmatically initialize and destroy editor instances.

---

## Installation

```bash
npm install svelte-froala-wysiwyg
```
## Development Setup
```bash
npm install
```

```bash
npm run dev 
```

## Quick Start

### Props
model
config
tag
editor
manual

### Methods
initializeEditor()
destroy()
getEditor()

### Basic Usage
Use the `FroalaEditor` component in your Svelte file:

```svelte
<script>
  // Import the Froala wrapper component
  import FroalaEditor from "svelte-froala-wysiwyg";

  // Load Froala's bundled editor styles (toolbar, editor UI)
  import 'froala-editor/css/froala_editor.pkgd.min.css';

  // Load Froala's content styles (applies inside the editable area)
  import 'froala-editor/css/froala_style.css';

  // Reactive variable holding the editor's HTML content
  // bind:model keeps this in sync with the editor in real time
  let htmlContent = "<p>Hello, Froala!</p>";
</script>

<!--
  FroalaEditor component
  - bind:model: two-way binding — editor changes update htmlContent and vice versa
-->
<FroalaEditor bind:model={htmlContent} />

<!--
  Live preview of the editor's HTML output
  - {@html} renders the raw HTML string returned by the editor
  - Wrap in a sandboxed container to scope Froala's content styles
-->
<div class="preview">
  {@html htmlContent}
</div>
```

---

## Advanced Usage

### Custom Configuration
Pass any [Froala Options](https://froala.com/wysiwyg-editor/docs/options/) via the `config` prop:

```svelte
<script>
  const myConfig = {
    placeholderText: 'Start typing...',
    charCounterCount: false,
    toolbarButtons: ['bold', 'italic', 'underline', 'insertLink']
  };
</script>

<FroalaEditor config={myConfig} />
```

### Handling Events
Events can be defined within the `config.events` object:

```svelte
<script>
  const config = {
    events: {
      'contentChanged': function () {
        console.log('Content updated!');
      },
      'focus': function () {
        console.log('Editor focused');
      }
    }
  };
</script>

<FroalaEditor {config} />
```

### Special Tags (Non-div Elements)
Initialize Froala on elements like images or buttons. In this mode, the `model` becomes an object representing the element's attributes.

```svelte
<script>
  let imageModel = {
    src: "https://froala.com/assets/img/logo.png",
    alt: "Froala Logo"
  };
</script>

<FroalaEditor tag="img" bind:model={imageModel} />
```

### Manual Initialization
If you need to delay initialization or control it manually:

```svelte
<script>
  let editorRef;

  function init() {
    editorRef.initializeEditor();
  }
</script>

<button on:click={init}>Initialize Editor</button>

<FroalaEditor 
  bind:this={editorRef} 
  manual={true} 
  model="<p>Wait for it...</p>" 
/>
```

## Documentation
For full documentation on the Froala Editor API, options, and events, visit the [Official Froala Documentation](https://froala.com/wysiwyg-editor/docs/).

## License

The `svelte-froala-wysiwyg` project is under the MIT license. Froala Editor itself is a commercial component with a 30-day free trial: see the [pricing page](https://cart.froala.com) and the [license terms](https://www.ideracorp.com/legal/Froala#tabs-2).

### License key

Froala requires a key, passed in the `key` option. Start with the trial key if you're just trying the product or comparing it with other solutions.

```svelte
<script>
  import FroalaEditor from 'svelte-froala-wysiwyg';

  const config = {
    key: 'TRIAL-LICENSE-KEY-FOR-FROALA-NOT-FOR-PRODUCTION-USE' // 30-day trial key or your commercial key
  };
</script>

<FroalaEditor {config} />
```

Before you go live, replace the trial key with your commercial key.

Without a valid key the editor shows a notice in the bar at the bottom, under "Powered by Froala", and writes the same text to the browser console once. The text says whether the key is missing, a trial, invalid or expired.

On a local host (`localhost`, `127.x.x.x`, `0.0.0.0`) you get the same notice as on a public site if there is no key, the key is a trial key, or the key has expired. A key that is still valid gives a clean editor there, so licensed developers are not affected.
