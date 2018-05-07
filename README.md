# learn-vue
汇总 vue 的常用语法知识，基于 vue 2.0。

# 必备知识点
- HTML
- CSS
- Javascript

# 模版语法

## 插值
- [插入文本](./src/demo1/demo1-1.html) - done
- [`v-once` 插入文本](./src/demo1/demo1-1.html) - done
- [插入原始 HTML](./src/demo1/demo1-2.html) - done

## 指令
- [`v-bind` 属性绑定](./src/demo2/demo2-1.html) - done
- [`v-if` 条件语法](./src/demo2/demo2-2.html) - done
- [`v-for` 循环语法](./src/demo2/demo2-3.html) - done
- [`v-on` 时间监听语法](./src/demo2/demo2-4.html) - done
对于事件等包含很多相关等修饰方法，如 `preventDefault`, `stopPropagation`等，在 VUE 中同样也可以通过链式规范来书写，如：`<form v-on:submit.prevent="onSubmit"></form>`，这些修饰符包括：
    - 事件修饰符
        - `.stop`
        - `.prevent`
        - `.capture`
        - `.self`
        - `.once`: 点击一次，在 vue2.1.4 中新增
        - `.passive`: 解决事件延迟问题，如滚动
    - 按钮修饰符：
        - `.enter`
        - `.tab`
        - `.delete` (捕获“删除”和“退格”键)
        - `.esc`
        - `.space`
        - `.up`
        - `.down`
        - `.left`
        - `.right`
        - [自定义按键修饰符别名](https://cn.vuejs.org/v2/api/#keyCodes)
    - 系统修饰键（2.1.0 新增）
        - `.ctrl`
        - `.alt`
        - `.shift`
        - `.meta`
    - 鼠标按钮修饰符（2.2.0 新增）
        - `.left`
        - `.right`
        - `.middle`
    - v-model 表单修饰符
        - `.lazy`: 在 `change` 而非 `input` 时更新
        - `.number`: 输入为数字类型
        - `.trim`: 去除首位空白字符
- [`v-model` 表单元素双向绑定语法](./src/demo2-5.html) - done
- [`v-show` 展示元素语法](./src/demo2-6.html) - done

## v-if VS v-show
- `v-if` 渲染是惰性的，只有当条件为真时才会渲染；而 `v-show` 是每一次都会处理渲染逻辑。
- `v-if` 在条件为假时会删除 dom 元素；而`v-show` 只是改变 `display`，而不是删除元素。

## 块级别渲染

块级别的渲染可以通过 `<template>` 进行包裹，最终展示内容为去除 `<template>` 之后的内容，如
```
<template v-if="ok">
    <span>apple</span>
    <span>orange</span>
</template>
<template v-else>
    <span>banana</span>
</template>
```

# 计算属性

## 简介

VUE 支持表达式计算，可以通过差值运算或计算属性方式存在。二者的区别在于：差值运算适合简单的表达式计算，复杂的则需要通过计算属性来进行解耦。

- [差值运算](./src/demo3/demo3-1.html) - done
- [计算属性](./src/demo3/demo3-2.html) - done

## 计算属性的 getter 和 setter

计算属性也拥有 [getter/setter](./src/demo3/demo3-3.html) - done，用于根据开发者的需求定制返回结果。

## 计算属性 VS 方法

相同点不用赘述，能够得到相同的结果。不同点 DEMO 见 [计算属性 VS 方法](./src/demo3/demo3-4.html) - done，具体细节如下：

- 写法不同
    - 方法
    ```
    <span>通过方法反转后的字符串：{{ reversedMessage() }}</span>
    ```
    - 计算属性
    ```
    <span>通过计算属性反转后的字符串：{{ reversedMessageByComputed }}</span>
    ```

- 计算属性会进行结果的缓存，在计算属性中的值未发生改变时，计算属性是直接返回结果的。方法则不会进行结果的缓存。DEMO 中对方法和计算属性的 function 中分别加入了 debugger 断点，可以在控制台中输入两次以下指令：

```
mv.reversedMessage();

mv.reversedMessageByComputed;
```

很容易看出计算属性并不会进入 debugger 阶段，所以结果是走的缓存。

## 计算属性 VS 侦听属性

侦听属性是当一个值发生改变时，会触发对应的 observer，从而触发对应的函数。

大多数情况下我们都可以通过计算属性来完成对应的功能，但由于计算属性是有缓存机制的，所以如果当计算属性中相关的值没有发生变化的话，计算属性是不会重新执行的。但如果当我们遇到需要请求一个异步接口等场景时，就无法通过计算属性来满足需求。如 [侦听属性](./src/demo3/demo3-5.html) - done 完成的异步请求功（此处引用与 VUE 官网源码）。

# class 与 style 绑定

# 注意事项

## 数组监听
 
VUE 只在指定数组方法执行时进行数据更新，包括 `push()`, `pop()`, `shift()`, `unshift()`, `splice()`, `sort()`, `reverse()`。当通过以下方式进行操作时，数组不会更新：

- 通过索引设置一个值，如 `vm.items[1] = 'Jack'`。
- 通过修改数组长度，如 `vm.items.length = 5`。

以上问题可以通过 Vue.set() 或以上的数组方法来解决，如：

```
Vue.set(vm.items, indexOfItem, newValue);

vm.items.splice(indexOfItem, 1, newValue);
```

## 对象监听

Vue 不能监听对象属性的添加或删除。可以通过 `Vue.set(object, key, value)` 解决。

## DOM 中的事件绑定如果传递 event

可以通过 `$event` 进行参数传递，如:

```
<button v-on:click="warn('Hello VUE', $event)">
  Submit
</button>
```

## 时间修饰符顺序
使用修饰符时，顺序决定了最后的效果；用 v-on:click.prevent.self 会阻止所有的点击，而 v-on:click.self.prevent 只会阻止对元素自身的点击。

## v-model 更新时机
当输入完成后更新，在输入法组合文字的过程中不进行更新。