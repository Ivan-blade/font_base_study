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