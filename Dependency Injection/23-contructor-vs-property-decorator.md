# What is the difference between a constructor parameter decorator and a property decorator in Angular's DI system?

In Angular's DI system, a constructor parameter decorator is used to declare the dependency injection token for a constructor parameter, while a property decorator is used to declare the dependency injection token for a class property.

The main difference between these two decorators is that the constructor parameter decorator is used to inject dependencies into the constructor of a class, while the property decorator is used to inject dependencies into the properties of a class.

When using the constructor parameter decorator, the dependency injection token is specified as a parameter of the constructor function. For example:

```typescript
class MyComponent {
  constructor(@Inject('myToken') private myService: MyService) { }
}
```

In this example, the `@Inject` decorator is used to specify the dependency injection token for the `myService` parameter of the `MyComponent` constructor.

On the other hand, when using the property decorator, the dependency injection token is specified as a decorator on the class property. For example:

```typescript
class MyComponent {
  @Inject('myToken') private myService: MyService;
}
```
In this example, the `@Inject` decorator is used to specify the dependency injection token for the `myService` property of the `MyComponent` class.

Overall, the choice between using a constructor parameter decorator or a property decorator in Angular's DI system depends on the specific use case and design of the application.



