---
sidebar_position: 3
---

# Tutorials

## Combine form data with files

You want to have a normal form, that you can drop files in, and send the whole form, including the files, with the click of a button? Well, you're in luck. It requires a bit of configuration, but it's not difficult to do.

First off, you need to make sure that Dropzone won't auto process your queue and start uploading each file individually. This is done by setting these properties:

* `autoProcessQueue: false` Dropzone should wait for the user to click a button to upload
* `uploadMultiple: true` Dropzone should upload all files at once (including the form data) not all files individually
* `parallelUploads: 100` that means that they shouldn't be split up in chunks as well
* `maxFiles: 100` and this is to make sure that the user doesn't drop more files than allowed in one request.

This is basically everything you need to do. *Dropzone automatically submits all input fields inside a form that behaves as a Dropzone*.

## Show me the code

This is what a very simple HTML looks like:

```html
<form id="upload-form" class="dropzone">
  <!-- this is were the previews should be shown. -->
  <div class="previews"></div>
  <!-- Now setup your input fields -->
  <input type="email" name="username" />
  <input type="password" name="password" />

  <button type="submit">Submit data and files!</button>
</form>
```

And now for the Dropzone configuration:

```javascript
Dropzone.options.uploadForm = { // The camelized version of the ID of the form element

  // The configuration we've talked about above
  autoProcessQueue: false,
  uploadMultiple: true,
  parallelUploads: 100,
  maxFiles: 100,

  // The setting up of the dropzone
  init: function() {
    var myDropzone = this;

    // First change the button to actually tell Dropzone to process the queue.
    this.element.querySelector("button[type=submit]").addEventListener("click", function(e) {
      // Make sure that the form isn't actually being sent.
      e.preventDefault();
      e.stopPropagation();
      myDropzone.processQueue();
    });

    // Listen to the sendingmultiple event. In this case, it's the sendingmultiple event instead
    // of the sending event because uploadMultiple is set to true.
    this.on("sendingmultiple", function() {
      // Gets triggered when the form is actually being sent.
      // Hide the success button or the complete form.
    });
    this.on("successmultiple", function(files, response) {
      // Gets triggered when the files have successfully been sent.
      // Redirect user or notify of success.
    });
    this.on("errormultiple", function(files, response) {
      // Gets triggered when there was an error sending the files.
      // Maybe show form again, and notify user of error
    });
  }
}
```

That's it.

## Tips & Tricks

:::info
Checkout the [Discussions section on Github](https://github.com/dropzone/dropzone/discussions) for more content.
:::

If you do not want the default message at all (»Drop files to upload (or click)«), you can put an element inside your dropzone element with the class `dz-message` and dropzone will not create the message for you.

Dropzone will submit any hidden fields you have in your dropzone form. So this is an easy way to submit additional data. You can also use the `params` option.

Dropzone adds data to the `file` object you can use when events fire. You can access `file.width` and `file.height` if it’s an image, as well as `file.upload`which is an object containing: `progress` (0-100), `total` (the total bytes) and `bytesSent`.

If you want to add additional data to the file upload that has to be specific for each file, you can register for the [`sending`](https://www.dropzonejs.com/#event-sending) event:

```javascript
myDropzone.on("sending", function(file, xhr, formData) {
  // Will send the filesize along with the file as POST data.
  formData.append("filesize", file.size);
});
```

To access the preview html of a file, you can access `file.previewElement`. For example:

```javascript
myDropzone.on("addedfile", function(file) {
  file.previewElement.addEventListener("click", function() {
    myDropzone.removeFile(file);
  });
});
```

If you want the whole body to be a Dropzone and display the files somewhere else you can simply instantiate a Dropzone object for the body, and define the[`previewsContainer`](https://www.dropzonejs.com/#config-previewsContainer) option. The `previewsContainer` should have the`dropzone-previews` or `dropzone` class to properly display the file previews.

```javascript
new Dropzone(document.body, {
  previewsContainer: ".dropzone-previews",
  // You probably don't want the whole body
  // to be clickable to select files
  clickable: false
});
```

Look at the [gitlab wiki](https://gitlab.com/meno/dropzone/wikis/home) for more examples.

If you have any problems using Dropzone, please try to find help on [stackoverflow.com](https://stackoverflow.com/questions/tagged/dropzone.js) by using the `dropzone.js` tag. Only create an issue on GitLab when you think you found a bug or want to suggest a new feature.
