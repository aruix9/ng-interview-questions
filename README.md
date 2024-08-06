# Angular Interview Questions
---

## 1. What is a directive, and what types do we have?
**Answer:**
A directive in Angular is a class with a decorator that can modify the DOM or its behavior. There are three types:
  1. Components: Directives with a template.
  2. Attribute Directives: Change the appearance or behavior of an element, component, or another directive.
  3. Structural Directives: Change the DOM layout by adding or removing elements (e.g., *ngIf, *ngFor).

## 2. How can we distinguish attribute directives from structural directives in a template?
**Answer:**
Attribute directives are applied as normal HTML attributes, while structural directives use an asterisk (*) before the directive name. For example:
- Attribute Directive: `<div appHighlight></div>`
- Structural Directive: `<div *ngIf="condition"></div>`

## 3. Can a component be considered a directive? Explain.
**Answer:**
Yes, a component is a type of directive with a template. It combines application logic with a view, defined by the `@Component` decorator, which extends the basic `@Directive` decorator.

```javascript
import { Component, Directive, Input } from '@angular/core';
// Basic directive
@Directive({
  selector: '[appHighlight]'
})
export class HighlightDirective {
  @Input() appHighlight: string;

  constructor() {
    // Directive logic here
  }
}

// Component, which is a type of directive
@Component({
  selector: 'app-example',
  template: `
    <div appHighlight="yellow">
      This is a highlighted text.
    </div>
  `
})
export class ExampleComponent {
  constructor() {
    // Component logic here
  }
}
```

## 4. Explain the lifecycle hooks of a component.
**Answer:**
Lifecycle hooks in Angular allow you to tap into key events in a component’s lifecycle:
- `ngOnInit`: Called once, after the first ngOnChanges.
- `ngOnChanges`: Called before ngOnInit and whenever one or more data-bound input properties change.
- `ngDoCheck`: Called during every change detection run.
- `ngAfterContentInit`: Called once after the first ngDoCheck.
- `ngAfterContentChecked`: Called after ngAfterContentInit and every subsequent ngDoCheck.
- `ngAfterViewInit`: Called once after the first ngAfterContentChecked.
- `ngAfterViewChecked`: Called after ngAfterViewInit and every subsequent ngAfterContentChecked.
- `ngOnDestroy`: Called once just before the instance is destroyed.

```javascript
// LifecycleDemoComponent
import { Component, OnInit, OnChanges, DoCheck, AfterContentInit, AfterContentChecked, AfterViewInit, AfterViewChecked, OnDestroy, SimpleChanges, Input } from '@angular/core';

@Component({
  selector: 'app-lifecycle-demo',
  template: `
    <div>
      <p>Check the console for lifecycle hooks demo</p>
      <p>Input value: {{ someInput }}</p>
    </div>
  `
})
export class LifecycleDemoComponent implements OnInit, OnChanges, DoCheck, AfterContentInit, AfterContentChecked, AfterViewInit, AfterViewChecked, OnDestroy {

  @Input() someInput: string;

  constructor() {
    console.log('Constructor called');
  }

  ngOnInit() {
    console.log('ngOnInit called');
  }

  ngOnChanges(changes: SimpleChanges) {
    console.log('ngOnChanges called', changes);
  }

  ngDoCheck() {
    console.log('ngDoCheck called');
  }

  ngAfterContentInit() {
    console.log('ngAfterContentInit called');
  }

  ngAfterContentChecked() {
    console.log('ngAfterContentChecked called');
  }

  ngAfterViewInit() {
    console.log('ngAfterViewInit called');
  }

  ngAfterViewChecked() {
    console.log('ngAfterViewChecked called');
  }

  ngOnDestroy() {
    console.log('ngOnDestroy called');
  }
}

// ParentComponent
import { Component } from '@angular/core';

@Component({
  selector: 'app-parent',
  template: `
    <div>
      <button (click)="updateInput()">Update Input</button>
      <app-lifecycle-demo [someInput]="inputValue"></app-lifecycle-demo>
    </div>
  `
})
export class ParentComponent {
  inputValue: string = 'Initial value';

  updateInput() {
    this.inputValue = 'Updated value';
  }
}

```


## 5. What is the purpose of the constructor in a component?
**Answer:**
The constructor in an Angular component is used for dependency injection. It should not contain complex logic. It's primarily used to inject services required by the component.

