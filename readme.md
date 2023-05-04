
### 先说结论
- dom 节点数量对内存影响没想象中大，js 变量才是内存占用的元凶
- 内存占用及运行性能对比：原生 js < v-for div ≈ v-for 函数式组件 < v-for 普通组件
- 去虚拟 dom 化框架正在崛起，成为一种新的选择
![](http://upyun.luckly-mjw.cn/Assets/vue-component-performance/000.png)


### dom 节点数量对内存影响没想象中大（十万 div 仅占用 400mb）
- [测试示例](https://blog.luckly-mjw.cn/tool-show/vue-component-performance/pure-js.html)
- 生成 100,000（十万）个 div，内存占用仅 400mb，平均每个 div 占用内存 400 * 1024 kb / 100,000 = 4kb
![](http://upyun.luckly-mjw.cn/Assets/vue-component-performance/001.png)
- 注意，这里内存占用是静态状态内存，即 div 生成完成后等待一会（约1min）内存回收完成后的内存占用，div 生成过程中会产生中间变量，内存占用会比静态时高。
- 如果短时间多少清空 div 再重新生成，会导致内存占用变大
![](http://upyun.luckly-mjw.cn/Assets/vue-component-performance/002.png)


### 组件过多，或许才是 vue 项目内存占用大、运行慢的原因
*分别以「纯 js 渲染 div」、「v-for 渲染 div」、「v-for 渲染函数式组件」、「v-for 渲染普通组件」为例做性能对比测试*

[「纯 js 渲染 div」运行示例](https://blog.luckly-mjw.cn/tool-show/vue-component-performance/pure-js.html)，[对应源码](https://github.com/Momo707577045/vue-component-performance/blob/main/pure-js.html)
- 10,000（一万）个 div
  - 渲染耗时：26ms，平均单个div渲染耗时：0.0026ms
  ![](http://upyun.luckly-mjw.cn/Assets/vue-component-performance/003.png)

- 100,000（十万）个 div
  - 渲染耗时：265ms，平均单个div渲染耗时：0.0027ms
  ![](http://upyun.luckly-mjw.cn/Assets/vue-component-performance/004.png)

- 100,000（十万）个 div
  - 占用内存：528mb，平均单个占用内存：528 * 1024 / 100,000 = 5.4kb
  ![](http://upyun.luckly-mjw.cn/Assets/vue-component-performance/002.png)



[「v-for 渲染 div」运行示例](https://blog.luckly-mjw.cn/tool-show/vue-component-performance/vue-div.html)，[对应源码](https://github.com/Momo707577045/vue-component-performance/blob/main/vue-div.html)
- 10,000（一万）个 div
  - 使用 key 缓存节点渲染耗时：61ms，平均单个div渲染耗时：0.0061ms
  - 不使用 key 缓存节点渲染耗时：91ms，平均单个div渲染耗时：0.0091ms
  ![](http://upyun.luckly-mjw.cn/Assets/vue-component-performance/007.png)
  

- 100,000（十万）个 div
  - 使用 key 缓存节点渲染耗时：499ms，平均单个div渲染耗时：0.005ms
  - 不使用 key 缓存节点渲染耗时：286ms，平均单个div渲染耗时：0.0029ms
  ![](http://upyun.luckly-mjw.cn/Assets/vue-component-performance/008.png)

- 100,000（十万）个 div
  - 占用内存：586mb，平均单个占用内存：586 * 1024 / 100,000 = 6kb
  ![](http://upyun.luckly-mjw.cn/Assets/vue-component-performance/009.png)

[「v-for 渲染函数式组件」运行示例](https://blog.luckly-mjw.cn/tool-show/vue-component-performance/vue-func-component.html)，[对应源码](https://github.com/Momo707577045/vue-component-performance/blob/main/vue-func-component.html)
 - 10,000（一万）个 div
  - 使用 key 缓存节点渲染耗时：85ms，平均单个div渲染耗时：0.0085ms
  - 不使用 key 缓存节点渲染耗时：82ms，平均单个div渲染耗时：0.0082ms
  ![](http://upyun.luckly-mjw.cn/Assets/vue-component-performance/010.png)
  

- 100,000（十万）个 div
  - 使用 key 缓存节点渲染耗时：392ms，平均单个div渲染耗时：0.0039ms
  - 不使用 key 缓存节点渲染耗时：505ms，平均单个div渲染耗时：0.005ms
  ![](http://upyun.luckly-mjw.cn/Assets/vue-component-performance/011.png)

- 100,000（十万）个 div
  - 占用内存：654mb，平均单个占用内存：654 * 1024 / 100,000 = 6.7kb
  ![](http://upyun.luckly-mjw.cn/Assets/vue-component-performance/012.png)

[「v-for 渲染普通组件」运行示例](https://blog.luckly-mjw.cn/tool-show/vue-component-performance/vue-component.html)，[对应源码](https://github.com/Momo707577045/vue-component-performance/blob/main/vue-component.html)
- 10,000（一万）个 div
  - 使用 key 缓存节点渲染耗时：122ms，平均单个div渲染耗时：0.0122ms
  - 不使用 key 缓存节点渲染耗时：360ms，平均单个div渲染耗时：0.0325ms
  ![](http://upyun.luckly-mjw.cn/Assets/vue-component-performance/013.png)
  

- 100,000（十万）个 div
  - 使用 key 缓存节点渲染耗时：918ms，平均单个div渲染耗时：0.0092ms
  - 不使用 key 缓存节点渲染耗时：13881ms(13.8s)，平均单个div渲染耗时：0.1388ms
  ![](http://upyun.luckly-mjw.cn/Assets/vue-component-performance/014.png)

- 100,000（十万）个 div
  - 占用内存：907mb，平均单个占用内存：907 * 1024 / 100,000 = 9.3kb
  ![](http://upyun.luckly-mjw.cn/Assets/vue-component-performance/015.png)

### 通过上述测试可得

|  | 纯 js 渲染 | v-for 渲染 div | v-for 渲染函数式组件 | v-for 渲染普通组件
| --- | --- | --- | --- | --- |
| 一万div复用key渲染耗时 | - | 61ms/6.1us | 85ms/8.5us  | 122ms/12.2us
| 一万div不复用key渲染耗时 | 26ms/2.6us | 91ms/9.1us | 82ms/8.2us | 360ms/36us
| 十万div复用key渲染耗时 | - | 286ms/2.9us | 392ms/3.9ms | 918ms/9.2us
| 十万div不复用key渲染耗时 | 265ms/2.7us | 499m/5us | 505ms/5us | 13881ms/138.8us
| 十万div内存占用 | 528mb/5.4kb | 586mb/6kb | 652mb/6.7kb | 907mb/9.3kb

- dom 节点数量对内存影响没想象中大，10万 div 占用内存 400mb（只生成一次），平均单个 div 占用内存仅 4kb
- vue 组件对内存占用较大，仅仅最简单的 div 组件就需要额外添加 9.3 - 5.4 = 3.9kb 内存，若组件复杂度上升，则内存占用将进一步增加。成为内存瓶颈
- vue 函数式组件、v-for div 是否使用 key 缓存节点，对运行耗时变化不大
- vue 组件是 key 缓存节点，对节点更新性能有很大提升，10万组件更新，从 13s 耗时优化到 0.9s 耗时，减少近 13 - 0.9 / 13 = 93% 运算耗时
- vue 组件并非越细越好，颗粒度太细，将导致 vue 组件数量急剧增加，内存占用急剧增加
- 纯 js 渲染 div 最快，使用 v-for div，v-for 函数式组件的渲染耗时至少是 纯 js 渲染耗时的两倍，这是由于虚拟节点 vnode 需要执行 diff 算法决定的。

### 去虚拟化正在崛起
*既然 vnode 计算如此消耗性能，那可否舍弃 vnode 来提高性能呢？答案是肯定的，svelte（类似 vue），solid(类似 react) 就是这么做的*
[Svelte](https://www.svelte.cn/)，[新兴前端框架 Svelte 从入门到原理](https://zhuanlan.zhihu.com/p/350507037)，[Virtual DOM is pure overhead](https://www.svelte.cn/blog/virtual-dom-is-pure-overhead)，[虚拟DOM是纯粹的浪费](https://juejin.cn/post/6844904137826533384)
![](http://upyun.luckly-mjw.cn/Assets/vue-component-performance/105.png)
![](http://upyun.luckly-mjw.cn/Assets/vue-component-performance/101.png)


[Solidjs](https://www.solidjs.com/)，[SolidJS入门](https://juejin.cn/post/7041485386010329118)
![](http://upyun.luckly-mjw.cn/Assets/vue-component-performance/104.png)
![](http://upyun.luckly-mjw.cn/Assets/vue-component-performance/106.png)
![](http://upyun.luckly-mjw.cn/Assets/vue-component-performance/102.png)

### [对于 Svelte，尤雨溪​怎么看](https://www.zhihu.com/question/53150351/answer/133912199)
![](http://upyun.luckly-mjw.cn/Assets/vue-component-performance/107.png)






