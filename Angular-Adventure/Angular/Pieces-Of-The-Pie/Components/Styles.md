Component Styles are held within `*.component.css` files

The CSS styles within the corresponding .css file describes the style for that component 

## View encapsulation

Since the CSS styles are encapsulated for each component they don't affect the rest of the applications styles

We have a few different modes of encapsulation to choose from:

- `ShadowDom`: view encapsulation uses the browsers native shadow DOM implementation to attach a shadow DOM to the components element and then pass the CSS style into it
- `Emulated`: (default) preprocesses the CSS styles (and renaming) such that each component's styles have their own unique names which "emulates" the way the shadowDOM works. e.g. even if two components have "app-wrapper" as a CSS style name the styles for that selector will be preprocessed so it is unique
- `None`: all styles within component CSS files are added to the global styles