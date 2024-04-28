# 六、性能、网络、安全

## 1、性能优化

### （1）资源加载层面

1）减少HTTP请求数量

* 不常变动资源缓存（使用如Nginx、Varnish等缓存代理，减少请求打到服务器）
* 合理拆分打包代码（Code Splitting）
* 按需加载
* 图片懒加载
* 合理使用图片sprite（雪碧图）
* 使用HTTP 2多路复用

2）压缩资源体积

* 移除注释、调试信息、不必要的空格缩进等代码（可以启用Tree Shaking）
* 缩小变量名长度
* 图片压缩
* 图片转换格式
* 启用GZIP压缩

3）使用CDN加载静态资源

4）缓存静态资源

5）异步加载非关键资源（如广告脚本、分析工具），这样它们不会阻塞页面的初始渲染

6）后端提高资源响应速度

* 使用SSD固态硬盘代替传统硬盘，能显著提升服务器响应速度
* 数据库优化：合理设计数据库表结构，添加索引，优化查询语句，提升数据读取速度
* 后端代码减少不必要的计算和I/O操作

### （2）渲染层面

1）合理分页分块：一次只渲染一部分数据

2）懒加载（包括图片懒加载）：一开始只给个占位、只有目标在当前视窗可见时才去真正加载

3）Virtual DOM：对频繁更新的数据，使用虚拟DOM，减少因操作DOM导致的性能问题

4）对滚动、输入等高频事件进行防抖处理

5）vue-router路由缓存：使用keep-alive缓存组件，避免重复渲染，提升切换效率

6）动画优化

* 使用CSS动画而非JavaScript：CSS动画通常比JavaScript动画更高效，因为CSS动画可以由浏览器的合成线程（Compositor Thread）处理，而不需要通过JavaScript线程，这意味着即使JavaScript线程忙于其他计算，CSS动画仍然可以流畅运行；\
  注：CSS Transitions 和 CSS Animations是制作常见动画（如过渡和关键帧动画）的理想选择。
* 使用硬件加速：transform（如translate、scale、rotate）、opacity等某些CSS属性可以触发硬件加速，这意味着GPU（而非CPU）可以被用来处理动画，从而提升性能。例如，使用 transform: translateX(100px) 移动元素，而不是使用 margin-left 或 left。
* 避免重绘和重排：某些属性的变化会导致页面的重绘或重排\
  \- 重绘（repaint）：元素的外观被改变，但不影响布局，如color；\
  \- 重排（reflow）：元素的布局或几何结构被改变，如width、height、margin。\
  重排是一种高成本操作，因为它可能影响到DOM树中的多个元素。\
  所以：尽量使用transform和opacity属性来制作动画，避免布局属性的动画。
* 使用requestAnimationFrame：如果必须使用JavaScript来创建动画，应该使用requestAnimationFrame()而不是setTimeout或setInterval，因为requestAnimationFrame()会在每次浏览器绘制之前执行，这样可以保证动画的平滑性，并且可以在浏览器忙于其他事务时自动暂停，节省计算资源。
* 优化JavaScript动画的性能：当使用JavaScript动画库时，确保最小化动画中每帧的计算量、使用库的优化特性（如GSAP的will-change属性自动优化）。
* 控制动画的复杂度：复杂的动画可能需要消耗大量的CPU或GPU资源，优化动画的关键帧，避免使用高复杂度的动画路径，有时候简化动画效果或减少动画中的元素数量可以显著提升性能。
* 测试和监控：使用浏览器的开发者工具（如Chrome DevTools）中的Performance面板来监控动画的性能，检查动画是否触发了GPU加速，并确保动画流畅运行无卡顿。

7）SSR：提高首屏渲染速度

8）使用Web Worker进行需要前端长时间计算的任务，避免主线程（渲染等）被占用

### （3）工程化

1）监控性能

* 合成监控：Google Lighthouse
* 真实用户监控：记录真实的用户访问页面的数据，可以通过打点来收集数据，把采集到的数据上报到服务器，经过数据清洗、加工等工作后，在监控平台上呈现监控数据

1. 通过requestAnimationFrame进行FPS（每秒帧数）监控，当FPS出现大幅波动时可以探查原因\
   &#x20;!\[]\(assets/1.png)
2. 通过Performance API监控主线程空闲时间，可以观察主线程是否过于繁忙导致响应卡顿\
   ! \[]\(assets/2.png)
3. 通过监听长任务事件（PerformanceObserver）分析长任务，长任务会占用主线程时间片、导致干扰响应，常见长任务包括复杂计算、大数据量渲染等\
   ! \[]\(assets/3.png)
4. 模拟慢网络环境。使用模拟工具（如Chrome开发者工具 Network可以选择网络环境）模拟2G、3G等慢网络环境，观察应用性能是否存在问题\
   ! \[]\(assets/4.png)

2）负载测试，观察应用的承受能力上限
