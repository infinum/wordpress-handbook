To register a block, you need a few things:
- Provide javascript and css scripts to the editor and frontend using two new hooks: `enqueue_block_editor_assets` and `enqueue_block_assets.`
- Register class that has block details in it.

All blocks are located inside the blocks folder. Block structure looks like this:
- src
  - blocks
    - block-name
      - assets
        - index.js
      - view
        - component-name.php
      - components
        - blocks-elements.js
        - block-options.js
      - class-block-name.php
      - index.js
      - edit.js
      - view.php
      - index.scss
      - editor.scss

## Src 
Src is a parent folder that holds all theme or plugins classes and functionality.

## Blocks
This folder contains all Gutenberg blocks defined in your project.

## Block-Name
Block name is the name that you block will be named upon registration. Folder name and block name should be the same. You will put all block related details in this folder.

## Assets
The assets folder is optional. This folder holds all additional javascript functionality for the block that you only need to use on the frontend. For example, if this block is a carousel, you will add some javascript lib that loads carousel on the frontend.

## View
View folder is also optional. If your frontend part of the block is complicated and you want to break down one significant component into smaller pieces, you will add this component in this folder. The naming of the files in the folder doesn't need to follow any convection, but it has to be logical. You need to know what this component is from its name.

## Components 
Components Folder holds two files `blocks-elements.js` and `blocks-options.js.` Each of these files represents a part of the Gutenberg block that is used in the editor. We have separated options and elements in separate components for the sake of readability and reusing components in different projects.

### Components - Blocks-element.js
This component holds edit functionality of the block that is displayed in the main editor part.

Example:
```javascript
import { __ } from '@wordpress/i18n';
import { RichText } from '@wordpress/editor';

export const BlockElements = (props) => {
  const {
    attributes: {
      rootClass,
      heading,
    },
    attributesStore: {
      onChangeHeading,
    },
  } = props;

  const blockClass = rootClass;
  const wrapClass = `${rootClass}__wrap`;
  const headingClass = `${rootClass}__heading`;

  return (
    <div className={blockClass}>
      <div className={wrapClass}>
        <div className={headingClass}>
          <RichText
            tagName="div"
            placeholder={__('Add your heading', 'academy')}
            onChange={onChangeHeading}
            value={heading}
          />
        </div>
      </div>
    </div>
  );
};
```

First, you pull all attributes and attributeStore data from the props; Then you define all classes that are going to be used in the HTML and the end you provide a return method that has HTML markup and components used to edit your block.

### Components - Blocks-options.js
This component holds edit functionality of the block that is displayed in the sidebar or toolbar. This part holds all options part of the block.

