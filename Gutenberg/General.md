Gutenberg is a new editor introduced in WordPress version 5.0 that was released in December, 2018. It adds content blocks and page builder-like functionality to every up-to-date WordPress website. When in use, it replaces TinyMCE as the default content editor. With Gutenberg, content is added in blocks of various types from the WordPress backend.

Now you can create blocks that you can preview and live edit from the backend without refreshing the frontend part of the website for every change.

All development details can be found on:
- [Github](https://github.com/WordPress/gutenberg/)
- [Handbook](https://developer.wordpress.org/block-editor/)

# Block types
Gutenberg gives us abilit to create blocks that we are going to use on the frontend and on the backend for the website. We have two types for block in our deposal.

## 1. Normal blocks.
When we are talking about normal blocks that means that you need to provide `edit` and `save` details in the javascript part of the block. `Edit` option defines layout and functionalities a block will have when you are using it in the editor. `Save` option defines how block output is going to be saved in the database opone saving the page.

We are never using this type of blocks because saving html markup in the database is not good for maintainability of a project, it is harder to search the content, every change to the block means that you need to resave every page where this block is used or you need to write migrations scripts. As you can see there are a lot of downsides in using this type of blocks.

The up side is that you write edit and save part in the javascript and you are generaly faster this way.

More development details can be found on [WordPress Handbook](https://developer.wordpress.org/block-editor/tutorials/block-tutorial/writing-your-first-block-type/).


Example of a normal block regustration:
```javascript
/**
 * Register block
 */
export default registerBlockType(
  'academy/paragraph',
  {
    title: __('Paragraph', 'academy'),
    description: __('Paragraph block with custom settings.', 'academy'),
    category: 'common',
    icon: {
      background: backgroundColor,
      src: 'editor-paragraph',
    },
    keywords: [
      __('Block', 'academy'),
      __('Text', 'academy'),
    ],
    supports: {
      html: true,
    },
    edit() {
      return (
        <RichText
          tagName="p"
          className={paragraphClass}
          placeholder={__('Add your paragraph', 'namespace')}
          onChange={onChangeContent}
          value={content}
        />
      );
    },
    save() {
      return (
        <p>{content}</p>
      );
    },
  }
);
```

## 2. Dynamic blocks.
This aproach defines edit part to the block in javascript and frontend part in the php. Registration is moastly the same but with some difference in the save method. In save method you put `return null` and that tells Gutenberg to save only block comments with filled attributes data and not the html markup in the database.

This aproach is slower because you need to write the same markup in php and javascript but it helps us overcome all the issues from the normal blocks.

More development details can be found on [WordPress Handbook](https://developer.wordpress.org/block-editor/tutorials/block-tutorial/creating-dynamic-blocks/).

Example of a dynamic block registration:
```javascript
/**
 * Register block
 */
export default registerBlockType(
  'academy/paragraph',
  {
    title: __('Paragraph', 'academy'),
    description: __('Paragraph block with custom settings.', 'academy'),
    category: 'common',
    icon: {
      background: backgroundColor,
      src: 'editor-paragraph',
    },
    keywords: [
      __('Block', 'academy'),
      __('Text', 'academy'),
    ],
    supports: {
      html: true,
    },
    edit() {
      return (
        <RichText
          tagName="p"
          className={paragraphClass}
          placeholder={__('Add your paragraph', 'namespace')}
          onChange={onChangeContent}
          value={content}
        />
      );
    },
    save() {
      return null;
    },
  }
);
```
