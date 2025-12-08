---
sidebar_position: 2
---

# Configuration

## Basics

In this section we'll co over the fundamentals of configuring Dropzones.

:::info
You should already be familiar on how to pass a configuration object to Dropzone in the [declarative](./intro#declarative-configuration) and the [imperative](./intro#imperative) way. If not, please visit the respective sections.
:::

This is what a typical configuration of Dropzone would look like:

~~~html
<form action="/target" class="dropzone" id="my-great-dropzone"></form>

<script>
  Dropzone.options.myGreatDropzone = { // camelized version of the `id`
    paramName: "file", // The name that will be used to transfer the file
    maxFilesize: 2, // MB
    accept: function(file, done) {
      if (file.name == "justinbieber.jpg") {
        done("Naha, you don't.");
      }
      else { done(); }
    }
  };
</script>
~~~

~~~javascript
import Dropzone from "dropzone";

let myDropzone = Dropzone({
  paramName: "file", // The name that will be used to transfer the file
  maxFilesize: 2, // MB
  accept: function(file, done) {
    if (file.name == "justinbieber.jpg") {
      done("Naha, you don't.");
    }
    else { done(); }
  }
});
~~~


:::info
For a list of all possible options, refer to the [`src/options.js`](https://github.com/NicolasCARPi/dropzone/blob/main/src/options.js) file.
:::

## Events

If you want to react to events happening (a file has been added or removed, an upload started, a progress changed, etc...) you want to be listening to events. Checkout the [events section](./events) for more information on events.

## Configuration Options

Here is a list of all available options for Dropzone. In case any of this information is outdated (please inform us, if that's the case) or you need more insight, you can always look at the [options.js](https://github.com/NicolasCARPi/dropzone/blob/master/src/options.js) that is used in the actual library.

| Name                    | Default     | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| ----------------------- | ----------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `url`                   | `null`      | Has to be specified on elements other than form (or when the form doesn't have an `action` attribute). You can also provide a function that will be called with `files` and must return the url (since `v3.12.0`)                                                                                                                                                                                                                                                 |
| `method`                | `"post"`    | Can be changed to `"put"` if necessary. You can also provide a function that will be called with `files` and must return the method (since `v3.12.0`).                                                                                                                                                                                                                                                                                                            |
| `withCredentials`       | `false`     | Will be set on the XHRequest.                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| `timeout`               | `null`      | The timeout for the XHR requests in milliseconds (since `v4.4.0`). If set to null or 0, no timeout is going to be set.                                                                                                                                                                                                                                                                                                                                            |
| `parallelUploads`       | `2`         | How many file uploads to process in parallel (See the Enqueuing file uploads documentation section for more info)                                                                                                                                                                                                                                                                                                                                                 |
| `uploadMultiple`        | `false`     | Whether to send multiple files in one request. If this it set to true, then the fallback file input element will have the `multiple` attribute as well. This option will also trigger additional events (like `processingmultiple`). See the events documentation section for more information.                                                                                                                                                                   |
| `chunking`              | `false`     | Whether you want files to be uploaded in chunks to your server. This can't be used in combination with `uploadMultiple`. See chunksUploaded for the callback to finalise an upload.                                                                                                                                                                                                                                |
| `forceChunking`         | `false`     | If `chunking` is enabled, this defines whether \*\*every\*\* file should be chunked, even if the file size is below chunkSize. This means, that the additional chunk form data will be submitted and the `chunksUploaded` callback will be invoked.                                                                                                                                                                                                               |
| `chunkSize`             | `2000000`   | If `chunking` is `true`, then this defines the chunk size in bytes.                                                                                                                                                                                                                                                                                                                                                                                               |
| `parallelChunkUploads`  | `false`     | If `true`, the individual chunks of a file are being uploaded simultaneously.                                                                                                                                                                                                                                                                                                                                                                                     |
| `retryChunks`           | `false`     | Whether a chunk should be retried if it fails.                                                                                                                                                                                                                                                                                                                                                                                                                    |
| `retryChunksLimit`      | `3`         | If `retryChunks` is true, how many times should it be retried.                                                                                                                                                                                                                                                                                                                                                                                                    |
| `maxFilesize`           | `256`       | The maximum filesize (in bytes) that is allowed to be uploaded.                                                                                                                                                                                                                                                                                                                                                                                                   |
| `paramName`             | `"file"`    | The name of the file param that gets transferred. \*\*NOTE\*\*: If you have the option `uploadMultiple` set to `true`, then Dropzone will append `[]` to the name.                                                                                                                                                                                                                                                                                                |
| `createImageThumbnails` | `true`      | Whether thumbnails for images should be generated                                                                                                                                                                                                                                                                                                                                                                                                                 |
| `maxThumbnailFilesize`  | `10`        | In MB. When the filename exceeds this limit, the thumbnail will not be generated.                                                                                                                                                                                                                                                                                                                                                                                 |
| `thumbnailWidth`        | `120`       | If `null`, the ratio of the image will be used to calculate it.                                                                                                                                                                                                                                                                                                                                                                                                   |
| `thumbnailHeight`       | `120`       | The same as `thumbnailWidth`. If both are null, images will not be resized.                                                                                                                                                                                                                                                                                                                                                                                       |
| `thumbnailMethod`       | `"crop"`    | How the images should be scaled down in case both, `thumbnailWidth` and `thumbnailHeight` are provided. Can be either `contain` or `crop`.                                                                                                                                                                                                                                                                                                                        |
| `resizeWidth`           | `null`      | If set, images will be resized to these dimensions before being **uploaded**. If only one, `resizeWidth` **or** `resizeHeight` is provided, the original aspect ratio of the file will be preserved. The `options.transformFile` function uses these options, so if the `transformFile` function is overridden, these options don't do anything.                                                            |
| `resizeHeight`          | `null`      | See `resizeWidth`.                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| `resizeMimeType`        | `null`      | The mime type of the resized image (before it gets uploaded to the server). If `null` the original mime type will be used. To force jpeg, for example, use `image/jpeg`. See `resizeWidth` for more information.                                                                                                                                                                                                                                                  |
| `resizeQuality`         | `0.8`       | The quality of the resized images. See `resizeWidth`.                                                                                                                                                                                                                                                                                                                                                                                                             |
| `resizeMethod`          | `"contain"` | How the images should be scaled down in case both, `resizeWidth` and `resizeHeight` are provided. Can be either `contain` or `crop`.                                                                                                                                                                                                                                                                                                                              |
| `filesizeBase`          | `1000`      | The base that is used to calculate the \*\*displayed\*\* filesize. You can change this to 1024 if you would rather display kibibytes, mebibytes, etc... 1024 is technically incorrect, because `1024 bytes` are `1 kibibyte` not `1 kilobyte`. You can change this to `1024` if you don't care about validity.                                                                                                                                                    |
| `maxFiles`              | `null`      | If not `null` defines how many files this Dropzone handles. If it exceeds, the event `maxfilesexceeded` will be called. The dropzone element gets the class `dz-max-files-reached` accordingly so you can provide visual feedback.                                                                                                                                                                                                                                |
| `headers`               | `null`      | An optional object to send additional headers to the server. Eg: `{ "My-Awesome-Header": "header value" }`                                                                                                                                                                                                                                                                                                                                                        |
| `clickable`             | `true`      | If `true`, the dropzone element itself will be clickable, if `false` nothing will be clickable.You can also pass an HTML element, a CSS selector (for multiple elements) or an array of those. In that case, all of those elements will trigger an upload when clicked.                                                                                                                                                          |
| `ignoreHiddenFiles`     | `true`      | Whether hidden files in directories should be ignored.                                                                                                                                                                                                                                                                                                                                                                                                            |
| `acceptedFiles`         | `null`      | The default implementation of `accept` checks the file's mime type or extension against this list. This is a comma separated list of mime types or file extensions.Eg.: `image/\*,application/pdf,.psd`If the Dropzone is `clickable` this option will also be used as [`accept`](https://developer.mozilla.org/en-US/docs/HTML/Element/input#attr-accept) parameter on the hidden file input as well. |
| `acceptedMimeTypes`     | `null`      | \*\*Deprecated!\*\* Use acceptedFiles instead.                                                                                                                                                                                                                                                                                                                                                                                                                    |
| `autoProcessQueue`      | `true`      | If false, files will be added to the queue but the queue will not be processed automatically. This can be useful if you need some additional user input before sending files (or if you want want all files sent at once). If you're ready to send the file simply call `myDropzone.processQueue()`.See the enqueuing file uploads documentation section for more information.                                   |
| `autoQueue`             | `true`      | If false, files added to the dropzone will not be queued by default. You'll have to call `enqueueFile(file)` manually.                                                                                                                                                                                                                                                                                                                                            |
| `addRemoveLinks`        | `false`     | If `true`, this will add a link to every file preview to remove or cancel (if already uploading) the file. The `dictCancelUpload`, `dictCancelUploadConfirmation` and `dictRemoveFile` options are used for the wording.                                                                                                                                                                                                                                          |
| `previewsContainer`     | `null`      | Defines where to display the file previews – if `null` the Dropzone element itself is used. Can be a plain `HTMLElement` or a CSS selector. The element should have the `dropzone-previews` class so the previews are displayed properly.                                                                                                                                                                                                                         |
| `disablePreviews`       | `false`     | Set this to `true` if you don't want previews to be shown.                                                                                                                                                                                                                                                                                                                                                                                                        |
| `hiddenInputContainer`  | `"body"`    | This is the element the hidden input field (which is used when clicking on the dropzone to trigger file selection) will be appended to. This might be important in case you use frameworks to switch the content of your page.Can be a selector string, or an element directly.                                                                                                                                                                        |
| `capture`               | `null`      | If null, no capture type will be specified If camera, mobile devices will skip the file selection and choose camera If microphone, mobile devices will skip the file selection and choose the microphone If camcorder, mobile devices will skip the file selection and choose the camera in video mode On apple devices multiple must be set to false. AcceptedFiles may need to be set to an appropriate mime type (e.g. "image/\*", "audio/\*", or "video/\*"). |
| `renameFilename`        | `null`      | \*\*Deprecated\*\*. Use `renameFile` instead.                                                                                                                                                                                                                                                                                                                                                                                                                     |
| `renameFile`            | `null`      | A function that is invoked before the file is uploaded to the server and renames the file. This function gets the `File` as argument and can use the `file.name`. The actual name of the file that gets used during the upload can be accessed through `file.upload.filename`.                                                                                                                                                                                    |
| `forceFallback`         | `false`     | If `true` the fallback will be forced. This is very useful to test your server implementations first and make sure that everything works as expected without dropzone if you experience problems, and to test how your fallbacks will look.                                                                                                                                                                                                                       |


## Layout

The HTML that is generated for each file by dropzone is defined with the option `previewTemplate` which defaults to this:

~~~html
<div class="dz-preview dz-file-preview">
  <div class="dz-details">
    <div class="dz-filename"><span data-dz-name></span></div>
    <div class="dz-size" data-dz-size></div>
    <img data-dz-thumbnail />
  </div>
  <div class="dz-progress"><span class="dz-upload" data-dz-uploadprogress></span></div>
  <div class="dz-success-mark"><span>✔</span></div>
  <div class="dz-error-mark"><span>✘</span></div>
  <div class="dz-error-message"><span data-dz-errormessage></span></div>
</div>
~~~

The container (`dz-preview`) gets the `dz-processing` class when the file gets processed, `dz-success` when the file got uploaded and `dz-error` in case the file couldn’t be uploaded. In the latter case, `data-dz-errormessage` will contain the text returned by the server.

To overwrite the default template, use the `previewTemplate` config.

You can access the HTML of the file preview in any of the events with `file.previewElement`.

If you decide to rewrite the `previewTemplate` from scratch, you should put elements with the `data-dz-*` attributes inside:

* `data-dz-name`
* `data-dz-size`
* `data-dz-thumbnail` (This has to be an `<img />` element and the `alt` and `src` attributes will be changed by Dropzone)
* `data-dz-uploadprogress` (Dropzone will change the `style.width` property from `0%` to `100%` whenever there’s a `uploadprogress` event)
* `data-dz-errormessage`

The default options for Dropzone will look for those element and update the content for it.

If you want some specific link to remove a file (instead of the built in `addRemoveLinks` config), you can simply insert elements in the template with the `data-dz-remove` attribute. Example:

~~~html
<img src="removebutton.png" alt="Click me to remove the file." data-dz-remove />
~~~

You are not forced to use those conventions though. If you override all the default event listeners you can completely rebuild your layout from scratch.

See the installation section on how to add the stylesheet if you want your Dropzone to look like the example dropzone.

See the [Theming](./configuration#theming) section, for a more in depth look at how to completely change Dropzone’s UI.

## Theming

If you want to theme your Dropzone to look fully customized, in most cases you can simply replace the preview HTML template, adapt your CSS, and maybe create a few additional event listeners.

It's covered in the [Layout section](./configuration#layout).

## Completely change the way Dropzone is displayed

Dropzone itself sets up a lot of event listeners when a Dropzone is created, that handle all your UI. They do stuff like: create a new HTML element, add the `<img>`element when provided with image data (with the `thumbnail` event), update the progress bar when the `uploadprogress` event fires, show a checkmark when the `success` event fires, etc…

*Everything* visual is done in those event handlers. If you would overwrite all of them with empty functions, Dropzone would still be fully functional, but wouldn’t display the dropped files anymore.

> If you like the default look of Dropzone, but would just like to add a few bells and whistles here and there, you should just [add additional event listeners](./events) instead.

Overwriting the default event listeners, and creating your own, custom Dropzone, would look something like this:

```javascript
// This is an example of completely disabling Dropzone's default behavior.
// Do *not* use this unless you really know what you are doing.
Dropzone.myDropzone.options = {
  previewTemplate: document.querySelector('#template-container').innerHTML,
  // Specifing an event as an configuration option overwrites the default
  // `addedfile` event handler.
  addedfile: function(file) {
    file.previewElement = Dropzone.createElement(this.options.previewTemplate);
    // Now attach this new element some where in your page
  },
  thumbnail: function(file, dataUrl) {
    // Display the image in your file.previewElement
  },
  uploadprogress: function(file, progress, bytesSent) {
    // Display the progress
  }
  // etc...
};
```

Obviously this lacks the actual implementation. Look at the source to see how Dropzone does it internally.

You should use this option if you don’t need any of the default Dropzone UI, but are only interested in Dropzone for it’s event handlers, file upload and drag’n’drop functionality.
