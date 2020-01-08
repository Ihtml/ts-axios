##TypeScript简介

[TypeScript](http://www.typescriptlang.org/) 是微软开源的 JavaScript 的一个超集，它提供了**类型系统**，弥补了JavaScript这门动态弱类型语言的不足。它可以编译成纯 JavaScript。编译出来的 JavaScript 可以运行在任何浏览器上。TypeScript 编译工具可以运行在任何服务器和任何系统上。

使用TypeScript很大的**增强了js代码的可读性和可维护性**。它在编译阶段就能发现语法错误，配合编辑器的智能提醒，在开发阶段就能杜绝大部分的语法错误。

相比于JavaScript, TypeScript 更可控、更容易重构、更适合大型项目、更容易维护。

TypeScirpt可以通过npm安装

`npm install -g typescript`

安装后，可以通过`tsc`命令编译.ts后缀的TypeScript文件。

## TypeScript常用语法

### 类型

TypeScript 支持与 JavaScript 几乎相同的数据类型。包括：布尔值、数字、字符串、数组、null和undefined

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

`object`表示非原始类型，也就除`number`，`string`，`boolean`，`symbol`，`null`或`undefined` 之外的类型。

此外，typescript还提供了其他一些类型：

**元组**

元组类型允许表示一个**已知元素数量和类型的数组**，各元素的类型不必相同。 比如，你可以定义一对值分别为 `string` 和 `number` 类型的元组。

```typescript
let x: [string, number]
x = ['hello', 10] // OK
x = [10, 'hello'] // Error
```

访问一个已知索引的元素，会得到正确的类型

```typescript
console.log(x[0].substr(1)) // OK
console.log(x[1].substr(1)) // Error, 'number' 不存在 'substr' 方法
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

`never` 类型表示的是那些永不存在的值的类型。一般用于错误处理函数。

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

