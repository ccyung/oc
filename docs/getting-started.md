# Getting started

1. [Component creation](#component-creation)
  * [Component structure](#component-structure)
  * [Structure of the package](#structure-of-the-package)
  * [Template](#template)
  * [Basic server](#basic-server)
1. [Editing, debugging, testing](#editing-debugging-testing)
1. [Publishing to a registry](#publishing-to-a-registry)

## Component creation

To create a component you need to install oc with a command:

```js
npm install oc -g
```

The next step is to initialise a component:

```
oc init hello-world
```

The above command will create the `hello-world` [directory](#component-structure-details).

It is also possible to set [template](#template) type during the initialisation as an additional init parameter:

```
oc init hello-world jade
```

By default this parameter is set to `handlebars`.

### Component structure

The basic component directory is composed by three files:

```
├── hello-world/
│   ├── package.json
│   ├── template.html
│   ├── server.js
```

* `package.json` contains the component definition, dependencies, and [more](#structure-of-the-package)
* `template.html` is a [template](#template) containing the markup
* `server.js` is an optional [file](#basic-server), needed when the component has some logic

Additionally the component can have static contents such as images, js, and files that will be referenced in the html markup and any other files that will be useful for the development such as tests, docs, etc.

### Structure of the package

The basic package file `package.json` looks as follows:

```js
{
  "name": "hello-world",
  "description": "description of my hello-world component",
  "version": "1.0.0",
  "repository": "",
  "oc": {
    "files": {
      "data": "server.js",
      "template": {
        "src": "template.html",
        "type": "handlebars"
      }
    }
  }
}
```

* `name`: `string`, represents the name of the component, by default the name of initialised component
* `description`: `string`, represents the description of the component, by default an empty string
* `version`: `string`, represents the version of the component, by default `1.0.0`
* `repository`: `string`, TODO, by default an empty string
* `oc`: `object`, represents the data involved with the component
  * `files`: `object`, represents non-static component files
    * `data`: `string`, represents the filename of the model provider, by default `server.js`
    * `template`: `object`, represents the data involved with template - view, template engine
      * `src`: `string`, represents the filename of the view, by default template.html
      * `type`: `string`, represents the type of a template engine, by default `handlebars`

### Template

Template represents the view of the component. Currently we support `handlebars` and `jade`. It can contain css under the `<style>` tag and cliend-side javascript under the `<script>` tag.

Initialisation produces empty template file.

### Basic server

Server is the entity that produces the view-model to compile the view. It is necessary when component template has logic, including consuming services. The basic version of `server.js` looks as follows:

```js
'use strict';

module.exports.data = function(req, callback){
  callback(null, {});
};
```

## Editing, debugging, testing

You may want to start a local test registry using a components' folder as a library with a watcher. This will allow to consume and debug it:

```sh
oc dev . 3030
```

Then you may want to create a blank html page to start playing with it and see how it looks:

```html
<html>
  <body>
    <oc-component href="http://localhost:3030/hello-world">
      Optionally, some failover text here
    </oc-component>
    <script src="http://localhost:3030/oc-client/client.js"></script>
  </body>
</html>
```

That's it. As soon as you make changes on the component, you will be able to refresh this page and see how it looks.

## Publishing to a registry

You will need an online registry connected to a library to do that. The only requisite is that a component with the same name and version cannot be already existing on that registry.
```sh
# you have to do the registry config first, just once
oc registry add http://my-components-registry.mydomain.com

# then, ship it
oc publish hello-world/
```

Now, it should be there at `http://my-components-registry.mydomain.com/hello-world`.