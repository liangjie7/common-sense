

 防抖，节流，去重
----------

### 防抖
- 连续点击，只有点击间隔超过指定时间间隔，才会执行

```
function debonce(fn){
  let timeout = null;
  return function(){
    clearTimeout(timeout);
    timeout = setTimeout(()=>{
      fu.call(this,arguments);
    },1000)
  }
}
```

### 节流

- 指定时间间隔里只会执行一次

```
  function throttle(fn){
    let isActive = true;
    return function(){
      if(!isActive){
        return
      }
      isActive = false;
      setTimeout(()=>{
        fu.call(this,arguments);
        isActive = true;
      },1000)
    }
  }

```

### 数组去重

```
var arr = [1,'1',1,2,3,1];
function unique(arr){
  console.log(new Set(arr)); //Set(4) {1, "1", 2, 3}
  console.log(Array.from(new Set(arr))) //[1, "1", 2, 3]
  console.log([...new Set(arr)]) //[1, "1", 2, 3]
  console.log(...new Set(arr)); //1 "1" 2 3
}

```

```
function unique(arr){
  const res = new Map();
  return arr.filter((item)=>!res.has(item) && res.set(a,1));
}

```

循环
----------
### array.forEach(function(item,index,array){});

- 无法遍历对象
- 无法在IE中使用
- 不能使用break,continue，使用return，效果和form循环中的continue一样。

###  for(let item in array/object ){}

遍历对象的可枚举属性，以及对象从其构造函数原型继承的属性，不建议使用 for...in 遍历数组，因为不仅遍历数组的数字键名。

### for(let key of array/object){}

推荐使用 for...of..遍历数组和对象，对于普通的对象循环会报错，可以使用Object.keys方法获取对象的键名生成的数组，然后遍历这个数组。


```
let obj = {name:'liang', age:18 };
for (let key of Object.keys(obj)){
	console.log(key + '--' + obj[key])
}
```
### map

array.map(function(currentValue, index, arr){},thisValue)

只支持数组（类似数组的对象，NodeList，String）， 返回一个新数组。

### every

和map一样只支持数组，所有元素符合条件返回ture，否则返回false。不会改变原数组。

```
var ages = [32, 33, 16, 40];

function checkAdult(age) {
    return age >= 18;
}
ages.every(checkAdult);//false
```
### some
对于数组中的任意一项满足条件则返回true.

```
var ages = [32, 33, 16, 40];

function checkAdult(age) {
    return age >= 18;
}
ages.some(checkAdult);//true
```




