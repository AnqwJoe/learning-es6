# learning-es6
###### 自己学习ES6过程中的记录

### 一、let 和 const 声明 变量和常量

#### let

与var 区别：  
> 1、var的作用域只体现在函数中，而let 的作用域只局限于当前代码块

```
{
    var str = 'aaa';
    console.log(str);
    
    let str1 = 'bbb';
    console.log(str1);
}

console.log('++++' + str);
console.log('----' + str1);//str1 is not defined
```
  
> 2、使用let 声明的变量作用域不会被提升，（var 会变量提升）

```
{
    console.log(str); //undefined
    var str = 'aaa';
}
{
    console.log(str1); //str1 is not defined
    let str1 = 'bbb';
}
```
> 3、相同的作用域下不能声明相同的变量，（var 可以覆盖值）


```
{
    var str = 'aaa';
    var str = 'bbb';
    console.log(str); //bbb
}
{
    let str1 = 'ccc';
    let str1 = 'ddd';
    console.log(str1); //Identifier 'str1' has already been declared
}
```
> 4、for 循环体现 let 的父子作用域  

**ES5中：**


```
<body>
    <button>按钮1</button>
    <button>按钮2</button>
    <button>按钮3</button>
    <button>按钮4</button>
    <button>按钮5</button>
    
    <script type="text/javascript">
        var btns = document.querySelectorAll('button');
        for (var i = 0; i < btns.length; i++) {
            (function(i){
                btns[i].onclick = function(){
                    alert('点击了第' + (i+1) + '个')
                };                    
            })(i)
        }
    </script>
</body>
```
**ES6 中：**


```
<body>
    <button>按钮1</button>
    <button>按钮2</button>
    <button>按钮3</button>
    <button>按钮4</button>
    <button>按钮5</button>
    
    <script type="text/javascript">
        let btns = document.querySelectorAll('button');
        for (let i = 0; i < btns.length; i++) {
            btns[i].onclick = function(){
                alert('点击了第' + (i+1) + '个')
            };                    
        }
    </script>
</body>
```
> 5、子作用域不影响父作用域：

**ES5：**

```
for (var i = 0; i < 5; i++) {
    var i = 20;
    console.log(i) //20
}
```
**ES6：**

```
for (let i = 0; i < 5; i++) {
    let i = 20;
    console.log(i) //20 20 20 20 20
}
```

#### const

> 1、只在当前代码块中有效 （同let）

```
{
    const a = 'aaa';
    console.log(a); //a
}
    console.log(a); //Error: a is not defined
```
> 2、作用域不会提升  （同let）


> 3、不能重复声明　　（同let）


> 4、声明的常量必须赋值


```
{
    const name;
    name = 'abc';
    console.log(a); //Error: Missing initializer in const declaration
}
```
> 5、常量只能一次声明，不能修改

```
{
    const name = 'abc';
    name = 'def';
    console.log(name); //Error: Assignment to constant variable.
}
```
但是：引用类型存放在堆区中，而在栈区中只是存放的指向地址，修改堆区的内容，指向地址并不变

```
const obj = {name: 'abc'};
console.log(obj); //{name: 'abc'}

obj.name = 'def';
console.log(obj); //{name: 'def'}
```


###  二、解构赋值
ES6允许按照一定模式 从数组和对象中提取值对变量进行赋值，这被称为解构  
> 1、基本用法

```
//let name = "张三",age = 18, sex = 'male';
let [name, age, sex] = ['李四', 20, 'female'];
console.log(name); //李四
console.log(age); //20
console.log(sex); //female
```
> 2、对象的解构赋值, key必须要一一对应

```
let {name, age, sex} = {name: '张三',age: 24, sex:'male'};
console.log(name);
console.log(age);
console.log(sex);
```
复杂对象也可以  


```
let {name, age, sex,friend,pet} = {name: '张三',age: 24, sex:'male',friend:['abc','def'],pet:{name:'lulu',age:5}};
console.log(name);
console.log(age);
console.log(sex);
console.log(friend[1]); //def
console.log(pet.age,pet.name);//5 "lulu"
```

> 3、数组解构赋值，匹配要一一对应

```
    let [arr1,[arr2,arr3,[arr4,arr5]]] = [1,[2,3,[4,5]]];
    console.log(arr1,arr2,arr3,arr4,arr5); //1 2 3 4 5
```

```
    let [a,,c] = [1,2,3];
    console.log(a) //1
    console.log(c) //3
```

> 4、其他，'string'类型存在构造器，而number类型不存在