## 6. What is Angular’s dependency injection, and what levels does it have?
**Answer:**
Angular's dependency injection (DI) is a design pattern to manage how components acquire their dependencies. It allows for injecting services or other dependencies into components. The levels of DI are:
- Module Level: Providers are available to all components in the module.
- Component Level: Providers are available to the component and its children.
- Element Level: Providers are available to the specific element and its children.

## 7. What is view encapsulation, and what types does Angular provide?
**Answer:**
View encapsulation in Angular controls how styles are applied to components. Angular provides three types:
- Emulated (default): Styles are scoped to the component using a unique attribute.
- None: Styles are applied globally.
- Shadow DOM: Uses the browser's native Shadow DOM for style encapsulation.

## 8. What is the difference between pure and impure pipes?
**Answer:**
- Pure Pipes: Only called when the input data changes. They are stateless and have no side effects.
- Impure Pipes: Called on every change detection cycle, regardless of changes to input data. They can have side effects and are less efficient.

## 9. How can components interact with each other?
**Answer:**
Components can interact in several ways:
- Input and Output Properties: Using `@Input` and `@Output` decorators to pass data and emit events.
- ViewChild and ContentChild: Accessing child components or directives.
- Services: Sharing data or functionality via dependency injection.
- Template References: Using template reference variables to call methods on child components.

## 10. What is lazy loading in Angular?
**Answer:**
Lazy loading in Angular is a technique to load modules only when they are needed, rather than at the application startup. This improves the application's initial load time and performance. It is typically configured using Angular's router with the loadChildren property in route definitions.

## 11. Describe Angular's HTTP client and its benefits.
**Answer:**
Angular's HTTP client, provided by HttpClientModule, is a modern, robust API for making HTTP requests. Benefits include:
- Observable-based API: Uses RxJS observables for request handling.
- Interceptors: Allows manipulation of requests and responses globally.
- Type Safety: Supports strong typing with TypeScript.
- Simplified Syntax: Provides a cleaner and more concise syntax compared to HttpClient.
- Automatic JSON Parsing: Automatically parses JSON responses.

## 12. What are hot and cold observables?
**Answer:**
Cold Observables: Emit values only when there are active subscriptions. Each subscriber receives a separate execution.
Hot Observables: Emit values regardless of subscriptions. All subscribers share the same execution and receive the same values.

## 13. How do you make a cold observable hot?
**Answer:**
You can make a cold observable hot using operators like publish and share. Example:
```javascript
import { Observable } from 'rxjs';
import { share } from 'rxjs/operators';

const coldObservable = new Observable(observer => {
  observer.next(Math.random());
});

const hotObservable = coldObservable.pipe(share());

```

## 14. Do you need to unsubscribe from every observable in Angular?
**Answer:**
No, you don't need to unsubscribe from observables that complete after emitting a single value (e.g., HTTP requests). However, for long-lived observables (e.g., event streams, subscriptions within components), you should unsubscribe to prevent memory leaks.


## 15. Explain Angular's change detection strategy.
**Answer:**
Angular uses change detection to update the DOM in response to data changes. It provides two strategies:
- Default: Checks every component in the component tree for changes.
- OnPush: Only checks the component and its children when an input reference changes or an event occurs within the component.

## 16. What is the difference between markForCheck and detectChanges in Angular?
**Answer:**
- markForCheck: Marks the component and its ancestors as needing change detection. Used in OnPush strategy.
- detectChanges: Triggers change detection for the component and its children immediately.

## 17. What are some techniques for optimizing an Angular application?
**Answer:**
- Lazy Loading: Load modules only when needed.
- OnPush Change Detection: Use OnPush strategy for better performance.
- Pure Pipes: Use pure pipes to optimize change detection.
- TrackBy in ngFor: Improve performance by tracking items in a list.
- Code Splitting: Break the application into smaller bundles.

## 18. What is the difference between ngOnInit and ngOnChanges?
**Answer:**
- ngOnInit: Called once after the component's first ngOnChanges. Used for component initialization.
- ngOnChanges: Called before ngOnInit and whenever input properties change. Used to respond to changes in input properties.

## 19. What is the role of the ngOnDestroy lifecycle hook?
**Answer:**
ngOnDestroy is called just before a component or directive is destroyed. It is used for cleanup tasks, such as unsubscribing from observables and detaching event handlers to prevent memory leaks.

