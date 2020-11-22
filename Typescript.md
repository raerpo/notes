## Typescript course

### Intro
To create a typescript project we need to install the typescript compiler with `npm install typescript`, after that we need a `tsconfig.json` that we can generate with the command:

```
npx tsc --init --rootDir src --outDir lib
```

With that command we will create a `tsconfig.json` file with a `rootDir` option setted to `src` and an output folder setted to `lib`.

After that, every **.ts** file that we write inside the `src` folder will be transpiled into javascript on the `lib` folder.

### Primitive types
- boolean
- number
- string

Two special types:
- `let notDefined = undefined;`
- `let notPresent = null;`

Also newer Javascript values like:
- `let penta = Symbol('star');`
- `let biggy = 24n; // Only available on ES2020`

### Instance types
We can also use classes to anotate types.

```
let regexp: RegExp = new RegExp('ab+c');
let array: Array<number> = [1, 2, 3];
```

It's possible to create class in typescript also:

```typescript
class Queue<T> {
    private data: Array<T> = [];
    push(item: T) { thiis.data.push(item); }
    pop(): T | undefined { return this.data.shift(); }
}

let queue: Queue<number> = new Queue();
```

### Arrays and tuples
We can anotate arrays like:

```typescript
let array: Array<number> = [1, 2, 3];
```

but because it's very common to use arrays typescript provide us a shorthand notation:

```typescript
let array: number[] = [1, 2, 3];
```

Every item of these array should be of the same type.

**tuples** are arrays of a fixed length like:

```typescript
let coordinates: [number, number] = [1,2];
```

It will fail if you pass more or less values, also if you pass values of different types.

### Object types and type aliases

To type an object we'd use a syntax similar to the object itself:

```typescript
let center: { x: number, y: number } = {
    x: 5,
    y: 5
}
```

we can use a **type alias** to avoid writing the type inline an also to avoid code duplication like this:

```typescript
type Point = { x: number, y: number };
let center: Point = { x = 5, y = 5 };
```

### functions
To type a function we can do it by typing its parameters and its return value.

```typescript
function add(a: number, b: number): number {
    return a + b;
}
```

if we have a function that does return anything we can use the special type called `void`:

```typescript
function print(text: string): void {
    console.log(text);
}
```

For example, we can type a function that uses a spreaded argument:

```typescript
function sum(...values: number[]): number {
    return values.reduce((acc, value) => acc + value, 0);
}
```

We can create add the type at definition and then use it:

```typescript
let add: (values: number[]) => number;
add = function(...values) {
    return values.reduce((acc, value) => acc + value, 0);
}
```

but the most common way is to use a type alias to do it:

```typescript
type Add = (values: number[]) => number;
const add: Add = function(...values) {
    return values.reduce((acc, value) => acc + value, 0);
}
```

### Structural typing

Typescript has something called **structural typing** also called **duck typing**. It means that if a type have the required values it doesn't matter if it have more values inside.

For example:

```typescript
type Point2D = { x: number, y: number };
type Point3D = { x: number, y: number, z: number };
```
Any `Point3D` value can be assign to a `Point2D` because it have its required values. It's not the case at assigning a `Point2D` into a `Point3D` because it's missing the `z` value.

### Classes

We can defined classes in typescript very similar to how we will use them in javascript.

```typescript
class Animal {
    name: string;
    constructor(name: string) {
        this.name = name;
    }
    move(distance: number): void {
        console.log(`${this.name} moved ${distance} meters`);
    }
}
```

In javascript every property and method are public but in typescript we can defined if that keeps being true.

We can set private properties:

```typescript
class Animal {
    private name: string;
    constructor(name: string) {
        this.name = name;
    }
    move(distance: number): void {
        console.log(`${this.name} moved ${distance} meters`);
    }
}
```

and in that case, this will throw a compile time error:

```typescript
let cat = new Animal('gardfield');
console.log(cat.name) // will throw a compile error
```

If we still want to have a private property but that be use when we extend the class we can use `protected` instead of `private`.

