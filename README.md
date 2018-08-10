# webworker-demo

演示webworker用法，以及其如何改善页面性能

## let me try

打开`index.html`([在线](https://clumsyme.github.io/webworker/index.html))，分别在左侧和右侧输入框输入`44`(取决于计算机性能)，点击计算，等待结果，查看页面是否有卡顿。

## that's it

在主脚本`main.js`及`woker.js`内都有一个递归版本的斐波那契数计算函数

```js
const fib = (n) => {
    if (n <= 2) {
        return 1
    } else {
        return fib(n-1) + fib(n-2)
    }
}
```

在输入框内输入数字`n`，点击计算，将计算第斐波那契数列第`n`项，结果显示在下边，左右区别为，当输入数字较大时（以40-45为例）：

### 左侧将直接使用主脚本内的函数进行计算，因此将阻塞UI线程，造成页面卡顿甚至卡死

注意点击`计算`后，上方标题有一个持续4s的`transition`，左侧将在`transition`进行1s时开始计算，可以看到开始计算时`transition`停止了。

### 右侧使用`worker`内的函数进行计算，运行在worker线程，不会造成页面卡顿

点击`计算`后，标题`transition`能流畅运行

## 关于`animation`

可以发现，不论是左边还是右边，有没有worker，下方小球的运动从来没有被阻塞，因为小球用的是`animation`，浏览器通常会将其运行在GPU线程上，因此不会被JS线程阻塞。