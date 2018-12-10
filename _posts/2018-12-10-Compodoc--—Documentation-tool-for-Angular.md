---
layout: post
title: Compodoc — Documentation tool for Angular 
description: Generate your Angular project documentation and analyse the code.
date: 2018-12-10 23:05:04+0100
date_modified: 2018-12-10 23:05:04+0100
categories: [Compodoc,documentation,Angular,typescript,Analyse]
tags:
  - Compodoc
  - documentation
  - Angular
  - typescript
  - Analyse
author: wechris
image:
  path: /img/2018-12-10-Compodoc---Documentation-tool-for-Angular/postpreview.jpg
  width: 629
  height: 600
---

# Compodoc

## What is Copodoc

Documentation generator for Angular Appliations. It generates a static documentation of your application.

The goal of the tool is to generate a documentation for all the common APIs of Angular : modules, components, injectables, routes, directives, pipes and classical classes.

## Get Compodoc

Homepage: [https://compodoc.app](https://compodoc.app)

## Installation

Install from npm : 

```
npm install -g @compodoc/compodoc
```

{% include lightbox.html src="/img/2018-12-10-Compodoc---Documentation-tool-for-Angular/image001.jpg" title="VirtualBox" width="400" %}

## Run

Define a script task for it in your package.json:

```s
"scripts": {
  "compodoc": "./node_modules/.bin/compodoc -p src/tsconfig.app.json"
}
````

{% include lightbox.html src="/img/2018-12-10-Compodoc---Documentation-tool-for-Angular/image002.jpg" title="VirtualBox" width="400" %}

Change to your project root folder
and run it like a normal npm script:

```
npm run compodoc
```

See [usage](https://compodoc.app/guides/usage.html) for more details.

{% include lightbox.html src="/img/2018-12-10-Compodoc---Documentation-tool-for-Angular/image003.jpg" title="VirtualBox" width="400" %}

After some logs in the console, you got the generated documentation in the ./documentation/ folder at the root of your project folder.

{% include lightbox.html src="/img/2018-12-10-Compodoc---Documentation-tool-for-Angular/image004.jpg" title="VirtualBox" width="400" %}

Open the index.html in your browser.

{% include lightbox.html src="/img/2018-12-10-Compodoc---Documentation-tool-for-Angular/image005.jpg" title="VirtualBox" width="400" %}
