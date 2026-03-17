---
title: "StarUML5.X-3.X破解方法"
slug: "68"
date: 2020-07-12T23:00:00+08:00
lastmod: 2020-07-12T23:00:00+08:00
categories: ["技术"]
tags: ["starUML"]
draft: false
---

## 说明

StarUML由2.0更新到3.0。原来的破解方法，修改license验证函数的方式不能用了。本文介绍如何破解5.0-3.0版本。

### 前言

[StarUML官网][1]

StarUML是用nodejs写的。确切的说是用Electron前端框架写的。新版本中所有的starUML源代码是通过asar工具打包而成。确切的代码位置在`C:\Program Files\StarUML\resources\app.asar`

#### 破解步骤

1.安装asar

```bash
npm install -g asar
```

2.解压app.asar

```bash
asar extract app.asar app
```

3.修改源代码

真正的验证license的代码在`app\src\engine\license-manager.js`

```js
/**
   * Check license validity
   *
   * @return {Promise}
   */
  validate () {
    return new Promise((resolve, reject) => {
      try {
        // Local check
        var file = this.findLicense()
        if (!file) {
          reject('License key not found')
        } else {
          var data = fs.readFileSync(file, 'utf8')
          licenseInfo = JSON.parse(data)
          var base = SK + licenseInfo.name +
            SK + licenseInfo.product + '-' + licenseInfo.licenseType +
            SK + licenseInfo.quantity +
            SK + licenseInfo.timestamp + SK
          var _key = crypto.createHash('sha1').update(base).digest('hex').toUpperCase()
          if (_key !== licenseInfo.licenseKey) {
            reject('Invalid license key')
          } else {
            // Server check
            $.post(app.config.validation_url, {licenseKey: licenseInfo.licenseKey})
              .done(data => {
                resolve(data)
              })
              .fail(err => {
                if (err && err.status === 499) { /* License key not exists */
                  reject(err)
                } else {
                  // If server is not available, assume that license key is valid
                  resolve(licenseInfo)
                }
              })
          }
        }
      } catch (err) {
        reject(err)
      }
    })
  }
```

这是个典型的javascirpt Promise。启动后会调用validate函数检查license。

```js
checkLicenseValidity () {
    this.validate().then(() => {
      setStatus(this, true)
    }, () => {
      // 原来的代码，如果失败就会将状态设置成false
      // setStatus(this, false) 
      // UnregisteredDialog.showDialog()

      //修改后的代码
      setStatus(this, true)
    })
  }
```

参照上面的代码将reject的callback原代码注释掉。换成`setStatus(this, true)`这样无论你注册与否都验证成功。

4.重新打包替换原来的app.asar

```bash
asar pack app app.asar
```

### 结束

替换完成之后，再次运行StarUML发现已经激活了！


  [1]: https://staruml.io/