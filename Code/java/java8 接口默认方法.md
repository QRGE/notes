# interface 的 default 方法

​       Java 8 中在 interface 中新增了default 级别的方法, 例如 Collection 中的 stream(), java 8 中 Collection 的流是个很好的东西, 我觉得基本上可以简化所有涉及列表循环的操作等

## default 方法的意义

《Java 函数式编程》中提到, 增加默认方法主要是为了在接口上**向后兼容**

- 例如你在 java 8 之前的环境中自定义了一个 Collection 的类, 如果 stream() 不是默认方法, 这个类就无法在 java 8 环境下编译

类中重写方法的优先级高于默认方法, 可以减少很多继承带来的麻烦