##TypeScript简介

[TypeScript](http://www.typescriptlang.org/) 是微软开源的 JavaScript 的一个超集，它提供了**类型系统**，弥补了JavaScript这门动态弱类型语言的不足。它可以编译成纯 JavaScript。编译出来的 JavaScript 可以运行在任何浏览器上。TypeScript 编译工具可以运行在任何服务器和任何系统上。

使用TypeScript很大的**增强了js代码的可读性和可维护性**。它在编译阶段就能发现语法错误，配合编辑器的智能提醒，在开发阶段就能杜绝大部分的语法错误。

相比于JavaScript, TypeScript 更可控、更容易重构、更适合大型项目、更容易维护。

TypeScirpt可以通过npm安装

`npm install -g typescript`

安装后，可以通过`tsc`命令编译.ts后缀的TypeScript文件。

## TypeScript常用语法

### 类型

TypeScript 支持与 JavaScript 几乎相同的数据类型。包括：布尔值、数字、字符串、数组、null和undefined等

```typescript
let flag:boolean = true
let num: number = 123
let str: string = 'abc'
let list: number[] = [1, 2, 3]
let strList: Array<string> = ['a', 'b', 'c']
let u: undefined = undefined
let n: null = null
let obj: object = {id: 1}
```

默认情况下，null和undefined可以赋值给其他声明了类型的变量，但如果编译的时候加上`--strictNullChecks`就会报错。

TypeScript还可以定义ECMAScript 标准提供的内置对象为类型，比如：`Boolean`、`Error`、`Date`、`RegExp` 等；以及DOM 和 BOM 提供的内置对象，如：`Document`、`HTMLElement`、`Event`、`NodeList` 等。

```typescript
let b: Boolean = new Boolean(1);
let e: Error = new Error('Error occurred');
let d: Date = new Date();
let r: RegExp = /[a-z]/;

let body: HTMLElement = document.body;
let allDiv: NodeList = document.querySelectorAll('div');
document.addEventListener('click', function(e: MouseEvent) {
  // Do something
});
```

如果一个变量没有声明类型，那它第一次赋值后，TypeScript会默认给它加上该值对应的类型，如果给这个变量赋值其他类型的值时，会报错。

```typescript
let a = 123
a = 'abc' // 会报错
```



此外，typescript还提供了其他一些类型：

**元组**

元组类型允许表示一个**已知元素数量和类型的数组**，各元素的类型不必相同。 比如，可以定义一对值分别为 `string` 和 `number` 类型的元组。

```typescript
let x: [string, number]
x = ['hello', 10] // OK
x = [10, 'hello'] // Error
```

访问一个已知索引的元素，会得到正确的类型。

```typescript
console.log(x[0].substr(1)) // OK
console.log(x[1].substr(1)) // Error, 'number' 不存在 'substr' 方法
```

可以只给元组中一项单独赋值

```typescript
x[0] = 'test'
```

但是当直接对元组类型的变量进行初始化或者赋值的时候，需要提供所有元组类型中指定的项。

```typescript
let x: [string, number]
x = ['abc', 123] // right
x = ['abc'] // wrong
```

**枚举**

使用枚举类型可以为一组数值赋予名字，适合用于取值被限定在一定范围内的场景，比如星期：

```typescript
enum Days {Sun, Mon, Tue, Wed, Thu, Fri, Sat};
console.log(Days["Sun"]); // 0
console.log(Days["Mon"]); // 1
console.log(Days["Tue"]); // 2
console.log(Days["Sat"]); // 6

console.log(Days[0]); // "Sun"
console.log(Days[1]); // "Mon"
console.log(Days[2]); // "Tue"
console.log(Days[6]); // "Sat"
```

枚举成员默认会被赋值为从 `0` 开始递增的数字，同时也会对枚举值到枚举名进行反向映射。

可以给枚举项手动赋值, 未手动赋值的枚举项会接着上一个枚举项递增（+1）。

```typescript
enum Days {Sun = 7, Mon = 1, Tue, Wed, Thu, Fri, Sat};

console.log(Days["Sun"] === 7); // true
console.log(Days["Mon"] === 1); // true
console.log(Days["Tue"] === 2); // true
console.log(Days["Sat"] === 6); // true
```

手动赋值的枚举项可以不是数字，此时需要使用类型断言来让 tsc 无视类型检查

```typescript
enum Days {Sun = 7, Mon, Tue, Wed, Thu, Fri, Sat = <any>"S"};
```

**any**

`any` 类型用来表示一个变量可以是任意类型。

比如一个数组可能有多个类型的元素，那可以用any来定义它。

```typescript
let list: any[] = [123, 'abc', true, {}]
```

**void**

`void` 类型表示没有任何类型。

比如一个函数没有返回值时：

```typescript
function sayHello(): void {
  console.log('Hello!')
}
```

声明 `void` 类型的变量，只能被赋予 `undefined` 和 `null`

**never**

`never` 类型表示的是那些**永不存在的值的类型**。一般用于错误处理函数。

```typescript
// 返回never的函数必须存在无法达到的终点
function error(message: string): never {
  throw new Error(message)
}

// 推断的返回值类型为never
function fail(): never {
  return error("Something failed")
}
```

**类型断言**

类型断言用来手动指定一个值的类型。

有两种语法：<类型>值，值 as 类型

```typescript
	let someValue: any = 'abcd'
  let valength = (<string>someValue).length
  let vlength = (someValue as string).length
```

注意一点：在React中使用TypeScript时，类型断言中只能使用as语法。

### 接口

接口用于**对象的形状进行描述**，一般首字母大写。

```typescript
interface Obj1 {
    key1: string,
    key2: number,
    key3?: object，
    readonly key4: number[]
}
function fn1(obj: Obj1) {
    console.log(obj);
}
let obj: Obj1 = {
    key1: 'abc',
    key2: 123,
    key3: {},
    key4: [1,2,3]
}
fn1(obj1)
```

**赋值的时候，变量的形状必须和接口的形状保持一致**。定义的变量比接口少属性和多属性都是不允许的。

如果不确定是不是需要某个属性，可以使用**可选属性**，接口的属性名字定义后面加一个`?`表示。可选属性的含义是该属性可以不存在。

如果希望对象中的一些字段在创建后不能再被赋值，可以定义它为**只读属性**，通过属性名字前加`readonly`定义。









