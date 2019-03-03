---
layout: post
title: 小程序本地缓存模式
tags: 微信小程序
---

把页面数据缓存到本地可以避免重复访问远程服务器来获取数据，从而加快页面响应速度、改善用户体验。

## 缓存

```JavaScript
  /**
   * 生命周期函数--监听页面加载
   */
  onLoad: function(options) {
    // Initialize Page.data from local storage, if no local storage exists, request remote server
    wx.getStorage({
      key: this.getSectionStorageKey(),
      complete: (res) => {
        if (/^getStorage:fail.*/.test(res.errMsg)) {
          // Fetch lesson lesson if no local cache is available
          wx.request({
            url: `${conf.SERVER_URL}actions/lesson/`,
            data: {
            },
            success: (res) => {
              // Save section to local storage
              this.cacheLesson();
              // use response data to initialize Page.data
              initialize(res.data);
            }
          }); //End request data
        } else {
          // Use local cached data to initialize the data
          initialize(res.data);
        }
      },
    }); // End getStorage

    let initialize = (properties) => {
      this.setData({
          prop1: properties.prop1
          prop2: properties.prop2
      });

      wx.setNavigationBarTitle({
        title: properties.pageTitle;
      })
    }
  },

```

不需要使用缓存数据的时候调用`wx.clearStorage`清楚缓存