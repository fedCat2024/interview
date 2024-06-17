# 一、框架/库（Vue、React）

## 1、Vue生命周期、顺序、各生命周期做了什么？

所有生命周期钩子的this上下文将自动绑定至实例中，因此可以访问data、computed和methods。这意味着不应该使用箭头函数来定义一个生命周期方法，因为箭头函数绑定了父级上下文、而不是预期的组件实例。

### （1）Vue2

<figure><img src=".gitbook/assets/1-1-1.png" alt="" width="375"><figcaption></figcaption></figure>

### （2）Vue3

<figure><img src=".gitbook/assets/1-1-2.png" alt="" width="375"><figcaption></figcaption></figure>

### （3）详细介绍

<table data-full-width="true"><thead><tr><th width="110">生命周期</th><th>Vue2</th><th>Vue3 选项式API</th><th>Vue3 组合式API</th><th width="340">详细信息</th><th>服务端渲染时是否调用</th></tr></thead><tbody><tr><td>创建</td><td>beforeCreate</td><td>beforeCreate</td><td>setup</td><td><ul><li>setup()钩子是在组件中使用组合式API的入口，会在所有选项式API钩子之前调用；</li><li>beforeCreate: 组件实例初始化完成且props被解析后立即调用。</li></ul></td><td>是</td></tr><tr><td>创建</td><td>created</td><td>created</td><td>setup</td><td>created: 在组件实例处理完所有与状态有关的选项之后调用，响应式数据、计算属性、方法和侦听器已设置完成。</td><td>是</td></tr><tr><td>挂载</td><td>beforeMount</td><td>beforeMount</td><td>onBeforeMount</td><td>组件已完成响应式状态设置，但还没创建DOM节点，它即将首次执行DOM渲染过程。</td><td>否</td></tr><tr><td>挂载</td><td>mounted</td><td>mounted</td><td>onMounted</td><td>在组件被挂载（除异步组件或suspense树内组件外所有同步子组件均已挂载，自身DOM树已创建并插入父容器）之后调用。</td><td>否</td></tr><tr><td>更新</td><td>beforeUpdate</td><td>beforeUpdate</td><td>onBeforeUpdate</td><td>在组件即将因为一个响应状态变更而更新其DOM树之前调用，能访问到DOM变更之前的状态。</td><td>否</td></tr><tr><td>更新</td><td>updated</td><td>updated</td><td>onUpdated</td><td>在组件因为一个响应式状态变更而更新其DOM树之后调用。<br>注：在updated钩子中更改组件状态，会导致无限更新循环！</td><td>否</td></tr><tr><td>销毁</td><td>beforeDestroy</td><td>beforeUnmount</td><td>onBeforeUnmount</td><td>在一个组件实例被卸载之前调用，组件实例还保有全部功能。</td><td>否</td></tr><tr><td>销毁</td><td>destroyed</td><td>unmounted</td><td>onUnmounted</td><td>在一个组件实例被卸载（所有子组件均已卸载，所有响应式作用都已停止）之后调用，可以在这个钩子中手动清理一些副作用（如计时器、DOM事件监听器、与服务器的连接等）。</td><td>否</td></tr><tr><td>keep-alive</td><td>activated</td><td>activated</td><td>onActivated</td><td>被keep-alive缓存的组件被插入到DOM中时调用。</td><td>否</td></tr><tr><td>keep-alive</td><td>deactivated</td><td>deactivated</td><td>onDeactivated</td><td>被keep-alive缓存的组件从DOM中被移除时调用。</td><td>否</td></tr><tr><td>捕获错误</td><td>errorCaptured(Vue2.5.0+新增)</td><td>errorCaptured</td><td>onErrorCaptured</td><td>在捕获了后代组件传递的错误时调用。<br>默认错误向上传递、直到顶层应用级 app.config.errorHandler；errorCaptured钩子本身抛出的错误将和原来捕获到的错误一起发送到 app.config.errorHandler；可通过在钩子里返回false来阻止错误继续向上传递。</td><td>是</td></tr><tr><td>调式Dev only</td><td></td><td>renderTracked</td><td>onRenderTracked</td><td>注册一个调试钩子，当组件渲染过程中追踪到响应式依赖时调用。</td><td>否</td></tr><tr><td>调式Dev only</td><td></td><td>renderTriggered</td><td>onRenderTriggered</td><td>注册一个调试钩子，当响应式依赖的变更触发了组件渲染时调用。</td><td>否</td></tr><tr><td>SSR only</td><td></td><td>serverPrefetch</td><td>onServerPrefetch</td><td>注册一个异步函数，在组件实例在服务器上被渲染之前调用。</td><td>仅在服务端渲染中执行</td></tr></tbody></table>
