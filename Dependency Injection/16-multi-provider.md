In Angular's DI system, a multi-provider is a way to register multiple providers for the same token. It allows different parts of the application to provide different implementations for the same token, and the DI system will inject all of them as an array.

The `multi` option is used to register a provider as a multi-provider. It can be used with any provider decorator, including `@Injectable`, `@Directive`, `@Component`, and `@Pipe`.

Here's an example of how to use `multi` to provide multiple values for the same token:

```typescript
import { InjectionToken } from '@angular/core';

export const MY_TOKEN = new InjectionToken<string>('my-token');

const provider1 = { provide: MY_TOKEN, useValue: 'value1', multi: true };
const provider2 = { provide: MY_TOKEN, useValue: 'value2', multi: true };
const provider3 = { provide: MY_TOKEN, useValue: 'value3', multi: true };

@Component({
  selector: 'my-component',
  providers: [provider1, provider2, provider3]
})
export class MyComponent {
  constructor(@Inject(MY_TOKEN) private myTokenValues: string[]) {
    console.log(myTokenValues); // Output: ['value1', 'value2', 'value3']
  }
}
```
In this example, we define `MY_TOKEN` as an `InjectionToken` and then create three providers, each with a different value for the `useValue` property and the multi option set to `true`. We then provide these providers in the `providers` array of the `@Component` decorator.

When we inject `MY_TOKEN` in the constructor of `MyComponent`, the DI system will provide an array of all the values for the token (`['value1', 'value2', 'value3']`) because each provider is marked as a multi-provider.