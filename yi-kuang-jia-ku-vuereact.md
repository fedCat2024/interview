# 一、框架/库（Vue、React）

## 1、Vue生命周期、顺序、各生命周期做了什么？

所有生命周期钩子的this上下文将自动绑定至实例中，因此可以访问data、computed和methods。这意味着不应该使用箭头函数来定义一个生命周期方法，因为箭头函数绑定了父级上下文、而不是预期的组件实例。

### （1）Vue2

<figure><img src=".gitbook/assets/1-1-1.png" alt="" width="375"><figcaption></figcaption></figure>

### （2）Vue3

<figure><img src=".gitbook/assets/1-1-2.png" alt="" width="375"><figcaption></figcaption></figure>

### （3）详细介绍

<table data-full-width="true"><thead><tr><th width="110">生命周期</th><th>Vue2</th><th>Vue3 选项式API</th><th>Vue3 组合式API</th><th width="340">详细信息</th><th>服务端渲染时是否调用</th></tr></thead><tbody><tr><td>创建</td><td>beforeCreate</td><td>beforeCreate</td><td>setup</td><td><ul><li>setup()钩子是在组件中使用组合式API的入口，会在所有选项式API钩子之前调用；</li><li>beforeCreate: 组件实例初始化完成且props被解析后立即调用。</li></ul></td><td>是</td></tr><tr><td>创建</td><td>created</td><td>created</td><td>setup</td><td>created: 在组件实例处理完所有与状态有关的选项之后调用，响应式数据、计算属性、方法和侦听器已设置完成。</td><td>是</td></tr><tr><td>挂载</td><td>beforeMount</td><td>beforeMount</td><td>onBeforeMount</td><td>组件已完成响应式状态设置，但还没创建DOM节点，它即将首次执行DOM渲染过程。</td><td>否</td></tr><tr><td>挂载</td><td>mounted</td><td>mounted</td><td>onMounted</td><td>在组件被挂载（除异步组件或suspense树内组件外所有同步子组件均已挂载，自身DOM树已创建并插入父容器）之后调用。</td><td>否</td></tr><tr><td>更新</td><td>beforeUpdate</td><td>beforeUpdate</td><td>onBeforeUpdate</td><td>在组件即将因为一个响应状态变更而更新其DOM树之前调用，能访问到DOM变更之前的状态。</td><td>否</td></tr><tr><td>更新</td><td>updated</td><td>updated</td><td>onUpdated</td><td>在组件因为一个响应式状态变更而更新其DOM树之后调用。<br>注：在updated钩子中更改组件状态，会导致无限更新循环！</td><td>否</td></tr><tr><td>销毁</td><td>beforeDestroy</td><td>beforeUnmount</td><td>onBeforeUnmount</td><td>在一个组件实例被卸载之前调用，组件实例还保有全部功能。</td><td>否</td></tr><tr><td>销毁</td><td>destroyed</td><td>unmounted</td><td>onUnmounted</td><td>在一个组件实例被卸载（所有子组件均已卸载，所有响应式作用都已停止）之后调用，可以在这个钩子中手动清理一些副作用（如计时器、DOM事件监听器、与服务器的连接等）。</td><td>否</td></tr><tr><td>keep-alive</td><td>activated</td><td>activated</td><td>onActivated</td><td>被keep-alive缓存的组件被插入到DOM中时调用。</td><td>否</td></tr><tr><td>keep-alive</td><td>deactivated</td><td>deactivated</td><td>onDeactivated</td><td>被keep-alive缓存的组件从DOM中被移除时调用。</td><td>否</td></tr><tr><td>捕获错误</td><td>errorCaptured(Vue2.5.0+新增)</td><td>errorCaptured</td><td>onErrorCaptured</td><td>在捕获了后代组件传递的错误时调用。<br>默认错误向上传递、直到顶层应用级 app.config.errorHandler；errorCaptured钩子本身抛出的错误将和原来捕获到的错误一起发送到 app.config.errorHandler；可通过在钩子里返回false来阻止错误继续向上传递。</td><td>是</td></tr><tr><td>调式Dev only</td><td></td><td>renderTracked</td><td>onRenderTracked</td><td>注册一个调试钩子，当组件渲染过程中追踪到响应式依赖时调用。</td><td>否</td></tr><tr><td>调式Dev only</td><td></td><td>renderTriggered</td><td>onRenderTriggered</td><td>注册一个调试钩子，当响应式依赖的变更触发了组件渲染时调用。</td><td>否</td></tr><tr><td>SSR only</td><td></td><td>serverPrefetch</td><td>onServerPrefetch</td><td>注册一个异步函数，在组件实例在服务器上被渲染之前调用。</td><td>仅在服务端渲染中执行</td></tr></tbody></table>

## 2、父子组件生命周期顺序