Example:
```javascript
import { __ } from '@wordpress/i18n';
import { InspectorControls } from '@wordpress/editor';
import { Fragment } from '@wordpress/element';
import { PanelBody, SelectControl } from '@wordpress/components';

export const BlockOptions = (props) => {
  const {
    attributes: {
      mediaType,
    },
    attributesStore: {
      onChangeMediaType,
    },
  } = props;

  return (
    <Fragment>
      <InspectorControls>
        <PanelBody title={__('Media Settings', 'academy')}>
          <SelectControl
            label={__('Type', 'academy')}
            value={mediaType}
            options={[
              { label: __('Image', 'academy'), value: 'image' },
              { label: __('Lottie', 'academy'), value: 'lottie' },
            ]}
            onChange={onChangeMediaType}
          />
        </PanelBody>
      </InspectorControls>
    </Fragment>
  );
};
```
First you pull all attributes and attributeStore data from the props, and you define all the options that are going to be used and returned to the editor or toolbar part of the block. You can use all predefined components from WordPress that you can find on [WordPress Handbook](https://developer.wordpress.org/block-editor/components/).

## Class-block-name.php
You will name this class with the same name as a blocks folder name and block registration name. When adding the new Class you will need to run `composer -o dump-autoload` in the terminal.

This class can extend `General_Block` or `Wrapper_Block` depending on the needs. We are using [Eightshift-Libs](https://github.com/infinum/eightshift-libs/) for block rendering and registration. Every block must have:
1. `BLOCK_NAME` constant where you define block name
2. `get_block_attributes()` method that holds all attributes that your block is going to use.

```php
<?php
/**
 * All info regarding the Intro block
 *
 * @since 1.0.0
 * @package Academy\Blocks
 */

namespace Academy\Blocks;

use Academy\Blocks\Wrapper_Block;

/**
 * Class Intro
 */
class Intro extends Wrapper_Block {

  /**
   * Block Name Constant
   *
   * @var string
   *
   * @since 1.0.0
   */
  const BLOCK_NAME = 'intro';

  /**
   * Get block attributes assigned inside block class.
   *
   * @return array
   *
   * @since 1.0.0
   */
  public function get_block_attributes() : array {
    return [
      'heading' => [
        'type' => parent::TYPE_STRING,
        'default' => '',
      ],
    ];
  }
}
```

## Index.js
Index.js holds the essential part of the block. It provides all the necessary data for the block.

```javascript
/* global wp */
/**
 * Block dependencies
 */

import { globalSettings } from './../assets/scripts/settings';
import { edit } from './edit';

/**
 * Internal block libraries
 */
const { __ } = wp.i18n;
const { registerBlockType } = wp.blocks;

/**
 * Block settings
 */
const { backgroundColor } = globalSettings;

/**
 * Register block
 */
export default registerBlockType(
  'academy/intro',
  {
    title: __('Intro', 'academy'),
    description: __('Intro block with custom settings.', 'academy'),
    category: 'common',
    icon: {
      background: backgroundColor,
      src: 'vault',
    },
    keywords: [
      __('Intro', 'academy'),
      __('Text', 'academy'),
    ],
    supports: {
      html: false,
    },
    edit,
    save() {
      return null;
    },
  }
);
```

Your block registration file must provide blocks full name that consists of the project namespace and block name. For example: `'academy/intro'`.
More details on this can be found in [WordPress Handbook](https://developer.wordpress.org/block-editor/tutorials/block-tutorial/writing-your-first-block-type/).

## Edit.js
Edit.js is the editing part of the block. It tells your block what data to provide in the editor.

Example if your block extends Wrapper Block:
```javascript
import { WrapperBlock } from '../wrapper-block';

import { BlockElements } from './components/block-elements';
import { BlockOptions } from './components/block-options';
import { validateBlock } from '../components/dynamic-blocks';

export const edit = (props) => {
  const {
    attributes,
    setAttributes,
  } = props;

  const attributesStore = {
    onChangeHeading: (value) => {
      setAttributes({
        heading: value,
      });
    },
  };

  // Make sure block is defined in PHP - needed only for dynamic blocks.
  validateBlock(props);

  return (
    <WrapperBlock
      props={props}
    >
      <BlockOptions
        attributes={attributes}
        attributesStore={attributesStore}
      />
      <BlockElements
        attributes={attributes}
        attributesStore={attributesStore}
      />
    </WrapperBlock>
  );
};
```

Example if your block extends General Block:
```javascript
import { Fragment } from '@wordpress/element';
import { BlockElements } from './components/block-elements';
import { BlockOptions } from './components/block-options';
import { validateBlock } from '../components/dynamic-blocks';

export const edit = (props) => {
  const {
    attributes,
    setAttributes,
  } = props;

  const attributesStore = {
    onChangeHeading: (value) => {
      setAttributes({
        heading: value,
      });
    },
  };

  // Make sure block is defined in php - needed only for dynamic blocks.
  validateBlock(props);

  return (
    <Fragment>
      <BlockOptions
        attributes={attributes}
        attributesStore={attributesStore}
      />
      <BlockElements
        attributes={attributes}
        attributesStore={attributesStore}
      />
    <Fragment>
  );
};
```

All Block attribute stores are defined here and passed into `BlockOptions` and `BlockElements` components. You need to pass `attributes` and `attributesStore` into components that are using this data.

In the attributeStore, you need to define an event handler for each attribute and use `setAttributes` method. This tells React store that attribute has been changed so it can re-render the data.

Also, `validateBlock(props);` method is a helper function that checks if your block has attributes.

## View.php
View.php is the frontend part of your dynamic block. This file has all the attributes inside the `$attributes` variable for you to use. The data comes from the block comments in the database. Also, it has some additional attributes that are shared across all blocks.

```php
<?php
/**
 * Template for the Intro block.
 *
 * @since 1.0.0
 * @package Academy\Blocks.
 */

namespace Academy\Blocks;

// $attributes are block's attributes.
$root_class = $attributes['rootClass'] ?? '';
$heading    = $attributes['heading'] ?? '';

$block_class   = $root_class;
$wrap_class    = "{$root_class}__wrap";
$heading_class = "{$root_class}__heading";
?>

<div class="<?php echo esc_attr( $block_class ); ?>">
  <div class="<?php echo esc_attr( $wrap_class ); ?>">
    <?php if ( ! empty( $heading ) ) { ?>
      <div class="<?php echo esc_attr( $heading_class ); ?>">
        <?php echo wp_kses_post( $heading ); ?>
      </div>
    <?php } ?>
  </div>
</div>
```

In View, you will define how you block is going to be displayed in the frontend for the website. You can get all the attributes from the `$attributes` variable.
Due to consistency and maintainability, class naming must be the same in the javascript and PHP part also classes order and HTML structure.

## Index.scss
Index.scss holds all the frontend and backend styling for the component. Each block has `rootClass` attribute that returns block name with block prefix, for example: `.block-intro.` You can/must use this naming convention when stying your block.

Just like any other SCSS components, Gutenberg block must also be standalone and easy to copy to a different project. All global variables must be defined in the map on the top.

```scss
.block-intro {
  display: flex;
  align-items: center;
  justify-content: space-between;

  &__heading {
    font-weight: bold;
  }
}
```

## Editor.scss
Index.scss holds only the backend styling for the component. You are using this file to override index.scss styles in the editor. In 90% of cases, you will not need to write any overrides here. But if you are using any columns layout like a grid, flex, etc., you will need to add some corrections.

Corrections in the columns layout are necessary because Gutenberg adds its additional HTML and you can't change it.
