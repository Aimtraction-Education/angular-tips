# How can you use the @Optional and @Self decorators in Angular's DI system?

## @Optional decorator

Let's say we have a component MyComponent that requires an instance of MyService, but it's possible that MyService may not have been provided by any module or component in the hierarchy. We can use the @Optional decorator to allow for a null value to be passed as the dependency, like this:

```typescript
import { Component, Optional } from '@angular/core';
import { MyService } from './my.service';

@Component({
  selector: 'app-my-component',
  template: '<p>{{ message }}</p>'
})
export class MyComponent {
  message: string;

  constructor(@Optional() private myService: MyService) {
    if (this.myService) {
      this.message = this.myService.getMessage();
    } else {
      this.message = 'MyService is not available.';
    }
  }
}
```

In this example, we use the @Optional decorator to allow for the MyService dependency to be null if it's not available. If the dependency is null, we simply display a message indicating that it's not available. If the dependency is not null, we call the getMessage() method on the MyService instance to get a message to display.

# @Self decorator

```typescript
import { Component, Self } from '@angular/core';
import { MyService } from './my.service';

@Component({
  selector: 'app-my-component',
  template: '<h1>{{ message }}</h1>',
  providers: [{ provide: MyService, useValue: { message: 'Hello from MyService!' } }]
})
export class MyComponent {
  message: string;

  constructor(@Self() private myService: MyService) {
    this.message = this.myService.message;
  }
}
```

In this example, we have a component that depends on a MyService. We provide an instance of MyService to the component using the providers array. However, if there is another provider for MyService higher up in the injector hierarchy, we want to make sure that the component only gets the provider that it explicitly asks for.

We use the @Self decorator on the constructor parameter to tell Angular to only look for a MyService provider in the component's injector, and not to look for a provider higher up in the hierarchy. If the component's injector doesn't have a provider for MyService, the @Self decorator will cause Angular to throw an error.