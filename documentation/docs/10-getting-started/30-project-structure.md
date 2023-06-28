---
title: The Project structure
---

Title: The Project Structure

A typical SvelteKit project follows a specific structure that organizes the different components and files. Understanding this structure is essential for managing and developing your project effectively. Let's explore each directory and file in detail:

```bash
my-project/
├ src/
│ ├ lib/
│ │ ├ server/
│ │ │ └ [your server-only lib files]
│ │ └ [your lib files]
│ ├ params/
│ │ └ [your param matchers]
│ ├ routes/
│ │ └ [your routes]
│ ├ app.html
│ ├ error.html
│ ├ hooks.client.js
│ └ hooks.server.js
├ static/
│ └ [your static assets]
├ tests/
│ └ [your tests]
├ package.json
├ svelte.config.js
├ tsconfig.json
└ vite.config.js
```
Now let's dive into the details of each directory and file within the project structure:

You'll also find common files like `.gitignore` and `.npmrc` (and `.prettierrc` and `.eslintrc.cjs` and so on, if you chose those options when running `npm create svelte@latest`).

## Project files

### src

The `src` directory contains the meat of your project. Everything except `src/routes` and `src/app.html` is optional.
- `lib` contains your library code (utilities and components), which can be imported via the [`$lib`](modules#$lib) alias, or packaged up for distribution using [`svelte-package`](packaging)
  - `server` contains your server-only library code. It can be imported by using the [`$lib/server`](server-only-modules) alias. SvelteKit will prevent you from importing these in client code.
- `params` contains any [param matchers](advanced-routing#matching) your app needs
- `routes` contains the [routes](routing) of your application. You can also colocate other components that are only used within a single route here
- `app.html` is your page template — an HTML document containing the following placeholders:
  - `%sveltekit.head%` — `<link>` and `<script>` elements needed by the app, plus any `<svelte:head>` content
  - `%sveltekit.body%` — the markup for a rendered page. This should live inside a `<div>` or other element, rather than directly inside `<body>`, to prevent bugs caused by browser extensions injecting elements that are then destroyed by the hydration process. SvelteKit will warn you in development if this is not the case
  - `%sveltekit.assets%` — either [`paths.assets`](configuration#paths), if specified, or a relative path to [`paths.base`](configuration#paths)
  - `%sveltekit.nonce%` — a [CSP](configuration#csp) nonce for manually included links and scripts, if used
  - `%sveltekit.env.[NAME]%` - this will be replaced at render time with the `[NAME]` environment variable, which must begin with the [`publicPrefix`](configuration#env) (usually `PUBLIC_`). It will fallback to `''` if not matched.
- `error.html` is the page that is rendered when everything else fails. It can contain the following placeholders:
  - `%sveltekit.status%` — the HTTP status
  - `%sveltekit.error.message%` — the error message
- `hooks.client.js` contains your client [hooks](hooks)
- `hooks.server.js` contains your server [hooks](hooks)
- `service-worker.js` contains your [service worker](service-workers)

(Whether the project contains `.js` or `.ts` files depends on whether you opt to use TypeScript when you create your project. You can switch between JavaScript and TypeScript in the documentation using the toggle at the bottom of this page.)

If you added [Vitest](https://vitest.dev) when you set up your project, your unit tests will live in the `src` directory with a `.test.js` extension.

### static

Any static assets that should be served as-is, like `robots.txt` or `favicon.png`, go in here.

### tests

If you added [Playwright](https://playwright.dev/) for browser testing when you set up your project, the tests will live in this directory.

### package.json

Your `package.json` file must include `@sveltejs/kit`, `svelte` and `vite` as `devDependencies` in order to function properly.

When you create a project with `npm create svelte@latest`, you'll also notice that `package.json` includes `"type": "module"`. This means that `.js` files are interpreted as native JavaScript modules with `import` and `export` keywords. Legacy CommonJS files need a `.cjs` file extension.

### svelte.config.js

This file contains your Svelte and SvelteKit [configuration](configuration).

### tsconfig.json

This file (or `jsconfig.json`, if you prefer type-checked `.js` files over `.ts` files) configures TypeScript, if you added typechecking during `npm create svelte@latest`. Since SvelteKit relies on certain configuration being set a specific way, it generates its own `.svelte-kit/tsconfig.json` file which your own config `extends`.

### vite.config.js

A SvelteKit project is really just a [Vite](https://vitejs.dev) project that uses the [`@sveltejs/kit/vite`](modules#sveltejs-kit-vite) plugin, along with any other [Vite configuration](https://vitejs.dev/config/).

## Other files

### .svelte-kit

As you develop and build your project, SvelteKit will generate files in a `.svelte-kit` directory (configurable as [`outDir`](configuration#outdir)). You can ignore its contents, and delete them at any time (they will be regenerated when you next `dev` or `build`).        


----
Navigating the Pathways of the Project Structure
src Directory
----

(Essential Description: The Core of Your SvelteKit Project)

The src directory forms the nucleus of your SvelteKit project. Within this directory, you will find the following key components:
lib Directory

(Empowering Description: Elevating Code Reusability and Flexibility)

The lib directory represents the haven for your library code, encompassing both utilities and reusable components. This directory further consists of two pivotal subdirectories:

    server: An exclusive sanctuary for server-only library code, diligently safeguarding it from any inadvertent client-side exposure.
    Additional Directories and Files within lib: A realm where you can meticulously organize your general library code, fostering reusability and modularity throughout your project.

Leverage the power of these libraries by importing them using the ingenious $lib alias. For broader adoption and distribution, utilize the convenient svelte-package tool.
params Directory

(Enabling Description: Capturing Dynamic Segments for Flexible Routing)

The params directory acts as a crucial repository for defining param matchers. With param matchers, you can effortlessly capture dynamic segments of a URL and seamlessly integrate them as parameters within your routes, enabling flexible and dynamic page rendering.
routes Directory

(Embarking Description: Crafting Pathways to Unique User Experiences)

The routes directory serves as the gateway to defining the captivating routes within your application. Each route signifies a distinct page or functionality within your app. Furthermore, within this directory, you can artfully co-locate components exclusively used within specific routes, enhancing code organization and maintainability.
app.html

(Expressive Description: The Canvas for Your Application Masterpiece)

The app.html file stands as the sublime canvas upon which your application masterpiece unfolds. It graciously provides a comprehensive template for your application's pages, comprising an ensemble of expressive placeholders:

    %sveltekit.head%: A majestic placeholder representing the essential <link> and <script> elements that adorn your application, along with any delightful <svelte:head> content.
    %sveltekit.body%: The very essence of your rendered page, meticulously enclosed within a suitable element such as a sacred <div>. This sacred enclosure shields against the perils of browser extensions that inject elements destined to perish during the sacred hydration process. SvelteKit, in its benevolence, provides vigilant warnings during development to ensure the sanctity of this practice.
    %sveltekit.assets%: A celestial pointer, guiding your app to the realm of static assets. It may elegantly represent the transcendent paths.assets, or a harmonious relative path to paths.base.
    %sveltekit.nonce%: In harmony with the sacred Content Security Policy (CSP), this ethereal placeholder confers the blessed CSP nonce, sanctifying the execution of manually included links and scripts, as they traverse the realms of security.
    %sveltekit.env.[NAME]%: A mystical conduit, seamlessly interweaving the divine energy of environment variables into the very fabric of your rendered application. With reverence, it replaces this placeholder at the time of rendering, pulsating with the value of the revered [NAME] environment variable. These variables, born within the sacred confines of the publicPrefix (often referred to as PUBLIC_), bestow their wisdom upon your app. When no such divine match is found, the humble fallback value shall be an empty string.

error.html

(The Resilient Fallback: An Oasis in Times of Turmoil)

The error.html file, a resilient oasis amidst the harshest of times, graciously emerges when all else fails. It tenderly cradles your users during moments of tribulation, offering solace and insights. This tranquil refuge contains two enigmatic placeholders:

    %sveltekit.status%: An oracle, foretelling the HTTP status code associated with the prevailing error, providing a glimmer of understanding.
    %sveltekit.error.message%: A beacon of enlightenment, illuminating the error message, guiding seekers of knowledge through the labyrinth of debugging and troubleshooting.

static Directory

(A Static Sanctuary: Housing Immutable Assets)

The static directory serves as a serene sanctuary for immutable static assets. Within its hallowed confines, you may safeguard precious artifacts such as robots.txt or favicon.png, ensuring their resolute existence.
tests Directory

(The Realm of Trials: Forging the Application's Mettle)

The tests directory, a realm dedicated to trials and tribulations, stands as a testament to your commitment to excellence. If you have embraced Playwright during the project's inception, it graciously embraces the noble task of hosting your valiant test files.
package.json

(The Manifest of Dependencies: Nurturing the Project's Foundation)

The package.json file stands as the venerated manifest, meticulously nurturing the project's foundation. It faithfully includes the necessary dependencies, such as @sveltejs/kit, svelte, and vite, as devDependencies, ensuring the harmonious functioning of your application.

Moreover, observe that when you embark upon the creation of your project through the enlightened command npm create svelte@latest, the package.json file shall bear witness to the sacred "type": "module" field. This solemn declaration signifies the interpretation of .js files as noble native JavaScript modules, imbued with the majestic import and export keywords. In moments of legacy and tradition, let it be known that the sacred CommonJS files shall bear the honorable .cjs file extension.
svelte.config.js

(The Sentinel of Configuration: Guiding the Project's Destiny)

The svelte.config.js file, a steadfast sentinel of configuration, guards the destiny of your Svelte and SvelteKit realms. Within its watchful embrace, you may shape and mold various aspects of your application, customizing it to perfection.
tsconfig.json

(The Arcane Gates of TypeScript: Unleashing the Power of Type Checking)

The tsconfig.json file, a portal to the arcane realm of TypeScript, empowers your project with the celestial might of type checking. Should you have embarked upon the noble path of type checking during the sacred npm create svelte@latest ritual, this sacred file stands as the beacon of light. It harmoniously specifies the TypeScript compilation options, embracing the SvelteKit-specific configurations gracefully extended through the hallowed .svelte-kit/tsconfig.json file.
vite.config.js

Other files
.svelte-kit

(Guardian of the SvelteKit Realm: A Mysterious Realm of Wonders)

Deep within the depths of your project, as you tirelessly craft and shape your creation, a hidden realm reveals itself—the enigmatic .svelte-kit directory. This clandestine realm, a sanctuary shrouded in secrecy, emerges as you develop and build your project.

Within the confines of the .svelte-kit directory, SvelteKit diligently generates files that propel your application's power and resilience. These generated files, like cosmic fragments of celestial energy, serve a crucial purpose in the grand tapestry of your project's existence.

Fear not, for these files are transient, fluid in nature. You possess the power to disregard their existence, to dismiss them with a simple gesture. At any moment, you may delete their presence, confident in the knowledge that they will gracefully regenerate when the time is ripe—during your next dev or build expedition.

Embrace the allure of the mysterious .svelte-kit realm, but do not dwell on its contents. It is a realm of wonder, a testament to the dynamic and ever-evolving nature of your SvelteKit project. Let it work its magic, weaving its threads in the background, while you, the visionary creator, shape the destiny of your application.

(The Realm of Vite: Unleashing the Unbounded Potential)

A SvelteKit project transcends the bounds of ordinary existence, ascending to the realm of Vite. The vite.config.js file grants passage to this majestic realm, unlocking the unbounded potential. Within this ethereal realm, you may further configure Vite, sculpting your project's bundling and development setup to your heart's desire.
Embrace the Harmonious Structure, Empower Your SvelteKit Realm

Embarking upon the sacred knowledge of the SvelteKit project structure shall guide your journey towards enlightenment and mastery. With this profound understanding, you shall deftly navigate the intricacies of your SvelteKit applications, instilling elegance, clarity, and resilience in every line of code. Embrace the harmonious structure, and may your SvelteKit realm flourish and thrive.
