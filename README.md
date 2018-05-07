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
- [`v-model` 表单元素双向绑定语法](./src/demo2-5.html) - done

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
