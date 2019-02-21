# Thrift 简介

`Apache Thrift` 框架是一种可扩展支持跨语言的 `RPC` 框架，`Thrift` 集成了代码生成，提供
了数据传输、数据序列化/反序列化以及应用层的处理等功能。它开始是由 `Facebook` 研发，现已贡献
给 `Apache` 组织作为一个顶级项目开源。

`Thrift` 提供了一系列的代码生成工具，开发者仅仅定义好的 `.thrift` 文件，通过 `Thrift` 
的代码生成工具即可生成 `RPC` 的客户端和服务端代码，非常方便。`Thrift` 通过 `.thrift` 
文件完美的解决了跨语言通讯问题。可以说这么说，使用 `Thrift` 作为 `RPC` 通讯框架，编写良好
的 `.thrift` 文件是至关重要的，更何况跨语言通讯也是通过它实现的。

`Thrift` 目前支持的语言有： `C++`, `Java`, `Python`, `PHP`, `Ruby`, `Erlang`, 
`Perl`, `Haskell`, `C#`, `Cocoa`, `JavaScript`, `Node.js`, `Smalltalk`, 
`OCaml`, `Delphi` 等。


