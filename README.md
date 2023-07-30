# 0.1

__NOT-TIMID Alpha 0.1: A minimal 'Coming Soon' page, bootstrapped with `npm init vue@latest` 3.3.4.__

-&nbsp; __Version:__ 0.1.0  
-&nbsp; __Repo:__ <https://github.com/not-timid/0.1>  
-&nbsp; __Homepage:__ <https://not-timid.com/0.1/>

## Recommended IDE setup

[VSCode](https://code.visualstudio.com/) + [Volar](https://marketplace.visualstudio.com/items?itemName=Vue.volar) (and disable Vetur) + [TypeScript Vue Plugin (Volar)](https://marketplace.visualstudio.com/items?itemName=Vue.vscode-typescript-vue-plugin).

## Type support for `.vue` imports in TS

TypeScript cannot handle type information for `.vue` imports by default, so we replace the `tsc` CLI with `vue-tsc` for type checking. In editors, we need [TypeScript Vue Plugin (Volar)](https://marketplace.visualstudio.com/items?itemName=Vue.vscode-typescript-vue-plugin) to make the TypeScript language service aware of `.vue` types.

If the standalone TypeScript plugin doesn't feel fast enough to you, Volar has also implemented a [Take Over Mode](https://github.com/johnsoncodehk/volar/discussions/471#discussioncomment-1361669) that is more performant. You can enable it by the following steps:

1. Disable the built-in TypeScript Extension
    1) Run `Extensions: Show Built-in Extensions` from VSCode's command palette
    2) Find `TypeScript and JavaScript Language Features`, right click and select `Disable (Workspace)`
2. Reload the VSCode window by running `Developer: Reload Window` from the command palette.

## Project setup

```sh
npm install
```

### Compile and hot-reload for development

```sh
npm run dev
```

### Type-check, compile, minify and serve locally for production

```sh
npm run start
```

### Run unit tests with [Vitest](https://vitest.dev/)

```sh
npm run test:unit
```

### Run end-to-end tests with [Playwright](https://playwright.dev)

```sh
# Install browsers for the first run
npx playwright install

# When testing on CI, must build the project first
npm run build

# Runs the end-to-end tests
npm run test:e2e
# Runs the tests only on Chromium
npm run test:e2e -- --project=chromium
# Runs the tests of a specific file
npm run test:e2e -- tests/example.spec.ts
# Runs the tests in debug mode
npm run test:e2e -- --debug
```

### Lint with [ESLint](https://eslint.org/)

```sh
npm run lint
```

## How to create a project like this

### Create an initial Vue 3 app, with deep linking provided by 404.html

