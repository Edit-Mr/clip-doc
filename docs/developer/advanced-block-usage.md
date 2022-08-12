---
sidebar_position: 6
title: Advanced Blocks API Usage
---
## Add/remove blocks in one times.
ClipCC provides support for add/remove blocks in one times. You can use ``api.addBlocks(blocks: BlockPrototype[])`` and ``api.removeBlocks(blocksOpcode: string[])`` to implement them.
## Filter
ClipCC provides support for filter since version 3.1.4. Here's an example:
```javascript
api.addBlock({
    opcode: 'example.block',
    type: type.BlockType.COMMAND,
    messageId: 'example.block',
    categoryId: 'example.category',
    function: () => {...},
    option: {
        // Only available in stage
        filter: type.FilterType.STAGE
    }
});
```
For more block options, see [BlockPrototype - BlockOption](https://doc.codingclip.com/developer/block#block)
## Menu
ClipCC provides support for menu input since version 3.1.2. The correct way to define a menu's inputs is to define a ``menu`` property within parameter.
### Static
```javascript
param: {
    PARAMETER: {
        type: type.ParameterType.STRING,
        menu: [{
           messageId: 'example.extension.menu.type',
           value: 'type'
            }, {
                   messageId: 'example.extension.menu.rainbow',
                   value: 'rainbow'
            }, {
                  messageId: 'example.extension.menu.zoom',
                  value: 'zoom'
            }],
        default: 'rainbow'
      }
}
```
### Dynamic
:::caution
The following extension APIs are in draft and the following are subject to change in the future.
:::
```javascript
param: {
    PARAMETER: {
        type: type.ParameterType.STRING,
        menu: () => {
            // return sprites
            const vm = api.getVmInstance();
            const sprites = [];
			for (const targetId in vm.runtime.targets) {
				if (!vm.runtime.targets.hasOwnProperty(targetId)) continue;
				const name = vm.runtime.targets[targetId].sprite.name;
				sprites.push([name, name]);
			}
			return sprites;
        },
        default: 'rainbow'
      }
}
```
If you want to define a menu that cannot embed reporters, then set the ``field`` property to true.
```javascript
param: {
    PARAMETER: {
        type: type.ParameterType.STRING,
        default: 'something',
        field: true,
        menu: [...]
      }
},
```
## Hat
There's a simple example illustrating how to define a block of type ``HAT``:
```javascript
api.addBlock({
        opcode: 'example.hat',
        type: type.BlockType.HAT,
        messageId: 'example.hat',
        categoryId: 'example.category',
        param: {
            CONDITION: {
                type: type.ParameterType.BOOLEAN
            }
        },
        function: (args) => {
            if (!!hasReported) {
                window.hasReported = false;
                return false;
            }
            const result = !!args.CONDITION;
            if (result) {
                hasReported = true;
                return true;
            }
            return false;
        }
});
```
When the block is dragged into the workspace, the entire project will be under the illusion of "unstoppable" but no scripts are being executed. The editor checks every frame to see if this HAT is triggerred. The hat block will only be called normally if the previous frame returns ``false`` and the current frame returns ``true``.

## Branch
ClipCC provides support for menu input since version 3.1.4. Here's the definition:
```javascript title="index.js"
api.addBlock({
    opcode: 'example.if',
    type: type.BlockType.COMMAND,
    messageId: 'example.if',
    categoryId: 'example.category',
    branchCount: 1,
    param: {
        COND: {
            type: type.ParameterType.BOOLEAN
        }
    },
    function: (args, util) => {
        // If the condition is true, start the branch.
        if (!!args.COND) util.startBranch(1, false);
    }
});
```

```json title="en.json"
{
    "example.if": "if [COND] [SUBSTACK]"
}
```
　　You should specify "branchCount" in BlockPrototype, which means the number of branches of the block. You should also specify the location of the branch in the translation file and name it with [SUBSTACKX].
　　For a BRANCH block, You can control the flow through the ``startBranch`` provided in the ``util`` object.
```javascript
/**
* Start a branch in the current block.
* @param {number} branchNum Which branch to step to (i.e., 1, 2).
* @param {boolean} isLoop Whether this block is a loop.
*/
startBranch (branchNum, isLoop) {...}
```
When the ``isLoop`` parameter is specified as true, the blocks will be executed repeatedly until no startBranch is triggered. When false, the block will be ejected immediately. When the ``branchNum`` parameter is not specified, it defaults to 1. There's an [example](https://github.com/SimonShiki/neurons).

Writing this type of block usually requires modifications to the thread and sequencer, so you need to have a certain level of understanding of the Scratch/ClipCC source code. In general, we do not recommend that you create such blocks.