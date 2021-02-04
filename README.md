# TypeScript
## 安装和引入
### 1、安装

```js
npm i typescript -g
```

### 2、转换单个文件

```js
tsc ./js/a.js ./js/a.js
```

### 3、初始化配置

```js
tsc --init
```

生成一个`tsconfig.json`文件

配置这个文件

```js
"target": "es5",
"module": "amd",
"outDir": "./js/",
```

新建ts文件夹,创建一个main.ts的文件

### 监视转换

```bash
tsc -w
```

### 调用转换后的es5模块

先将require.js拷贝项目根目录下。
创建index.html文件，并且在html中增加下面的内容

```js
<script src="./require.js" data-main='./js/main'></script>
```

`require.js`就是`amd模块`开发引入文件  
`main.js`就是所有js模块最后执行的入口文件














## 基础类型

`typescript` 是一个强类型语言，所有的变量都需要给定类型，并且当设置类型后，这个变量的类型就不能替换了

javascript是弱类型

```js
var a=3;
a="ab";
```

typescript强类型

```js
var a:Number=3;
a="ab"//报错
a=5;
```

### 1、数值型\字符型\布尔型

```js
//大写类型是包括该类型的构造函数创建实例化对象类型，比如数值对象类型
var a:Number=5;
var b:number=6;

a=new Number(10);//数值对象类型
b=new Number(11);//报错

var b1:String="b";
var a1:string="a";

var a2:Boolean=true;
var b3:boolean=false;
```

### 2、数组和元组、枚举

#### 数组
数组类型在强引用类型后，就必须对于该数组的所有值给定类型，这就意味着数组的必须限定里面的元素数据类型统一

```js
var arr:Array<number>=[]; // 写法一
var arr1:number[]=[];   // 写法二

弱类型
// var arr=[
//     {name:"a",num:1},
//     {name:"b",num:2},
//     {name:"c",num:3},
// ]

强类型
var arr:Array<object>=[
    {name:"a",num:1},
    {name:"b",num:2},
    {name:"c",num:3},
];

arr.push({name:"d",num:5});
```
#### 元组
如果给出的列表是一个不统一的类型列表，这时候使用元组，元组的数量一般是固定

```js
// 必须得一一对应
var list:[number,string,boolean];
list=[5,"a",true];
```
#### 枚举
枚举类型 主要是限定一些固定的数据，比如经常使用常量

```js
enum Color {RED="red",BLUE="blue",GREEN="green"}; // enum枚举
console.log(Color.RED)
```

### 3、Any
  
当不确定数据类型时可以设置为Any，也就是这个变量是任意类型

注意谨慎使用，尽量不使用

```js
var a:any=5;
a="a"
```

### 4、Void、undefined、null、never

#### void
void一般用函数执行返回时的类型，当函数不适用return返回任何类型时，就可以使用void返回

```js
function abc():number
{
    return 1;
}

var a:number = abc();

function cde():void
{
    console.log("abc");
}
```
#### undefined
undefined 是未定义型
```js
var a:undefined;
a = undefined;
```

1. `a:number|undefined`  这个undefined必须是一个值  
2. `a?:number`  也是数值和undefined 但是做为参数使用时，允许undefined不输入
```js 
function abc(a:number|undefined):number|undefined // | 或者
{
    console.log(a);
    return a;
}
abc(undefined)
```
直接用于变量或者函数返回类型是不行的,主要用于参数或者类中的实例化属性
 ```js
var b?:string;//错误的

 function abc(a?:number):number|undefined
{
    console.log(a);
    return a;
}
abc()


class Box{
    public a?:number=1;
}
 ```
#### null
`null`就是返回为空的类型  
**如果一个对象不设置null就意味着未来无法设置为null，被垃圾回收**
 ```js
 var a:null=null
 var o:object|null={a:1};
 o=null;
 ```

#### Never

