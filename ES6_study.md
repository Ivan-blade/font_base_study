### let与const
    + let与var声明区别
        + let代码块内有效,var全局有效
            ```
                {
                    let a = 0;
                    var b = 1;
                }
                a  // ReferenceError: a is not defined
                b  // 1
            ```
        + 不能重复声明，var可以重复声明
            ```
                let a = 1;
                let a = 2;
                var b = 3;
                var b = 4;
                a  // Identifier 'a' has already been declared
                b  // 4
            ```
        + let不存在变量提升，但是var存在变量提升
            ```
                console.log(a);  //ReferenceError: a is not defined
                let a = "apple";
                
                console.log(b);  //undefined未被赋值但是可以使用
                var b = "banana";
                console.log(b)
            ```
            + 变量提升(函数及变量的声明都将被提升到函数的最顶部，变量可以在使用后声明，也就是变量可以先使用再声明）
                + 所有的var声明都会提升到作用域的最顶上去（let不在此列）
                    ```
                        for (var i = 0; i < 10; i++) {
                            setTimeout(function(){
                                console.log(i);
                            })
                        }
                        // 输出十个 10
                        // i作为全局变量只能是一个数，所以将会优先循环十次变为10，再执行函数循环，使用一般循环变量用let
                    ```
                + 同一个变量只会声明一次，其他的会被忽略掉
                    ```
                        num = 6;
                        var num = 7;
                        var num;
                        console.log(num);  // 输出7
                        以后声明的为准
                    ```

                + 函数声明的优先级高于变量声明的优先级，并且函数声明和函数定义的部分一起被提升
                    ```
                        func(); 
                        var func;
                        function func() {
                            console.log(1);
                        }
                        func = function() {
                            console.log(2);
                        }
                        // 输出1而不是2
                        // 函数声明和变量声明都会被提升，但是需要注意的是函数会先被提升，然后才是变量
                        // var func作为变量优先级没有函数高，于是作为重复声明被无视
                    ```
    + const
        + const 声明一个只读变量，声明之后不允许改变。意味着，一旦声明必须初始化，否则会报错。（即也没有变量提升一说）
        + const 其实保证的不是变量的值不变，而是保证变量指向的内存地址所保存的数据不允许改动。简单类型和复合类型保存值的方式是不同的。对于简单类型（数值 number、字符串 string 、布尔值 boolean）,值就保存在变量指向的那个内存地址，因此 const 声明的简单类型变量等同于常量。而复杂类型（对象 object，数组 array，函数 function），变量指向的内存地址其实是保存了一个指向实际数据的指针，所以 const 只能保证指针是固定的，至于指针指向的数据结构变不变就无法控制了，所以使用 const 声明复杂类型对象时要慎重，如果数据结构既定会发生改变则不能用const
### 解构赋值
+ 定义
    + 解构赋值是对赋值运算符的扩展，他是一种针对数组或者对象进行模式匹配，然后对其中的变量进行赋值。
    + 解构模型（等式左边解构目标，等式右边解构源）
    + 数据模型解构
        + 基本
            ```
                let [a, b, c] = [1, 2, 3];
                // a = 1
                // b = 2
                // c = 3
            ```
        + 嵌套
            ```
                let [a, [[b], c]] = [1, [[2], 3]];
                // a = 1
                // b = 2
                // c = 3
            ```
        + 忽略
            ```
                let [a, , b] = [1, 2, 3];
                // a = 1
                // b = 3
            ```
        + 不完全解构
            ```
                let [a = 1, b] = []; // a = 1, b = undefined
            ```
        + 剩余运算符
            ```
                let [a, ...b] = [1, 2, 3];
                //a = 1
                //b = [2, 3]
            ```
        + 字符串等(在数组的解构中，解构的目标若为可遍历对象，皆可进行解构赋值。可遍历对象即实现 Iterator 接口的数据)
            ```
                let [a, b, c, d, e] = 'hello';
                // a = 'h'
                // b = 'e'
                // c = 'l'
                // d = 'l'
                // e = 'o'
            ```
        + 解构默认值(当解构模式有匹配结果，且匹配结果是 undefined 时，会触发默认值作为返回结果。)
            ```
                let [a = 2] = [undefined]; // a = 2
            ```
    + 对象模型解构（大同小异）
        + 不完全解构
            ```
                let {a, b, ...rest} = {a: 10, b: 20, c: 30, d: 40};
                // a = 10
                // b = 20
                // rest = {c: 30, d: 40}
            ```
    + exact不完全解构的简单应用
        ```
            计算数组之和
            function Sum(...nums){
                let sum = nums.reduce((x,y)=>{return x+y})
                return sum
            }
            // reduce 为数组中的每一个元素依次执行回调函数，不包括数组中被删除或从未被赋值的元素，接受四个参数：初始值（上一次回调的返回值），当前元素值，当前索引，原数组 
        ```
### symbol
+ ES6 引入了一种新的原始数据类型 Symbol ，表示独一无二的值，最大的用法是用来定义对象的唯一属性名
+ 用法
    + Symbol 函数栈不能用 new 命令，因为 Symbol 是原始数据类型，不是对象。可以接受一个字符串作为参数，为新创建的 Symbol 提供描述（只是描述，相同描述值并不一样），用来显示在控制台或者作为字符串的时候使用，便于区分。
        ```
            let sy = Symbol("KK");
            console.log(sy);   // Symbol(KK)
            typeof(sy);        // "symbol"
            
            // 相同参数 Symbol() 返回的值不相等
            let sy1 = Symbol("KK"); 
            sy === sy1;       // false
        ```
    + 使用场景-属性名
        ```
            let sy = Symbol("key1");
            // 写法1
            let syObject = {};
            syObject[sy] = "kk";
            console.log(syObject);    // {Symbol(key1): "kk"}
            
            // 写法2
            let syObject = {
            [sy]: "kk"
            };
            console.log(syObject);    // {Symbol(key1): "kk"}
            
            // 写法3
            let syObject = {};
            Object.defineProperty(syObject, sy, {value: "kk"});
            console.log(syObject);   // {Symbol(key1): "kk"}

            这边有点诡异呀。。。自己还没有碰到应用场景感触不是很深刻
        ```
    + Symbol.for()
        + Symbol.for() 类似单例模式，首先会在全局搜索被登记的 Symbol 中是否有该字符串参数作为名称的 Symbol 值，如果有即返回该 Symbol 值，若没有则新建并返回一个以该字符串参数为名称的 Symbol 值，并登记在全局环境中供搜索。
            ```
                let yellow = Symbol("Yellow");
                let yellow1 = Symbol.for("Yellow"); // yellow1 = Yellow
                console.log(yelloew1)
                yellow === yellow1;      // false
                
                let yellow2 = Symbol.for("Yellow");
                yellow1 === yellow2;     // true
            ```
        + Symbol.keyFor() 返回一个已登记的 Symbol 类型值的 key ，用来检测该字符串参数作为名称的 Symbol 值是否已被登记。
            ```
                let yellow1 = Symbol.for("Yellow");
                Symbol.keyFor(yellow1);    // "Yellow"
            ```

### map与set
+ map
    + map对象
        + Map 对象保存键值对。任何值(对象或者原始值) 都可以作为一个键或一个值。
    + map和object的区别
        + 一个 Object 的键只能是字符串或者 Symbols，但一个 Map 的键可以是任意值。
        + Map 中的键值是有序的（FIFO 原则），而添加到对象中的键则不是。
        + Map 的键值对个数可以从 size 属性获取，而 Object 的键值对个数只能手动计算。
        + Object 都有自己的原型，原型链上的键名有可能和你自己在对象上的设置的键名产生冲突。
    + map中的key
        + key是字符串
            ```
                var myMap = new Map();
                var keyString = "a string"; 
                
                myMap.set(keyString, "和键'a string'关联的值");
                
                myMap.get(keyString);    // "和键'a string'关联的值"
                myMap.get("a string");   // "和键'a string'关联的值"
                // 两种方式获取的结果相同因为 keyString === 'a string'
            ```
        + key是对象
            ```
                var myMap = new Map();
                var keyObj = {}, 
                
                myMap.set(keyObj, "和键 keyObj 关联的值");
                myMap.get(keyObj); // "和键 keyObj 关联的值"
                myMap.get({}); // undefined, 因为 keyObj !== {}
            ```
        + key是函数
            ```
                var myMap = new Map();
                var keyFunc = function () {}, // 函数
                
                myMap.set(keyFunc, "和键 keyFunc 关联的值");
                
                myMap.get(keyFunc); // "和键 keyFunc 关联的值"
                myMap.get(function() {}) // undefined, 因为 keyFunc !== function () {}
            ```
        + key是NAN（这个有什么应用场景吗）
            ```
                var myMap = new Map();
                myMap.set(NaN, "not a number");
                
                myMap.get(NaN); // "not a number"
                
                var otherNaN = Number("foo");       // 此时的otherNaN值为NAN
                myMap.get(otherNaN); // "not a number" 
                // 虽然 NaN 和任何值甚至和自己都不相等(NaN !== NaN 返回true)，NaN作为Map的键来说是没有区别的。
            ```
    + map的迭代方式
        + for...of
            ```
                var myMap = new Map();
                myMap.set(0, "zero");
                myMap.set(1, "one");
                
                // 将会显示两个 log。 一个是 "0 = zero" 另一个是 "1 = one"
                for (var [key, value] of myMap) {
                    console.log(key + " = " + value);
                }
                for (var [key, value] of myMap.entries()) {
                    console.log(key + " = " + value);
                }
                /* 这个 entries 方法返回一个新的 Iterator 对象，它按插入顺序包含了 Map 对象中每个元素的 [key, value] 数组。 
                * problems: 这两种方法有什么区别呢
                */
                
                // 将会显示两个log。 一个是 "0" 另一个是 "1"
                for (var key of myMap.keys()) {
                    console.log(key);
                }
                /* 这个 keys 方法返回一个新的 Iterator 对象， 它按插入顺序包含了 Map 对象中每个元素的键。 */
                
                // 将会显示两个log。 一个是 "zero" 另一个是 "one"
                for (var value of myMap.values()) {
                    console.log(value);
                }
                /* 这个 values 方法返回一个新的 Iterator 对象，它按插入顺序包含了 Map 对象中每个元素的值。 */
            ```
        + foreach
            ```
                var myMap = new Map();
                myMap.set(0, "zero");
                myMap.set(1, "one");
                
                // 将会显示两个 logs。 一个是 "0 = zero" 另一个是 "1 = one"
                myMap.forEach(function(value, key) {
                    console.log(key + " = " + value);
                }, myMap)
                // problems：这边的foreach最后为什么要加myMap呢？不加好像也没什么影响？
            ```
    + map对象的操作
        + map与array的转换
            ```
                var kvArray = [["key1", "value1"], ["key2", "value2"]];
    
                // Map 构造函数可以将一个 二维 键值对数组转换成一个 Map 对象
                var myMap = new Map(kvArray);
                
                // 使用 Array.from 函数可以将一个 Map 对象转换成一个二维键值对数组
                var outArray = Array.from(myMap);
            ```
        + map的克隆
            ```
                var myMap1 = new Map([["key1", "value1"], ["key2", "value2"]]);
                var myMap2 = new Map(myMap1);
                
                console.log(original === clone); 
                // 打印 false。 Map 对象构造函数生成实例，迭代出新的对象。
                // problems它想表达什么？original和clone并没有声明应该是内置变量，他们代表了什么？
            ```
        + map的合并
            ```
                var first = new Map([[1, 'one'], [2, 'two'], [3, 'three'],]);
                var second = new Map([[1, 'uno'], [2, 'dos']]);
                
                // 合并两个 Map 对象时，如果有重复的键值，则后面的会覆盖前面的，对应值即 uno，dos， three
                var merged = new Map([...first, ...second]);
                console.log(merged)

                // problems 为什么这边first代表一个map但是合并时不能用first而要用...first呢？
            ```
+ set
    + set对象
        + Set 对象允许你存储任何类型的唯一值，无论是原始值或者是对象引用。
    + set中的特殊值
        + Set 对象存储的值总是唯一的，所以需要判断两个值是否恒等。有几个特殊值需要特殊对待：
            + +0 与 -0 在存储判断唯一性的时候是恒等的，所以不重复；
            + undefined 与 undefined 是恒等的，所以不重复；
            + NaN 与 NaN 是不恒等的，但是在 Set 中只能存一个，不重复。
    + 代码
        ```
            let mySet = new Set();
 
            mySet.add(1); // Set(1) {1}
            mySet.add(5); // Set(2) {1, 5}
            mySet.add(5); // Set(2) {1, 5} 这里体现了值的唯一性
            mySet.add("some text"); 
            // Set(3) {1, 5, "some text"} 这里体现了类型的多样性
            var o = {a: 1, b: 2}; 
            mySet.add(o);
            mySet.add({a: 1, b: 2}); 
            // Set(5) {1, 5, "some text", {…}, {…}} 
            // 这里体现了对象之间引用不同不恒等，即使值相同，Set 也能存储
        ```
    + 类型转换
        + Array
            ```
                // Array 转 Set
                var myarr = ["value1", "value2", "value3"];
                var mySet = new Set(myarr);
                // 用...操作符，将集合转换为数组 Set 转 Array
                var myArray = [...mySet];
                String
                // String 转 Set
                var mySet = new Set('hello');  // Set(4) {"h", "e", "l", "o"}
                // 注：Set 中 toString 方法是不能将 Set 转换成 String
            ```
    + set对象作用
        + 并集
            ```
                var a = new Set([1, 2, 3]);
                var b = new Set([4, 3, 2]);
                var union = new Set([...a, ...b]); // {1,2,3,4}
                // 如果使用var union = new Set([a, b]);那么union就是包含了a，b两个集合的集合位{Set(3),Set(3)}
            ```
        + 数组去重
            ```
                var mySet = new Set([1, 2, 3, 4, 4]);
                [...mySet]; // [1, 2, 3, 4]
            ```
        + 交集
            ```
                var a = new Set([1, 2, 3]);
                var b = new Set([4, 3, 2]);
                var intersect = new Set([...a].filter(x => b.has(x))); // {2, 3}
            ```
        + 差集
            ```
                var a = new Set([1, 2, 3]);
                var b = new Set([4, 3, 2]);
                var difference = new Set([...a].filter(x => !b.has(x))); // {1}
            ```
        + filter()方法
            + 简介
                + filter() 方法创建一个新的数组，新数组中的元素是通过检查指定数组中符合条件的所有元素。
                + 注意： filter() 不会对空数组进行检测。
                + 注意： filter() 不会改变原始数组。
            + 用法
                ```
                    array.filter(function(currentValue,index,arr), thisValue)
                    currentValue	必须。当前元素的值
                    index	可选。当前元素的索引值
                    arr	可选。当前元素属于的数组对象
                    thisValue	可选。对象作为该执行回调时使用，传递给函数，用作 "this" 的值。
                    如果省略了 thisValue ，"this" 的值为 "undefined"
                ```

### 字符串
+ 拓展方法
    + 子串识别
        + ES6 之前判断字符串是否包含子串，用 indexOf 方法，ES6 新增了子串的识别方法。
        + includes()：返回布尔值，判断是否找到参数字符串。
        + startsWith()：返回布尔值，判断参数字符串是否在原字符串的头部。
        + endsWith()：返回布尔值，判断参数字符串是否在原字符串的尾部
            ```
                let string = "apple,banana,orange";
                string.includes("banana");     // true
                string.startsWith("apple");    // true
                string.endsWith("apple");      // false
                string.startsWith("banana",6)  // true
            ```
        + 注意这三个方法只返回布尔值，如果需要知道子串的位置，还是得用 indexOf 和 lastIndexOf 。
        + 这三个方法如果传入了正则表达式而不是字符串，会抛出错误。而 indexOf 和 lastIndexOf 这两个方法，它们会将正则表达式转换为字符串并搜索它。

+ 模板字符串
    + 模板字符串相当于加强版的字符串，用反引号 `,除了作为普通字符串，还可以用来定义多行字符串，还可以在字符串中加入变量和表达式。
        + 普通字符串
            ```
                let string = `Hello'\n'world`;
                console.log(string); 
                // "Hello'
                // 'world"
            ```
        + 多行字符串
            ```
                let string1 =  `Hey,
                can you stop angry now?`;
                console.log(string1);
                // Hey,
                // can you stop angry now?
            ```
        + 字符串中加入变量和表达式
            ```
                let name = "Mike";
                let age = 27;
                let info = `My Name is ${name},I am ${age+1} years old next year.`
                console.log(info);
                // My Name is Mike,I am 28 years old next year.
            ```
        + 字符串中调用表达式
            ```
                function f(){
                    return "have fun!";
                }
                let string2= `Game start,${f()}`;
                console.log(string2);  // Game start,have fun!
            ```
        + 模板字符串中的换行和字符都是会被保留的
+ 标签模板
    + 标签模板，是一个函数的调用，其中调用的参数是模板字符串。
        ```
            alert`Hello world!`;
            // 等价于
            alert('Hello world!');
        ```
    + 当模板字符串中带有变量，会将模板字符串参数处理成多个参数
        ```
            function f(stringArr,...values){
            let result = "";
            for(let i=0;i<stringArr.length;i++){
            result += stringArr[i];
            if(values[i]){
                 result += values[i];
                }
              }
              return result;
            }
            let name = 'Mike';
            let age = 27;
            f`My Name is ${name},I am ${age+1} years old next year.`;
            // "My Name is Mike,I am 28 years old next year."
            
            f`My Name is ${name},I am ${age+1} years old next year.`;
            // 等价于
            f(['My Name is',',I am ',' years old next year.'],'Mike',28);
        ```
    + 应用过滤字符串，防止输入恶意内容（前端过滤有意义吗？截包直接改啊。。。https加密内容不清楚第三方有没有加解密方式，如果没有，可能安全性比较高，不过主要还是后端吧）
        ```
            function f(stringArr,...values){
            let result = "";
            for(let i=0;i<stringArr.length;i++){
            result += stringArr[i];
            if(values[i]){
                result += String(values[i]).replace(/&/g, "&amp;")
                        .replace(/</g, "&lt;")
                        .replace(/>/g, "&gt;");
                }
            }
            return result;
            }
            name = '<Amy&MIke>';
            f`<p>Hi, ${name}.I would like send you some message.</p>`;
            // <p>Hi, &lt;Amy&amp;MIke&gt;.I would like send you some message.</p>
        ```
+ 对象
    + 对象字面量
        + ES6允许对象的属性直接写变量，这时候属性名是变量名，属性值是变量值。
        ```
            const age = 12;
            const name = "Amy";
            const person = {age, name};
            person   //{age: 12, name: "Amy"}
            //等同于
            const person = {age: age, name: name}
        ```
    + 方法名可以简写(哦豁。。。原来我一直用的简写方法。。。)
        ```
            const person = {
            sayHi(){
                console.log("Hi");
              }
            }
            person.sayHi();  //"Hi"
            //等同于
            const person = {
            sayHi:function(){
                console.log("Hi");
              }
            }
            person.sayHi();//"Hi"
        ```
    + 如果是generator函数需要在前面加*
        ```
            const obj = {
            * myGenerator() {
                yield 'hello world';
              }
            };
            //等同于
            const obj = {
            myGenerator: function* () {
                yield 'hello world';
              }
            };
        ```
+ 对象的拓展运算符
    + 拓展运算符（...）用于取出参数对象所有可遍历属性然后拷贝到当前对象。
        + 基本用法
            ```
                let person = {name: "Amy", age: 15};
                let someone = { ...person };
                someone;  //{name: "Amy", age: 15}
            ```
        + 合并两个对象
            ```
                let age = {age: 15};
                let name = {name: "Amy"};
                let person = {...age, ...name};
                person;  //{age: 15, name: "Amy"}
            ```
        + 注意：自定义的属性和拓展运算符对象里面属性的相同的时候：自定义的属性在拓展运算符后面，则拓展运算符对象内部同名的属性将被覆盖掉。
            ```
               let person = {name: "Amy", age: 15};
                let someone = { ...person, name: "Mike", age: 17};
                someone;  //{name: "Mike", age: 17} 
            ```
            + 自定义的属性在拓展运算度前面，则变成设置新对象默认属性值。
                ```
                    let person = {name: "Amy", age: 15};
                    let someone = {name: "Mike", age: 17, ...person};
                    someone;  //{name: "Amy", age: 15}

                ```
    + 对象新方法
        + Object.assign(target, source_1, ···)用于将源对象的所有可枚举属性复制到目标对象中。
            + 如果目标对象和源对象有同名属性，或者多个源对象有同名属性，则后面的属性会覆盖前面的属性。
            + 如果该函数只有一个参数，当参数为对象时，直接返回该对象；当参数不是对象时，会先将参数转为对象然后返回。
            ```
                let target = {a: 1};
                let object2 = {b: 2};
                let object3 = {c: 3};
                Object.assign(target,object2,object3);  
                // 第一个参数是目标对象，后面的参数是源对象
                target;  // {a: 1, b: 2, c: 3}
            ```
        + 注意assign是浅拷贝，当拷贝源和目标任意值变动后，两个参数将会自动同步
            + 浅拷贝，只是单纯的复制了目标地址
            + 深拷贝，单纯申请了内存空间存储拷贝的参数
### 数组
+ 数组创建
    + Array.of()将参数中所有的值转变为数组
        ```
            console.log(Array.of(1, 2, 3, 4)); // [1, 2, 3, 4]
 
            // 参数值可为不同类型
            console.log(Array.of(1, '2', true)); // [1, '2', true]
            
            // 参数为空时返回空数组
            console.log(Array.of()); // []
        ```
    + Array.from()将类数组对象或可迭代对象转化为数组。
        ```
            // 参数为数组,返回与原数组一样的数组
            console.log(Array.from([1, 2])); // [1, 2]
            
            // 参数含空位
            console.log(Array.from([1, , 3])); // [1, undefined, 3]
        ```
    + 参数
        ```
            Array.from(arrayLike[, mapFn[, thisArg]])
        ```
        返回值为转换后的数组。

        + arrayLike想要转换的类数组对象或可迭代对象。
            ```
                console.log(Array.from([1, 2, 3])); // [1, 2, 3]                
            ```
        + mapFn可选，map 函数，用于对每个元素进行处理，放入数组的是处理后的元素。
            ```
                console.log(Array.from([1, 2, 3], (n) => n * 2)); // [2, 4, 6]
            ```
        + thisArg可选，用于指定 map 函数执行时的 this 对象。
            ```
                let map = {
                    do: function(n) {
                        return n * 2;
                    }
                }
                let arrayLike = [1, 2, 3];
                console.log(Array.from(arrayLike, function (n){
                    return this.do(n);
                }, map)); // [2, 4, 6]
            ```
    + 转换可迭代对象
        ```
            console.log(Array.from(map));
            console.log(Array.from(set));
            console.log(Array.from(str));
        ```
    + 3.2.4太多了。。。

### 箭头函数
+ 箭头函数提供了一种更加简洁的函数书写方式。基本语法是：
    + 结构：参数 => 函数体
    + 基本用法：
        ```
            var f = v => v;
            //等价于
            var f = function(a){
            return a;
            }
            f(1);  //1
            当箭头函数没有参数或者有多个参数，要用 () 括起来。

            var f = (a,b) => a+b;
            f(6,2);  //8
            当箭头函数函数体有多行语句，用 {} 包裹起来，表示代码块，当只有一行语句，并且需要返回结果时，可以省略 {} , 结果会自动返回。

            var f = (a,b) => {
            let result = a+b;
            return result;
            }
            f(6,2);  // 8
            当箭头函数要返回对象的时候，为了区分于代码块，要用 () 将对象包裹起来

            // 报错
            var f = (id,name) => {id: id, name: name};
            f(6,2);  // SyntaxError: Unexpected token :
            
            // 不报错
            var f = (id,name) => ({id: id, name: name});
            f(6,2);  // {id: 6, name: 2}
            注意点：没有 this、super、arguments 和 new.target 绑定。

        ```
    + 箭头函数中this指向
        ```
            var func = () => {
            // 箭头函数里面没有 this 对象，
            // 此时的 this 是外层的 this 对象，即 Window 
            console.log(this)
            }
            func(55)  // Window 
            
            var func = () => {    
            console.log(arguments)
            }
            func(55);  // ReferenceError: arguments is not defined
        ```
    + 普通函数的this指向
        ```
            this指针的指向并不是在创建时定义的，而是在被调用时确定的，谁调用了包含this指针的对象，this就会指向该函数的上层，所以在web工程中，当函数中嵌套的函数包含this指针时，this指针无法指向上层函数外的数据对象，这时就需要在上层函数引入_this = this将最外层的this指针引入函数内部
        ```
    + 箭头函数体中的 this 对象，是定义函数时的对象，而不是使用函数时的对象。
        ```
            function fn(){
            setTimeout(()=>{
                // 定义时，this 绑定的是 fn 中的 this 对象
                console.log(this.a);
            },0)
            }
            var a = 20;
            // fn 的 this 对象为 {a: 19}
            fn.call({a: 18});  // 18
        ```
    + 不可以作为构造函数，也就是不能使用 new 命令，否则会报错

    + 适合使用的场景------ES6 之前，JavaScript 的 this 对象一直很令人头大，回调函数，经常看到 var self = this 这样的代码，为了将外部 this 传递到回调函数中，那么有了箭头函数，就不需要这样做了，直接使用 this 就行。
        ```
            // 回调函数
            var Person = {
                'age': 18,
                'sayHello': function () {
                setTimeout(function () {
                    console.log(this.age);
                });
                }
            };
            var age = 20;
            Person.sayHello();  // 20
            
            var Person1 = {
                'age': 18,
                'sayHello': function () {
                setTimeout(()=>{
                    console.log(this.age);
                });
                }
            };
            var age = 20;
            Person1.sayHello();  // 18
        ```
    + 不适合使用的场景------定义函数的方法，且该方法中包含 this
        ```
            var Person = {
                'age': 18,
                'sayHello': ()=>{
                    console.log(this.age);
                }
            };
            var age = 20;
            Person.sayHello();  // 20
            // 此时 this 指向的是全局对象
            
            var Person1 = {
                'age': 18,
                'sayHello': function () {
                    console.log(this.age);
                }
            };
            var age = 20;
            Person1.sayHello();   // 18
            // 此时的 this 指向 Person1 对象
        ```
        problems：for...in和for..of的区别
    + 看样子今天是极限了

### es6其他
+ 参考链接
    + https://blog.csdn.net/qq_39207948/article/details/80678800

+ DOM
+ BOM
+ Ajax
    + AJAX = Asynchronous JavaScript and XML（异步的 JavaScript 和 XML）。
    + AJAX 不是新的编程语言，而是一种使用现有标准的新方法。
    + AJAX 最大的优点是在不重新加载整个页面的情况下，可以与服务器交换数据并更新部分网页内容。
    + AJAX 不需要任何浏览器插件，但需要用户允许JavaScript在浏览器上执行
+ JSON
    + JSON 是用于存储和传输数据的格式。
    + JSON 通常用于服务端向网页传递数据 
    + 什么是 JSON?
        + JSON 英文全称 JavaScript Object Notation
        + JSON 是一种轻量级的数据交换格式。
        + JSON是独立的语言 *
        + JSON 易于理解
    + JSON 语法规则
        + 数据为 键/值 对。
        + 数据由逗号分隔。
        + 大括号保存对象
        + 方括号保存数组
    + 常用函数
        + JSON.parse()	用于将一个 JSON 字符串转换为 JavaScript 对象。
        + JSON.stringify()	用于将 JavaScript 值转换为 JSON 字符串。
+ Vue生命周期
    + beforeCreate
        + 实例（页面）创建之前的生命周期
    + created
        + 实例已经完成数据观测（data），属性方法运算，watch，event事件回调，挂载阶段还未开始el属性不可见
    + beforemount
        + 挂载之前被调用，相关渲染函数首次被调用
    + mounted
        + el被新创建的vm.$el替换挂载成功
    + beforeupdate
        + 数据更新时调用
    + update
        +数据更新时
+ css/js性能优化，解决多浏览器兼容问题

+ 将嵌套数组转化为一维数组
    ``` 
        var arr = ['mu','zi',['dig',['big','love']]]
        function fun1 (arr) {
            var res = []
            for(let i=0;i<arr.length;i++) {
                if(Array.isArray(arr[i])) {
                    res = res.concat(fun1(arr[i]))
                } else {
                    res.push(arr[i])
                }
            }
            return res
        }
        console.log(fun1(arr))
    ```