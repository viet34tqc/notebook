# Angular Basic

<https://www.simplilearn.com/tutorials/angular-tutorial/>

## Setup

The best way to create an Angular project is using Angular CLI

```bash
npm i -g @angular/cli
```

Afterwards, you can use `ng new [your-app-name] --style=scss` to create a new project.
Some other libraries you might consider to use:

- @angular/material: Material UI for Angular

## Adding a new package

Angular CLI offers `ng add [package_name]` to install new packages to the project. On top of that, it also updates your project's configuration (`angular.json` file).

## Main concepts

In Angular, there are 4 main concepts you need to know

### Components

Each component defines a class that holds the application logic and data. 

It is recommended to use Angular CLI to create a component

```bash
ng generate component [component-name]
```
When you run that command, a folder named after your component is created in the `src/app` folder
Here is the basic structure of a component

- `[name].component.html`: html template
- `[name].component.css`: CSS of the component
- `[name].component.ts`: component file
- `[name].component.spec`: testing file

The most important file is `[name].component.ts`. Let's see what's in this file

```js
@Component({
  selector: 'app-component-overview', // CSS selector
  templateUrl: './component-overview.component.html', // HTML template
  styleUrls: ['./component-overview.component.css'] // CSS template
})

export class ComponentOverviewComponent {

}
```

- CSS selector: a selector instruct Angular to instantiate the component whenever it finds the corresponding tag in the template HTML. For example, a component `component-overview.component.ts` will define a selector `app-component-overview`. This selector intructs Angular to instantiate any time the tag `<app-component-overview>` appears in the template
- HTML template: a HTML block that tells Angular how to render the component. You can either use a link to extenal file (`templateUrl`) or define the template directly with `template`, like this:
- 
```js
@Component({
  selector: 'app-component-overview',
  template: `
    <h1>Hello World!</h1>
    <p>This template definition spans multiple lines.</p>
  `
})
```
- Style: Like HTML template, you can declate the component's styles in one of two ways: extenal file or directly within the component file

### Templates and data binding

Basically, Angular template is the combination of HTML markup and Angular markup.

Angular markup uses the concept of two-way binding. Any UI changes is reflected in the corresponding and specific modal state. Conversely, any modal state changes reflect in the UI state. This ensures the DOM is connected to model data

There are two types of data binding

#### Interpolation binding

Interpolation is the procedure that allows to bind a value to the user interface element. Interpolation bind the data one-way, which means the data moves from the component to HTML elements.

We defines the data in the component class and use the data in the html template

```js
@Component({
  ....
})
export class AppComponent {
  title = 'ng-video-games';
}
```

```html
<span>{{ title }} app is running!</span>
```

#### Property binding

Property binding is a one-way data binding mechanism that allows you to set the properties for HTML elements. It uses the "[]" syntax for data binding

```js
@Component({
  ....
})
export class AppComponent {
  image = '/assets/logo.png';
}
```

```html
<img [src]="image" alt="">
```

#### Event binding

Event binding happens when an event is triggered, then the data flows from the view to the component. The event could mouse click or keypress.

```js
export class AppComponent {
  onClick() {
    console.log('Thanks for subscribing');
  }
}
```

```html
<button (click)="onClick()">Subscribe</button>
```

#### Two-way binding

As the name suggests, two-way binding is the mechanism where data flows from the component to the view and back. This binding ensures that the component and the view are always in sync. The general syntax is "[()]"

**NOTE**: in order to use `ngModel`, you have to import `FormsModule` into root's module.

```js
export class AppComponent {
  random = ''
}
```

```html
<input type="text" [(ngModel)]="random">
{{random}}
```

### Dependency Injection (DI)

DI is a coding pattern where a class receives its dependencies from external resource rather than creating them itself.

For example, we have two classes:
```js
class Number {}
class Address {}
```

And we want to use these classes in another `PostalDetails` class

```js
class PostalDetails {
  Number;
  Address;
  constructor() {
    this.number = new Number();
    this.Address = new Address();
  }
}
```

Although this code works, there are some drawbacks with this approach

- The first drawback is that the code is not flexible. Any time the dependencies change, the `PostalDetails` class needs to be changed as well.
- The second drawback is the code is not suitable for testing. Anytime you instantiate a new `PostalDetails` class, you get the same `Number` and `Address`

So, we need to modify the code above by letting `PostalDetails` receive its dependencies rather than creating them.

```js
class PostalDetails {
  number;
  address;
  constructor(number, address) {
    this.number = number;
    this.Address = address;
  }
}
```
In the code above, we have moved the definition of the dependencies from inside the constructor to the constructor's parameters. So the `PostalDetails` doesn't create dependencies anymore, it just connsumes them.

### Modules

An Angular App has a root module, named AppModule, which provides bootstrap mechanism to launch the application.

Angular CLI command to create module

```bash
ng generate module [module_name]
```

Here is the example of root module

```js
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
@NgModule({
  imports:      [ BrowserModule ],
  providers:    [ Logger ],
  declarations: [ AppComponent ],
  exports:      [ AppComponent ],
  bootstrap:    [ AppComponent ]
})
export class AppModule { }
```