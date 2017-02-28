# Microsites
A monorepo containing all Angular 2 microsites.

## Dependencies
* [Harp.js](http://harpjs.com/), a static web server with a built-in preprocessor.
* [Material Design Lite](https://getmdl.io/)
* [microsite-ui](https://github.com/ericjim/microsite-ui), a shared repo containing that contains all components shared among the microsites.

## Architecture
Angular.io has five microsites: Mobile, CLI, Universal, Protractor, and Material; all which exist under a *.angular.io subdomain. Each site consists of a hero with an image, some features with descriptions, a footer with helpful links, and a call-to-action. Depending on the status of the library/tool that the site advertises, the call-to-action will vary. For example, some sites will lead to the code base on GitHub (Material, Mobile, CLI), while the others take the viewer to their respective documentation (Universal, Protractor). Besides the slight variation, the intended theme for all microsites is to retain a similar look and behaviour. To accomplish this, we built `microsite-ui` as a shared resource among all the sites.

## Partials
The `microsite-ui` dependency contains [partials](https://harpjs.com/docs/development/partial) which are the building blocks of all of our microsites. Similarly, each microsite has its own set of partials which best suit what it is trying to convey. Generally speaking, any partial that can be reused should be in `microsite-ui`, and any partial that is specific to a microsite should be within its own folder outside of `microsite-ui`. Before javascript frameworks were a thing, the use of partials was used to render sites from the server-side. Since we are not presenting a site with much interactivity, the use of server-side rendering was preferred due to its improvements in SEO, and their overall ease of use.

## Installing dependencies
We have one external dependency, `harp.js`. It’s our server, our compiler, and our linter. Let’s install it.
```
> npm install -g harp  
```

## Quickstart
Run the following command:
```
> npm run clean-start
```

If you'd like to know more about the build, read the section "How to serve locally". Otherwise skip to Building and Hosting.

## How to serve locally
To begin develop our sites, we must first clone the `angular/microsites` repository locally.
```
> git clone git@github.com:angular/microsites.git  
```

Once cloned, choose a site to work on. Let’s pick Universal as an example
```
> cd universal.angular.io
```

We now need to get all the dependencies for Universal, namely `material-design-lite` and `microsite-ui`, so we will use npm to fetch them.
```
> npm install
```

If you inspect `package.json` you’ll notice that `microsite-ui` is not an NPM package.
```
…
  "dependencies": {
    "material-design-lite": "^1.1.3",
    "microsite-ui": "git://github.com/ericjim/microsite-ui.git"
  },
…
```
As a result, we’ll have to go to `node_modules/microsite-ui` and manually install all of its dependencies. NOTE: this is a pitfall we would like to fix. Possibly by assimilating `microsite-ui` into `angular/microsites`

```
> cd ./node_modules/microsite-ui
> npm install
```

Now that all of our packages have their sub dependencies, we can run a development server using `harp.js`. At the root of the chosen microsite (in this example, Universal). Run the following command:
```
> harp server
```

Unless you’ve changed the configuration port of `harp.js`, we can visit `localhost:9000` to preview the site.

## Building and Hosting
All of our microsites are hosted on Firebase. You should install their command-line tool to follow along:
```
> npm install -g firebase-tools
```

Since a `firebase.json` file already exists, we can see the name of a microsites bucket in Firebase; this is a unique name. As an example, here is how Universal’s file looks.
```  
{
  "firebase": "universal-angular-io",
  "public": "www",
  "ignore": [
    "firebase.json",
    "**/.*"
  ]
}  
```

### Building

Requirements:

- You need Node 4 to compile, Node 6 doesn't seem to work properly with Harp 0.23.0.
- Harp 0.20.1 is verified to work, but the latest Harp seems to not like some node_modules directories and even though it compiles, the resulting `www/` won't work.

**WARNING: Always works from a clean repo, then npm install the microsite and go to node_modules/microsites-ui and npm install that.**

Installing any other local packages (including harp) will create some problems in harp which scan all the directories to find templates.

You’ll notice that the official release app key on Firebase is `universal-angular-io`. Assuming you have access to the instance, you’ll be able to deploy everything under the `www` folder. To create this folder run `harp compile` from the root of the specific microsite folder.

>_There should be no errors_. Sometimes, you'll see the following error though:
>
>```json
{
  "source": "EJS",
  "dest": "HTML",
  "filename": "/Users/hansl/Sources/microsites/cli.angular.io/node_modules/microsite-ui/guides/_partials/guide.ejs",
  "lineno": 1,
  "name": "TypeError",
  "message": "/Users/hansl/Sources/microsites/cli.angular.io/node_modules/microsite-ui/guides/cli-setup.ejs:1\n >> 1| <%- partial('_partials/guide', {\n    2|     title: 'Page Title',\n    3|     selectedGuide: '../docs/1-cli-setup'\n    4| }) %>\n\nCannot read property 'docs' of undefined",
}
```
>
> The way to fix this is to open the `node_modules/microsites-ui/guides/_partials/guide.ejs` and replace line 3:
>
> ```javascript
> var docs = public.guides.docs._contents
> ```
> with the following:
>
> ```javascript
var docs = []
```
>
> I do not understand Harp and how it finds templates, but there seems to be no `guides` at all, while the folder is there and there are templates in it. It also seems that those guides are not used and were something being developed and left in an unfinished state. Removing these does not change the result.

`Harp.js` will prevent you from compiling a full site if any linting errors arise. At which point the platform will indicate where the issues are located. Once the site is compiled under `www`, you may deploy to Firebase using the following command:

```
> firebase deploy
```

And you may preview the site using:

```
> firebase open
```

*Hint: If you’d like to deploy a test instance of the microsite you are  working on, consider creating a new instance on Firebase and replacing the “firebase” key in `firebase.json`. Just be careful not to commit it.*

## Todos:
* [X] Add mobile
* [X] Add cli
* [X] Add universal
* [X] Add protractor
* [X] Add material
* [ ] How do we handle the shared code from [microsite-ui](https://github.com/ericjim/microsite-ui)? 
* [ ] Streamline build process
* [x] Document build process