<p align="center">
  <a href="https://github.com/qiwi">
    <img alt="Tech-radar" src="https://github.com/qiwi/tech-radar/blob/master/img/radar.png?raw=true?raw=true" width="546">
  </a>
</p>

<h2 align="center">

📡 [QIWI Radars](https://qiwi.github.io/tech-radar/) • [iOS](https://qiwi.github.io/tech-radar/ios/) • [JS](https://qiwi.github.io/tech-radar/js/) • [Backend](https://qiwi.github.io/tech-radar/backend/) • [QA](https://qiwi.github.io/tech-radar/qa/)

</h2>
<div align="center">

[![CI](https://github.com/qiwi/tech-radar/workflows/CI/badge.svg)](https://github.com/qiwi/tech-radar/actions)
[![Maintainability](https://api.codeclimate.com/v1/badges/b04b40063c8974a8ca31/maintainability)](https://codeclimate.com/github/qiwi/tech-radar/maintainability)
[![Test Coverage](https://api.codeclimate.com/v1/badges/b04b40063c8974a8ca31/test_coverage)](https://codeclimate.com/github/qiwi/tech-radar/test_coverage)  
Fully automated tech-radar generator. Based on [zalando/tech-radar](https://github.com/zalando/tech-radar). Boosted with [11ty](https://github.com/11ty/eleventy/)
</div>

## Table of contents
- [Getting started](#getting-started)
  - [Key features](#key-features)
  - [Requirements](#requirements)
  - [Install](#install)
  - [Usage](#usage)
    - [CLI](#cli)
    - [JS API](#js-api)
    - [Input-examples](#input-examples)
    - [CI/CD](#cicd)
- [Alternatives](#alternatives)
- [Contributing](#contributing)
- [License](#license)

## Key features
* Builds radars by `csv`, `json` or `yaml` data
* Renders a separate description page for each radar entry
* Сompares radars of the same scope with each other and shows the movement of points
* Assembles all the radars refs on the main navigation page
* Redirects scope urls to the latest version of their radars
* CLI / JS / TS API

## Requirements
* Node.js >= 14.13
* macOS / linux

## Install
```shell
yarn add @qiwi/tech-radar
```

## Usage
### CLI
```
techradar --input "/path/to/files/*.{json, csv, yml}" --output /radar
npx @qiwi/tech-radar --input "/path/to/files/*.{json, csv, yml}" --output /radar
```

| Option | Description | Default
|---|---|---
| cwd | Current working dir | `process.cwd()`
| input | [glob pattern](https://github.com/mrmlnc/fast-glob) to find radar data: csv/json/yml | `<cwd>/data/**/*.{json,csv,yml}`
| output | Output directory | `<cwd>/radar`
| autoscope | idenfify same-scoped files as subversions of a single radar | `false`
| base-prefix | base context path for web statics | `tech-radar`
| nav-page | create navigation page | `false`
| nav-title | navigation page title | 
| nav-footer | navigation page footer |
| temp | temporary assets dir | [`tempy.directory()`](https://github.com/sindresorhus/tempy)

### JS API
```js
import {run} from '@qiwi/tech-radar'

await run({
  input : 'data/*.{csv,json,yml}',
  output : 'dist',
  basePrefix: 'your project',
  autoscope: false
})
```
### Input examples
<details>
  <summary>json</summary>

```json
{
  "meta":{
    "title": "tech radar js",
    "date": "2021-06-12"
  },
  "data":[
    {
      "name": "TypeScript",
      "quadrant": "languages-and-frameworks",
      "ring": "Adopt",
      "description": "Статически типизированный ЖС",
      "moved": "1"
    },
    {
      "name": "Nodejs",
      "quadrant": "Platforms",
      "ring": "Adopt",
      "description": "",
      "moved": ""
    },
    {
      "name": "codeclimate",
      "quadrant": "tools",
      "ring": "Trial",
      "description": "Статический анализатор кода",
      "moved": "0"
    },
    {
      "name": "Гексагональная архитектура",
      "quadrant": "Techniques",
      "ring": "Assess",
      "description": "Унификации контракта интерфейсов различных слоев приложений",
      "moved": "-1"
    }
  ],
  "quadrantAliases": {
    "languages-and-frameworks": "q1",
    "platforms": "q2",
    "tools": "q3",
    "techniques": "q4"
  },
  "quadrantTitles": {
    "q1": "Languages and frameworks",
    "q2": "Platforms",
    "q3": "Tools",
    "q4" :"Techniques"
  }
}
```
</details>
<details>
  <summary>yaml</summary>

```yaml
meta:
  title: tech radar js
  date: "2021-06-11"
data:
  -
    name: TypeScript
    quadrant: languages-and-frameworks
    ring: Adopt
    description: Статически типизированный ЖС
    moved: 1
  -
    name: Nodejs
    quadrant: Platforms
    ring: Adopt
    description:
    moved:
  -
    name: codeclimate
    quadrant: tools
    ring: Trial
    description: Статический анализатор кода
    moved: 0
  -
    name: Гексагональная архитектура
    quadrant: Techniques
    ring: Assess
    description: Унификации контракта интерфейсов различных слоев приложений
    moved: -1
quadrantAliases:
  languages-and-frameworks: q1
  platforms: q2
  tools: q3
  techniques: q4
quadrantTitles:
  q1: Languages and frameworks
  q2: Platforms
  q3: Tools
  q4: Techniques
```

</details>
<details>
  <summary>csv</summary>

```
title
tech radar js
===
date
2021-06-18
===
name,                       quadrant,   ring,   description,                                                    moved
TypeScript,                 language,   Adopt,  "Статически, типизированный ЖС",                                1
Nodejs,                     Platforms,  Adopt,  ,
codeclimate,                tools,      Trial,  Статический анализатор кода,                                    0
Гексагональная архитектура, Techniques, Assess, Унификации контракта интерфейсов различных слоев приложений,    -1
===
quadrant,   alias
q1,         language
q1,         Languages-and-frameworks
q2,         Platforms
q3,         Tools
q4,         Techniques
===
quadrant,   title
q1,         Languages and frameworks
q2,         Platforms
q3,         Tools
q4,         Techniques
```
</details>

### CI/CD
Follow [gh-action usage example](https://github.com/qiwi/tech-radar/blob/master/.github/workflows/ci.yaml):
<details>
  <summary>release_radar action</summary>

```yaml
  release_radar:
    name: Publish radar to gh-pages
    # https://github.community/t/trigger-job-on-tag-push-only/18076
    if: github.event_name == 'push' && github.ref == 'refs/heads/master'
    runs-on: ubuntu-latest
    needs: test
    steps:
      - name: Checkuout
        uses: actions/checkout@v2

      - name: Setup NodeJS
        uses: actions/setup-node@v2
        with:
          node-version: 16

      - name: Install deps
        run: yarn

      - name: Generate
        run: yarn generate

      - name: Publish gh-pages
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./dist
          commit_message: "docs: update tech-radar static"
          allow_empty_commit: true
          enable_jekyll: false
```
</details>
<details>
  <summary>generator script</summary>

```json
"scripts": {
  "generate": "node ./src/main/js/cli.mjs --input \"data/**/*.{csv,json,yml}\"  --output dist --base-prefix tech-radar --autoscope true --nav-page true && touch dist/.nojekyll"
},
```
</details>

## Contributing
Feel free to open any issues: bug reports, feature requests or questions.
You're always welcome to suggest a PR. Just fork this repo, write some code, add some tests and push your changes.
Any feedback is appreciated.

## Alternatives
* [https://github.com/thoughtworks/build-your-own-radar](https://github.com/thoughtworks/build-your-own-radar)
* [https://github.com/zalando/tech-radar](https://github.com/zalando/tech-radar)
* [https://www.npmjs.com/package/@backstage/plugin-tech-radar](https://www.npmjs.com/package/@backstage/plugin-tech-radar)

## License
[MIT](./LICENSE)
