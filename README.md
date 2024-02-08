<p align="center">
  <img src="https://avatars.githubusercontent.com/u/79112097?s=200&v=4" />
</p>

<h2 align="center">minecraft-pathfinding :tm:</h2>
<h4 align="center">A pathfinder to get a Minecraft bot from point A to point B with unnecessarily stylish movement</h4>

<p align="center">
  <a href="https://discord.gg/zDzugD3ywn">
    <img src="https://img.shields.io/badge/discord-000000?style=for-the-badge&logo=discord" alt="Discord">
  </a>
</p>

> [!WARNING]
> This pathfinder is still in **HEAVY** development. It is not meant to be used in production. However, you can use the code in `examples` to understand how the pathfinder works.

<h3 align="center">Why use this pathfinder?</h3>

-----

Presently, its execution is better than both Baritone's and mineflayer-pathfinder's. It also follows an entirely different paradigm - each move's execution is split into individual parts, which combined with a modular movement provider/executor/optimizer system, it makes this pathfinder ***extremely*** customizable. So as long as a single movement's execution handles all entry cases (which is possible), it can be immediately integrated into this bot.

Examples will be added as the project undergoes more development.

<h3 align="center">What is left to be done?</h3>

-----

**Many** things.

- [ ] Proper breaking of blocks (Cost + Execution)
- [ ] Parkour
- [ ] Proper jump sprinting
- [ ] Offloading the world thread


<h3 align="center">API and Examples</h3>

-----

| Link | Description |
| --- | --- |
| [API](https://github.com/GenerelSchwerz/minecraft-pathfinding) | The documentation with the available methods and properties. |
| [Examples](https://github.com/GenerelSchwerz/minecraft-pathfinding/tree/main/examples) | The folder with the examples. |

### Goals
There is currently only one goal: `goals.GoalBlock`. You can easily make a goal via: `GoalBlock.fromVec(<vec3>)`

This goal will have the bot stand on top of the block chosen.

Code example:
```ts
const {Vec3} = require('vec3')
const {goals} = require('@nxg-org/mineflayer-pathfinder')

const {GoalBlock} = goals;


// ...

const goal = GoalBlock.fromVec(new Vec3(0, 0, 0))
```


### Pathfinder Usage
There is currently:
- bot.pathfinder.getPathTo(\<vec>)
- bot.pathfinder.getPathFromTo(\<startVec>, \<endVec>)

Both of these provide an async generator that generates partial paths until a successful path is found, or no path is found.

To move around, use:
- bot.pathfinder.goto(\<goal>)

This returns a Promise\<void>, so you can wait until it reaches its destination.

Code example:

```ts
const {createPlugin, goals} = require('@nxg-org/mineflayer-pathfinder')

const {GoalBlock} = goals;
const pathfinder = createPlugin();


bot.loadPlugin(pathfinder)

// ...

await bot.pathfinder.goto(GoalBlock.fromVec(0,0,0))
```

### Settings
There are already a lot of settings, but there will be *plenty* more.

```ts
export interface MovementOptions {
  forceLook: boolean;
  jumpCost: number;
  placeCost: number;
  canOpenDoors: boolean;
  canDig: boolean;
  dontCreateFlow: boolean;
  dontMineUnderFallingBlock: boolean;
  allow1by1towers:boolean
  maxDropDown:number;
  infiniteLiquidDropdownDistance:boolean;
  allowSprinting: boolean;
  careAboutLookAlignment: boolean;
}
```
- forceLook: whether or not the pathfinder uses `force` in `bot.lookAt`
- jumpCost: heuristic cost for jumping
- placeCost: heuristic cost for placing
- canOpenDoors: not used yet.
- canDig: whether or not the bot can dig.
- dontCreateFlow: care about liquid flowing when breaking blocks (keep this on).
- dontMineUnderFallingBlock: ouch, sand! (keep this on).
- allow1by1towers: figure it out. Keep it on, no issue with it not being on.
- maxDropDown: DEFAULT IS 4. max continous drop-down. 
- infiniteLiquidDropdownDistance: it's in the name.
- allowSprinting: yes.
- careAboutLookAlignment: With this off, moves can be started without having the proper look vector established. With forceLook on, this means nothing. With forceLook off, movements may become unreliable in exchange for faster execution while still being compliant with anticheats.
