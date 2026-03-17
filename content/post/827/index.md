---
title: "微信小程序canvasToTempFilePath:fail fail canvas is empty"
slug: "827"
date: 2024-09-10T23:14:00+08:00
lastmod: 2024-09-10T23:14:22+08:00
categories: ["技术"]
tags: ["微信小程序", "canvas"]
draft: false
---

## 说明

如题，最近在写一个小程序时，canvas报错，canvasToTempFilePath:fail fail canvas is empty，记录一下如何修复

### 步骤

首先吐槽一句，微信小程序官方文档真的不好，很多东西太旧也不修改，写的很乱，这个问题就是由于使用了错误的传参导致的。

这个报错是canvas为空，可能有几个原因

 1. canvas没有被正确的初始化，没有设置宽度高度；
 2. canvas画布内容没有被绘制
 3. canvasToTempFilePath方法没有正确的调用

本文遇到的问题就是第三个原因，话不多说，附上代码：

```js
  onReady() {
    const query = wx.createSelectorQuery();
    query
      .select("#signCanvas")
      .fields({ node: true, size: true, id: true })
      .exec((res) => {
        const canvas = res[0].node;
        if (!canvas) {
          console.error("Canvas not found");
          return;
        }

        this.canvasId = res[0].id; // 保存 canvasId
        this.ctx = canvas.getContext("2d");
        if (!this.ctx) {
          console.error("Failed to get canvas context");
          return;
        }

        const dpr = wx.getSystemInfoSync().pixelRatio;
        canvas.width = res[0].width * dpr;
        canvas.height = res[0].height * dpr;
        this.ctx.scale(dpr, dpr);

        console.log("Canvas initialized:", canvas);
        console.log("Canvas size:", canvas.width, canvas.height);
        console.log("Canvas ID:", this.canvasId);
        console.log("Canvas initialized, size:", canvas.width, "x", canvas.height);
      });
  },

  startDrawing(e) {
    const { x, y } = e.touches[0];
    this.setData({
      lastPoint: { x, y },
      isDrawing: true,
      hasDrawn: true, // 更新变量
    });
  },

  stopDrawing() {
    this.setData({
      isDrawing: false,
    });
  },

  moveDrawing(e) {
    if (!this.data.isDrawing) return;
    if (!this.ctx) {
      console.error("Canvas context is not initialized");
      return;
    }

    const { x, y } = e.touches[0];
    this.ctx.strokeStyle = this.data.color;
    this.ctx.lineWidth = this.data.thickness;
    this.ctx.lineJoin = "round";
    this.ctx.lineCap = "round";

    this.ctx.beginPath();
    this.ctx.moveTo(this.data.lastPoint.x, this.data.lastPoint.y);
    this.ctx.lineTo(x, y);
    this.ctx.stroke();

    this.setData({
      lastPoint: { x, y },
      hasDrawn: true,
    });
    console.log("Drawing at:", x, y, "with color", this.data.color, "and thickness", this.data.thickness);
  },

  clearCanvas() {
    const query = wx.createSelectorQuery();
    query
      .select("#signCanvas")
      .fields({ node: true, size: true })
      .exec((res) => {
        const canvas = res[0].node;
        if (!canvas) {
          console.error("Canvas not found");
          return;
        }

        const ctx = canvas.getContext("2d");
        if (!ctx) {
          console.error("Failed to get canvas context");
          return;
        }

        ctx.clearRect(0, 0, canvas.width, canvas.height);
        this.setData({
          hasDrawn: false, // 清空画布后更新变量
        });

        console.log("Canvas cleared");
      });
  },

  saveSignature() {
    console.log("Attempting to save signature. Canvas ID:", this.canvasId, "Has Drawn:", this.data.hasDrawn);
    if (!this.data.hasDrawn) {
      wx.showToast({
        title: "画布为空",
        icon: "none",
        duration: 2000,
      });
      return;
    }

    const query = wx.createSelectorQuery();
    query.select("#" + this.canvasId)
        .fields({ node: true, size: true })
        .exec((res) => {
            const canvas = res[0].node;
            if (!canvas) {
              console.error("Canvas not found");
              return;
            }

            console.log("Saving canvas with ID:", this.canvasId);

            wx.canvasToTempFilePath({
              canvas: canvas,
              success: (res) => {
                wx.showToast({
                  title: "保存成功",
                  icon: "success",
                  duration: 2000,
                });
                console.log("File saved at:", res.tempFilePath);
              },
              fail: (err) => {
                wx.showToast({
                  title: "保存失败",
                  icon: "none",
                  duration: 2000,
                });
                console.error("Save failed:", err);
              },
            });
        });
},
```

这是全部代码，其中，`wx.canvasToTempFilePath`方法新版canvas 2d传参是一个对象，不是字符串，也不是canvasId，官方文档介绍如下图：

![canvasToTempFilePath](https://cdn.rumosky.com/usr/uploads/2024/09/2745029731.png)

作为对比，我原来的代码如下：

```js
saveSignature() {
    if (!this.data.hasDrawn) {
      wx.showToast({
        title: "画布为空",
        icon: "none",
        duration: 2000,
      });
      return;
    }

    console.log("Saving canvas with ID:", this.canvasId);

    wx.canvasToTempFilePath({
      canvasId: this.canvasId,
      success: (res) => {
        wx.showToast({
          title: "保存成功",
          icon: "success",
          duration: 2000,
        });
        console.log("File saved at:", res.tempFilePath);
        console.log("canvasId", this.canvasId);
      },
      fail: (err) => {
        wx.showToast({
          title: "保存失败",
          icon: "none",
          duration: 2000,
        });
        console.error("Save failed:", err);
      },
    });
  },
```

原来传参是这样的`canvasId: this.canvasId,`，其实不需要这个参数，需要的是`canvas: canvas,`

现在就可以正确保存了

## 总结

代码里面已经包含了全部的调试语句，根据控制台输出可以判断错误点，请自行判断。