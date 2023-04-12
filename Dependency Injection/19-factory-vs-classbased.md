# What is the difference between a factory function and a class-based provider in Angular?

```typescript
// useClass provider
class UserService {
  getUser() {
    return { name: 'John Doe', age: 30 };
  }
}

@Injectable({ providedIn: 'root' })
class UserComponent {
  constructor(private userService: UserService) {}

  ngOnInit() {
    const user = this.userService.getUser();
    console.log(user); // { name: 'John Doe', age: 30 }
  }
}

// useFactory provider
function getUserService() {
  return new UserService();
}

@Injectable({
  providedIn: 'root',
  useFactory: getUserService
})
class UserComponent {
  constructor(private userService: UserService) {}

  ngOnInit() {
    const user = this.userService.getUser();
    console.log(user); // { name: 'John Doe', age: 30 }
  }
}
```

In this example, we have a `UserService` class that provides a `getUser` method that returns a user object. We then have a `UserComponent` that injects this `UserService` and logs the user to the console.

We first provide the `UserService` using the `useClass` syntax, where we simply specify the class as the provider. We then create an instance of the `UserComponent`, which logs the user object to the console.

Next, we provide the `UserService` using the `useFactory` syntax, where we define a factory function that returns a new instance of the `UserService`. We then create another instance of the `UserComponent`, which again logs the user object to the console.

Both approaches achieve the same result, but the `useFactory` approach allows for more flexibility in how the service is instantiated, as we can customize the factory function to create the service instance in a more complex or dynamic way.