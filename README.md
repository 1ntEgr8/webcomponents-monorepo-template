# webcomponents-monorepo-template
💪 A template monorepo that lets you write, showcase, and publish web components seamlessly

## Features
* Uses [yarn workspaces](https://yarnpkg.com/features/workspaces) and [lerna](https://lerna.js.org/) to maintain the monorepo
* Written in Typescript
* [Storybook](https://storybook.js.org/) support is builtin
* A simple script generates boilerplate code for you

## Usage
After cloning the repo, install dependencies by running `yarn`.

The template comes with a handy script that generates boilerplate code for you. As of now, the script generates boilerplate for [hybrids](https://hybrids.js.org/). However, support for other web components frameworks can be easily added. Feel free to shoot a PR!

### Creating a new web component
NOTE: The script currently supports code autogeneration for [hybrids](https://hybrids.js.org/). To use a different framework, see [using other web components frameworks](#using-other-web-components-frameworks).

To create a new component, run
```
> ./scripts/component add <name-of-component>
```

Eg:
```
> ./scripts/component add nk-button
```
The script will parse `nk-button` in the following way
```
<short-org-name>-<component-name>
```
In this case, the short version of the org name would be `nk`, and the name of the component would be `button`.

After that, it will create a package named `button` in the `packages` folder. It will also update the global `tsconfig.json` and add a project reference to this package. The `button` package will have the following file structure:
```
packages/button/
├── package.json
├── src
│   ├── nk-button.stories.ts
│   └── nk-button.ts
└── tsconfig.json
```

You can now start writing your web component in `nk-button.ts`. To view the web component in [Storybook](https://storybook.js.org/), you will need to write a story in `nk-button.stories.ts`. Read the Storybook docs to learn more about it!

Since the template uses `yarn workspaces`, you will see only a single `node_modules` folder at the root directory.

### Removing a web component
To remove a component, run
```
> ./scripts/component remove <name-of-component>
```

Eg:
```
> ./scripts/component remove nk-button
```

This will remove the `button` package from `packages`. It will also update the global `tsconfig.json` and remove the respective project reference.

## Using other web components frameworks
The script packaged with this template support code autogeneration for the [hyrbrids](https://hybrids.js.org/) framework. 

You can still a web component using a different framework. You just need to create a regular javascript package under `packages`, but use a differet dependency. For example, if you wanted to write a web component using [lit-element](https://lit-element.polymer-project.org/), add it as a dependency in the `package.json` of the respective package.

You will need to add a `tsconfig.json` to your package. Refer `./scripts/templates/tsconfig.json` for the one that gets autogenerated for the hybrids one. Your config file should largely remain the same. (if you are going to use this config file, make sure to put your code in the `src` folder of your package).

## Gotchas

### Naming components
The web components spec requires you to have a `-` in the name of your component. This distinguishes it from existing html tags. The template parses the name in the following manner:
```
<short-org-name>-<component-name>
```
For example, if my org name is `material web components`, it would make sense for me to name my components as `mwc-button`, `mwc-textfield`, ...
The script uses this heuristic to autogenerate the package name (which will be `component-name`), `package.json`, and project references in `tsconfig.json`.

### Reusing web components from the repo
You might want to use a web component in another package, but it's in the same repo, and its not deployed. Not to worry, Typescript project references to the rescue!

Let's say that you are creating a carousel named `<nk-carousel>`, and you want to reuse the `<nk-item>` web component that you wrote.
To do this, add a project reference in the `tsconfig.json` file of the `nk-carousel` package.
```
tsconfig.json (nk-carousel)
{
  ...
  "references": [
    { "path": "../item" }
  ]
}
```

## TODOS
* Make publishing and versioning components simpler. Right now, you will have to do it manually.
