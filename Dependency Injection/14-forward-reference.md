# How do you use hierarchical injectors in Angular?

Suppose we have two services, ServiceA and ServiceB, and ServiceA depends on ServiceB:

```typescript
@Injectable()
export class ServiceB {}

@Injectable()
export class ServiceA {
  constructor(private serviceB: ServiceB) {}
}
```
Now let's say we want to inject ServiceA into ServiceB. We might try to do it like this:

```typescript
@Injectable()
export class ServiceB {
  constructor(private serviceA: ServiceA) {}
}

@Injectable()
export class ServiceA {
  constructor(private serviceB: ServiceB) {}
}
```
However, this will cause a circular dependency error. To solve this, we can use a forward reference:

```typescript
@Injectable()
export class ServiceB {
  constructor(@Inject(forwardRef(() => ServiceA)) private serviceA: ServiceA) {}
}

@Injectable()
export class ServiceA {
  constructor(private serviceB: ServiceB) {}
}
```
Here, we use the forwardRef function to create a forward reference to ServiceA. This allows us to inject ServiceA into ServiceB without causing a circular dependency error.