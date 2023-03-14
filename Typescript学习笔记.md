### 1，数据类型
#### 原始数据类型（number bool string null undefined object symbol）
```typescript
let a1: string = 'xxx'
let a2: number = 1
let a4: boolean = false
let a5: object = {}
let a3: null = null
let a6: undefined = undefined
let fibonacci: number[] = [1, 1, 2, 3, 5];

```
#### 任意值any
任意值（Any）表示允许被赋值为任意类型。
如果是一个普通类型，在赋值过程中改变类型是不被允许的：
```typescript
let myFavoriteNumber: string = 'seven';
myFavoriteNumber = 7;

// index.ts(2,1): error TS2322: Type 'number' is not assignable to type 'string'.
```
但如果是 any 类型，则允许被赋值为任意类型。
```typescript
let myFavoriteNumber: any = 'seven';
myFavoriteNumber = 7;
```
变量如果在声明的时候，未指定其类型，那么它会被识别为任意值类型
```typescript
let something;
something = 'seven';
something = 7;
```

#### 空值void
```typescript
function alertName(): void {
    alert('My name is Tom');
}
```
#### 枚举（Enum）
用于取值被限定在一定范围内的场景，比如一周只能有七天，颜色限定为红绿蓝等。
```typescript
//枚举  
enum Gender {
  Man = 'man',
  Woman = 'woman',
}

let gender: Gender = Gender.Man
console.log(gender); //man
gender = Gender.Woman;
console.log(gender); //woman
```
#### 联合类型
联合类型（Union Types）表示取值可以为多种类型中的一种。
```typescript
let myFavoriteNumber: string | number;
myFavoriteNumber = 'seven';
myFavoriteNumber = 7;
```
### 2，函数
#### 函数的声明
```typescript

function add(a:number, b:number = 1): number {
    return a + b
}
```
#### 可选参数
```typescript
function buildName(firstName: string, lastName?: string) {
    if (lastName) {
        return firstName + ' ' + lastName;
    } else {
        return firstName;
    }
}
```
#### 函数重载
前几次都是函数定义，最后一次是函数实现
```typescript
 function add(n1: number, n2: number);
 function add(n1: string, n2: string);
 function add(n1, n2) {
 	return n1 + n2;
 }

 add(1, 2); // 3
 add('frank', 'jack'); // 'frankjack'
```
### 3，接口
接口：用代码描述一个对象必须有什么属性或者方法。接口一般首字母大写
```typescript
interface Person {
    name: string,
    age: number,
}
const zhangsan: Person  = {name: 'zhangsan', age: 18 }

//属性不能多也不能少
let tom: Person = {
    name: 'Tom',
    age: 25,
    gender: 'male'
};

// index.ts(9,5): error TS2322: Type '{ name: string; age: number; gender: string; }' is not assignable to type 'Person'.
//   Object literal may only specify known properties, and 'gender' does not exist in type 'Person'.
```
#### 只读属性和可选属性
```typescript
interface Person {
    readonly id: number;
    name: string;
    age?: number;
}

let tom: Person = {
    id: 89757,
    name: 'Tom',
    gender: 'male'
};

tom.id = 9527;

// index.ts(14,5): error TS2540: Cannot assign to 'id' because it is a constant or a read-only property.
```
#### 接口可以继承
```typescript
interface Shape {
    color: string;
}

interface Square extends Shape {
    sideLength: number;
}

let square:Square = {
	color: 'red',
  sideLength: 10
} 
```
### 4, 类型别名
```typescript
type StrOrNum = string | number;

// 使用
let sample: StrOrNum;
sample = 123;
sample = '123';

// 会检查类型
sample = true; // Error

```
```typescript
type Name = string;
type NameResolver = () => string;
type NameOrResolver = Name | NameResolver;

function getName(n: NameOrResolver): Name {
    if (typeof n === 'string') {
        return n;
    }
    else {
        return n();
    }
}
```
### 5，类
```typescript
class Animal {
  name: string;
  age: number
  constructor(name: string, age:number) {
    this.name = name;
    this.age = age
  }
  sayHi(): string {
    return `My name is ${this.name}`;
  }
}

let a: Animal = new Animal('Jack', 18);
console.log(a.sayHi()); // My name is Jack
```
#### 抽象类
抽象类是不允许被实例化，只能被继承
抽象类中的抽象方法必须被子类实现
```typescript
abstract class Animal {
  name;
  constructor(name) {
    this.name = name;
  }
  abstract sayHi();
}

class Cat extends Animal {
  sayHi() {
    console.log(`Meow, My name is ${this.name}`);
  }
}

let cat = new Cat('Tom');
```
### 6，类型推论和断言
如果没有明确的指定类型，那么 TypeScript 会依照类型推论（Type Inference）的规则推断出一个类型。
```typescript
let myFavoriteNumber = 'seven';
myFavoriteNumber = 7;

// index.ts(2,1): error TS2322: Type 'number' is not assignable to type 'string'.
```
断言：确定一个变量是某个类型
两种使用方式：
1，值 as 类型
2，<类型>值
```typescript
(<string>a).length;
(a as string).length;
```
```typescript
interface Cat {
    name: string;
    run(): void;
}
interface Fish {
    name: string;
    swim(): void;
}

function fn(animal: Cat | Fish) {
  // animal.swim() 会报错
	(animal as Fish).swim()
}  
```
### 7，泛型
泛型：广泛的类型。不预先指定具体的类型，而在使用的时候再指定类型。
```typescript
function createArray(length: number, value: any): Array<any> {
    let result = [];
    for (let i = 0; i < length; i++) {
        result[i] = value;
    }
    return result;
}

createArray(3, 'x'); // ['x', 'x', 'x']
```
这段代码有一个缺点，它并没有准确的定义返回值的类型：
```typescript
function createArray<T>(length: number, value: T): Array<T> {
    let result: T[] = [];
    for (let i = 0; i < length; i++) {
        result[i] = value;
    }
    return result;
}

createArray<string>(3, 'x'); // ['x', 'x', 'x']

```
上例中，我们在函数名后添加了 <T>，其中 T 是指输入的类型，在后面的输入 value: T 和输出 Array<T> 中即可使用了。

