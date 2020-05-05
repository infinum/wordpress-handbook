New core editor is a powerful tool that gives you the ability to utilize the power of blocks. Blocks are a basic structure of your website.

Because the new editor is in a phase of rapid development, at the time of writing this page (May 2020), it's known to make some breaking changes. This is why we opted out for creating our own block library and project library, to serve as an interface between the fast changing system.

Since we use dynamic blocks, we wanted to find a way to reuse logic and components in blocks, to avoid having to write tons of boilerplate code, and to avoid breakage during development - which usually happens with standard blocks.

## Blocks folder

All Eightshift blocks are contained in the `src/blocks` folder. More in developer documentation about that folder can be found [here](https://infinum.github.io/eightshift-docs/docs/guides/blocks-structure).

There are few key pieces of information to grasp.

### manifest.json

This is the key file in any block, as well as the overall project settings. We have one in the `blocks` folder, but also in the each block element, that are located in the `custom` folder.

This file is used so that certain key information about the block is located in one place.

### components

Components, in our case, mimic React components - they are code pieces that can be reused in multiple blocks.

### custom

This is the folder where all project blocks live. One block can reuse multiple components (depending on its complexity)

### assets

This is a folder where JS and SCSS registration specific to blocks lives. It's where the code for registering blocks is held, but also where you can place any JS or SCSS related to blocks only.

Note that all block / component specific JS / SCSS should be added to that specific block / component folder rather than here.

### wrapper

This is a special component that will be used if we want to share variables between multiple blocks and if we want to have a section ability for the block.

## Building a block

Building a block, especially in a custom system like ours, can be intimidating at first. Particularly if you are new to blocks or our libraries. Where do you start when you want to create a new custom block?

The best thing to do is to explore a library a bit. See what components there exist, look at how other blocks are created. This will give you some idea how things interact in the library and during the build process.

Then you'll probably need a piece of paper.

1. Write all the options you think your block will need. From the toolbar options, sidebar options and editor screen.
2. Then write the basic block structure according to the [documentation](https://infinum.github.io/eightshift-docs/docs/guides/blocks-structure-block-item). Be careful about adhering to the structure, because our build system depends on certain files being in a certain place.
3. Start implementing one by one component - implement the options first and make sure the options work as intended (e.g. when you add image editor component, you'll want it to work when you click on it).
4. Write the editor screen and markup.
5. When editor markup is done, you can copy that HTML to the PHP, and change the naming a bit, and you should be done.
6. Test to see if everything works in the admin editor screen, and on the front end
7. Celebrate when you've created a functional block 🎉

## Reuse, reuse, reuse

The main thing we wanted to achieve using our libs is reusability across projects. There are a lots of ready components and blocks we worked on (you can check them out in the [Storybook](https://infinum.github.io/eightshift-docs/storybook)). You can just copy/paste them in your project, style them and that's it.

