# 拖拽
    设置在操作元素上的：
        dragstart
        dragend
    设置在放置元素上的：
        dragenter
        dragleave
        dragover //需要preventDefault才能触发dragdrop
        dragdrop
    外部文件拖到浏览器会打开该资源，需要在document ondragover上preventDefault

# mysql
    主键：对于关系表，有个很重要的约束，就是任意两条记录不能重复，指能够通过某个字段唯一区分出不同的记录，这个字段被称为主键。选取主键的一个基本原则是：不使用任何业务相关的字段作为主键。我们一般把这个字段命名为id。常见的可作为id字段的类型有：自增整数类型、全局唯一GUID类型。主键也不应该允许NULL。
