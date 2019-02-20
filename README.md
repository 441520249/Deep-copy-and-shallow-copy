# Deep-copy-and-shallow-copy
所谓深浅拷贝，都是进行复制，那么区别主要在于复制出来的新对象和原来的对象是否会互相影响，改一个，另一个也会变。

//1、 利用 递归 来实现深复制，对属性中所有引用类型的值，遍历到是基本类型的值为止。

```js
        function deepClone(source){    
        if(!source && typeof source !== 'object'){      
            throw new Error('error arguments', 'shallowClone');    
        }    
        var targetObj = Array.isArray(source) ? [] : {};    
        for(var keys in source){       
            if(source.hasOwnProperty(keys)){          
            if(source[keys] && typeof source[keys] === 'object'){  
                targetObj[keys] = deepClone(source[keys]);    //递归      
            }else{            
                targetObj[keys] = source[keys];         
            }       
            }    
        }    
        return targetObj; 
        }
        var a = {name:"jack",age:20};
        var b = deepClone(a);
        console.log(a === b);//false
        a.age = 30;
        console.log(a);//{name: "jack", age: 30}
        console.log(b);//{name: "jack", age: 20}
 ```
 //2、 jQuery中的 extend 复制方法  可以用来扩展对象，这个方法可以传入一个参数:deep(true or false)，表示是否执行深复制(如果是深复制则会执行递归复制)。
        //栗子：
        //深拷贝：
```js
        var obj = {name:'xixi',age:20,company : { name : '腾讯', address : '深圳'} };
        var obj_extend = $.extend(true,{}, obj); //extend方法，第一个参数为true，为深拷贝，为false，或者没有为浅拷贝。
        console.log(obj === obj_extend);//false
        obj.company.name = "deep";
        obj.name = "deepcopy";
        console.log(obj);//{name:'deepcopy',age:20,company : { name : 'deep', address : '深圳'} }
        console.log(obj_extend);//{name:'xixi',age:20,company : { name : '腾讯', address : '深圳'} }
```
//3、JSON 对象的 parse 和 stringify
        //JOSN 对象中的 stringify 可以把一个 js 对象序列化为一个 JSON 字符串，parse 可以把 JSON 字符串反序列化为一个 js 对象，这两个方法实现的是深拷贝。
        //栗子：
```js
        var obj = {name:'xixi',age:20,company : { name : '腾讯', address : '深圳'} };
        var obj_json = JSON.parse(JSON.stringify(obj));
        console.log(obj === obj_json);//false
        obj.company.name = "deep";
        obj.name = "deepcopy";
        console.log(obj);//{name:'deepcopy',age:20,company : { name : 'deep', address : '深圳'} }
        console.log(obj_json);//{name:'xixi',age:20,company : { name : '腾讯', address : '深圳'} }
```
