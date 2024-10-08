<p align="center">
  <img src="github-hydra-ai.png" alt="Hydra AI Logo">
</p>
<p align="center">
  <a href="https://www.npmjs.com/package/hydra-ai"><img src="https://img.shields.io/npm/v/hydra-ai.svg" alt="npm version"></a>
  <a href="https://www.npmjs.com/package/hydra-ai"><img src="https://img.shields.io/npm/dm/hydra-ai.svg" alt="npm downloads"></a>
  <a href="https://github.com/michaelmagan/hydraai/blob/main/LICENSE"><img src="https://img.shields.io/github/license/michaelmagan/hydraai.svg" alt="license"></a>
  <a href="https://github.com/michaelmagan/hydraai/commits/main"><img src="https://img.shields.io/github/last-commit/michaelmagan/hydraai.svg" alt="GitHub last commit"></a>
  <a href="https://discord.gg/dJNvPEHth6"><img src="https://img.shields.io/discord/1251581895414911016?color=7289da&label=discord" alt="Discord"></a>
    <a href="https://github.com/michaelmagan/hydraai/stargazers"><img src="https://img.shields.io/github/stars/michaelmagan/hydraai.svg?style=social" alt="GitHub stars"></a>

</p>


https://github.com/user-attachments/assets/2501fd99-f768-43f5-96cc-d113c4f95903

Generate React components on-the-fly at runtime using AI. Register your components, and let Hydra choose when to show them in your App.

```typescript
const dynamicComponent = await hydra.generateComponent(context);
```
## Star History
[![Star History Chart](https://api.star-history.com/svg?repos=michaelmagan/hydraai&type=Timeline)](https://star-history.com/#michaelmagan/hydraai&Timeline)
## Table of Contents

- [Getting Started](#getting-started)
- [Notes](#notes)
- [Report a Bug](#report-a-bug-or-request-a-feature)
- [How is this different from Vercel V0?](#how-is-this-different-from-vercel-v0)
- [Report a bug or Request a feature](#report-a-bug-or-request-a-feature)
- [This Repo](#this-repo)
- [License](#license)

## Getting Started
Use our [template](https://github.com/MichaelMilstead/hydra-template) project or add it to an existing application with the instructions below.

1. **Install the package**

```shell
npm i hydra-ai
```

2 **Set your OpenAI API key environment variable**

In a file called `.env.local`, add:

```
OPENAI_API_KEY=<your openai api key>
```

This will be used by the HydraBackend class server-side and is used to make requests to OpenAI.

3. **Initialize HydraClient and register components**

Somewhere in your app, create a new instance of `HydraClient`.

Then to create a list of components that the AI can choose from, call `registerComponent(name, component, propsDefinition, getComponentContext)` with each, where:

- `name` is a unique name for the component
- `component` is the actual component
- `props` is an object that describes each available prop of the component.
- `getComponentContext` is an optional prop that takes a function which Hydra will use to pre-fetch any data Hydra should know about when hydrating this component. For example, if I am registering a "ToDoList" component, I might pass a function that fetches and returns my list of todo items, so Hydra can show real data.

```typescript
//hydra-client.ts

import { HydraClient } from "hydra-ai";
import CurrentWeather from "./components/current-weather";
import RainChart from "./components/rain-chart";
import WeatherTimeChart from "./components/weather-timechart";
import WindTimeChart from "./components/wind-timechart";

const hydra = new HydraClient();

hydra.registerComponent("CurrentWeather", CurrentWeather, {
  temperatureFahrenheit: "number",
  description: "string",
  weather: '"rain" | "sun" | "cloud" | "snow" | "clear"',
});

hydra.registerComponent("RainChart", RainChart, {
  data: "Array<{ hourOrDay: string; rainChancePercent: number }>",
});

hydra.registerComponent(
  "TodoList",
  TodoList,
  {
    todoItems: "{id: string; title: string; isDone: boolean}[]",
  },
  getTodoItems
);

export default hydra;
```

3. **Generate components**

```typescript
const component = await hydra.generateComponent(message);
```

You will likely want to have a state variable to hold the generated component. Here's a full example page (using NextJS) that uses Hydra, assuming the `hydra-client.ts` file shown above is created:

```typescript
"use client";

import { ReactElement, useEffect, useState } from "react";
import hydra from "./hydra-client";

export default function Home() {
  const [dynamicComponent, setDynamicComponent] = useState<ReactElement | null>(
    null
  );

  const fetchComponent = async (message: string) => {
    const component = await hydra.generateComponent(message);
    setDynamicComponent(component);
  };

  useEffect(() => {
    fetchComponent("please show me a weather forecast");
  }, []);

  return (
    <main className="flex min-h-screen flex-col items-center justify-center">
      {dynamicComponent}
    </main>
  );
}
```

## Notes
Hydra development is still early, and patterns for different types of components and apps are still being developed. Join the discord to chat with the developers.
- expects to be used in a NextJS project
- components that have function props do not work.

Chat with other users and the developers here:
<p>  <a href="https://discord.gg/8RMRUPZ9RS"><img src="https://img.shields.io/discord/1251581895414911016?color=7289da&label=discord" alt="Discord"></a></p>


## Report a bug or Request a feature

Make a GitHub issue [here.](https://github.com/michaelmagan/hydraai/issues/new)


## How is this different from Vercel V0?

We've been asked this a few times. For clarification, Vercel V0 lets you generate a component that you can copy/paste into your app's code. Hydra is for injecting components into your running app's UI on-the-fly based on context.

## Report a bug or Request a feature

Make a GitHub issue [here.](https://github.com/michaelmagan/hydraai/issues/new)

## This Repo

- `hydra-ai` package is here: [/package](/package)

- Example apps that use `hydra-ai`:
  - Charts app here [/examples/charts-demo](/examples/charts-demo/)
  - Social app (Mage) here: [/examples/mage-demo](/examples/mage-demo)
  - ToDo app demo here: [/examples/todo-app-demo](/examples/todo-app-demo/)
  - Weather app demo here: [/examples/weather-app-demo](/examples/weather-app-demo/)

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.
