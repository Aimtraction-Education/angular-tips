# №13 How do you implement a custom provider in Angular?

To implement a custom provider in Angular, you need to define it in the providers array of the NgModule metadata. This can be done in two ways:

Use a class as a provider: You can provide a class as a provider by adding it to the providers array of your NgModule. When a class is added to the providers array, Angular creates an instance of that class for each component that requests it. For example:
```typescript
import { NgModule } from '@angular/core';
import { MyService } from './my-service';

@NgModule({
  providers: [MyService],
  // ...
})
export class MyModule { }
```
Use a value as a provider: You can also provide a value directly instead of a class by using the useValue property of the provider object. This can be useful when you want to provide a constant value that doesn't need to be instantiated. For example:

```typescript
import { NgModule } from '@angular/core';

@NgModule({
  providers: [
    { provide: 'API_URL', useValue: 'http://localhost:3000' }
  ],
  // ...
})
export class MyModule { }
```

In the above example, we are providing a constant value of http://localhost:3000 as a string using the token 'API_URL'.

Once you have defined your custom provider, you can inject it into your components, services, or other providers using the constructor of the class. For example:

```typescript
import { Component } from '@angular/core';
import { MyService } from './my-service';

@Component({
  selector: 'app-my-component',
  template: '<p>{{myService.getMessage()}}</p>'
})
export class MyComponent {
  constructor(private myService: MyService) { }
}
```
In the above example, we are injecting the MyService provider into the MyComponent component by adding it to the constructor parameters.