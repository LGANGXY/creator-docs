# 使用计时器

在 Cocos Creator 中，我们为组件提供了方便的计时器，这个计时器源自于 Cocos2d-x 中的 `cc.Scheduler`，我们将它保留在了 Cocos Creator 中并适配了基于组件的使用方式。

也许有人会认为 `setTimeout` 和 `setInterval` 就足够了，开发者当然可以使用这两个函数，不过我们更推荐使用计时器，因为它更加强大灵活，和组件也结合得更好！

下面来看看它的具体使用方式：

1. 开始一个计时器

    ```
    component.schedule(function() {
        // 这里的 this 指向 component
        this.doSomething();
    }, 5);
    ```

    上面这个计时器将每隔 5s 执行一次。

2. 更灵活的计时器

    ```
    // 以秒为单位的时间间隔
    var interval = 5;
    // 重复次数
    var repeat = 3;
    // 开始延时
    var delay = 10;
    component.schedule(function() {
        // 这里的 this 指向 component
        this.doSomething();
    }, interval, repeat, delay);
    ```

    上面的计时器将在10秒后开始计时，每5秒执行一次回调，重复3次。

3. 只执行一次的计时器（快捷方式）

    ```
    component.scheduleOnce(function() {
        // 这里的 this 指向 component
        this.doSomething();
    }, 2);
    ```

    上面的计时器将在两秒后执行一次回调函数，之后就停止计时。

4. 取消计时器

    开发者可以使用回调函数本身来取消计时器：

    ```
    this.count = 0;
    this.callback = function () {
        if (this.count === 5) {
            // 在第六次执行回调时取消这个计时器
            this.unschedule(this.callback);
        }
        this.doSomething();
        this.count++;
    }
    component.schedule(this.callback, 1);
    ```

下面是 Component 中所有关于计时器的函数：

- schedule：开始一个计时器
- scheduleOnce：开始一个只执行一次的计时器
- unschedule：取消一个计时器
- unscheduleAllCallbacks：取消这个组件的所有计时器

这些 API 的详细描述都可以在 [Component API](http://cocos.com/docs/creator/api/classes/Component.html) 文档中找到。

除此之外，如果需要每一帧都执行一个函数，请直接在 Component 中添加 `update` 函数，这个函数将默认被每帧调用，这在[生命周期文档](life-cycle-callbacks.md#update)中有详细描述。

### **注意：`cc.Node` 不包含计时器相关 API**
