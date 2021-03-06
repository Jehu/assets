# ![Hybrid](http://i.imgur.com/jUDMlbO.png) Assets

[![Join the chat at https://gitter.im/buildhybrid/platform](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/buildhybrid/platform?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

> Process image assets for web, mobile, and desktop 

Assets aims to take all of the work out of creating image assets for your app. Let Assets generate all of your cross-platform app icons, splashscreens, favicons, app store preview images, @2x retina images, thumbnails, optimized web images, or even sprites. Now if you want to update your app icon on every platform.. it's only one command! Assets build tool watches for changes in your config file and source images and will automatically reprocess your assets if you modify them, completely automating your asset build pipeline!

## Installation

#### Requirement

First download and install [GraphicsMagick](http://www.graphicsmagick.org/). 

In Mac OS X, you can simply use [Homebrew](http://mxcl.github.io/homebrew/) and do:
```
brew install graphicsmagick
```

In Ubuntu
```
sudo apt-get install graphicsmagick
```

In Windows

[Windows Installation Instructions](http://www.graphicsmagick.org/INSTALL-windows.html)

#### NPM Module
To use manually using require
```
npm install hybrid-assets
```
TODO: Include docs on how to use manually

#### Build Plugins

For use as a Meteor build plugin use this package ([notes](https://github.com/meteorhybrid/assets/wiki/Meteor-Build-Tool))
```
meteor add hybrid:assets
```

* [ ] Grunt Plugin
* [ ] Gulp Plugin
* [ ] Brocolli Plugin

## Features

#### Image Optimization
* [x] Resizing
* [x] Retina Images - *Generates a `@2x` and regular version of the image.*
* [x] Image Quality Adjustment - *Good for lowering the filesize of larger images.*
* [ ] Sprites

#### Platform Icons Presets
* [x] Favicons + (iOS/Windows Pin Icons) - *favicon*
* [x] iOS - *ios* - [(platform design notes)](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/MobileHIG/AppIcons.html#//apple_ref/doc/uid/TP40006556-CH19-SW1)
* [x] Android - *android* - [(platform design notes)](http://developer.android.com/design/style/iconography.html)
* [x] Mac OS X - *mac* - [(platform design notes)](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
* [x] Windows Universal - *windows*
* [x] [Chrome Homescreen Installed Apps](https://developer.chrome.com/multidevice/android/installtohomescreen) - *chrome-homescreen*
* [x] Chrome Extension - *chrome-ext*
* [x] Chrome App - *chrome-app*
* [ ] Android TV - *android-tv*
* [ ] Blackberry - *blackberry*

#### Platform Launch Screen Presets
* [ ] iOS - *ios-splash*
* [ ] Android - *android-splash*
* [ ] Windows Phone - *windows-phone-splash*
* [ ] Blackberry - *blackberry-splash*

#### Other Platform Presets
* [x] Chrome Extension Preview - *chrome-ext-preview*

#### Other Posible Features
* [ ] Automatic CDN Uploading

### Contributing
If you want to help contribute presets look in `presets.js` for examples :) 

## Configuration
Make a new file in your app directory called `config.assets`

Example `config.assets`
```json
{
    "appIcons": {
        "source": "private/icon.png",
        "output": "resources/icons/",
        "type": ["ios", "android", "chromeext", "mac"]
    },
    "webIcons": {
        "source": "private/icon.png",
        "output": "public/assets/images/favicon/",
        "type": ["favicon"]
    },
    "webImages": {
        "source": [
            "private/image.jpg",
            "private/image2.jpg"
        ],
        "output": "public/assets/images/",
        "retina": true,
        "quality": 80
    },
    "customNameThumbnail": {
        "source": [
            "private/image.jpg"
        ],
        "name": "{{source}}-thumb.jpg",
        "output": "public/assets/images/",
        "height": 20,
        "width": 20,
        "retina": true,
        "quality": 40
    }
}
```

* **source** - Relative path to source image .
* **output** - Relative path of where to save the new images (will store in output/type/name if using a type).
* **type** - Array of types to include.
* **name** - Name of the outputed file. For an array of sources you can use `{{source}}` and it will automatically fill in the source file name here;
* **height** - Height of the (non-retina) image output.
* **width** - Width of the (non-retina) image output.
* **retina** - Include an @2x version of the image (this assumes the sources image is @2x).
* **quality** - Adjust the compression level. val ranges from 0 to 100 (best).

## Assets CLI

To use Assets from the command line, clone the repo and do the following

```
cd assets
npm install
./assets --help
```

```
Usage:
  assets [OPTIONS] [ARGS]

Options:
  -c, --config FILE      Path to assets config file
  -r, --root PATH        Root directory of application
  -s, --cache PATH       Directory to store cache
  -f, --force            Force build even if cache hasn't changed
  -h, --help             Display help and usage details
```


### Rerun Note
To avoid recreating images every time a file is changed during development, asset builder only reruns if `config.assets` changes or any of the source images referenced in it change.

### Source Icon
The source image for icon types should be a 1024x1024 `png` file. ([Example Source Icon](http://i.imgur.com/FWZofOo.png))

iOS generated icons from example
![iosoutput](http://i.imgur.com/gPGb4p7.png)

### Favicons
If using the `favicon` type, include the assets in your `<head>`
```html
<!-- Standard Favicon -->
<link rel="icon" type="image/x-icon" href="/assets/images/favicon/favicon.png" />

<!-- For iPhone 4 Retina display: -->
<link rel="apple-touch-icon-precomposed" sizes="114x114" href="/assets/images/favicon/apple-touch-icon-114x114-precomposed.png">

<!-- For iPad: -->
<link rel="apple-touch-icon-precomposed" sizes="72x72" href="/assets/images/favicon/apple-touch-icon-72x72-precomposed.png">

<!-- For iPhone: -->
<link rel="apple-touch-icon-precomposed" href="/assets/images/favicon/apple-touch-icon-57x57-precomposed.png">

<!-- For Windows 8: -->
<meta name="msapplication-TileImage" content=“/assets/images/favicon/pinned.png”>
<meta name="msapplication-TileColor" content="#ef0303”>

<!-- For Opera Coast: -->  
<link rel="icon" href="/assets/images/favicon/favicon-coast.png" sizes="228x228">
```

![favicons](http://i.imgur.com/Rzrxoz4.png)

### Retina Images
If generating retina images, you may want to look into [retina.js](http://imulus.github.io/retinajs/) or use [css pixel ratio media queries](https://css-tricks.com/snippets/css/retina-display-media-query/)
