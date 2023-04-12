# How do you use the @Host decorator in Angular's DI system?

Here is an example of how to use the @Host decorator to limit the scope of dependency lookup:

```typescript
import { Component, Host, Inject } from '@angular/core';
import { AppConfig } from './app-config';

@Component({
  selector: 'app-root',
  template: `<app-child></app-child>`
})
export class AppComponent {
  constructor(@Host() @Inject(AppConfig) private config: AppConfig) {}
}

@Component({
  selector: 'app-child',
  template: ``
})
export class ChildComponent {
  constructor(@Inject(AppConfig) private config: AppConfig) {}
}
```
In this example, the `AppComponent` requests an instance of AppConfig using `@Host` and `@Inject`. Angular will search for the provider starting at AppComponent, then moving up the component tree until it finds a matching provider or reaches the top level. Since there is a provider for `AppConfig` at the `AppComponent` level, the dependency is resolved successfully.

On the other hand, the `ChildComponent` requests an instance of `AppConfig` without using `@Host`. Angular will search for the provider starting at `ChildComponent`, then moving up the component tree until it finds a matching provider or reaches the top level. Since there is no provider for `AppConfig` at the `ChildComponent` level, Angular will continue to search up the component tree and eventually find the provider at the `AppComponent` level.