1. Create a GitHub repo named `0.1`
2. In Settings -> Pages -> Branch choose 'main' and '/docs'
3. `git clone git@github.com:<GITHUB_USERNAME>/0.1.git && cd 0.1`
4. Follow the ['Creating a Vue Application' Quick Start:
   ](https://vuejs.org/guide/quick-start.html#creating-a-vue-application)
   ```
   npm init vue@latest

   Need to install the following packages:
     create-vue@3.7.1
   Ok to proceed? (y) 
   
   Vue.js - The Progressive JavaScript Framework
   
   ✔ Project name: … 0.1
   ✔ Add TypeScript? … Yes
   ✔ Add JSX Support? … No
   ✔ Add Vue Router for Single Page Application development? … Yes
   ✔ Add Pinia for state management? … Yes
   ✔ Add Vitest for Unit Testing? … Yes
   ✔ Add an End-to-End Testing Solution? › Playwright
   ✔ Add ESLint for code quality? … Yes
   ✔ Add Prettier for code formatting? … Yes
   
   Scaffolding project in /Users/ ... /0.1...
   ```
5. Move everything apart from the README.md out of the newly created folder
   into the top level of the repo, and delete that folder (note that there are
   invisible items, whose filenames start with a '.')
6. In package.json, correct the version and private values, and add description,
   repository, author, license, bugs and homepage
7. Also in package.json:
   - Append ` --ignore-pattern /docs/` to the `lint` script
   - Add `"poststart": "mv 0.1 docs",`
   - Add `"start:build": "npm run build && cp 404.html docs/404.html",`
   - Add `"start:clean": "rm -rf docs && rm -rf 0.1",`
   - Add `"start:serve": "mv docs 0.1 && static-server -p 5173 -n 0.1/404.html",`
8. `npm install` installs 426 packages with 0 vulnerabilities:  
   182.6 MB for 11,865 items
9. Add `base: '/0.1/',` and `build: { outDir: 'docs' },` to vite.config.ts
10. `npm run format && npm run dev`  
    and open <http://127.0.0.1:5173/0.1/about>  
    Note that deep linking works out of the box for `npm run dev`, but we will
    need to add a special 404.html file to support deep linking to static
    servers like GitHub Pages.
11. Control-C to stop `npm run dev`, and then `touch 404.html` and paste in:
    ```html
    <!DOCTYPE html>
    <html lang="en">
      <head>
        <meta charset="UTF-8">
        <link rel="icon" href="/tryout-vue3-threejs-drie/favicon.ico">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>NOT-TIMID</title>
      </head>
      <body>
        <script>!function(base){ // IIFE which redirects deep links to index.html
          sessionStorage.setItem(
            '404_REFERRER',
            '/' + // don't use a 'template literal' - the browser may predate ES6
              location.href.split(base)
                .slice(1)
                .join(base)) // just in case the route also contains `base`
          location.href = base
        }('/0.1/')</script>
      </body>
    </html>
    ```
12. Add the following above `app.mount('#app')` in src/main.ts:
    ```ts
    // Detect forwarding from a deep link, via 404.html.
    const REFERRER_KEY = '404_REFERRER'
    const referrerPath = sessionStorage.getItem(REFERRER_KEY)
    if (referrerPath !== null) {
      sessionStorage.removeItem(REFERRER_KEY) // keep sessionStorage tidy
      const url = new URL(referrerPath, 'x:/') // developer.mozilla.org/docs/Web/API/URL
      router.replace({
        hash: url.hash, // eg '#scroll-to-some-id'
        path: url.pathname, // eg '/item/123'
        query: Object.fromEntries( // eg '?a=1&b=c' -> { abc: "1", b: "c" }
          new URLSearchParams(url.search)),
      })
    }
    ```
13. Assuming you have the NPM module `static-server` installed globally,  
    `npm start` and refresh <http://127.0.0.1:5173/0.1/about>  
    You should see a red `404 /0.1/about/` log in you console, and a 404
    response in the ‘Network’ tab of your browser's developer tools.  
    The page should redirect to a `200 OK` response at /0.1/ and the browser
    should show Vue's 'This is an about page'.
14. Control-C to stop `static-server`, which should `mv 0.1 docs` ready for
    GitHub Pages

### Add a placeholder Three.js scene, using Drie

1. Follow the [Drie Getting Started Guide
   ](https://www.drie.dev/guide/getting-started) to install dependencies
   @janvorisek/drie, three and dev-dependency @types/three:  
   10 packages with 0 vulnerabilities, 49.2 MB for 1,999 items
2. Use the [Drie plugin
   ](https://www.drie.dev/guide/getting-started#vue-3-plugin) by just adding  
   `import drie from '@janvorisek/drie'` and  
   `app.use(drie)` to src/main.ts
3. Replace src/views/HomeView.vue with the [Drie 'Basic setup' example
   ](https://www.drie.dev/examples/basic-setup#code)  
   (the `import`s are not needed, because of `app.use(drie)` in src/main.ts)
4. You should see the blue box, and be able to rotate it by dragging

<!-- 145,886,189 bytes (182.6 MB on disk) for 11,865 items -->
<!-- 189,006,161 bytes (231.8 MB on disk) for 13,864 items -->
