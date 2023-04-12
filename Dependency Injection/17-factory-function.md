# How can you use a factory function to provide a dependency in Angular?

## Example 1
Let's say we have a simple service `LoggerService` that logs messages to the console, and we want to create a factory function to provide a new instance of this service with a prefix added to each message.

First, we define the factory function as follows:

```typescript
function loggerFactory(prefix: string) {
  return new LoggerService(prefix);
}
```
Here, `loggerFactory` takes a `prefix` string as a parameter and returns a new instance of `LoggerService` with the prefix added.

Next, we define a provider for this factory function:

```typescript
{
  provide: LoggerService,
  useFactory: loggerFactory,
  deps: ['prefix'],
},
{
  provide: 'prefix',
  useValue: 'CUSTOM_PREFIX',
}
```
Here, we're providing `LoggerService` using the `useFactory` property, which tells Angular to use the `loggerFactory` function to create a new instance of `LoggerService`. We also define a dependency `prefix` which is provided by a separate provider with the useValue property.

Now, when we inject `LoggerService` into a component or service, Angular will use our `loggerFactory` function to create a new instance of the service with the prefix `'CUSTOM_PREFIX'`.

## Example 2

```typescript
@Injectable({
  providedIn: 'root',
  useFactory: (http: HttpClient) => {
    if (environment.production) {
      return new ProductionService(http);
    } else {
      return new DebugService(http);
    }
  },
  deps: [HttpClient]
})
export abstract class AppService {
  // ...
}
```
In this example, the `useFactory` property of the `@Injectable` decorator is used to create an instance of either `ProductionService` or `DebugService` based on the value of the `environment.production` flag. The `deps` property is used to specify that the factory function requires an instance of the `HttpClient` class to be injected as a parameter. The resulting instance of `AppService` is then provided as a multi-provider, allowing it to be used as a dependency in other parts of the application.