
## 防抖节流

### 防抖
- 连续点击 只有点击间隔超过一定时间间隔，才会执行
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
