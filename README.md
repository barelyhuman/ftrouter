<p align="center">
		<img width="125" src="docs/logo.svg">
</p>
<h1 align="center">ftrouter</h1>
<p align="center">A minimal file tree based api router for building rest api's with node</p>

### About

ftrouter started as a clone of the Next.js' Api Routes implmentation and is now on it's path to compete with other frameworks as the simplest way to setup API routes. There's been numerous posts on why using the folder tree makes it atomic and easier to handle the separation between logic. While you cannot bundle code with ftrouter since each file is independent of the other and doesn't need the others for its execution.

The Idea and Inspiration for the creation remains to be Vercel's Next.js

### Perks

-   Custom Port and Directory
-   Minimal so can be used with any bundler or process handler.
-   [Performance](#Performance) focused
-   Pre-compiled state for handling route requests

### Performance

Screenshot of `autocannon` to benchmark `/api` from the examples folder

![Performance Image](/docs/perf.png)

### Warning

This library is still in active development and is bound to have bugs , kindly make sure you use it only for testing and not for production as of now.

### Installation

#### Stable Cli

```sh
# for global install to avoid installing the devDependencies
npm i -g barelyhuman/ftrouter --only=prod
# for local install to avoid installing the devDependencies
npm i barelyhuman/ftrouter --only=prod

```

### Usage

You can run `ftrouter` in any folder and the `.js` files will be considered as routes.
The CLI considers the `api` folder to be the root and will pass down http `req,res` to the exported function.

Then go ahead and create directories and files under any folder as mentioned or check the `examples` folder for reference.

Example file tree:

We create a folder `example` you might want to call it something like `routes` and point `ftrouter` to it using `-d ./routes` to give you an http Server running for the files inside of the `routes` folder.

```
-example
  - api
    - me.js // this compiles to <host>:<port>/api/me
    - [id].js; // this compile to <host>:<port>/api/<dynamicParameterId>
```

Example `me.js` that only handles `GET` requests:

```
module.exports = (req, res) => {
  if(req.method === 'GET'){
    res.writeHead(200, { 'Content-Type': 'text/plain' });
    res.write('Hello World!');
    res.end();
    return;
  }

  res.statusCode = 404;
  res.end();
  return;
};


```

Example `[id].js` that handles the dynamic path param:

`GET|POST|DELETE /api/1`

```
module.exports = (req, res) => {
    res.write('path param ' + JSON.stringify(req.params)) // {"id":1};
    res.end();
};

```

Then run ftrouter on the root folder of the project, this folder should contain the `api` folder or specify a directory using the `-d` or `--dir` command.

```sh
# If installed globally
ftrouter -d ./example
# If installed locally
npx ftrouter -d ./example

```

### CLI Commands

-   `-d | --dir` to specify the directory to be used for routes, defaults to `api`
-   `-p | --port` to specify the port to start the http server on , defaults to `3000`

Example, the following would use the `example` folder as the base path for the routes and start the server on port `3001`

```sh
  ftrouter -d ./example -p 3001

```

### Custom Server

You can create a custom server to handle req,res manually by using the following example

```js
const app = require('ftrouter')
const http = require('http')
const path = require('path')

const PORT = process.env.PORT || 3000

app({
    basePath: path.join(process.cwd(), 'example'),
}).then((appHandler) => {
    http.createServer((req, res) => {
        appHandler(req, res)
    }).listen(PORT, () => {
        console.log('Listening on, ' + PORT)
    })
})
```

<a href="https://www.buymeacoffee.com/barelyhuman"><img src="https://img.buymeacoffee.com/button-api/?text=Buy me a coffee&emoji=&slug=barelyhuman&button_colour=000000&font_colour=ffffff&font_family=Inter&outline_colour=ffffff&coffee_colour=FFDD00"></a>