## 20. How does Angular handle dependency injection for lazy-loaded modules?
**Answer:**
Angular creates a separate injector for each lazy-loaded module. Providers defined in a lazy-loaded module are scoped to that module and its children, preventing them from affecting the root injector or other modules.

## 21. What is the role of interceptors in Angular's HTTP client?
**Answer:**
Interceptors in Angular's HTTP client are used to intercept and manipulate HTTP requests and responses. They can:
- Add headers or authentication tokens.
- Log requests and responses.
- Handle errors globally.
- Transform request and response bodies.
- Interceptors are implemented as services that implement the HttpInterceptor interface and are provided in the providers array.

## 22. What is the purpose of the providedIn property in Angular services?
**Answer:**
The providedIn property in Angular services specifies the injector where the service is available. It can be set to:
- 'root': The service is available application-wide.
- 'any': A new instance of the service is created for each lazy-loaded module.
- A specific module or component: Limits the availability to that module or component.

## 23. Explain Angular’s HTTP client interceptors and the design pattern behind them.
**Answer:**
Angular’s HTTP client interceptors use the middleware design pattern. This pattern allows for preprocessing or postprocessing of HTTP requests and responses. Interceptors can be chained, meaning each interceptor can modify the request or response before passing it to the next interceptor or the HTTP client itself.

## 24. How can you bypass an HTTP interceptor in Angular?
**Answer:**
To bypass an HTTP interceptor, you can create a custom HTTP client service that does not include the interceptor in its provider configuration. Alternatively, you can use specific logic within the interceptor to conditionally skip processing for certain requests.

## 25. What is the role of schedulers in RxJS?
**Answer:**
Schedulers in RxJS control the execution context of observables. They manage how and when tasks like emissions, notifications, and subscriptions are executed. Common schedulers include:
- asyncScheduler: Schedules tasks asynchronously.
- queueScheduler: Schedules tasks synchronously in a queue.
- animationFrameScheduler: Schedules tasks using `requestAnimationFrame`.
Schedulers are used to optimize performance and manage concurrency in RxJS.

## 26. What is memoization, and how can it be used in Angular?
**Answer:**
Memoization is an optimization technique that caches the results of expensive function calls based on input arguments. In Angular, it can be used to improve performance by storing the results of computationally intensive functions and reusing them when the same inputs occur again. Libraries like lodash provide memoization utilities, or you can implement your own using JavaScript.

## 27. What is the difference between the default and onPush change detection strategies?
**Answer:**
- Default: Angular runs change detection for every component in the component tree on each event cycle or asynchronous operation.
- OnPush: Change detection runs only when the component's input properties change or an event occurs within the component. This strategy improves performance by reducing the number of change detection runs.

## 28. Explain the role of BehaviorSubject in RxJS.
**Answer:**
BehaviorSubject is a type of subject in RxJS that requires an initial value and emits its current value to new subscribers. It is useful for representing state or a value that changes over time, ensuring that subscribers always receive the latest value upon subscription.

## 29. What is the difference between share and shareReplay in RxJS?
**Answer:**
- share: Converts a cold observable into a hot one by sharing the source among multiple subscribers without retaining the emitted values.
- shareReplay: Shares the source among multiple subscribers and replays the specified number of emitted values to new subscribers. It combines the behaviors of share and replay

## 30. What is a structural directive, and how do we use it?
**Answer:**
A structural directive is a directive that changes the structure of the DOM by adding or removing elements.
Examples include *ngIf, *ngFor, and *ngSwitch. Structural directives are applied with an asterisk (*) prefix. For example:
```javascript
<div *ngIf="isVisible">Visible Content</div>
```
This directive conditionally includes or excludes the <div> element based on the value of isVisible.

## 32. What is the difference between providedIn: root and provided in a component?
**Answer:**
- providedIn: 'root': The service is a singleton and available application-wide.
- providedIn a component: A new instance of the service is created for the component and its children. This scopes the service to the specific component hierarchy, preventing global availability.

## 33. Explain the lifecycle of an NGRX store.
**Answer:**
The NGRX store lifecycle involves:
- Initialization: The store is created with an initial state.
- Dispatching Actions: Components and services dispatch actions to trigger state changes.
- Reducers: Actions are processed by reducers, which return a new state.
- Selectors: Selectors query the store for specific pieces of state.
- Effects: Side effects (e.g., API calls) are handled by effects, which can dispatch further actions.

