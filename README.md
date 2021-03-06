## Yet Another Golang binary parser for IDAPro

**中文 | [English](./README_en.md)**

----------------------------------------------------------------------

受 [golang_loader_assist](https://github.com/strazzere/golang_loader_assist) 和 [jeb-golang-analyzer](https://github.com/pnfsoftware/jeb-golang-analyzer) 启发，为 IDAPro 写了一个更完备的 Go 二进制文件解析工具。

### 核心功能：

1. 自动定位 **firstmoduledata** 的位置并解析；
2. 根据 firstmoduledata 中的信息定位到 **pclntab**(PC Line Table)，并从 pclntab 入手解析、恢复**函数符号**，抽取**源码文件列表**；
3. 解析 **strings** 和 string **pointers**；
4. 根据 firstmoduledata 中的信息，解析所有 **types** 并为 types 各种属性打上有意义的 comment 或 dref；
5. 解析 **itab**(Interface Table)。

Go 语言二进制文件中对逆向分析有帮助的信息如下：

![](./imgs/go_binary_info.png)

另外，**go_parser** 还有两个很有用的特性：

1. 以上功能对于 **buildmode=pie** 类型的 Go binary 文件依然有效；
2. 对于文件头信息尤其是 Section Header 信息损坏的二进制文件依然有效。

DDGMiner v5029 (MD5: 95199e8f1ab987cd8179a60834644663) 样本中核心的配置文件 struct 解析结果示例如下：

![](./imgs/map_type_parse_eg.png)

样本源码文件列表：

![](./imgs/srcfiles.png)

### 文件列表：

- **go_parser.py** ：整套工具的入口文件，在 IDAPro 中 **[Alt+F7]** 组合键，执行此脚本；
- **common.py**: 通用变量和函数定义；
- **pclntbl.py**: 解析 **pclntab**(PC Line Table);
- **strings.py**: 解析 strings 和 string pointers；
- **moduldata.py**: 解析 **firstmoduledata**；
- **types_builder.py**: 解析所有 **types** ；
- **itab.py**: 解析 **itab**(Interface Table)；.

### Note

1. 此工具只在 IDA7.2/IDA7.0 上测试过，其他的 IDA 版本未经测试；
2. strings 解析模块从 [golang_loader_assist](https://github.com/strazzere/golang_loader_assist) 移植过来，我自己又增加了 string pointers 解析的功能，目前只支持 x86 架构。

### Refer

1. [Analyzing Golang Executables](https://www.pnfsoftware.com/blog/analyzing-golang-executables/)
2. [Reversing GO binaries like a pro](https://rednaga.io/2016/09/21/reversing_go_binaries_like_a_pro/)
3. [Reconstructing Program Semantics from Go binaries.pdf](http://home.in.tum.de/~engelke/pubs/1709-ma.pdf)