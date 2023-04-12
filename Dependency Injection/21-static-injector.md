# What is a static injector in Angular?


## Example 1

```typescript
import { Injector } from '@angular/core';

// create a static injector
const injector = Injector.create({
  providers: [
    { provide: 'name', useValue: 'John Doe' },
    { provide: 'age', useValue: 30 },
    { provide: 'city', useValue: 'New York' }
  ]
});

// retrieve values from the injector
const name = injector.get('name');
const age = injector.get('age');
const city = injector.get('city');

console.log(name); // John Doe
console.log(age); // 30
console.log(city); // New York
```

In this example, we're using the `Injector` class to create a static injector with three value providers: `name`, `age`, and `city`. We then use the `get` method of the injector to retrieve the values of these providers and log them to the console. This is just a simple example, but static injectors can be very useful for providing dependencies in non-Angular contexts, such as in server-side rendering or testing.


## Example 2
```typescript
import { Injector } from '@angular/core';

// Define a token for a configuration object
const CONFIG_TOKEN = new InjectionToken<any>('app.config');

// Define a factory function to create the configuration object
export function configFactory() {
  return {
    apiUrl: 'https://api.example.com',
    debug: true
  };
}

// Create a static injector with the configuration object as a value provider
const injector = Injector.create({
  providers: [
    { provide: CONFIG_TOKEN, useFactory: configFactory }
  ]
});

// Use the injector to retrieve the configuration object
const config = injector.get(CONFIG_TOKEN);

// Log the configuration object
console.log(config);
```

In this example, we define an `InjectionToken` for a configuration object and a factory function to create the object. We then create a static injector with the configuration object as a value provider using the `Injector.create()` method. Finally, we use the injector to retrieve the configuration object and log it to the console.


## Example 3

Let's say we have a service called `UserService` that is responsible for fetching user data from an external API. This service requires an access token to authenticate requests to the API, which is obtained from a separate service called `AuthService`. We want to use a static injector to manually provide an instance of `AuthService` to `UserService` without injecting it via constructor or using the `@Injectable` decorator.

Here's how we can do it:

```typescript
import { Injectable, Injector } from '@angular/core';
import { AuthService } from './auth.service';
import { HttpClient } from '@angular/common/http';

@Injectable({
  providedIn: 'root'
})
export class UserService {
  private accessToken: string;

  constructor(private http: HttpClient, private injector: Injector) {
    const authService = this.injector.get(AuthService);
    this.accessToken = authService.getAccessToken();
  }

  getUserData() {
    const headers = { 'Authorization': `Bearer ${this.accessToken}` };
    return this.http.get('/api/user', { headers });
  }
}
```

In this example, we inject the `Injector` service into `UserService` via constructor, which allows us to manually get an instance of `AuthService` using `this.injector.get(AuthService)`. We then use this instance to retrieve the access token, which is used to authenticate requests to the API.

Note that we do not need to use the `@Injectable` decorator for `AuthService`, since we are manually providing an instance of it through the static injector.