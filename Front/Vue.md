# 虚拟Dom diff 算法

## Key 

key 是虚拟 dom(以下简称vDom) 中对象的标识, 当状态中的数据发生改变时, Vue 会根据新的数据生成nVDom, 随后和oVDom 进行对比:

- oVDom 中找到和 nVDom 相同的key
  - 若 vDom 中的内容没有改变, 使用 oVDom 的内容
  - 若 vDom 中的内容改变, 使用 nVDom 的内容生成新的真实Dom
- oVDom 中没有 nVDom 的key
  - 创建新的真是 Dom 随后渲染到页面

如果使用 index 作为 key

- 对数据进行, 添加、逆序、删除等**破坏顺序**的操作时, 则原来数据对应的 key 就会修改, 即会产生没必要的新的真实dom, 效率较低
- 如果结构中包含输入类的 dom, 界面会出现问题(对比时不会输入的内容)

key 建议使用唯一标识