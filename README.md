# iOS 11 scroll offset example

After iOS 11 was released, an app I work on encountered an issue whereby the status bar was replaced a white strip.
The prevailing advice seemed to be to add `viewport-fit=cover` to the 'viewport' meta tag, which did give me back me
back my status bar. But, the fix introduced a weird offset bug with scrollable regions within the page.

The bug adds a 20px offset to a scrollable region. The scrollable region renders correctly, the offset only appears after interaction with the element. This repo demonstrates the interaction, using a text box.

I have a potential workaround, using wkwebview, that can be seen [here](https://github.com/kim3er/ios11-scroll-offset/tree/wkwebview).

## Install

```
npm i
./node_modules/.bin/cordova platform add ios
./node_modules/.bin/cordova build ios
```

## Examples

![Simulator example](https://d26dzxoao6i3hh.cloudfront.net/items/3b2U1U1Q1b420G3y1D3u/Screen%20Recording%202017-10-22%20at%2008.49%20pm.gif "Simulator example")

[iPhone 6 MP4](./iphone6.mp4)