```
    let [a,b,c,d,e] = '我是中国人';
    console.log(a) //我
    console.log(b) //是
    console.log(c) //中
    console.log(d) //国
    console.log(e) //人
```

### 三、数据集合 set

> 1、特点：
- 类似数组，但没有重复的元素（唯一的）；
- 开发中用于去除重复数据
- key 和 value都是相等的

> 2、开发中经常用于一些去重的操作；

```
let set = new Set(['张三','李四','王五','张三']);
console.log(set); //Set {"张三", "李四", "王五"}
```
> 3、一个属性，集合的长度用size表示

```
let set = new Set(['张三','李四','王五','张三']);
console.log(set); //Set {"张三", "李四", "王五"}
console.log(set.size); //3
```
> 4、四个方法

add 添加项，支持链式语法

delete 删除项

has 判断集合中有没有某个元素

clear 清掉集合里的所有元素


```
let set = new Set(['张三','李四','王五','张三']);

console.log(set); //Set {"张三", "李四", "王五"}
//add
set.add('刘德华');
console.log(set); //Set {"张三", "李四", "王五", "刘德华"}
//delete
set.delete('王五');
console.log(set); //Set {"张三", "李四", "刘德华"}
//has
console.log(set.has('张三')); //true
console.log(set.has('王五')); //false
//clear
set.clear();
console.log(set.clear()); //undefined
console.log(set); //Set {}
```

### 四、数据集合 map
> 1、特点：
- 类似对象，本质上是键值对的集合
- 键不局限于字符串，各种类型的值（包括对象）都可以当作键
- 对象“字符串-值”，Map“值-值” 是一种更加完善的hash 结构实现

> 2、使用对象当作键，但是无法将对象区别出来


```
let obj1 = {a:1},obj2 = {b:2},obj = {};
obj.name = '张三';
obj[obj1] = '天空';
console.log(obj); //Object {name: "张三", [object Object]: "天空"}
obj[obj2] = '大海';
console.log(obj); //Object {name: "张三", [object Object]: "大海"}

console.log(obj1.toString()); //[object Object]
console.log(obj1.toString()); //[object Object]
console.log(obj1.toString() === obj2.toString()); //true
```

使用map 后

```
const map = new Map([
    ['name', '张三'],
    ['age', '18'],
    ['sex', 'male']
]);
console.log(map); //Map {"name" => "张三", "age" => "18", "sex" => "male"}
```


```
let obj1 = {a:1},obj2 = {b:2};
const map = new Map([
    ['name', '张三'],
    ['age', '18'],
    ['sex', 'male'],
    [obj1,'今天天气很好'],
    [obj2,'适合敲代码']
]);
console.log(map); //Map {"name" => "张三", "age" => "18", 
"sex" => "male", Object {a: 1} => "今天天气很好", Object {b: 2} => "适合敲代码"}
```

> 3、五个方法：  

set：添加进map

```
let obj1 = {a:1},obj2 = {b:2};
const map = new Map([
    ['name', '张三'],
    ['age', '18'],
    ['sex', 'male']
]);
map.set('friends',['赵六','李七']).set(['dog'],'小花');
console.log(map);
//Map {"name" => "张三", "age" => "18", "sex" => "male", "friends" => ["赵六", "李七"], ["dog"] => "小花"}
```
get：获取值


```
const map = new Map([
    ['name', '张三'],
    ['age', '18'],
    ['sex', 'male']
]);

console.log(map.get('name')); //张三
```
delete ：删除值


```
const map = new Map([
    ['name', '张三'],
    ['age', '18'],
    ['sex', 'male']
]);

console.log(map.get('name')); //张三
map.delete('sex');
console.log(map); //Map {"name" => "张三", "age" => "18"}
```
has ：同set

clear ： 同set

> 3、遍历


```
const map = new Map([
    ['name', '张三'],
    ['age', '18'],
    ['sex', 'male']
]);
map.forEach(function(value,index){
    console.log(index +':'+value);
    //name:张三
    //age:18
    //sex:male
})
```

### 五、ES6新增数据类型 symbol

表示独一无二的值，不会与其他属性名产生冲突

对对象或者变量命名时，使用symbol不会产生冲突


```
let str1 = Symbol();
let str2 = Symbol();

let str3 = Symbol('name');
let str4 = Symbol('name');

console.log(str3 === str4); //false
        
const obj = {};
obj.name = '张三';
obj.name = '李四';
obj[Symbol('name')] = '张三';
obj[Symbol('name')] = '李四';

console.log(obj);
//Object {name: "李四", Symbol(name): "张三", Symbol(name): "李四"}
```

