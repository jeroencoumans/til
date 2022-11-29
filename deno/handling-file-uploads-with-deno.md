# File upload with Deno

Exiting middleware (such as [Oak Uploader](https://github.com/SaeedP11/oak_uploader/blob/master/mod.ts), [Fastly](https://github.com/hviana/faster)) or tutorials (e.g. https://medium.com/deno-the-complete-reference/handling-multipart-form-data-in-deno-66ca008c5c56, https://github.com/scottlepp/deno-file-upload-example)) all use [form multipart](https://stackoverflow.com/questions/3508338/what-is-the-boundary-in-multipart-form-data) parsing to accomplish this. But it turns out there's a much easier way ([see issue #1778]https://github.com/denoland/deno_std/issues/1778): `FormData`.

Some background first.

Upload an image in HTML as follows:
```html
<form method="post" enctype='multipart/form-data'>
  <input type='file' name='here-it-is' accept='image/*' multiple />
  <button type="submit">Submit</button>
</form>
```

This allows the user to select one or more images and submit them to the webserver. The request handler for this looks as follows:

```js
async function uploadHandler (request, context) {
  let form = await request.formData();
  let images = form.get('here-it-is');
  // BAM - nothing else required!
}
```

This is pretty amazing; we now have access to the `File` and can further process or store them.