`createArray(3, 'x'); // ['x', 'x', 'x']`

#### 泛型约束
```typescript
function fn<T>(arg: T): T{
    console.log(arg.length) // error
    return arg;
}
```
```typescript
interface HasLength{
    length: number
}

function fn<T extends HasLength>(arg: T): T{
    console.log(arg.length) // no error
    return arg;
}
```
### 8，TS X React
#### 函数式组件的声明方式：
```typescript
type AppProps = {
  message: string
  children?: React.ReactNode
}

const App = ({ message, children }: AppProps) => (
  <div>
    {message}
    {children}
  </div>
)
```
#### Hooks
**useState<T>**
```typescript
// `val`会推导为boolean类型， toggle接收boolean类型参数
const [val, toggle] = React.useState(false)
// obj会自动推导为类型: {name: string}
const [obj] = React.useState({ name: 'sj' })
```
**useRef<T>**
```typescript
const ref1 = React.useRef<HTMLInputElement>(null)
```
#### 常用Props TS类型
##### 基础属性类型
```typescript
type AppProps = {
  message: string
  count: number
  disabled: boolean
  names: string[]
  status: 'waiting' | 'success'
  obj: object
  obj2: {}
  obj3: {
    id: string
    title: string
  }
  objArr: {
    id: string
    title: string
  }[]
  dict1: {
    [key: string]: MyTypeHere
  }
  /** 任意完全不会调用的函数 */
  onSomething: Function
  /** 没有参数&返回值的函数 */
  onClick: () => void
  /** 携带参数的函数 */
  onChange: (id: number) => void
  /** 携带点击事件的函数 */
  onClick(event: React.MouseEvent<HTMLButtonElement>): void
  optional?: OptionalType
}
```
##### 常用 React 属性类型
```typescript
export declare interface AppBetterProps {
  children: React.ReactNode 
  functionChildren: (name: string) => React.ReactNode
  style?: React.CSSProperties // 传递style对象
  onChange?: React.FormEventHandler<HTMLInputElement>
}

export declare interface AppProps {
  children1: JSX.Element // 差, 不支持数组
  children2: JSX.Element | JSX.Element[] // 一般, 不支持字符串
  children3: React.ReactChildren // 忽略命名，不是一个合适的类型，工具类类型
  children4: React.ReactChild[] // 很好
  children: React.ReactNode // 最佳，支持所有类型 推荐使用
  functionChildren: (name: string) => React.ReactNode // recommended function as a child render prop type
  style?: React.CSSProperties // 传递style对象
  onChange?: React.FormEventHandler<HTMLInputElement> // 表单事件, 泛型参数是event.target的类型
}
```
#### 使用 Type（类型别名） 还是 Interface？
有几种常用规则：

- 在定义公共 API 时(比如编辑一个库）使用 **interface**，这样可以方便使用者继承接口
- 在定义组件属性（Props）和状态（State）时，建议使用 **type**，因为 **type**的约束性更强

**interface** 和 **type** 在 ts 中是两个不同的概念，但在 React 大部分使用的 **case** 中，**interface** 和 **type** 可以达到相同的功能效果，**type** 和 **interface 最大的区别**是：

- **type** 类型不能二次编辑，而 **interface** 可以随时扩展

  


### 参考文章
1， [https://ts.xcatliu.com/](https://ts.xcatliu.com/)
2，[https://jkchao.github.io/typescript-book-chinese/](https://jkchao.github.io/typescript-book-chinese/)