## 34. When should you use state management libraries like NGRX?
**Answer:**
State management libraries like NGRX are useful when:
- The application has complex state interactions.
- Multiple components need to share state.
- State persistence and debugging are required.
- You need a predictable state management pattern.
For smaller applications, simpler state management strategies may suffice.

## 35. Explain the benefits of using Angular's HTTP client over fetch or XMLHttpRequest.
**Answer:**
Angular's HTTP client (HttpClientModule) provides a simplified and consistent API for making HTTP requests. Benefits over fetch or XMLHttpRequest include:
- Observable-based API: Uses RxJS for request handling.
- Interceptors: Modify requests and responses globally.
- Type Safety: Supports strong typing with TypeScript.
- Automatic JSON Parsing: Automatically parses JSON responses.
- Simplified Syntax: Cleaner and more concise syntax.

## 37. How do multiple HTTP interceptors work together in Angular?
**Answer:**
Multiple HTTP interceptors in Angular are executed in the order they are provided. Each interceptor can modify the request or response before passing it to the next interceptor in the chain. This allows for layered processing, such as adding authentication headers, logging, and error handling.

## 38. What are the main parts of RxJS, and how do they relate to Angular?
**Answer:**
The main parts of RxJS include:
- Observables: Represent streams of data over time.
- Observers: Consumers of observable data.
- Operators: Functions that transform, filter, or combine observables.
- Subjects: Both observable and observer, allowing multicasting.
- Schedulers: Control concurrency and execution context.
In Angular, RxJS is used extensively for handling asynchronous data, such as HTTP requests and event streams.

## 39. What are the advantages of using RxJS in Angular?
**Answer:**
Advantages of using RxJS in Angular include:
- Declarative Code: Simplifies complex asynchronous code.
- Composable Operators: Easy manipulation of data streams.
- Reactive Programming: Makes handling events and data streams intuitive.
- Performance: Efficient management of asynchronous operations.
- Consistency: Unified approach to handling async operations across the application.

## 40. Explain how dependency injection lookup logic works in Angular.
**Answer:**
Dependency injection (DI) in Angular follows a hierarchical lookup. When Angular resolves a dependency, it starts at the component’s injector and traverses up the injector tree to the root injector. If a provider is found at any level, it is used; otherwise, Angular throws an error.

## 42. How can you affect DI lookup logic in Angular?
**Answer:**
You can affect DI lookup logic by using decorators like `@Optional()`, `@Self()`, `@SkipSelf()`, and `@Host()`. These decorators modify the injector behavior, such as making a dependency optional or restricting the lookup to specific injectors.

## 43. How do you manage subscriptions in Angular to prevent memory leaks?
**Answer:**
Manage subscriptions in Angular by:
- Unsubscribing manually in the `ngOnDestroy` lifecycle hook.
- Using `takeUntil` with a `Subject` that emits when the component is destroyed.
- Using the `async` pipe in templates to handle subscription and unsubscription automatically.

## 44. What is the role of applicationRef.tick() in Angular?
**Answer:**
`applicationRef.tick()` triggers change detection manually in the entire application. It is useful for situations where Angular does not detect changes automatically, such as when external libraries modify the DOM or when you need to force change detection after making programmatic changes.

## 45. How does Angular's change detection mechanism work?
**Answer:**
Angular’s change detection mechanism compares the current state of the data model with the previous state. When a change is detected, Angular updates the DOM to reflect the new state. This process is triggered by events, asynchronous operations, and manual invocations of `detectChanges` or `markForCheck`.

## 46. How do you handle HTTP request errors in Angular?
**Answer:**
Handle HTTP request errors in Angular by using the `catchError` operator in RxJS. You can also create an HTTP interceptor to handle errors globally and provide user-friendly messages or retry logic. Example:
```javascript
import { catchError } from 'rxjs/operators';
import { throwError } from 'rxjs';

this.http.get('/api/data').pipe(
  catchError(error => {
    console.error('Error occurred:', error);
    return throwError(error);
  })
);

```

## 47. How does Angular's HTTP client handle observables?
**Answer:**
Angular’s HTTP client returns observables from HTTP methods like `get`, `post`, etc. These observables can be manipulated using RxJS operators, allowing for advanced handling of asynchronous data, error handling, and retries.
