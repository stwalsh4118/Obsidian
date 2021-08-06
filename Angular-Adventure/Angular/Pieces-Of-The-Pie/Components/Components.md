The `app.component.ts` file is the file that is initialized to be the root component behavior file.

# *.component.ts files
 #### Creating a component
 You can create a component with the Angular CLI using the command:
 
 `ng generate component {COMPONENT_FILE_PATH/COMPONENT_NAME}`
 
 This command generates the component.ts file as well as the other files associated with the component (CSS, [[Templates | HTML]], Etc.).
 
 It also imports it into your [[Modules | module.ts]] file for you so you can immediately 

All component.ts files within your project designate the behavior for its corresponding [[Templates | template]] file

The initial component file looks like so:

```ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  title = 'testing';
}
```

Component files are designated by the class having a `@Component` decorator.
 
 Within said decorator we can set various properties of the component
 - selector: the html tag that we can create an instance of this component with
 - templateUrl: the file which holds the html template for our component
 - styleUrls: the files which hold the CSS styles for the component


## [[Lifecycle Hooks]]

Lifecycle Hooks are functions that are called at certain points in a components Lifecycle. We can use these functions to run code at certain times, like initializing data with the `ngOnInit` function or cleaning up with the `ngOnDestroy` function, etc.

## Component Behavior

The [[Templates | component template]] determines what you actually *see* on the screen. We can change what we see on the screen with the component.ts file which determines the components behavior, things like data processing, [[Component Interactions]], etc.