```typescript
class Animal {
    protected name: string;
    constructor(name: string) {
        this.name = name;
    }
    move(distance: number): void {
        console.log(`${this.name} moved ${distance} meters`);
    }
}

class Bird extends Animal {
    fly(distance: number): void {
        console.log(`${this.name} flew ${distance} meters`);
        // This is allow because this.name is protected instead of private
    }
}
```

`private` block every try to read some value outside the class, but `protected` allow to access the property or method within the class or its subclasses.

### Target compiler option

By using the **target** property inside **compilerOptions** we can specify in which runtime out javascript compiled code will run.

So for example, if we use a class with a private or protected properties and we target `ES5` our code will be transpiled into a function with it's prototype modified. But if we write the upcoming `#` prefix to denote a private property (an stage 3 feature) it can only be compile to `ES2015` since it depends con `WeakMap` that it's only available in that version but it will fail on `ES5` target since it cannot be compiled into that.

### Generics

Generics allow us to use type values that are not defined yet. For example, on a class Queue like this:

```typescript
class Queue {
    data = [];
    push(item) { this.data.push(item) }
    pop() { return this.data.shift() }
}
```

We cannot be sure of the methods that we can use on the returned values from `.pop` since we are no sure of the values'type that was add it inside. For this cases we can use generics.

```typescript
class Queue<T> {
    data: T[] = [];
    push(item: T) { this.data.push(item) }
    pop(): T | undefined { return this.data.shift() }
}
```

Now we will get proper validation if we instanciate Queue in any of this ways:

```typescript
let a = new Queue<number>();
let b = new Queue<string>();
```

### any and unknown
By typing something with `any` we can use everything we want and it works as an escape of the typing system. We can even access properties of `any` typed value and typescript won't complain.

```typescript
let a: any;
a.c.hello(); // typescript won't complain about this
```

With `unknown` we can also use anything but we need to do checks to make safer by ourselves.

```typescript
let b: unknown;
b.c.hello() // this will throw an error so we need to check its values manually
if (b.c && typeof b.c.hello === 'function') {
    b.c.hello();
}
```

`unknown` is very useful for refactorings from javascript to typescript since it will force you to add typeof conditions to make sure your code is correct without having to type everything.

for example if we want to create a function that logs something an use a method we can do something like this:

```typescript
function log(value: any) {
    console.log(value.toFixed(2)) // this will throw a runtime error if value is not a number
}
```

Instead we would want to do this:

```typescript
function log(value: unknown) {
    if (typeof value === 'number') {
        console.log(value.toFixed(2))
    } else {
        console.log(value);
    }
}
```

### type assertions

Somethings the compiler cannot infer the correct type of a variable but you are sure of the value that is coming. To avoid complaints from the compiler you can use a type assertion in which you tell the compiler the type that the variable should have.

```typescript
let hello = load();
const trimmed = hello.trim(); // The compiler will complaint here
```

we can tell the compiler that hello is in fact a string:

```typescript
let hello = load();
const trimmed = (hello as string).trim();
```

There's another syntaxis for type assertion `(<string>hello).trim()` but as it relays on angle brackets and this will not work on .tsx files it's better to go with the `as` assertion syntax.
Either way, it's better to add runtime checks if you're not 100% sure of the type of the variable:

```typescript
let hello = load();
if (typeof hello === 'string') {
    const trimmed = hello.trim();
}
```

### Type declarations

When you are using a variable that depends on a module without types or a variable that comes from the runtime like `process` inside a node application you can create your own declarations:

```typescript
console.log('Logged in user: ', process.env.USER); // This will failed since it doesn't recognize `process`
```

To fix this we can add a declaration for it:

```typescript
declare const proccess: any;
console.log('Logged in user: ', process.env.USER);
```

We should put our declarations in a file that ends with `d.ts` that are called **declaration files**.

