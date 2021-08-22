Component Styles are held within `*.component.css` files

The CSS styles within the corresponding .css file describes the style for that component 

## View encapsulation

Since the CSS styles are encapsulated for each component they don't affect the rest of the applications styles

We have a few different modes of encapsulation to choose from:

- `ShadowDom`: view encapsulation uses the browsers native shadow DOM implementation to attach a shadow DOM to the components element and then pass the CSS style into it
- `Emulated`: (default) preprocesses the CSS styles (and renaming) such that each component's styles have their own unique names which "emulates" the way the shadowDOM works. e.g. even if two components have "app-wrapper" as a CSS style name the styles for that selector will be preprocessed so it is unique
- `None`: all styles within component CSS files are added to the global styles

#### Style Scope
Styles that are designated for components are not inherited from parents nor passed down to children, any styles in the css file designated for the component are ***only*** for that component.

This is useful to maintain style modularity!
- It keeps every css file close to its component so you know where everything is
- If something looks wrong in a component you know which css file to look for to fix it
- No collisions between selector names

### Special Selectors

- :host
This selector is used to get a reference to the selector that encapsulates a component, e.g. `<app-container>`. The template ***inside*** the component has no actual reference selector for the component itself so we can use the `:host` selector to get access to that

```css
:host {
  display: block;
  border: 1px solid black;
}
```

We can also use the *function form* to apply styles to the host conditionally if the host has another selector on it

```css
:host(.active) {
  border-width: 3px;
}
```

Above we will apply the `border-width` property to the host if the host has the `active` class

- :host-context
The `:host-context` selector in conjunction with the *function form* we saw before, to select any element within its ancestry all the way up until the document root. This is useful to be able to react to an ancestors css styling

```css
:host-context(.theme-light) h2 {
  background-color: #eef;
}
```


### Loading component Styles
There are a few ways to load your styles for your components
- Setting `styles` or `styleUrls` in the component metadata
- Inline styling in the HTML template
- CSS Imports

#### Styles in component metadata
You can set the specific styling themselves within the styles metadata array, with each string corresponding to a single style

```ts
@Component({
  selector: 'app-root',
  template: `
    <h1>Tour of Heroes</h1>
    <app-hero-main [hero]="hero"></app-hero-main>
  `,
  styles: ['h1 { font-weight: normal; }']
})
export class HeroAppComponent {
/* . . . */
}
```

You can also set a file url in the `styleUrls` metadata array to get a reference to a css file within your project to pull styles from

```ts
@Component({
  selector: 'app-root',
  template: `
    <h1>Tour of Heroes</h1>
    <app-hero-main [hero]="hero"></app-hero-main>
  `,
  styleUrls: ['./hero-app.component.css']
})
export class HeroAppComponent {
/* . . . */
}
```

You can apply multiple styleUrls or style strings or a combination of the two in any of your components

#### Template inline styles

You can do it the old fashioned way by having inline styling within your HTML template within `<style>` tags

```ts
@Component({
  selector: 'app-hero-controls',
  template: `
    <style>
      button {
        background-color: white;
        border: 1px solid #777;
      }
    </style>
    <h3>Controls</h3>
    <button (click)="activate()">Activate</button>
  `
})
```

You can also link to your style sheets within the HTML template as will with the `<link>` tags

```ts
@Component({
  selector: 'app-hero-team',
  template: `
    <!-- We must use a relative URL so that the AOT compiler can find the stylesheet -->
    <link rel="stylesheet" href="../assets/hero-team.component.css">
    <h3>Team</h3>
    <ul>
      <li *ngFor="let member of hero.team">
        {{member}}
      </li>
    </ul>`
})
```

#### CSS Imports

You can also use the standard CSS `@import` rule to get css styling into your components css file 

```css
/* The AOT compiler needs the `./` to show that this is local */
@import './hero-details-box.css';
```

You must configure your `angular.json` file to include all of your external assets so that the CLI knows which assets you need when its building

Also you can use Non-CSS style files with Angular, you just need to add the appropriate file ending when you import them into your component like so:

```ts
@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.scss']
})
...
```