---
layout: post
title: 递归调用JavaScript Promise
tags: JavaScript, 微信小程序
---

递归调用JavaScript Promise的例子

## 连续重定向网页

```JavaScript
function getRedirectUrl(url, redirectCount) {
    redirectCount = redirectCount || 0;
    if (redirectCount > 10) {
        throw new Error("Redirected too many times.");
    }

    return new Promise(function (resolve) {
        var xhr = new XMLHttpRequest();

        xhr.onload = function () {
            var redirectsTo;

            if (xhr.status < 400 && xhr.status >= 300) {
                redirectsTo = xhr.getResponseHeader("Location");
            } else if (xhr.responseURL && xhr.responseURL != url) {
                redirectsTo = xhr.responseURL;
            }

            resolve(redirectsTo);
        };

        xhr.open('HEAD', url, true);
        xhr.send();
    })
    .then(function (redirectsTo) {
        return redirectsTo
            ? getRedirectUrl(redirectsTo, redirectCount+ 1)
            : url;
    });
}
```

## 小程序顺序播放音频

```JavaScript
    /**
     * play audio
     */
    playAudio: function(items) {
      return new Promise(resolve => {
        // Move element from head of the array and process it
        let cElement = items.shift();
        let audioCtx = wx.createInnerAudioContext();
        audioCtx.src = cElement[0];
        audioCtx.play();

        // enable the button upon audio finished
        audioCtx.onEnded(() => {
          setTimeout(() => {
            let aPlayedItems = this.data.aPlayedItems;
            aPlayedItems.push(cElement[1]);
            this.setData({
              aPlayedItems
            });
            // Recursively paly audio
            resolve(items);
          }, 1000);
        });
      }).then(items => {
        if (items.length !== 0)
          return this.playAudio(items);
      });
    },
```