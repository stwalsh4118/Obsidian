The `app.module.ts` file houses all of our module dependencies as well as component dependencies.

# *.module.ts files
The initial file looks like so:

```ts
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';

import { AppRoutingModule } from './app-routing.module';
import { AppComponent } from './app.component';

@NgModule({
  declarations: [
    AppComponent
  ],
  imports: [
    BrowserModule,
    AppRoutingModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }

```

We use the `@NgModule` decorator to designate that this class will an NgModule

We declare our component dependencies within the *declarations* array, and our module dependencies within the *imports* array.

The providers array is for importing various service providers and whatnot and the bootstrap array is where you decide which component is the root component.

