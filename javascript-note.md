# Hoisting
# var, let, const
`var` có thể khai báo lại biến, let, const không thể khai báo lại  
```
var a = 2;
var a = 3;
```

```
let a = 2;
let a = 3;
```  
Lỗi


`var` và `let` có thể khởi tạo biến mà không gán trá trị, `const` không thể  

`var` chỉ tồn tại trong function scope tức là chỉ tồn tại trong hàm mà nó được định nghĩa trong đó

`let` và `const` tồn tại trong block scope (if, for, ...)  


# Arrow function

```
function sum(a, b) {
    return a + b
}

var sum = function (a, b) {
    return a +b
}

sum = (a, b) => {
    return a + b
}

sum = (a, b) => a+b
```  

# Arguments
`Array like object` có dạng như sau:
```
const obj = {
    0: "0",
    1: "1",
    2: "2",
    length: 3
}
```  
có thể dùng vòng for để duyệt qua các phần tử  
`arguments` là một arry like object và luôn tồn tại trong scope của 1 function  
# Call
Gọi hàm và cho phép bạn truyền vào một object và các đối số phân cách nhau bởi dấu phẩy  
```
var person1 = {firstName: 'Jon', lastName: 'Kuperman'};
var person2 = {firstName: 'Kelly', lastName: 'King'};

function say(greeting1, greeting2) {
 console.log(greeting1 + ',' + greeting2 + ' ' + this.firstName + ' ' + this.lastName);
}

say.call(person1, 'Hello', 'Good morning'); // => Hello,Good morning Jon Kuperman
say.call(person2, 'Hello', 'Good morning'); // => Hello,Good morning Kelly King
```  
# Apply
Gọi hàm và cho phép bạn truyền vào một object và các đối số thông qua mảng (Array) hoặc array like object  
```
var person1 = {firstName: 'Jon', lastName: 'Kuperman'};
var person2 = {firstName: 'Kelly', lastName: 'King'};

function say(greeting0, greeting1) {
 console.log(greeting0 + ',' + greeting1 + ' ' + this.firstName + ' ' + this.lastName);
}

say.apply(person1, ['Hello', 'Good moring']); // => Hello,Good moring Jon Kuperman
say.apply(person2, ['Hello', 'Good moring']); // => Hello,Good moring Kelly King
```  
call và apply cơ bản không khác gì nhau  

