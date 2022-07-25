In Infinum's WordPress boilerplate and WordPress eco-system there are some frequently used terms. Some of them can mean different things, depending on the contenxt.

## Component

Components are (in most cases) standalone elements that are used by blocks or templates to build parts of the website. Each component has its own set of properties (attributes) that define how it can look and behave (font size, font family, color .etc).

Components can be basic and more complex (meaning that they contain multiple other components).

Examples of basic components are: paragraph, heading, image, list, video, icon, button.

Complex components can contain multiple other components and/or multiple instances of the same component with different default properties. Examples of complex components are: card, accordion, jumbotron (hero section).

Some components that were listed as basic, can sometimes be complex. For example, a button component can contain an optional icon in some variations.

Components should be agnostic to the layout that they are placed in and their style should not have explicit overrides based on location that they are used in. For example, a heading with size property “large” should have the same font size if used inside a card component or as a standalone heading. Components are intended to be reusable so that there is no need to develop the same element or functionality multiple times.


### Component properties

As stated above, component properties define how component will look and behave. Properties can have textual, numerical, boolean (true/false) or complex values. All properties have default values to represent the most common version of the component.


## Block

Blocks are a mechanism in WordPress that allow building custom pages or posts in WordPress. They can use components or be completely standalone (they have elements and functionality that does not need to be reused). Each block consists of:

* frontend view - file that will render block on frontend
 * editor view - file that is used to preview or edit content of the block in the editor
* block options - controls in the editor sidebar that allow changes to the block attributes
 * block toolbar - controls in the block toolbar that allows changes to the block attributes

Similar to components, blocks also have their properties (attributes). These attributes can be related directly to the block or override values of component properties. Options and toolbar parts of the block are used to control values of block and component attributes.


### Parent/ inner block

Blocks can have varying layouts and different arrangements of their elements. In a case where blocks need to be in some certain layout they can be set as a parent block that will have the option to add multiple inner blocks. Examples of this are: carousel block, accordion block, and columns block.

### Reusable block

Certain sections of the page will show up on multiple different pages (e.g. newsletter call to action). In that case it is good to create a reusable block. Reusable block is a collection of blocks that are already preconfigured and have the same content in all instances. This can be created inside the editor and will be available everywhere on the site where the block editor is present.

Important thing with reusable blocks is that their style and content cannot change from one page to another. Changing the options or content of the reusable block on one page will reflect that change on every other page that contains that block.

### Block variation

Certain blocks may have multiple versions that have equal importance on the page and constantly changing block options may become cumbersome. In that case it is a good thing to create a dedicated block variation. Block variation will take the form of a new block in the block library, but it’s really just a version of the original block that has different default values for attributes. Example of this would be a card block that has many optional elements that are used in certain combinations for certain use cases. Each of these combinations would be a one block variation of a card block.


### Block pattern

Block patterns are similar to reusable blocks, but their attributes can independently be changed for each instance. They are registered in the code and accessible from a Patterns tab in the block library.

Block patterns are a collection of blocks that are preconfigured for a specific use case but they can still be modified. Patterns should generally not contain any content and should focus on specific styling and layout configuration.

Multi-column layouts that have a lot of element reordering on smaller screens are a good candidate for a block pattern. Block patterns can be used as a template for a page or just parts of the page.

## Template

Templates are used in multiple scenarios. WordPress templates are PHP files that are used to display certain types of pages (single pages, post listings, taxonomy listings, .etc). They contain some logic for fetching data and also they render components.

Post types can have their own block templates which means that certain blocks can automatically be added to the editor of the newly created post. Template term is also used to refer to the frontend view of the block/component.


## Post type

WordPress comes with a couple of post types and allows registering new ones. Main post types are “Pages” and “Posts”. Custom post types can be created when a certain type of content needs to have a clear separation from existing types. Pages and posts don’t have a prefix in their URL, while other (custom) post types should. Each post type can have its own taxonomy associated with it or it can share taxonomies and terms from other post types.


## Page

Pages in WordPress represent non-chronological content and are the main elements that represent website structure. Pages have a simple hierarchy where some pages can bi set as a parent page and other are child pages of a certain parent page. Pages are mostly built with blocks.


## Post

Post can refer to the default post type that comes with WordPress installation which are used for creating a blog or news part of the web. Posts (as a term) are also used when talking about any other custom post type and their content pieces. Posts are used to display some chronological content (like news, events, .etc) and content that needs to be grouped and ordered in a certain way. Taxonomies are used to achieve that.

## Taxonomy

Taxonomy is a mechanism for grouping and organizing content by similar terms. Default posts come with two taxonomies registered: Categories and Tags. Custom post type called Event could have a taxonomy called Event Location that will have terms like: local, virtual, hybrid, .etc. Taxonomies can be used to filter certain content so users can find exact content they are interested in.
