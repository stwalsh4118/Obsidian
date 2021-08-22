The `app.component.html` file is the initialized template for the root when you first create your project

# *.component.html files

An example file can look like so:

```html
<div class="task-container">
	<div class="task-description">{{ timerTask.taskDescription }}</div>
	<cd-timer
		[startTime]="timerTask.taskLength"
		format="intelli"
		(onStart)="autoStop()"
		#timer
		class="task-length"
	></cd-timer>
	<fa-icon [icon]="faTimes" size="lg" (click)="deleteTask()"></fa-icon>
</div>
```

## Template Syntax
There are many different things that we can add to our templates to make them more dynamic and useful than just making a basic HTML website

#### Text Interpolation
Text Interpolation can let you add dynamic text values into your HTML template. This allows you to change your views given different data, text, etc.

You can basically put any JavaScript expression into a text interpolation within your HTML template by surrounding it with 2 curly braces like so:

```html
<div>Hello {{name}}!</div>
```

Given the name variable has the name "Sean" in it the above will display "Hello Sean!" within the div, it takes the text from the variable and puts it within the div text

This also applies to attribute values so for example you can set the source for an image in your template using text interpolation like so:

```html
<p>{{title}}</p>
<div><img src="{{itemImageUrl}}"></div>
```

You can also evaluate expressions within the interpolation like so:

```html
<!-- "The sum of 1 + 1 is not 4" -->
<p>The sum of 1 + 1 is not {{1 + 1 + getVal()}}.</p>
```

Like above you can use various JavaScript functions as well as functions within the component itself

The process that Angular takes with interpolation is as so:
- Evaluate all expression in double curly braces
- Convert the expression results to strings
- Link the results to any adjacent literal strings
- Assign the composite to an element or directive property

There are limitations to things you can put into interpolations which include (mostly things that create side effects):
- Assignments (=, +=, -=, ...)
- Operators such as (new, typeof, or instanceof)
- Chaining expressions with ; or ,
- The increment and decrement operators ++ and --
- Some of ES2015+ operators

As well as:
- No support for the bitwise operators such as | and &
- New template expression operators, such as |, ?. and !

#### Expression context
Expression each have a context that they belong to and can take values from. Most of the time it is the component that the expression is being evaluated in

```html
<h4>{{recommended}}</h4>
<img [src]="itemImageUrl2">
```

Or an expression can refer to values within its own template like a template input variable or reference variable

```html
<ul>
  <li *ngFor="let customer of customers">{{customer.name}}</li>
</ul>
```

The above gets access to the `customer` that is inputted into it through the `*ngFor` directive

```html
<label>Type something:
  <input #customerInput>{{customerInput.value}}
</label>
```

As well the above gets a reference to the reference variable `#customer` to access its values

###### Note: template expressions cannot refer to anything in the global namespace, (except undefined). E.g, no access to window or document or calling console.log() or Math functions.

The context that an expression pulls from is created from the union of all the variables of the contexts that it belongs to, so there can be name collisions within two different contexts e.g. a template variable name being the same as a component property name. The order that Angular decides which context to use is as so:

1. The template variable name
2. A name in the directive's context
3. The component's member names


#### Expression best practices

- Use short expressions
	- Keep application and business logic in the component as it can be hard to test within the template 
- Quick execution expressions
	- Expressions may be evaluated many times and will slow down the application if they are as well slow
- No visible side effects

### Template Statements
You can use template statements to respond to events within your template like so: 

```html
<button (click)="deleteHero()">Delete hero</button>
```

The above triggers the `deleteHero()` function whenever the element is clicked as we see the `(click)` event

Template statements can only refer to data that's within its context which is usually the component instance, they can also refer to data that is passed into the template like expressions above:  

```html
<button (click)="onSave($event)">Save</button>
<button *ngFor="let hero of heroes" (click)="deleteHero(hero)">{{hero.name}}</button>
<form #heroForm (ngSubmit)="onSubmit(heroForm)"> ... </form>
```

We can see that within the `deleteHero(hero)` call we get a reference to the `hero` object that was passed into the element through the `*ngFor` directive

### Transforming Data Using [[Pipes]]

Pipes can be used within the template to take data that's been inputted and transform it to the form that the template wants to display

### Interaction between components/templates
Templates can receive and send data between each other using various data flow techniques, [[Component Interactions| interactions between components/templates]] is vital for dynamic applications. 

