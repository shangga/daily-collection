# VUE生命周期
![img](https://sfault-image.b0.upaiyun.com/134/301/1343014756-5aea7ac2cec80)
## 父子组件生命周期
### 父子组件初始化
- parent－－－beforeCreate
- parent－－－created
- parent－－－brforeMount
    - child －－－beforeCreate（data未初始化）
    - child －－－created
    - child －－－beforeMount
    - child －－－mounted
- parent －－－ mounted
- parent －－－ beforeUpdate
- parent －－－ updated
### 父子组件prop变化
- parent－－－beforeUpdate
    - child－－－beforeUpte
    - child－－－updated
- parent－－－updated
### 父子组件销毁时
- parent－－－beforeDestory
    - child－－－beforeDestory
    - child－－－destoryed
- parent－－－destoryed
### tip
- 仅当子组件完成挂载后，父组件才会挂载
- 当子组件完成挂载之后，父组件会主动执行一次beforeUpdate／updated钩子函数（仅首次）
- 父子组件载在data变化中时分别监控的，但是在更新props中的数据是关联的
- 销毁父组件时，先销毁子组件后才会销毁父组件
## 兄弟组件的生命周期
### 组建初始化
- parent－－－beforeCreate
- parent－－－created
- parent－－－beforeMounted
    - child1－－－beforeCreate（data未初始化）
    - child1－－－created
    - child1-－－beforeMounted
    - child2-－－beforeCreated（data未初始化）
    - child2-－－created
    - child2-－－beforeMounted
    - child1-－－mounted
    - child2-－－mounted
- parent－－－mounted
- parent－－－beforeMounted
- parent－－－updated
### tip
- 组件的初始化分开进行，挂载是从上到下依次进行的
- 当没有数据关联时，兄弟组件之间的更新和销毁是互不关联的

## nextTick
- 对未来更新的后的视图进行操作，将需要执行的函数传递给this.$nextTick方法
- 涉及到js中EventLoop的运行机制
- 

## keep-alive
- 实现：created和destoryed钩子函数，会创建一个cache对象，用来作为缓存容器，保存vnode节点（可以理解为javascript对象保存起来的节点，包含tag，name，class，parent，children等节点信息）
- 然后是render函数的操作，首先通过getFirstComponentChild获取一个子组件，获取该组件的name（存在组件名则直接使用组件名，否则会使用tag）。接下来会将这个name通过include与exclude属性进行匹配，匹配不成功（说明不需要进行缓存）则不进行任何操作直接返回vnode
- watch会调用purneCache监听include和exclude属性改变，在改变的时候修改cache缓存中的缓存数据。purneCache会遍历cache中所有项，如果不符合filte指定的规则的话，则会执行purneCacheEntry，purneCacheEntry会调用组件实例的$destory方法将组件销毁
- Vue.js内部将DOM节点抽象成了一个个的VNode节点，keep-alive组件的缓存也是基于VNode节点的而不是直接存储DOM结构。它将满足条件（pruneCache与pruneCacheEntry）的组件在cache对象中缓存起来，在需要重新渲染的时候再将vnode节点从cache对象中取出并渲染。