```js
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

### 5、object

任意对象在初始设置完成都不允许增加新属性

```js
var o={a:1,b:2};
o.a=10;
o.c=5;//不能增加这个属性
```

### 6、断言
如果通过设置以后的类型较大或者种类较多，这时候需要假设这个类型是一种固定类型，就是断言

```js
// 有一个列表，存放了人类，大猩猩，长颈鹿

var list:动物类=[]
list.forEach(function(item:动物类){
    (item as 人类).fire();  // fire，人类会生活，自己拟定的函数方法
})


let someValue: any = "this is a string";

let strLength: number = (<string>someValue).length;
```
### 7、泛型
`identity函数`叫做泛型，因为它可以适用于多个类型。 不同于使用 any，它不会丢失信息保持准确性，传入数值类型并返回数值类型。
```js
function identity<T>(arg: T): T {
    return arg;
}

// function identity(arg: any): any {
//     return arg;
// }
// any会丢失数据类型
```
案例
```js
function fn1<T>(A:new ()=>T):T
{
    let b=new A();
    return b;
}
// Box 和 Ball 都是另一个文件下的类
var bs:Box = fn1(Box);
console.log(bs);
var arr:Ball = fn1(Ball);
console.log(arr);



function fn2<T>(names:T):T
{
     return names;
}
var a:Box=fn2(new Box);
console.log(a);
```












## 类

class定义，当模块化开发时，仍然使用`export default`

继承使用`extends`来完成继承

当使用模块开发时，ts实现了自动导入模块,自动导入时，ts是不带扩展名，es6的导入需要带入扩展名，这个不需要

Box.js

```js
export default class Box{
    
}
```

Ball.js

```js
import Box from "./Box"; //不需要加扩展名

export default class Ball extends Box{
    
}
```

### constructor

构造函数，当类设置构造函数时，一般都是直接在实例化实现了参数的调用或者在实例时执行某个函数，否则构造函数可以不写

```js
export default class Box{
    constructor(){

    }

}
//上下是相同
export default class Box{
    
}
```

构造函数在其他语言中可以有多个，但是ts中**仅能使用一个**，并且这个名字统一都是`constructor`

当使用继承后，子类的构造函数，如果不写，标识默认执行父类的构造函数，

```js
export default class Ball extends Box{
  
}

//这两个是一样的
export default class Ball extends Box{
    constructor(){
        super();
    }
}
```

下面的案例中我们遇到public这个修饰符  
针对类中所有属性和方法可以使用public private protected
这三种进行修饰

```js
export default class Box{
    public a:number=1;
    public b?:number;
    constructor(){

    }
    public run():void
    {

    }
}
```

`public` 公有  
可以做为当前类以及当前类实例化元素调用的属性和方法

`private` 私有  
只能被当前类内部方法调用，不能在实例化的属性和方法调用

`protected` 受保护的  
可以在当前类和其子类中被调用或者子类中覆盖，但是不暴露在外，不能通过当前类的实例化对象调用这个受保护的方法

>protected 是介于public和private之间修饰
>>`public` 是暴露，
`private` 和 `protected` 都是暴露，
暴露意味着实例化对象可以调用，不暴露不能调用。  
`private` 只能自身类中的方法调用，但是 `protected` 可以在自身类和其子类中调用或者覆盖

Box.js
```js
export default class Box{
    public a:number=1;
    private a1:number=2;
    public b?:number;
    constructor(){

    }
    // 公有
    public run():void
    {
        console.log("aaa");
        document.addEventListener("click",e=>this.clickHandler(e))
    }
    // 私有
    private run1():void{
        console.log("bbb");
    }
    // 受保护
    protected run2():void{
        console.log("cccc");
    }

    private clickHandler(e:MouseEvent):void{

    }
}
```

Ball.js

```js
import Box from "./Box";

export default class Ball extends Box{
    constructor(){
        super();
    }

    // 覆盖父类的run方法
    public run():void
    {

    }
    // 这是不能覆盖父类的方法的，这个等于新建一个新的run1方法
    // 如果使用私有定义run1，系统认为你是准备覆盖父类的run1方法，所以就会报错
    // private run1():void{

    // }

