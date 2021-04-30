# Scrollbar.kak

This is a scrollbar for [*kakoune*](https://github.com/mawww/kakoune), the educated programmer's terminal editor of choice.

It uses the line-flagging feature and a compiled script to provide a real-time, smooth-as-silk scrollbar display. A limitation of this is that the scrollbar isn't a clickable UI element—you'll still have to roll your sleeves up and apply finger to keyboard to navigate around that document. This is kak, so you oughtta either be or get used to it!

This is **version 0.0.4**. The whole feature is—and will remain—somewhat experimental, and won't promise anything like a perfect experience, because it's not easy to implement as a plugin.

![Scrollbar image](https://i.ibb.co/kSsjsVj/scrollbar.png)

## See selections outside your current view

The scrollbar will show the locations of your selections as you make them, allowing you to see selections outside of your current view.

## Installation

Just put the `scrollbar.kak` into either your plugins or your autoload folder in kak's configuration directory. I highly recommend [plug.kak](https://github.com/andreyorst/plug.kak) to handle this for you.

## Using the scrollbar

Once you have scrollbar.kak and calc-scrollbar-kak installed, there is not much to do. Use the `scrollbar-enable` command to make it appear in the current window. You can set it for all windows in your `kakrc` like so:

```
hook global WinCreate .* %{ scrollbar-enable }
```

## Features & limitations

* The scrollbar can't display past the last line of the buffer, meaning that it will start to disappear as your view scrolls past the end of your document.

* In a window where one source line is soft-wrapped across two or more display lines, the scroll thumb size may be inaccurate, since it's not possible to put different flags on different display lines.

* The scrollbar is composed of simple terminal characters. Currently, it doesn't make use of any fancy tricks to make the display less granular than the height of a single character. 

* Some built-in kak commands mess with the flags list (scrollbar.kak's `line-spec` value), and there'll be a brief graphical glitch as one character-height of scrollbar is deleted and re-inserted. I haven't figured out a way to make the re-application apply more instantly.

### A built-in scrollbar for kakoune?

My next project is to write an update for kak's core that will allow you to build your own custom version of kak with 'baked-in' scrollbar functionality, and which won't have these limitations.

Hopefully it'll be possible to host this feature on the wonderful [hakoune](https://github.com/Delapouite/hakoune) project, a fork of kak with optional power features—for the most power-hungry of power users.

### Get in touch with more ideas

In the meantime, if you have any ideas as to how I can make `scrollbar-kak` more efficient in its execution, and thereby help it display even more smoothly, please let me know.

## Customization

There are a number of *face* options you can edit to customize your scrollbar's look:

`Scrollbar`: This sets the face used for the "track" of the scrollbar, all the portions of the buffer not currently visible.

`ScrollThumb`: This sets the face used for "thumb" of the scrollbar, the portion of the buffer currently visible.

In both cases, the background colour of the face paints the regular scrollbar appearance, while the foreground colour is used to mark the number of selections in that portion of the buffer (see the `selection_chars` option below). You probably want to set the `ScrollThumb` foreground to be neutral (you can already see the position of cursors on-screen) and the `Scrollbar` foreground to be bright and eye-catching, to remind you of the presence of off-screen cursors.

Here are a few examples of how you can change the scrollbar colours. Please enter `:doc faces` into kak for more information:

```kak
set-face global Scrollbar bright-yellow,white
set-face global ScrollThumb bright-black,black
```

You can change how selections are marked on the scrollbar with the following option:

`selection_chars`: A space-separated list of characters used as icons to represent the number of selections in a region of the buffer. For example, if the option is set to "0 1 2 M", then "0" will be displayed in portions of the scrollbar with no selections, "1" in portions with one selection, "2" in portions with two selections, and "M" in portions with three or more. If the option begins with a space, an actual space will be used to represent "no selections".

If you've set up the scrollbar and played about with adding new highlighters, you might want to push it back to its left-most position on the highlighter stack. To do so, use the `move-scrollbar-to-left` command.

## Integrating with Other Plugins

Most of the time, when the view changes it's because the user pressed a key, so we use Kakoune's `RawKey` hook to trigger redrawing the scrollbar. However, there are ways for the view to move without a keypress, like using `execute-keys` or the `:select` command. If you are the author of a plugin that uses such a technique, and you want the scrollbar to update automatically afterward without having to press a key, you can trigger the `view-scrolled` user hook:

    trigger-user-hook view-scrolled

## License

Oi! 'Ave you got a license for that scrollbar?

Actually, yes you have, because I'm releasing this under the MIT license. Though, to be honest, if you have grand ideas and want to use this elsewhere without the disclaimer or indeed under another license, just let me know and we can arrange it.
