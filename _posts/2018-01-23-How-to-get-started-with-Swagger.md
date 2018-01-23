---
layout: post
title: How to get started with Swagger 
description: Install and run a swagger example project.
date: 2018-01-23 22:04:24+0100
date_modified: 2018-01-23 22:04:24+0100
categories: [swagger,rest,nodejs]
tags:
  - swagger
  - rest
  - nodejs
author: wechris
image:
  path: /img/2018-01-23-How-to-get-started-with-Swagger/image000.jpg
  width: 629
  height: 600
---
### Swagger

Swagger is the most widely used standard for specifying and documenting REST Services.

The real power of the Swagger standard comes from the ecosystem of powerful tools that surrounds it.

For example, there’s Swagger Editor for writing the Swagger spec, Swagger Codegen for automatically generating code based on your Swagger spec, and Swagger UI for turning your Swagger spec into beautiful documentation.

#### Installation

Swagger is supported in node.js through its own module, [here](https://github.com/swagger-api/swagger-node) is the link to the github project: The [documentation](https://github.com/swagger-api/swagger-node/tree/master/docs) is pretty straight-forward to help beginners understand and configure Swagger.

Once we open the command line we install the module (global).

``` shell
npm install -g swagger
````

Now let's create our project.

``` shell
swagger project create hello_world
````

The prompt asks for the framework we want to use, choose express.

{% include lightbox.html src="/img/2018-01-23-How-to-get-started-with-Swagger/image001.jpg" title="swagger" width="300" %}

{% include lightbox.html src="/img/2018-01-23-How-to-get-started-with-Swagger/image002.jpg" title="swagger" width="500" %}

Let's have a look at the project directory structure

```shell
├── api
├── config
├── node_modules
└── test
```

Each subfolder contains a README.md.

* **api**: As the name suggests, everything related to the API development, the controllers, mocks, helpers. A special mention goes to the /swagger folder which contains the file swagger.yaml, an important file we are going to edit to define everything related to the project information and routes. Throughout the tutorial I am going to explain it all so don't worry for now.
* **config**: It contains default.yaml that, as the documentation states, drives the application's config directory. Though we are not going to customize the file I still suggest you guys to read the related documentation to understand the engine beneath the "magic" of declaring APIs without writing a line of code but through yaml or json.
* **test**: Your test for controllers and helpers are (guess what!) created here.

The app.js is the main file which runs the server.

There is already a controller called hello_world.js? Whenever you create a new project, the module adds an example route, a GET request to /hello which takes a name as parameter and greets the person. Let see Swagger in action and get insight on how it works.

What about taking a look at the example in action?

#### Swagger editor.

[Swagger editor](http://editor.swagger.io/#/) is an elegant browser-based editor which really simplifies our efforts to develop a web API. In particular, it provides:

* Validation and endpoint routing.
* Docs on the fly generation.
* Easy-to-read Yaml.

{% include lightbox.html src="/img/2018-01-23-How-to-get-started-with-Swagger/image003.jpg" title="swagger" width="500" %}

The picture above shows you the UI of the Swagger editor. On the left you can see the yaml file, on the right the list of routes. By clicking on any root the UI shows detailed information like required parameters, the format of request and responses, more generally a description of the route.

Start the app by running

```shell
swagger project start
```

{% include lightbox.html src="/img/2018-01-23-How-to-get-started-with-Swagger/image004.jpg" title="swagger" width="500" %}

Whenever a file is modified, it automatically restarts the server.

Then, open a second shell and launch the editor with:

```shell
swagger project edit
```

The editor opens a new tab in your browser. On the right side of the page you should notice the example path for a GET request to <code>/hello</code>, so open the tab and, at the bottom, _click_ on the button try this operation.

{% include lightbox.html src="/img/2018-01-23-How-to-get-started-with-Swagger/image005.jpg" title="swagger" width="500" %}

{% include lightbox.html src="/img/2018-01-23-How-to-get-started-with-Swagger/image006.jpg" title="swagger" width="500" %}

#### Test it
Enter a paramenter name: Go for it and test the route. 
Result:

{% include lightbox.html src="/img/2018-01-23-How-to-get-started-with-Swagger/image007.jpg" title="swagger" width="500" %}

{% include lightbox.html src="/img/2018-01-23-How-to-get-started-with-Swagger/image008.jpg" title="swagger" width="500" %}