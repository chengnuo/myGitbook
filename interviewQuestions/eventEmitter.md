# eventEmitter

## 需求


Node.js EventEmitter

Node.js 所有的异步 I/O 操作在完成时都会发送一个事件到事件队列。

Node.js 里面的许多对象都会分发事件：一个 net.Server 对象会在每次有新连接时触发一个事件， 一个 fs.readStream 对象会在文件被打开的时候触发一个事件。 所有这些产生事件的对象都是 events.EventEmitter 的实例。


## EventEmitter [在 CodePen 中打开](https://codepen.io/chengnuo/pen/NoBrJZ)

```javascript
class EventEmitter{
    constructor() {
        this.handler = {};
    }
    on(eventName, callback){
        if(!this.handler){
            this.handler = {};
        }
        if(!this.handler[eventName]){
            this.handler[eventName] = [];
        }
        this.handler[eventName].push(callback);
    }
    emit(eventName, ...arg){
        if(this.handler[eventName]){
            for(let i=0;i<this.handler[eventName].length;i++){
                // this.handler[eventName][i](...arg)
                this.handler[eventName].forEach(fn => fn.call(this, ...arg));
            }
        }
    }
}


let event=new EventEmitter();
event.on('say',function(str){
   console.log(str);
});
event.emit('say','hello Jony yu');
```

## 总结

其实 eventEmitter 就是观察者模式的一种

## 参考

```
```