## 3、Vue双向数据绑定原理

## 4、Vue2、Vue3实现响应式区别

## 5、Virtual DOM

### （1）Snabbdom

Snabbdom是一个高效的虚拟DOM库，以其简单和灵活的架构著称。核心库小巧、快速，同时提供了丰富的接口供开发者定制。Snabbdom可以与多种框架或纯JavaScript项目一起使用。它的核心特征包括：

1）模块化：它采用了模块化的设计，允许开发者通过引入不同的模块来扩展功能，如事件处理、样式处理等；

2）灵活性：允许开发者通过钩子（hooks）函数在节点的生命周期不同阶段执行自定义逻辑；

3）高效：通过对比新旧虚拟节点来计算最小的DOM更新操作，这种方法有效减少了不必要的DOM操作，提高了应用性能。

### （2）Vue的Virtual DOM

### （3）Vue的VirtualDOM的2个主要方法是什么？

### （4）React的Virtual DOM

React的虚拟DOM是React框架的核心特征之一，旨在提高应用的性能和效率。虚拟DOM是对真实DOM的一个轻量级的JavaScript对象表示。使用虚拟DOM，React能够最小化与真实DOM的交互次数，因为直接操作DOM通常是Web应用中性能瓶颈的主要原因之一。

工作原理：

#### 1）创建虚拟DOM树

当React组件渲染时，它会基于组件的状态（state）和属性（props）创建一个虚拟DOM树。

#### 2）比较虚拟DOM树（diff）

当组件状态或属性发生变化时，React会创建一个新的虚拟DOM树，然后比较新旧虚拟DOM树，以确定实际DOM需要做的最小更新。

我们知道DOM树是多叉树，比较两颗多叉树的算法（即找出两棵树的最小编辑距离并进行更新），这个算法被Pawlik and N.Augsten优化到O(n^3)（先找到两棵树的最小更新方式、两两比较就需要O(n^2)，找到差异后还要计算最小转换方式，所以是O(n^3)）。React团队优化了算法，只对比同层的节点，而不进行跨层对比，可以把时间复杂度优化到O(n)。

具体过程为进行树的深度优先遍历，同时给每个节点添加索引，便于最后渲染差异；一旦节点有子节点，就去判断子节点是否有不同。

【总结版】

* 只进行同级比较，当前节点发生改变，直接删除当前节点及其子节点；
* 对于没有设置key的节点只有增删改操作；
* 对于设置了key的节点，有增删移操作：\
  维护2个index（oldIndex和maxIndex），遍历到当前新节点，通过key找到其在旧列表中的index，即oldIndex，如果新节点index < oldIndex，则不移动，maxIndex设置为oldIndex；如果新节点index > oldIndex，将该旧节点移动到maxIndex的位置；遍历完一个列表节点后，继续遍历还没遍历完的列表，如果旧节点列表没遍历完，这些节点全都要删除，如果新节点列表没遍历完，这些节点全都要新增。
* 但对于只有文本的列表，不设置key的性能会更好，因为不设置key，就直接用innerText替换文本，而没有移动、增加、删除等对虚拟DOM的操作。

【细节版】diff过程细节为：

对DOM节点进行逐层比对，有3种情况：

* 没有新节点，什么都不做
* 新节点tagName或key与旧节点不同，则需要替换掉这个节点，不再继续遍历子元素，因为整个旧节点都被删掉了
* 新节点和旧节点的tagName和key都相同（可能新旧节点都没有key），那么开始遍历子节点，进行diffChildren 和 diffProps：
  * 判断两个列表的差异、进行listDiff，判断列表差异也分3步骤 【删、增、移】
    1. 遍历旧的节点列表，查看每个节点是否还存在新的节点列表中，以找到被删除的列表节点；
    2. 遍历新的节点列表，查看是否有新增的节点，以找到新增的列表节点；
    3. 在第2步的同时查看是否有节点被移动了，以找到被移动的节点（及其从哪移动到哪）
  * 给节点打上标记
  * 此时还需要判断是否有属性更新、进行diffProps，判断属性变更也分3步骤 【删、改、增】
    1. 遍历旧的属性列表，查看是否每个属性都出现在新属性列表中，以找到被删除的属性；
    2. 遍历新的属性列表，查看新旧属性列表都出现的属性是否有变化，以找到发生变化的属性；
    3. 第2步的同时查找旧属性列表中没有的属性，以找到新增的属性。

#### 3）更新真实DOM

最后渲染差异，拿着前面得到的两棵树的差异，去局部更新DOM。

* 深度遍历树，将需要做变更操作的节点取出来；
* 局部更新DOM（setAttribute、removeAttribute、node.remove()、node.insertBefore()等方法）。

### （5）Vue与React的Virtual DOM区别

### （6）Virtual DOM有什么作用？
