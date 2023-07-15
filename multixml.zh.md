目前的Groot 1不支持加载、保存多个xml文件。
为了解决这个问题，对Groot进行一定的修改。让其支持多xml文件的加载和保存。

特性如下：
1. 加载文件时支持include关键字
2. 保存文件时按照以下的方式保存：
2.1 首先，选择一个索引文件保存，名称可以为index.xml或者main.xml

main.xml文件内容如下：
```
<?xml version="1.0"?>
<root main_tree_to_execute="BehaviorTree">
    <include path="model.xml"/>
    <include path="BehaviorTree.xml"/>
</root>
```

文件model.xml为固定文件名称，保存所有model。
```
<?xml version="1.0"?>
<root>
    <TreeNodesModel>
        <Action ID="A">
            <inout_port name="AA"/>
            <inout_port name="BB"/>
            <inout_port name="CC"/>
        </Action>
    </TreeNodesModel>
    <!-- ////////// -->
</root>
```

文件BehaviorTree.xml为子树，文件名称与子树的名称一致。
```
<?xml version="1.0"?>
<root main_tree_to_execute="BehaviorTree">
    <!-- ////////// -->
    <BehaviorTree ID="BehaviorTree">
        <Action AA="111" BB="222" CC="333" ID="A"/>
    </BehaviorTree>
    <!-- ////////// -->
</root>
```

以上main.xml、model.xml、BehaviorTree.xml这三个文件中，只有main.xml需要用户填写，其他的文件都是自动保存。并且目前固化在代码中。

目前使用该版本需要注意的问题包括：
1. 保存文件的时候不一定会按照原来的方式保存，而是固定的方式保存为多个文件。
2. 保存的时候一定是保存为多个文件，即使只读到一个文件。所以要以目录为单位保存。
3. 如果model格式变了，需要手动清除模型，如果不手动清除，并不会自动覆盖面板上的model。后续可能会优化该问题。
