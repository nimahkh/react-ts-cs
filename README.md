## Development

### Testing

```
npm run test
```

### Building

```
npm run build
```

### Storybook

To run a live-reload Storybook server on your local machine:

```
npm run storybook
```

To export your Storybook as static files:

```
npm run storybook:export
```

You can then serve the files under `storybook-static` using S3, GitHub pages, Express etc. I've hosted this library at: https://www.harveydelaney.com/react-component-library

### Generating New Components

I've included a handy NodeJS util file under `util` called `create-component.js`. Instead of copy pasting components to create a new component, you can instead run this command to generate all the files you need to start building out a new component. To use it:

```
npm run generate YourComponentName
```

This will generate:

```
/src
  /YourComponentName
    YourComponentName.tsx
    YourComponentName.stories.tsx
    YourComponentName.test.tsx
    YourComponentName.types.ts
    YourComponentName.scss
```

The default templates for each file can be modified under `util/templates`.

Don't forget to add the component to your `index.ts` exports if you want the library to export the component!

## Publishing

### Hosting via NPM

First, make sure you have an NPM account and are [logged into NPM using the `npm login` command.](https://docs.npmjs.com/creating-a-new-npm-user-account)

Then update the `name` field in `package.json` to reflect your NPM package name in your private or public NPM registry. Then run:

```
npm publish
```

The `"prepublishOnly": "npm run build"` script in `package.json` will execute before publish occurs, ensuring the `build/` directory and the compiled component library exist.

### Hosting via GitHub

I recommend you host the component library using NPM. However, if you don't want to use NPM, you can use GitHub to host it instead.
You can then install your library into other projects by running:

```
npm i --save git+https://github.com/nimahkh/react-ts-cs.git#branch-name
```

OR

```
npm i --save github:nimahkh/react-ts-cs#branch-name
```

## Usage

Let's say you created a public NPM package called `react-component-library` with the `TestComponent` component created in this repository.

Usage of the component (after the library installed as a dependency into another project) will be:

```TSX
import React from "react";
import { TestComponent } from "react-component-library";

const App = () => (
  <div className="app-container">
    <h1>Hello I'm consuming the component library</h1>
    <TestComponent theme="primary" />
  </div>
);

export default App;
```

### Using Component Library SASS Variables

I've found that it's helpful to export SASS variables to projects consuming the library. As such, I've added the `rollup-plugin-copy` NPM package and used it to copy the `typography.scss` and `variables.scss` into the `build` directory as part of the Rollup bundle process. This allows you to use these variables in your projects consuming the component library.

For example, let's say you installed `react-component-library` into your project. To use the exported variables/mixins, in a SASS file you would do the following:

```Sass
@import '~react-component-library/build/typography';

.example-container {
    @include heading;

    color: $white;
}
```

## Additional Help

### Using Alternatives to Sass

#### Less or Stylus

The Rollup plugin `rollup-plugin-postcss` supports Sass, Less and Stylus:

- For Stylus, install stylus: `yarn add stylus --dev`
- For Less, install less: `yarn add less --dev`

You can then remove `node-sass` from your dependencies.

#### CSS Modules

If you want to use CSS Modules, update `postcss` in `rollup-config.js` to:

```
postcss({
  modules: true
})
```

#### Styled Components

If you want to use [`styled-components`](https://styled-components.com/), the changes required are a bit more involved. As such, I've created a branch where I've got `styled-components` working in this component library.

### Can I code split my components?

Yes you can.

Please note, there's an issue with code splitting and using `rollup-plugin-postcss`. I recommend using `rollup-plugin-sass` instead for code splitting.

### Supporting Images

Add the following library to your component library [@rollup/plugin-image](https://github.com/rollup/plugins/tree/master/packages/image):

```
npm i -D @rollup/plugin-image
```

Then add it to `rollup-config.js`:

```
...
plugins:[
  ...,
  image(),
  ...
]
...
```

You can then import and render images in your components like:

```tsx
import logo from "./rollup.png";

export const ImageComponent = () => <div>{logo}</div>;
```