    protected run2():void{

    }
}
```

Main.js

```js
import Box from "./Box";
var b=new Box()
b.run();
console.log(b.a);
```
`public` 公有  
该数据成员、成员函数是对所有用户开放的，所有用户都可以直接进行调用

`private` 私有  
除了class自己之外，任何人都不可以直接使用。

`protected` 受保护的  
对于子女、朋友来说，就是公有的，可以自由使用，没有任何限制，而对于其他的外部class而言，就变成了私有的。

>注意TS没有Object.assign**

CSSStyleDeclaration、DOM元素的CSS行内样式类别、HTMLElement、HTML标签元素的类别等标签作为父类时，如果继承的子类需要修改或者覆盖父类的某个方法和属性，就需要在父类中设置这个方法或者属性为 `protected`   
1. 覆盖的方式就是在子类中重写该方法  
2. 修改的方式就是先在子类中执行超类的该方法，然后再添加对于的内容

```js
 protected createElement():HTMLDivElement
    {
        // 执行了超类的createElement方法
       let div:HTMLDivElement=super.createElement();
       div.style.backgroundColor="blue";
       div.style.borderRadius="50px";
       return div;
    }

 // protected 就是超类中的这个方法描述，表示可以被子类修改或者覆盖
 // super.createElement();  就是先执行超类这个方法，然后把超类执行结果返回给div
```
### 抽象类

#### 抽象类定义

abstract class Circle

```js
export default abstract class Circle {
    constructor() {
        
    }
    public run():void
    {
        console.log("aaa");
    }

    abstract play():void;

}
```

抽象类是不可以实例化的  
`var c2=new Circle()`这样是报错的

```js
import Circle from "./Circle";

export default class Loop extends Circle
{
    public play():void
    {
        console.log("cccc");
    }
}
```

抽象类的作用仅仅是被用来继承的  
它具备有接口和继承这些特性









## 接口


接口文件命名必须以大写字母I起头后面跟如接口名

### 接口定义

使用`interface`

```js
export default interface ITimer{
   update():void
}
```

在接口中不可以 `public` `private` `protected` 来修饰方法  
接口中只定义方法名字，参数和返回值，不写具体实现内容

### 接口的接入

implements
> 引入接口必须使用里面的方法，否则会报错
```js
import ITimer from "./ITimer";

export default class Rect implements ITimer
{
    public a?:number=1;
    constructor(){

    }
    public update():void
    {
        console.log("bbbb");
    }
}
```

在这个类中就可以实现接入接口中的所有方法  
注意，在接口中定义的方法都必须使用`public`修饰设置  
>接口是可以接入多个的，但是继承只能继承一个父类

```js
export interface IA{
  a():void
}
export interface IB{
  b():void
}
export interface IC{
  c():void
}

export interface Iobj{
  a:number;
  b:number;
}

export interface ID extends IA,IB{
  d():void;
}
```

```js
import { IA, IB, IC, ID} from "./IObj"

export class A implements IA,IB{
  public a():void{
    console.log("使用接口IA的a方法");
  }

  public b():void{
    console.log("使用接口IA的b方法");
    
  }
}

export class B implements IA,IC{
  public a():void{
    console.log("使用接口IA的a方法");
  }

  public c():void{
    console.log("使用接口IC的c方法");
  }
}

export class C implements IB,IC
{
    public b():void
    {
        console.log("使用接口IB的b方法");
    }
    public c():void{
        console.log("使用接口IC的c方法");
    }
}

export class D implements ID{
  // ID 继承了多个方法
  public d():void
  {

  }
  public a():void{
    console.log("继承多个接口行不行a");
    
  }
  public b():void{
    console.log("继承多个接口行不行b");
  }
}
```

```js
var a1 = new A()
var b1 = new B()
var c1 = new C()
var d1 = new D()
a1.a()
b1.c()
c1.b()
d1.b()

// a1和b1的关系是 具有相同的IA接口
// b1和c1的关系是 具有相同IC接口
// a1和c1的关系是 具有相同IB接口
// ...
```

> 谢柳宁整理