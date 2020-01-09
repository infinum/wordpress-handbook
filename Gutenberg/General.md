Gutenberg is a new editor introduced in WordPress version 5.0 that was released in December 2018. It adds content blocks and page builder-like functionality to every up-to-date WordPress website. When in use, it replaces TinyMCE as the default content editor. With Gutenberg, content is added in blocks of various types from the WordPress back end.

You can create blocks that you can preview and live edit from the back end without refreshing the front-end part of the website for every change.

All development details can be found on:
- [Github](https://github.com/WordPress/gutenberg/)
- [Handbook](https://developer.wordpress.org/block-editor/)

# Block types
Gutenberg gives us the ability to create blocks we are going to use on the front end and the back end for the website. We have two kinds of blocks at our disposal.

## 1. Standard blocks.
When we talk about standard blocks, we refer to blocks for which you need to provide the `edit` and `save` details in the JavaScript part of the block. The `edit` option defines the layout and functionalities a block will have when you use it in the editor. The `save` option specifies how the output is going to be saved in the database upon saving the page.

We never use this type of block because saving HTML markup in the database does not support the maintainability of a project, it makes it harder to search the content, and every change to the block means that you need to resave every page where this block is used or write migration scripts. As you can see, there are a lot of downsides to using this type of block.

The upside is that you write, edit, and save the part in JavaScript, and you are generally faster this way.

More development details can be found in the [WordPress Handbook](https://developer.wordpress.org/block-editor/tutorials/block-tutorial/writing-your-first-block-type/).


## 2. Dynamic blocks.
This approach defines the edit part of the block in JavaScript and the front-end part in PHP. Registration is mostly the same, but there are some differences in the `save` method. In the `save` method, you use `return null`, which tells Gutenberg to save only the block comments with filled attributes data and not the HTML markup in the database.

This approach is slower because you need to write the same markup in PHP and JavaScript, but it helps us overcome all the issues from the standard blocks.

More development details can be found in the [WordPress Handbook](https://developer.wordpress.org/block-editor/tutorials/block-tutorial/creating-dynamic-blocks/).

## Documentation
We work with our custom-built Eightshift Boilerplate Internal project. You will be working with these libs and should also read their Readme and Wiki materials:

1. [Eightshift Boilerplate](https://github.com/infinum/eightshift-boilerplate)
2. [Eightshift Libs](https://github.com/infinum/eightshift-libs)
3. [Eightshift Frontend Libs](https://github.com/infinum/eightshift-frontend-libs)
