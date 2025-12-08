---
sidebar_position: 1
---

# Getting started

Let's discover **Dropzone in less than 5 minutes**.

## Installation

Install the library in your project with `npm` or `yarn`:

`npm i @deltablot/dropzone`

`yarn add @deltablot/dropzone`

## Configuration

### Declarative configuration

The easiest way to use dropzone is by creating a form element with the class `dropzone`:

~~~html
<form action="/file-upload"
      class="dropzone"
      id="my-awesome-dropzone"></form>
~~~

That’s it. Dropzone will find all form elements with the class dropzone, automatically attach itself to it, and upload files dropped into it to the specified `action` attribute. The uploaded files can be handled just as if there would have been a html input like this:

~~~html
<input type="file" name="file" />
~~~


:::info
If you want another name than `file` you can configure Dropzone with the option `paramName`.
:::


#### Configuration

When declaring a Dropzone like this, you might be wondering how to configure it then, and fortunately it's quite easy:

1. Give the element that you want to configure an html id
2. Setup the configuration on Dropzone.options

Example:

~~~javascript
// Note that the name "myDropzone" is the camelized
// id of the form.
Dropzone.options.myDropzone = {
// Configuration options go here
};
~~~

~~~html
<form action="/target" class="dropzone" id="my-dropzone"></form>
~~~

You can create as many options on Dropzone.options as you need — for each HTML element that you created, you can simply add the configuration.

For information on available configuration options, see the configuration section.

The "auto discover" feature

The auto discover feature is disabled by default. You can enable it with:

~~~javascript
Dropzone.discover();
~~~

The way this works is called "auto discover". Dropzone checks when the DOM content finished loading, and then parses all HTML elements and looks for one with the class dropzone. If it finds an element, it checks if the element has an id. If it does, then it converst the id name to camel case (my-dropzone -> myDropzone) and checks if there is an entry in Dropzone.options with that key. If there is, then it uses that configuration object to instantiate the Dropzone for this object.

This behaviour can be disabled in two ways:

- Either set Dropzone.autoDiscover = false; somewhere in your JS. Just make sure it's invoked before the DOM ready event is dispatched.
- Set Dropzone.options.myDropzone = false; for individual dropzones. This allows you to use the auto discover feature, and only disable it for specific elements.

Dropzone parses the document when the document has finished loading, looks for elements that have the dropzone class, checks if there is a configuration in Dropzone.options for it, and then creates a Dropzone instance for that element. If all of that happened, before you defined your configuration, then Dropzone will miss the configuration. So make sure, that your configuration code is reached before Dropzone auto discovers these elements.

If you're not too keen on this approach, don't worry. Go to the next section to see how to create them imperatively.

### Imperative

How to create a Dropzone upload with JavaScript.

As an alternative to the declarative way, you can create Dropzones imperatively (even on non `<form>` elements) by instantiating the Dropzone class:

~~~javascript
// The constructor of Dropzone accepts two arguments:
//
// 1. The selector for the HTML element that you want to add
//    Dropzone to, the second
// 2. An (optional) object with the configuration
let myDropzone = new Dropzone("div#myId", { url: "/file/post"});
~~~

Don’t forget to specify an url option if you’re not using a `<form>` element, since Dropzone doesn’t know where to post to without an action attribute. On form elements, Dropzone defaults to the action attribute.

For a list of all configuration options, refer to the configuration section.

## Server Side Implementation

Dropzone does **not** provide the server side implementation of handling the files.

The way files are uploaded is identical to a standard file upload with a standard HTML form like this:

~~~html
<form action="" method="post" enctype="multipart/form-data">
  <input type="file" name="file" />
</form>
~~~

So if your server accepts files, uploaded like this, it will accept files uploaded with Dropzone.

So please look at the corresponding documentation of the server implementation you're using. Here are a few documentations, if you think I should add some, please contact me.

* [Spring](https://spring.io/guides/gs/uploading-files/)
* [NodeJS with express and multer](https://expressjs.com/en/resources/middleware/multer.html)
* [Ruby on rails](http://guides.rubyonrails.org/form_helpers.html#uploading-files)
* [Complete PHP tutorial](https://startutorial.com/view/dropzonejs-php-how-to-build-a-file-upload-form) by startutorial.com
* [Basic PHP file upload](https://www.php.net/manual/en/features.file-upload.post-method.php#example-354)
* [Tutorial for Dropzone and Lavarel (PHP)](https://maxoffsky.com/code-blog/howto-ajax-multiple-file-upload-in-laravel/) written by Maksim Surguy
* [Symfony2 and Amazon S3](https://www.jesuisundev.fr/upload-drag-drop-via-dropzonejs-symfony2-on-cloud-amazon-s3/)
* [File upload in ASP.NET MVC using Dropzone JS and HTML5](http://venkatbaggu.com/file-upload-in-asp-net-mvc-using-dropzone-js-and-html5/)
* [How to build a file upload form using DropzoneJS and Go](https://hackernoon.com/how-to-build-a-file-upload-form-using-dropzonejs-and-go-8fb9f258a991)
* [How to display existing files on server using DropzoneJS and Go](https://hackernoon.com/how-to-display-existing-files-on-server-using-dropzonejs-and-go-53e24b57ba19)

Paid documentations:

* [eBook for Dropzone with PHP](https://www.startutorial.com/homes/dropzonejs_php_the_complete_guide?utm_source=dzj&utm_medium=banner&utm_campaign=dropzonejs) by startutorial.com.


## Fallback for no JavaScript

Sometimes you need to build websites that should work even if the user doesn't have JavaScript enabled or the browser is not supported. Of course, a drag'n'drop JavaScript library won't work in these cases, but Dropzone got you covered by offering a way to define a fallback.

Inside your dropzone HTML element (in most cases that's a `<form>` element), you can define an additional element with the class `fallback` . When (or in this case: "if") Dropzone initializes, it will simply remove this element.

Here is an example of what this might look like:

~~~html
<form action="/file-upload" class="dropzone">
  <div class="fallback">
    <input name="file" type="file" multiple />
  </div>
</form>
~~~

So in case the browser doesn't support JavaSript, the user will simply see the `fallback` div with the file input element, and everything will work normally but with no drag and drop support.

*If* the browser is supported however, this element will be removed, and Dropzone will add a notice that it accepts dropped files and handle them.
