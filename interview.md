
## 防抖节流

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