#### 基础类型

##### 定义布尔值 

```typescript
let isDone: boolean = false;
```

##### 定义数字

>  js 和 ts 所有数字都是浮点数，ts另外支持二进制和八进制字面量

```JS
let decLiteral: number = 6;
let hexLiteral: number = 0xf00d;
```

##### 定义字符串

```typescript
let name: string = 'bob';
```

##### 定义数组

```typescript
let list: number[] = [1, 2, 3];
let list: Array<number> = [1, 2, 3];

let readOnlyList: ReadonlyArray<number> = [1, 2, 3];// 定义只读数组
let anyList: any[] = [1, '2', true]
```

##### 定义元组 Tuple

> 定义一对值分别为 `string`和`number`类型的元组。

```typescript
let x: [string, number];

x = ['hello', 10]; // OK
// Initialize it incorrectly
x = [10, 'hello']; // Error
```

> 访问越界元素，会使用联合类型替代

```typescript
x[3] = 'world' // OK
x[6] = true // Error, 布尔不是（string | numner) 类型
```

##### 定义枚举

> 默认情况下，从0开始为元素编号，可以由枚举的值得到它的名字

```typescript
enum Color {Red, Green, Blue}
let c: Color = Color.Green

let ColorName: string = Color[2]
console.log(colorName); // 'Green'
```

##### 定义对象

> `object`表示非原始类型，也就是除`number`，`string`，`boolean`，`symbol`，`null`或`undefined`之外的类型，也就是说是object，或者array

```typescript
let myObj: object = {size: 10}
```

##### 定义任意类型 any

```typescript
let notSure: any = 4
notSure = 'may be a string instead'

let list: any[] = [1, true, 'false']
```

##### Void

> void 表示没有任何类型，当一个函数没有返回值时，其返回值类型就是void

```typescript
function warnUser(): void {
    console.log('This is my warning message')
}
```

##### Null和Undefined

> 默认情况下null 和 undefined是所有类型的子类型，可以将其复制给其他类型的变量

```typescript
let u: undefined = undefined
let n: null = null

```

##### Never

> 表示永不存在的值的类型，比如throw new Error

```typescript
// 返回never的函数必须存在无法达到的终点
function error(message: string): never {
    throw new Error(message);
}

// 推断的返回值类型为never
function fail() {
    return error("Something failed");
}

// 返回never的函数必须存在无法达到的终点
function infiniteLoop(): never {
    while (true) {
    }
}
```

##### 类型断言

> 在Typescript里使用jsx时，只可以用as语法

```typescript
let someValue: any = 'this is a  string';
// 尖括号写法
let strLength: number = (<string>someValue).length
// as 语法
let strLength: number = (someValue as string).length
```

#### 接口

##### 接口

```typescript
interface LabelledValue {
    label: string; // 必包含的属性，且类型为string，
    color?: number; // 可选属性
    [propName: string]: any // 额外属性
}

interface Point {
    readonly x: number; // 只读属性
}
let p1: Point = {x: 123}
p1.x = 4 // error
```

##### 函数类型

```typescript
interface SearchFunc {
    (source: string, subString: string): boolean
}
let mySeach: SeachFunc
mySeach = function (source: string, substring: string) {
    let result = source.search(substring)
    return result > -1
}
```

##### 可索引的类型

```typescript
interface StringArray {
    [index: number]: string;
}
let myArray: StringArray;
myArray = ["Bob", "Fred"];

let myStr: string = myArray[0];

interface NumberDictionary {
    [index: string]: number;
    length: number; // OK
    name: string;  // Error
}
```

##### 类类型

```typescript
interface ClockInterface {
    currentTime: Date;
    setTime(d: Date)
}
class Clock implements ClockInterface {
    currentTime: Date;
    setTime(d: Date) {
        this.currentTime = d
    }
    constructor(h: number, m: number) {
        
    }
}
```

```typescript
interface ClockConstructor {
    new (hour: number, minute: number): ClockInterface;
}
interface ClockInterface {
    tick()
}

function createClock(ctor: ClockConstructor, hour: number, munute: number): ClockInterface {
    return new ctor(hour, minute);
}
    
class DigitalClock implements ClockInerface {
	constructor(h: number, m: number) {}
    tick() {
        console.log("beep beep")
    }
}
class AnalogClock implements ClockInterface {
    constructor (h: number, m: number) {}
    tick() {
        console.log("tick tock");
    }
}
let digital = createClock(DigitalClock, 12, 17)
let analog = createClock(AnalogClock, 7, 21)
```

##### 继承接口

```typescript
interface Shape {
    color: string;
}
interface Square extends Shape {
    sideLength: number
}

let square = <Square>{};
square.color = 'blue';
square.sideLength = 10；
```

```typescript
interface Shap {
    color: string
}
interface PenStroke {
    penWidth: number
}
interface Square extends Shape, PenStroke {
    sideLength: number
}
let square = <Sauare>{}
```

##### 混合类型

> 一个对象可以同时做为函数和对象使用，并带有额外的属性。

```typescript
interface Counter {
    (start: number): string;
	interval: number;
	reset(): void;
}
function getCounter(): Counter {
    let counter = <Counter>function(start: number) {};
    counter.interval = 123;
    counter.reset = function () {};
    return counter;
}
let c = getCounter();
c(10)
```

