# How can you use the @SkipSelf decorator in Angular's DI system?

Let's say we have the following component hierarchy:

```typescript
// app.component.ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  template: `
    <h1>App Component</h1>
    <app-child></app-child>
  `
})
export class AppComponent {}

// child.component.ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-child',
  template: `
    <h2>Child Component</h2>
    <app-grandchild></app-grandchild>
  `
})
export class ChildComponent {
  constructor(private message: string) {}
}

// grandchild.component.ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-grandchild',
  template: '<h3>Grandchild Component</h3>'
})
export class GrandchildComponent {
  constructor(@SkipSelf() private message: string) {}
}
```

In this example, we want to inject a `string` value into the `ChildComponent` and the `GrandchildComponent`. However, we only want to provide the value in the `AppComponent`, and not in any of its child components.

To do this, we can use the `@SkipSelf` decorator in the constructor of the `GrandchildComponent`. This tells Angular to skip the current injector and look for the value in the parent injector.

Here's how we can provide the value in the `AppComponent`:

```typescript
import { NgModule } from '@angular/core';
import { AppComponent } from './app.component';
import { ChildComponent } from './child.component';
import { GrandchildComponent } from './grandchild.component';

@NgModule({
  declarations: [
    AppComponent,
    ChildComponent,
    GrandchildComponent
  ],
  providers: [
    { provide: 'message', useValue: 'Hello from App Component' }
  ],
  bootstrap: [AppComponent]
})
export class AppModule {}
```
With this configuration, the `ChildComponent` will get the `message` value injected directly from the `AppComponent` injector, but the `GrandchildComponent` will skip the `ChildComponent` injector and get the value from the `AppComponent` injector.