ví dụ cơ bản về apply  
```
function sum() {
  const arr = Array.from(arguments)
  return arr.reduce((sum, num) => sum+num, 0)
}

function average() {
  const s = sum.apply(null, arguments)
  return s/arguments.length
}

console.log(sum(1, 2, 3, 4))
console.log(average(1, 2, 3, 4))
```  
# Enhanced object literals
### Object literals
```
const obj = {
    name: 'Toni',
    run: function() {
        console.log(`Hello ${this.name}`)
    }
}
```  
### Enhanced object literals
```
const name = 'Tom'
const cat = {
    name,
    run() {
        console.log(`Hello ${this.name`)
    }
}
```  
# Class 
```
class Mouse {
    constructor(name) {
    this.name = name
  }

  run() {
    console.log(`Hello ${this.name}`)
  }
}
```  
Khai báo như trên chỉ để code viết giống OOP  
# Class inheritance
Kế thừa theo cú pháp class  
```
class Animal {
  constructor(name) {
    this.name = name
  }

  eat() {
    console.log("Eating")
  }
}

class Bird extends Animal {
  fly() {
    console.log("Flying")
  }
}

class Parrot extends Bird {
  speak() {
    console.log("Speaking")
  }
}

const bird = new Bird('Quack')
bird.fly()

const par = new Parrot("hi")
par.speak()
```  

Kế thừa theo cú pháp function constructor  
```
function Animal(name) {
  this.name = name
}

Animal.prototype.eat = function () {
  console.log("eating")
}

function Bird() {
  Animal.apply(this, arguments)
}

Bird.prototype = new Animal() 
const bird = new Bird('hi')
bird.eat()
```  
# Method overriding
```
class Cat {
  work() {
    console.log('hunt mouse')
  }
}

class LazyCat extends Cat {
  work() {
    console.log('Sleep zzzzz')
  }
}

const tom = new LazyCat()
tom.work
```  
# super
# static
```
class Foo {
  static someMethod() {
    console.log('Some method')
  }

  anotherMethod() {
    console.log('Another method')
  }
}

Foo.someMethod() // Some method
Foo.anotherMethod() // Error

const foo = new Foo()
foo.someMethod() // Error
foo.anotherMethod() // Another method
```  
static method dùng khi không đòi hỏi phải tạo một object mới để sử dụng được method đó  
# rest
Phần còn lại các tham số được truyền vào, phải là tham số cuối cùng
rest có dạng là array khác với arguments là object like array
```
function log(a, ...numbers) { // numbers ở đây là rest
  console.log(a) // 1
  cosole.log(numbers) // [2, 3, 4]
}

log(1, 2, 3, 4)
```  
# spread
```
const a = [2, 3]
const b = [1, ...a, 4]
console.log(b)
```

> rest gom các phần thử thành một array còn spread thì trải array ra thành các phần tử riêng lẻ  

# Value types vs Reference types

```
// Value types
let a1 = 1
let a2 = 1

console.log(a1 === a2) // true


// Reference types
let obj1 = { a: 1 }
let obj2 = { a: 1 }

console.log(obj1 === obj2) // false
// vì với các kiểu phức tạp như object và array nên obj1 và obj2 lưu các giá trị tham chiếu đến object nên khi so sánh thực chất là so sánh 2 giá trị tham chiếu

let a3 = a2
a3 = 2
console.log(a2) // 1


let obj3 = obj2
obj3.a = 2
console.log(obj2) // { a: 2 }
// vì việc gán obj3 = obj2 là chỉ gán giá trị tham chiếu của obj2 cho obj3 cả obj2 và obj3 đều trỏ đến một object

let obj4 = obj1
console.log(obj4 === obj1) // true
obj4.a = 2
console.log(obj4 === obj1) // true
// vì phép gán chỉ gán giá trị tham chiếu nên cả 2 kết quả đều là true


function log(x) {
  x = 3
}
let x = 1
console.log(x) // 1
log(x)
console.log(x) // 1

function log(x) {
  x.a = 2
}
x = { a:1 }
console.log(x) // { a: 1 }
log(x)
console.log(x) // { a: 2 }
// Cần chú ý ở đây vì đối với value type sau khi gọi hàm giá trị của biến không thay đổi còn đối với reference type gía trị của biến sau khi gọi hàm bị thay đổi  
```  
# spread (part 2)

## shadow-clone:
Ví dụ: 
```
const obj1 = {
  a: 1,
  b: 2,
  c: 3
}

let obj2 = {}
for (let key in obj1) {
  obj2[key] = obj1[key]
}

obj2.a = 10
console.log({obj1, obj2}) 
// { obj1: { a: 1, b: 2, c: 3 },
//   obj2: { a: 10, b: 2, c: 3 } }
```  
> Trường hợp khác: 
```
const obj1 = {
  a: 1,
  b: 2,
  c: 3,
  d: { e: 4 }
}

let obj2 = {}
for (let key in obj1) {
  obj2[key] = obj1[key]
}

obj2.d.e = 10
console.log({obj1, obj2}) 
// { obj1: { a: 1, b: 2, c: 3, d: { e: 10 } }, 
//   obj2: { a: 1, b: 2, c: 3, d: { e: 10 } } }
```  
### deep clone
```
const obj1 = {
  a: 1,
  b: 2,
  c: 3,
  d: { e: 4 }
}

let obj2 = {
  ...obj1,
  z: 9
}

console.log({obj1, obj2}) 
// { obj1: { a: 1, b: 2, c: 3, d: { e: 4 } }, 
//   obj2: { a: 1, b: 2, c: 3, d: { e: 4 }, z: 9 } }
```  
Thực chất obj2 vẫn shadow clone obj1 và chỉ thêm phần tử mới, nếu thay giá trị reference types ví dụ: `obj2.d.e = 11` thì obj1 vẫn bị thay đổi
# closure

```
function sum(a, b) {
  const c = a + b
  return function() {
    console.log(c)
  }
}
//Hàm được viết ở return được gọi là clossure vì nó có thể truy cập được các biến bên ngoài hàm đó
sum(1, 2)() // 3
```
# High order function
Với "Higher-Order Function (HOF)", tham số kia có thể làm hàm, và kết quả trả về lại là một hàm khác
```
function waitAndRun(ms, func) {
  setTimeout(func, ms)
}

function run() {
  console.log("Run...")
}

waitAndRun(2000, run)
```  
`waitAndRun` là một High order function  
# Destructuring
Chỉ áp dụng cho array hay objects  

Ví dụ destructure array:  
```
const arr = [1, 2, 3, 4, 5]
const [a, b, c, d, e] = arr
console.log(a, b, c, d, e)
// a=1, b=2, c=3, d=4, e=5
```  
```
const arr = [1, 2, 3, 4, 5]
const [a, e] = arr
console.log(a, b, c, d, e)
// a=1, e = 2
```  
```
const arr = [1, 2, 3, 4, 5]
const [a, b, c, d, e] = arr
console.log(a, , , , e)
// a=1, e=5
```
```
const arr = [1, 2, 3, 4, 5]
const [a, ...b] = arr // rest
console.log(a, b, c, d, e)
// a=1, b=[2, 3, 4, 5]
```  
Làm tương tự đối với object:  
```
const obj = {
  a: 1,
  b: 2,
  c: 3
}
const { a:a, b:b, c:c} = obj
// Hoặc viết cách khác: const { a, b, c} = obj
```  