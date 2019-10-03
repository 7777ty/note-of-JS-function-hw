# JS函数的执行时机
## setTimeout运行机制

在了解setTimeout的运行机制前我们需要先看下下列代码段：
```Javascript
let i = 0
for(i = 0; i<6; i++){
  setTimeout(()=>{
    console.log(i)
  },0)
}
```
上方代码段的最后打印结果为六次6。很显然setTime延迟了console.log的执行。而这一现象的出现就是setTimeout的运行机制造成的。setTimeout和setInterval的运行机制是，将指定的代码移出本次执行，等到下一轮Event Loop时，再检查是否到了指定时间。如果到了，就执行对应的代码；如果不到，就等到再下一轮Event Loop时重新判断。这意味着，setTimeout指定的代码，必须等到本次执行的所有代码都执行完，才会执行。

那么，由上面setTimeout的运行机制我们可以很快发现一个问题：如果后面立即运行的任务（当前脚本的同步任务））非常耗时，过了setTimeout的延迟执行时间还无法结束的话，那么会出现怎样一种情况呢？答案是：如果立即运行的任务在规定时间内还未完成的话，那么被推迟运行的someTask就只有等着，等到前面的veryLongTask运行结束，才轮到它执行。

为了让上述代码的打印结果为0，1，2，3，4，5。我们可以将for，let配合使用，即：

```Javascript
for(let i = 0; i<6; i++){
  setTimeout(()=>{
    console.log(i)
  },0)
}
```

或者是，删除setTimeout（~~机灵鬼上线~~）：
```Javascript
let i = 0
for(i = 0; i<6; i++){

    console.log(i)

}
```
