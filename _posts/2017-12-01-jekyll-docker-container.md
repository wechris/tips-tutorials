---
layout: post
title: Jekyll Docker Container
description: A Jekyll Docker image to build and view github-pages on my MacOS X.
date: 2017-12-01 21:44:00+0100
date_modified: 
categories: [docker,jekyll]
tags:
  - docker
  - jekyll
  - github
author: wechris
image:
  path: /img/2017-12-01-jekyll-docker-container/octojekyll.jpg
  width: 629
  height: 600
---
# A Docker image for Github's Jekyll

A Jekyll Docker image to build and view github-pages on my MacOS X.
:whale:

## Introduction

Build and run your Jekyll github-pages in a Docker container.

Checkout the project and build the Docker image:
```bash
docker build -t "wechris-jekyll" .
```

Checkout the Repo of your github-page to a subfolder and copy the Gemfile into the folder.
Run the Docker Jekyll image:
```
docker run -v $(pwd)/<path github-page-repo>:/home/mypage -w /home/mypage -p 4000:4000 wechris-jekyll bundle exec jekyll serve --host 0.0.0.0
```

If you want to run the Jekyll container with an interactive shell
```
docker run -it -v $(pwd)/<path github-page-repo>:/home/mypage -w /home/mypage -p 4000:4000 wechris-jekyll /bin/sh
```

{% include lightbox.html src="/img/2017-12-01-jekyll-docker-container/jekyll-docker.jpg" title="Docker Jekyll" width="600" %}