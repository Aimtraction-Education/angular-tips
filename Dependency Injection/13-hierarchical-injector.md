# How do you use hierarchical injectors in Angular?
In Angular, hierarchical injectors allow you to create multiple instances of the same service, each with its own set of dependencies and configuration. This is useful when you have different components or modules that require different instances of a service.

To use hierarchical injectors in Angular, you can create a new instance of an injector using the createInjector() method of the Injector class. This method takes an array of providers as an argument, which defines the set of dependencies and configuration for the new injector.

Here's an example of how to use hierarchical injectors in Angular:


```typescript
import { Component, Injector } from '@angular/core';

import { Injectable } from '@angular/core';

@Injectable()
class MyService {
  message: string;
}

@Component({
  selector: 'my-app',
  template: `
    <button (click)="onClick()">Click me</button>
  `,
  providers: [{ provide: MyService, useValue: { message: 'App Component' } }],
})
export class AppComponent {
  constructor(private injector: Injector) {
    // get an instance of MyService from the new injector
    const myService = injector.get(MyService);

    // call a method on MyService
    console.log(myService.message); // output: "App Component"
  }

  onClick() {
    // create a new injector with a different value for MyService
    const newInjector = Injector.create(
      [{ provide: MyService, useValue: { message: 'Button Clicked' } }],
      this.injector
    );

    // get an instance of MyService from the new injector
    const myService = newInjector.get(MyService);

    // call a method on MyService
    console.log(myService.message); // output: "Button Clicked"
  }
}
```

In this example, we have an AppComponent that provides an instance of MyService with a default message of "App Component". When the button is clicked, we create a new injector with a different value for MyService, and then retrieve an instance of MyService from the new injector. We can then call a method on the new instance of MyService and get a different message based on the configuration of the new injector.