---
title: "PicGo image compression plugin"
slug: "882"
date: 2026-03-20T11:46:00+08:00
lastmod: 2026-03-20T11:46:00+08:00
categories: ["技术"]
tags: ["linux", "Windows"]
draft: false
---

## Introduction

Recently, I've been using picgo as a tool for managing images. I've tried the compress1.4 and compress-next1.5.2 versions in the plugin marketplace, as well as the tinypng plugin, but none of them worked properly. I'm not sure why, so I wrote a plugin myself.

## Usage

![Plugin icon](https://cdn.rumosky.com/usr/uploads/2026/03/202603208736.png)

[picgo-plugin-compress-pro](https://github.com/rumosky/picgo-plugin-compress-pro)

1. Supports **local compression** and **online compression**.

Local compression uses two libraries, `pngquant` and `jpegoptim`, while online compression uses tinypng.

2. Supports compression of the following image formats:

- PNG (compressed using pngquant)
- JPG/JPEG (compressed using jpegoptim)
- TinyPNG (supports compression of PNG and JPG formats through the TinyPNG API)

## Usage

![Settings](https://cdn.rumosky.com/usr/uploads/2026/03/202603208713.png)

1. To select the local mode, please set the paths for `pngquant` and `jpegoptim`. Remember to append `/` at the end.
2. To select the online mode, please set the API Key for tinypng.