In this case, the best way to go is to install the type declarations from a project called [definelyTyped](https://github.com/DefinitelyTyped/DefinitelyTyped) in this case the one that we need is `@types/node`.

### ts-node
instead of compiling our code every time to be able to run it with node we can run it directly by using **ts-node**.

`npx ts-node src/index.ts`

### readonly modifier

In javascript we can define variables that cannot be reassign by using `const` but this don't assure that you can modify it.

```typescript
type Point = {
    x: number,
    y: number
}

const myPoint: Point = {
    x: 10,
    y: 20
}

// even thought myPoint is a const we can do this without getting any errors

myPoint.x = 20;
myPoint.y = 30;
```

If we want to avoid this kind of behaviour at compile time we can add the modifier `readonly` to the type definition:

```typescript
type Point = {
    readonly x: number,
    readonly y: number
}
```

And by adding the modifier we will get a compile error if at any point in our code we try to modify those properties.

We can also use this modifier on class properties:

```typescript
class Animal {
    readonly name: string;
    constructor(name: string) {
        this.name = name;
    }
}

const sheep = new Animal('sheep');
sheep.name = 'cat'; // This is disallow by the readonly modifier
```

### Union types

We can define more than one type for our variables when it's need it. For example, a function that receives one string or an array of strings and we trim them and return them.

```typescript
function trimmer(input: any) {
    if (typeof input === 'string') {
        return input.trim();
    } else {
        return input.map((x: any) => x.trim());
    }
}
```

That won't give us any compile error if we use it like this:

```typescript
trimmer('hello');
trimmer(456);
```

but the second line will throw a runtime error. To avoid this we can define `input` as a string and an arry of strings.

```typescript
function trimmer(input: string | string[] ) {
    if (typeof input === 'string') {
        return input.trim();
    } else {
        return input.map(x => x.trim());
    }
}
```

Not only that change will give and error when we try to use something like `trimmer(456)` but also avoid us to define the type of `x` inside the map since typescript can infer that it's a variable of type `string`.

You can also create a type alias for this unions:

```typescript
type Padding = string | string[];
```

if your type gets to long you can separate it in multiple lines and even add a `|` at the beggining like this:

```typescript
type Padding = 
    | string
    | string[];
```

This is a totally valid TS syntax and it's easier to read.

### Literal types

A literal type is an exact value or group of values (using an union) that a variable can be assign. For example:

```typescript
type CardinalDirection = 'North' | 'South' | 'West' | 'East';

const myDirection: CardinalDirection = 'Here' // This will throw an error since CardinalDirection type only allows four finite possible vales
```

We can do this also with numbers, for example on a dice throwing function

```typescript
type DiceValue = 1 | 2 | 3 | 4 | 5 | 6;
```

### Type narrowing

When we have a union and add conditions to check the type, typescript can infer which type should have a value inside a condition. Take this code for example:

```typescript
function log(value: string | number) {
    if (typeof value === 'string') {
        console.log(value.trim()); // Inside this block, TS have narrow down the type of value to only string
    } else {
        console.log(value); // and in this block value is a number since the other possible value is covered on the first part of the conditional
    }
}
```

Another example of type narrowing could be seen in a function that calculates the area of a shape. Take this in consideration:

```typescript
type Square = {
    size: number
}

type Rectangle = {
    width: number,
    height: number
}

type Shape = 
    | Square 
    | Rectangle;

function getArea(shape: Shape) {
    if ('size' in shape) {
        return shape.size * shape.size;
    } 
    if ('width' in shape) {
        return shape.width * shape.height;
    }
}
```
In the first condition TS will know that shape is a Square and it will give us autocomplete and type safety for its type. But in the second condition it will know that it is a Rectangle.

### Discriminated unions

We can add a new property to our types that will help us to discriminate which type should be applied. And instead of checking for one of the properties like we did in our previous example we can do the check by using this new discrimination property.

```typescript
type Square = {
    type: 'square',
    size: number
}

type Rectangle = {
    type: 'rectangle',
    width: number,
    height: number
}

type Shape = Square | Rectangle;

function getArea(shape: Shape) {
    if (shape.type === 'square') {
        return shape.size * shape.size;
    }
    if(shape.type === 'rectangle') {
        return shape.width * shape.height;
    }
}
```

Now our code is more explicit on the checks that we do. The discrimination properties doesn't need to be a literal string. They can be of any type.

### Class parameter properties

Normally when we define a class that receive parameters we should define them in 3 places:

```typescript
class Person {
    name: string; // 1
    lastname: string;
    constructor(name: string, lastname: string) { //2
        this.name = name;  //3
        this.lastname = lastname;
    }
}
```

With **class parameter properties** when can write our properties and their types like this:

```typescript
class Person {
    constructor(public name: string, public lastname: string) {}
}
```

We need to specify the modifier on the constructor parameters and we can avoid all that boilerplate at the moment of creating a class.

### Checking for null or undefined

We can check if something is `null` or `undefined` by using double equal operator since:

```typescript
null == null // true
undefined == undefined // true
undefined == null // true

false == null // false
0 == null // false
'' == null // false
```

As we can see, `== null` checks safely for null and undefined without having false positives with other values

### Intersection types

Very often we have types that have a lot of similar properties and as we do with classes we can extend a type to create a new one. Let's take for example this two points:

```typescript
type Point2D = {
    x: number,
    y: number
}

type Point3D = {
    x: number,
    y: number,
    z: number
}
```

As you can see `Point3D` shares `x` and `y` with `Point2D` so instead of write those same proporties again we can extend `Point2D` to became 3D:

```typescript
type Point2D = {
    x: number,
    y: number
}

type Point3D = Point2D & {
    z: number;
}
```

This syntaxis is easier to remember if you read it as `Point3D is equals to Point2D AND z that is a number`

### Non-null assertions

Sometimes typescript won't be able to do a deep analysis to know that effectivaly a value is defined. To tell the compiler that the value exists we can use the **non-null operator**

```typescript
type Point = {
    x: number,
    y: number
}

let point: Point;
function init() {
    point = { x: 0, y: 0 };
}

init();
console.log(point.x, point.y); // This will throw an error even thought we know is defined
```

to fix this we can use the non-null operator like this:

```typescript
console.log(point!.x, point!.y);
```

now, in this case is easier to return the point instead of creating the variable outsise and avoid the undefined error.

### Types vs Interfaces

Interfaces are mostly used because of declaratioin merging and familirity with the extend keyword from other languages.

Types support unions, intersections, primitives, etc...

I can conclude from this that it's better to use types instead of interfaces

### Never type

the `never` type is used in cases that will never happen. Take this function in consideration:

```typescript
const myFunction = (value: string) => {
    throw new Error();
}
```

That function will have a return type of `never` since typescript can infer that the function will never return anything.

Only `never` can be assign to a never type so it's not so evident which use can be give to it. Take this for example:

```typescript
type Square = {
    kind: "square",
    size: number
}

type Rectangle = {
    kind: "rectangle",
    width: number,
    height: number
}

type Shape = Square | Rectangle;

function area(s: Shape) {
    if (s.kind === 'square') {
        return s.size * s.size;
    } else if (s.kind === 'rectangle') {
        return s.width * s.height;
    }
    const _ensureAllCasesAreHandle: never = s;
}
```

We add the variable `_ensureAllCasesAreHandle` with the only purpose of giving us a compile error in case not all cases are handle because if there's a case that doesn't have a `if` it will be assign to the variable and the compiler will complain.

### Implement keyword

We can make sure that a class implements a type and give us a nice compile time error with that doesn't happen.

```typescript
type Animal = {
    name: string,
    voice(): string
}

class Cat implements Animal {

}
```

In that example, the class Cat will give us a runtime error since it's missing the name property and the voice method. To fix it we can add what it's missing to ensure that it match the Animal implementation:

```typescript
class Cat implements Animal {
    contructor(public name: string) {}
    voice() {
        return 'meow';
    }
}
```