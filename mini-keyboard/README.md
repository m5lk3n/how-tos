# How to configure a Mini Keyboard (for Windows 10/11) on Windows 11

## Disclaimer

*I'm not responsible for external links! Use at your own risk!*

## Intro

I recently got myself a mini keyboard with 3 keys and 1 knob ([here](https://github.com/kriomant/ch57x-keyboard-tool/blob/master/doc/keyboard-3-1.jpg)'s an example picture of the model that I have).
My goal was to configure it for a Windows 10 laptop to easily switch desktops and to zoom in/out turning the knob. In my setup, the device is placed "horizontally" to the left of my normal keyboard, with its know to the right. I use the mini keyboard with my left hand.

## Setup

Using my Windows 11 gaming PC and with the "Setting Software" [Update-20230714](https://cdn.shopify.com/s/files/1/0655/8570/9299/files/Update-20230714.zip?v=1689644183) installed from [this](https://sikaicase.com/blogs/support/setting-for-software) page, the plugged in mini keyboard immediately connected.

Here's a [screenshot](screenshot.png) of the connected device showing the programming knob's right turn.

Programming is done by clicking

1. on a green button (see below)
2. on the desired key stroke on the virtual keyboard
3. "Download"

As it turned out, the green buttons for my device are defined as follows:

- "KEY1" maps to the key farthest away from the knob. I assigned `Ctrl Win Left` to it which means "switch to the left desktop".
- "KEY2" maps to the middle key. I assigned `Win Tab` to it which opens the task viewer with an overview of desktops to switch to.
- "KEY3" maps to the key closest to the knob. I assigned `Ctrl Win Right` to it which means "switch to the right desktop".
- "K1 Left" maps to the knob's left turn. I assigned `Win -` to it to zoom out.
- "K1 Centre" maps to the knob press. I assigned `Win Esc` to it which means reset any zoom.
- "K1 Right" maps to the know's right turn. I assigned `Win +` to it to zoom in.

This is all on Layer1; I haven't tried the others.

## Failed Attempts

I tried the following alternatives to no avail:

1. [ch57x-keyboard-tool](https://github.com/kriomant/ch57x-keyboard-tool):
According to Window's device manager, my keyboard has the following: 

- vendor ID: 1189
- product ID: 8890

Even with these IDs explicitely passed to the tool, the device wasn't recognized. I don't know why, but it kept on showing this [issue](https://github.com/kriomant/ch57x-keyboard-tool/issues/3).

2. With "[The Newest Software](https://cdn.shopify.com/s/files/1/0655/8570/9299/files/New_Software.7z?v=1689124275)" from [aforementioned](https://sikaicase.com/blogs/support/setting-for-software) page, the device connection could not be established.
