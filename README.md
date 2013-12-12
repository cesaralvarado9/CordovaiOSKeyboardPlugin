# iOS Keyboard Plugin

This plugin allows you to have more control over the behavior of the iOS keyboard and subscribe to native UIKeyboard
events via jQuery. Allows for smoothly animated shrinking of the content view. While other plugins attempt to
do similar things (such as the official Cordova/PhoneGap plugins), they were glitchy on iOS7 (in my experience), or didn't
satisfy position:fixed/absolute layouts. Some also required that you add height: device-height to the viewport, which
causes issues with universal iPad apps.

Some of the things you can do :

- Have the keyboard shrink viewport without pushing it up (and animate in smoothly)
- Subscribe to jQuery events for native UIKeyboard event notifications (willShow, didShow, willHide, didHide)
- See if the keyboard is open or not
- Get the height of the keyboard (to accommodate different languages, etc)

# Installation

Assuming you're running Cordova 2.9+ and using the command line interface, you can install using:

    $ cd /path/to/project
    $ cordova plugin add https://github.com/mhweiner/CordovaiOSKeyboardPlugin

Note: At this time, jQuery is required for event handling. Feel free to replace the jQuery code with vanilla
javascript (or submit a pull request).
    
# Usage / API

The plugin creates a global variable called `Keyboard` when it is installed.

```js
// Have the keyboard resize the app instead of pushing it up
Keyboard.resizeApp(true);

// Have the keyboard push up the app (default behavior)
Keyboard.resizeApp(false);

// See if keyboard is open or not
var is_open = Keyboard.isOpen();

// Get height of open keyboard (including inputAccessoryView toolbar)
var height = Keyboard.getHeight();

// The following jQuery events are available:
// keyboardWillShow, keyboardDidShow, keyboardWillHide, keyboardDidHide

// Set callback
$('body').on('keyboardWillShow', myCallback);

// Remove callback
$('body').off('keyboardWillShow')

```

# Advanced Usage

From the time between keyboardWillShow to keyboardDidShow events, the viewport will be an extra 600 pixels taller to
make the animation smoother. So, if you're animating something using CSS bottom property, you must add an extra 600
pixels. Example:

```js

// Animate element up while keyboard is animating.
$('body').on('keyboardWillShow', function(){
    $('#bottomElement').animate({
        bottom: Keyboard.getHeight() + 600 //extra 600 pixels
    }, {
        duration: 300
    });
});

// Position back to 0 after keyboard is done animating.
$('body').on('keyboardDidShow', function(){
    $('#bottomElement').css({bottom:0});
});

```

# License

The MIT License

Copyright (c) 2013 Marc H. Weiner

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.

