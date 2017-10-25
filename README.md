# iOS 11 scroll offset bug

I say iOS 11, I only noticed this bug after "fixing" a related bug, that as far as I'm aware is a result of a change in iOS 11, that affects web containers.

The related bug, a white strip where the status bar should be, first appeared to me when preparing a new release of [Flood Aware](http://flood-aware.k3r.me), for iOS 11. Flood Aware is built with [Cordova](https://cordova.apache.org/), and makes use of the [StatusBar](https://www.npmjs.com/package/cordova-plugin-statusbar) plugin. After building for iOS 11, the status bar became white and featureless. Turns out what was happening, was that the 20 pixels of web canvas, behind the status bar was now outside the default viewport. The white strip is actually the default white background of the html body, and can be affected by CSS. Though weirdly, only colour, not images.

The prevailing advice seemed to be to add `viewport-fit=cover` to the 'viewport' meta tag, which did give me back me
back my status bar, by extending the viewport back to the top of the screen. The fix unfortunately introduced a weird offset bug, which wasn't obvious until after I released the latest version of Flood Aware. And not that obvious, it took me a while to track down the cause, helped in part by this [Jira thread](https://issues.apache.org/jira/browse/CB-12886).

The offset bug manifests itself as a jump. A scrollable HTML element, decorated with `-webkit-overflow-scrolling: touch`, renders correctly initially. Then, after some interaction gains 20 pixels at the top of the element. Within Flood Aware, the easiest way to recreate the bug was by giving focus to a text input within a scrollable region. I've recreated the bug in the [master](https://github.com/kim3er/ios11-scroll-offset/tree/master) branch of this [repo](https://github.com/kim3er/ios11-scroll-offset). You can see below for an animated GIF of the bug, or follow the [install instructions](#install).

![Simulator example](https://d26dzxoao6i3hh.cloudfront.net/items/3b2U1U1Q1b420G3y1D3u/Screen%20Recording%202017-10-22%20at%2008.49%20pm.gif "Simulator example")

As you can see, as a result of the additional space, the cursor's position is skewed. Now I learnt a couple of things from the fore mentioned Jira ticket:

1. Looks like WebKit may have already identified and fixed the [bug](https://bugs.webkit.org/show_bug.cgi?id=175949). (Yay)
2. A suggestion that UIWebView is [deprecated](https://issues.apache.org/jira/browse/CB-12886?focusedCommentId=16151408&page=com.atlassian.jira.plugin.system.issuetabpanels:comment-tabpanel#comment-16151408), presumably by Apple, Cordova or both. (Not yay)

Switching Flood Aware to WKWebView isn't that big a deal, as it's a pretty simple app. But I think of at least one other that is going to give me some grief! C'est la vie.

## The fix

So, what have I got for you, by way of a fix? Three additional branches, that's what.

### [wkwebview](https://github.com/kim3er/ios11-scroll-offset/tree/wkwebview)

The same as master branch, but with the [cordova-plugin-wkwebview-engine](https://www.npmjs.com/package/cordova-plugin-wkwebview-engine) installed. There was some suggestion that problem would be resolved by doing this. In fact it is, but you get a bar a the bottom of the page instead.

### [realworld](https://github.com/kim3er/ios11-scroll-offset/tree/realworld)

After accepting that there are degrees to this problem, I decided to create a branch that more closely reflected the Flood Aware environment.

1. Flood Aware uses images for the background.
2. The starting postion did not include the viewport fix.

### [realworld-fix](https://github.com/kim3er/ios11-scroll-offset/tree/realworld-fix)

The money shot. 

## Install<a name="install"></a>

```
npm i
./node_modules/.bin/cordova platform add ios
./node_modules/.bin/cordova build ios
```