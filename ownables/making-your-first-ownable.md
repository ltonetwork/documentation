# Making your first ownable

Ownables can vary from simple static widgets to dynamic stateful widgets. In this guide, we will provide you with an example to create a **static ownable**, since it's the easiest to get started with.

## What is a static ownable?

A static ownable is a non-interactive asset. A common example of this would be an image, such as a piece of art, or a video, representing a 3D model.

To create a simple static ownable, all you need to do is create 3 files, zip them, and upload them to the ownable wallet. In the guide below we will describe this process.

## Required files

There are at least two files required to create a static ownable, a `package.json` and a `index.html`. These files are described in more detail below.

Alongside these 2 files, you should also provide a representation of the ownable, such as an image or video. All of these files should be placed inside a folder, which you should name after the ownable that you want to create.

In this example, we will create an ownable that represents the LTO Network logo. For this, we first have to create a folder named `lto-logo`. which contains an image `lto.jpg`. The image can be downloaded [here](https://avatars.githubusercontent.com/u/50703120).

```
lto-logo
  package.json
  index.html
  lto.jpg
```

Feel free to change the image and name of the folder, based on the ownable you want to create.

### package.json

This file contains basic information about the ownable, such as its name and description, and should at least contain the following:

```json
{
  "name": "ownable-lto-logo",
  "description": "The LTO logo as an ownable"
}
```

You should replace the name and description based on what your ownable should represent.

### index.html

This file contains the actual representation of the ownable widget. It can be styled using html and css. In this case, we are loading the `lto.jpg` file, which is the image that is used to represent the widget.

```html
<html lang="">
  <head>
    <title>LTO logo</title>
    <style>
      html, body {
        margin: 0;
        height: 100%;
      }

      body {
        width: 100%;
        height: 100%;
        overflow: hidden;
      }

      video {
        width: 100%;
        height: 100%;
      }
    </style>
  </head>
  <body>
    <image src="lto.jpg"/>
  </body>
</html>
```

You can use this template and customize it to display your own ownable widget. All you have to do is replace the title used in `<title>` and the image used in `<image src>`.

{% hint style="warning" %}
Please note that widgets cannot make HTTP calls, which means that every asset used should be provided in the folder of the ownable. You cannot load things such as images from the internet, they need to be provided in the folder of the ownable.
{% endhint %}

## Publishing the ownable

Now that we have created all of the required files, we can upload them to the ownables wallet by following these steps:

1. Zip up the contents of the ownable folder. In this example, we created a folder named `lto-logo`, so we will zip that. Ensure that the zip does not contain the folder, but that the files of the folder are directly in it.
2. Navigate to [https://demo.ownables.info](https://demo.ownables.info). Keep in mind that this is our testnet environment, and you will not be charged to create an ownable. In the future when we have a mainnet environment, creating an ownable will require a fee.
3. You will be prompted to create an account or import an existing LTO wallet based on a seed. Choose whichever option applies to you. For testing purposes just create a new account.
4. Click on the plus icon on the bottom right. A popup window will be shown. Now click on the `Import packages` button and select your zip file.
5. Your new ownable should now be imported. Select your ownable in the list that is shown in the popup window and it should be forged!

## What to do next?

You have now created your first static ownable! Feel free to browse our example ownable templates, which vary from easy to complex. You can find our example ownables [here](https://github.com/ltonetwork/ownables-sdk#examples). For more information about ownables, please continue reading to discover our ownables SDK.