##### 接口继承类

> 当接口继承了一个类类型时，它会继承类的成员但不包括其实现。

```typescript
class Control {
	private state: any;
}
interface SelectableControl extends Control {
    select(): void;
}
class Button extends Control implements SelectableControl {
    select () {}
}
class TextBox extends Control {
    select () {}
}

// 错误： Image类型缺少state属性
class Image implements SelectableControl {
    select() {}
}
class Location {
    
}
```

#### 类

##### 类

```typescript
class Greeter {
    greeting: string;
    constructor(message: string) {
        this.greeting = message
    }
    greet() {
        return "Hello, " + this.greeting;
    }
}
let greeter = new Greeter("world")
```

##### 继承

> private 不能被外界访问，protected 的成员在派生类中任然可以访问
>
> 构造函数也可以被标记成protected，代表这个类不能被实例化，但是可以被继承

```typescript
class Animal {
    name: string;
    constructor(theName: string) { this.name = theName; }
    move(distanceInMeters: number = 0) {
        console.log(`${this.name} moved ${distanceInMeters}m.`)
    }
}
class Snake extends Animal {
    constructor(name: string) { super(name) };
    move(distanceInMeters = 5) {
        console.log("Slithering...");
        super.move(distanceInMeters)
    }
}
class Horse extends Animal {
    constructor(name: string) { super(name) }
    move(distanceInMeters = 45) {
        console.log("Galloping...");
        super.move(distanceInMeters);
    }
}

let sam = new Snake("Samey the Python");
let tom: Animal = new Horse("Tommy the Palomino");

sam.move();
tom.move(34);
```

##### readonly修饰符

```typescript
class Octopus {
    readonly name: string;
    readonly numberOfLegs: number = 0;
    constructor (theName: string) {
        this.name = theName
    }
}
```

##### 参数属性

> 我们把声明和赋值合并至一处，对于publich，private， protected也是一样

```typescript
class Octopus {
    readonly numberOfLegs: number = 0;
    constructor(readonly name: string) {}
}
```

##### 存取器

```typescript
let passcode = 'secret passcode';
class Employee {
    private _fullName: string;
    get fullName(): string {
        return this._fullName
    }
    set fullName(newName: string) {
        if (passcode && passcode === 'secret passcode') {
            this._fullName = newName
        }
        else {
            console.log('Error: Unauthorized update of employee!');
        }
         
    }
}
let employee = new Employee();
employee.fullName = "Bob Smith";
if （employee.fullName) {
    alert(employee.fullName);
}
```

##### 静态属性

```typescript
class Grid {
    static origin = {x: 0, y: 0};
    calculateDistanceFromOrigin(point: {x: number; y: number}) {
        let xDist = (point.x - Grid.origin.x);
        let yDist = (point.y - Grid.origin.y);
        return Math.sqrt(xDist * xDist + yDist * yDist) / this.scale;
    }
    constructor(public scale: number)
}
```

##### 抽象类

> 抽象类中的抽象方法不包含具体实现并且必须在派生类中实现。 抽象方法的语法与接口方法相似。 两者都是定义方法签名但不包含方法体。 然而，抽象方法必须包含 `abstract`关键字并且可以包含访问修饰符。

```typescript
abstract class Animal {
    abstract makeSouce(): void;
    move(): void {
        console.log('roaming the earch...')
    }
}

abstract class Department {
    constructor(public name: string) {
        
    }
    printName (): void {
        console.log('Department name' + this.name);
    }
    abstract printMeeting(): void;
}

class AccountingDepartment extends Department {
    constructor() {
        super('Accounting and Auditing');
    }
    
    printMeeting(): void {
        console.log('The Accounting Department meets each Monday at 10am.')
    }
    
    generateReports(): void {
        console.log('Generating accounting reports...');
    }
}

let deparment: Department;
department = new Department(); // 错误，不允许创建一个抽象类的实例
department = new AccountingDepartment(); 
department.printName();
department.printMeeting();
department.generateReports() // 错误，方法在声明的抽象类中不存在
```

#### 泛型

```typescript
interface GenericIdentityFn {
    <T>(arg: T): T
}
interface GenericIdentityFnWithType<T> {
    (arg: T): T
}
function identify<T>(arg: T): T {
    return arg
}

let myIdentify: <T>(arg: T): T = identify
let myIdentify1：GenericIdentifyFn = identify
let myIdentify2: GenericIdentifyFnWithType<number> = identify

class GenericNumber<T> {
    zeroValue: T;
    add: (x:T, y:T) => T
}
/**
* 引用工厂函数时，引用构造函数的类类型
*/
function createInstance<A extends Animal>(c: new () => A): A {
    
}
```



#### 枚举

```typescript
enum Response {
    No = 0,
    Yes = 1
}
enum Direction {
    Up,
    Down,
    Left,
    Right
}

enum ShapeKind {
    Circle,
    Square
}

interface Circle {
    kind: ShapeKind.Circle;
    radius: number
}

// 反向映射
let a = Direction.Up
let nameOfA = Enum[a]

// const 枚举
const
```

