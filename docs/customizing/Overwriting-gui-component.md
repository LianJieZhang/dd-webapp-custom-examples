# Overwriting a GUI component

In this tutorial we'll update the application to be using an overwritten GUI component.

The entire GUI exists out of components, a component is a reusable piece within the UI which can be used in one or many places on the screen.

> You can find the code of this tutorial [here](../../custom-webapp/gui/src)

## Prerequisites

1. You've completed the [getting started guide](../Getting-started.md)
2. You've completed the [Creating a custom home page](./Creating-custom-homepage.md) tutorial
3. You are familiar with [TypeScript](https://egghead.io/courses/up-and-running-with-typescript), [JavaScript (ES6)](https://egghead.io/courses/learn-es6-ecmascript-2015), [Webpack](https://webpack.js.org/) and [React](https://egghead.io/courses/react-fundamentals)

## Starting the application in debug mode

See [Starting the application in debug mode](./Change-the-skin.md#starting-the-application-in-debug-mode).

## Creating a custom breadcrumbs component

In this tutorial I'll update the breadcrumbs to use a custom url for the home page.
I'll be using the url I've created in the [Creating a custom home page](./Creating-custom-homepage.md) tutorial.

1. Create a new component file inside the `src/custom-components` directory called `Breadcrumbs.tsx`

```typescript
import * as React from "react";
import { Link } from "react-router";
import { Utils } from "@sdl/delivery-ish-dd-webapp-gui";
import { Breadcrumbs as BreadcrumbsBase } from "@sdl/dd/base/presentation/Breadcrumbs";

const { path } = Utils.Path;

export class Breadcrumbs extends BreadcrumbsBase {
    public render(): JSX.Element {
        /**
         * See source implementation of render method to know what jsx is created:
         * node_modules/@sdl/delivery-ish-dd-webapp-gui/src/components/presentation/Breadcrumbs.tsx
         *
         * In this customization we've overwritten the Link component used for the Home item (className="home")
         *
         * <div className={"sdl-dita-delivery-breadcrumbs"}>
         *     <ul>
         *         <li>
         *             <Link className="home" title={homeLabel} to={`${path.getRootPath()}home`}>{homeLabel}</Link>
         *             <span className="separator" />
         *         </li>
         *         ...
         *     </ul>
         * </div>
         */
        const divComp = super.render();
        if (divComp.props && divComp.props.children) {
            const ulComp = divComp.props.children;
            if (ulComp.props && Array.isArray(ulComp.props.children)) {
                const liComp = ulComp.props.children[0];
                if (liComp.props && Array.isArray(liComp.props.children)) {
                    const liCompChilds = liComp.props.children as { props: { to: string, children: string } }[];
                    const homeLinkComp = liCompChilds[0];
                    const props = { ...homeLinkComp.props };
                    props.to = path.getRootPath() + "productfamilylist";
                    liCompChilds[0] = <Link {...props}>{props.children}</Link>;
                }
            }
        }
        return divComp;
    }
}
```

We've imported the default BreadCrumbs implementation using `@sdl/dd/base/presentation/Breadcrumbs` as it's location. 
The `@sdl/dd/base` prefix is to provide a way of still having access to the default implementation when overwriting the alias of a certain component.

For this to work we'll need to change some configuration inside our webpack file, we'll handle this in the next section.
To get an understanding of how the component is working have a look at the source file which is packaged with the npm package.

## Overwriting the default breadcrumbs implementation

To use the component we created in the previous step we'll need to change the webpack configuration which can be found inside `src/webpack.config.js`.

We'll add some extra configuration inside the `config > resolve > alias` configuration.

Put the custom overwrite after the `// Custom components overwrites` comment and before the `// Components aliases` comment. 
This is important as the sequence is used to resolve dependencies in a certain order.

```javascript
const config = {
    // ...
    resolve: {
        // ...
        alias: {
            React: 'react',
            ReactDOM: 'react-dom',
            ReactDOMServer: 'react-dom/server',
            // Custom theme
            'theme-styles.less': path.resolve(__dirname, 'src/theming/styles.less'),
            // Custom components overwrites
            '@sdl/dd/presentation/Breadcrumbs': path.resolve(__dirname, 'src/custom-components/Breadcrumbs.tsx'),
            // Components aliases
            '@sdl/dd/base': path.resolve(__dirname, 'node_modules/@sdl/delivery-ish-dd-webapp-gui/dist/lib/components'),
            '@sdl/dd': path.resolve(__dirname, 'node_modules/@sdl/delivery-ish-dd-webapp-gui/dist/lib/components')
        },
        // ...
    },
    // ...
}
```

In order for these change to take effect you'll need to stop your dev server and restart it using `npm start`.