# pi_front


## 组件系统介绍

1. 源代码为TS，会被编译为ES5 + AMD
2. html的解析参考的[htmlparser2](https://github.com/fb55/htmlparser2)


## BUGLIST

bug:

1. 绘制子节点特别是子widget的时候一定要提供位置，不然理论上一定会存在bug的
	- 等发现了对应的BUG再改
	- 到现在为止还没在实际开发中遇到这个问题，太神奇了

2. 新旧vdom的比较过程中子节点的增删和移动过程中位置的指定很有可能有BUG
	- 编辑距离算法中对于修改的定义和vdom对于修改的定义是完全不同的
	- 等发现了对应的bug再修改

3. 如果节点是后来动态加入的，且和源节点有相同的事件监听，则总是源节点的事件监听被调用，即使明明是点击的新节点
	- 示例中如果show默认是false后来改为true,则总是func1被调用,永远不会调用func2

	```
	<div on-click=func1></div>
	<? it.show>
		<div on-click=func2></div>
	<?>
	```
	
	- 本质上是vdom的比较有问题
4. 没有_nid本质上根本区分不出来两个同类型元素是不是同一个元素
5. 我的比较算法不够精确
6. CSS不加分号会挂掉
7. 修改时要注意对样式和事件的特殊处理
8. 比较的时候没有考虑到父组件传递给子组件的数据修改造成的dom改动

9. 无法向下层传递数组
	- 原因：tpl_str中 "" + [123,456]的结果为"123,456"而不是"[123,456]";
10. 应该提供用户自定义widgetid的功能，这样可以方便的获取到
11. 




优化：

1. 应该提供缓存机制
	- 在SPA中经常会出现两个页面片段不停来回切换的情况，如果每次都要重头构建，显得太傻了
	- 如果能够把部分页面片段缓存起来，下次要用的时候直接替换上去就好了

2. 组件要提供注册机制
	- 在组件嵌套的时候，A组件依赖B组件，则在A中需要先注册B
3. 传data的时候必须给组件的每一个属性都赋值，不然会挂掉
4. 应该提供更完善的模板机制
5. vdom比较机制要修改
6. 报错机制太差了，模板函数经常出错，但是又不知道错在哪里
7. if/else语法有歧义
8. vdom里面的parentNode不是在模板解析的时候生成的，而是在绘制的时候生成的
	- 理论上在解析的时候就能生成
9. 没有生成_nid
10. 还是需要提供对外部样式的支持，内联样式并不能解决所有问题
	- 伪类伪元素动画等都需要外部样式
	- 对于一些通用的样式，定义在在全局更方便
11. 销毁机制肯定有问题，自己都不敢用
12. 应该时候一种capture的事件传递机制，有时还是会用到的
13. 感觉CSS和TPL的复用率是够的，如何进一步提高JS的复用率
14. 数据无法向上传递
	- 数据可以通过widget向上传递
15. 更友好的错误提示，绘制失败应该告诉具体原因
16. 异步代码非常难于调试，可以在调试的时候提供同步版本
17. 一个更好的方式是将组建直接替换为一个div
18. 添加了移动之后把代码搞得好复杂，先不要移动了
19. 没有考虑可能同时从两个父节点开始绘制的情况
20. 两个同样的组件向上传递数据，会分不清是谁传的
21. 父组件有时想给子组件传值有时又希望不传值
22. 让props能自动提示
23. 向下的数据传递和数据显示没有分开
24. 两个相同组件之间难于区分




无关需求：

1. 希望能提供一个切图工具，把大图按照规则切成小图