### 六、新增的语法糖 class

面向对象时  
ES5中:


```
function Person(name,age){
    this.name = name;
    this.age = age;
}
Person.prototype = {
    constructor:Person,
    print(){
        console.log('我叫'+ this.name + ',今年' + this.age +'岁')
    }
}

let person = new Person('张三',19);
console.log(person); //Person {name: "张三", age: 19}
```

ES6中:

```
class Person{
    constructor(name,age){
        this.name = name;
        this.age = age;
    }
    
    print(){
        console.log('我叫'+ this.name + ',今年' + this.age +'岁')
    }
}
let person = new Person('张三',19);
console.log(person); //Person {name: "张三", age: 19}
```

 由于本质还是函数，所以类的原型和构造函数的原型指向是一样的。
 
###  七、内置对象扩展

> 1、模板字符串

```
    <style type="text/css">
        .test{
            width: 100px;;
            height: 100px;
            background-color: red;
        }
    </style>
<body>
    <script type="text/javascript">
        //模板字符串
        let str = '适合敲代码';
        let className = 'test';
        let html = `<html>
                        <head></head>
                        <body>
                            <p>今天的天气很好</p>
                            //加载class 和 文字
                            <div class='${className}'>${str}</div>
                        </body>
                    </html>`;
        
        console.log(html);
    </script> 
</body> 
```
结果：

```
<html>
    <head></head>
    <body>
        <p>今天的天气很好</p>
            <div class="test">适合敲代码</div>
     </body>
</html>
```

>  2、数组扩展


```
//Array.from  非正式数组转换成正式的数组

    let allLis= document.querySelectorAll('li');
    console.log(allLis); //[li, li, li, li, li, li]  伪数组
    console.log(Array.isArray(allLis)); false
    console.log(Array.from(allLis));//[li, li, li, li, li, li]  数组
    console.log(Array.isArray(Array.from(allLis))); //true
    
    
//Array.of 零零散散的数值统一成数组
    
    console.log(Array.of(1,2,3,4)); //[1.2.3.4]
    console.log(Array.of('abc','def','ghi')); //["abc", "def", "ghi"]
```

> 3、对象扩展

key 和 value是一样的，写一个就够了：

```
//key 和 value是一样的，写一个就够了
let name = '张三';
let age = 18;
let obj = {
    name,
    age
};
console.log(obj);
//Object {name: "张三", age: 18}
```
 Object.assign(); 合并多个对象到一个对象内
 
 
```
let obj1 = {name:'张三'};
let obj2 = {age:14};
let obj3 = {sex:'male'};
let obj4 = {friend:'李四'};
let obj = {}

Object.assign(obj,obj1,obj2,obj3,obj4);
console.log(obj);
//Object {name: "张三", age: 14, sex: "male", friend: "李四"}
```
延展操作符：展开数值


```
let str = '今天天气不错；';
let strArr = [...str];
console.log(strArr); 
//["今", "天", "天", "气", "不", "错", "；"]
```

**一个数组去重的小实例：**

```
//数组去重
let myArr = [1,2,5,10,'张三',5,2,"王五",1];
console.log(new Set(myArr));
//Set {1, 2, 5, 10, "张三","王五"}
//转换数组
let myNewArr = [...new Set(myArr)];
console.log(myNewArr);
//[1, 2, 5, 10, "张三", "王五"]
```

### 八、函数扩展
>  1、形参设置默认值
```
//相当于做了容错
function sum(num1 = 10,num2 = 10){
    console.log(num1 + num2);
}
sum(10,30); //40
sum(); //20
```
>  2、参数形式


```
function sum(name,sex,...nums){
    let result = 0;
    for(let value of nums){
        result += value;
    }
    console.log(name); //张三
    console.log(sex); //male
    return result;
}

console.log(sum('张三','male',10,20,30,50)); //110
```
>  九、箭头函数


```
//() => {}
let sum = (num1,num2) => {return num1 + num2};
console.log(sum(100,300)); //400

//用作回调函数
let nameArr = ['张三','李四','王五'];
nameArr.forEach((value,index)=>{
    console.log(index +':'+value);
    //0:张三
    //1:李四
    //2:王五
});
//用作this指向，内部会自动做一个绑定
function demo(){
    setTimeout(function(){
        console.log(this); //window
    },1000)
    
    setTimeout(() => {
        console.log(this); //Object{}
    },1000)
}

let obj = {};
//obj 调用 demo方法
demo.call(obj);
```
