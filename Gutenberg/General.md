Gutenberg is a new editor introduced in WordPress version 5.0 that was released in December 2018. It adds content blocks and page builder-like functionality to every up-to-date WordPress website. When in use, it replaces TinyMCE as the default content editor. With Gutenberg, content is added in blocks of various types from the WordPress backend.

Now you can create blocks that you can preview and live edit from the backend without refreshing the frontend part of the website for every change.

All development details can be found on:
- [Github](https://github.com/WordPress/gutenberg/)
- [Handbook](https://developer.wordpress.org/block-editor/)

# Block types
Gutenberg gives us the ability to create blocks that we are going to use on the frontend and the backend for the website. We have two kinds for a block in our deposal.

## 1. Normal blocks.
When we are talking about standard blocks, that means that you need to provide `edit` and `save` details in the javascript part of the block. `Edit` option defines layout and functionalities a block will have when you are using it in the editor. `Save` option specifies how the output is going to be saved in the database upon saving the page.

We are never using this type of blocks because saving HTML markup in the database is not suitable for maintainability of a project, it is harder to search the content, every change to the block means that you need to resave every page where this block is used, or you need to write migrations scripts. As you can see, there are a lot of downsides in using this type of blocks.

The upside is that you write edit and save the part in the javascript and you are generally faster this way.

More development details can be found in [WordPress Handbook](https://developer.wordpress.org/block-editor/tutorials/block-tutorial/writing-your-first-block-type/).


## 2. Dynamic blocks.
This approach defines edit part to the block in javascript and frontend part in the PHP. Registration is mostly the same, but with some difference in the `save` method. In the `save` method, you put `return null,` and that tells Gutenberg to save only block comments with filled attributes data and not the HTML markup in the database.

This approach is slower because you need to write the same markup in PHP and javascript, but it helps us overcome all the issues from the standard blocks.

More development details can be found in [WordPress Handbook](https://developer.wordpress.org/block-editor/tutorials/block-tutorial/creating-dynamic-blocks/).

## Documentation
We are working with our custom build Eightshift Boilerplate Internal project. Wou will be working with these libs and should also read their Readme and Wiki materials:
1. [Eightshift Boilerplate Internal](https://github.com/infinum/eightshift-boilerplate-internal)
2. [Eightshift Libs](https://github.com/infinum/eightshift-libs)
3. [Eightshift Blocks](https://github.com/infinum/eightshift-blocks)
4. [Eightshift Frontend Libs](https://github.com/infinum/eightshift-frontend-libs)

