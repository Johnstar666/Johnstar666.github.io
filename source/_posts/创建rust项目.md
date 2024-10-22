---
titsle: 创建rust项目
tags:
 - rust
categories: rust
---

# Hello,World!

## 创建文件夹

在cmd终端操作(首先打开即将创建的文件夹想要的路径)

```cpp
mkdir projects
cd projects
mkdir  hello_world
cd hello_world
```

更改文件名

```
mv hello_world.rs main.rs
```

编译并运行这个文件

```
rustc main.rs
.\main.exe
```

hello world

```
fn main()
{
    println!("Hello World");//println!是一个rust macro（宏）
}
```

运行rust程序之前必须先编译

.pdb文件包含调试信息

# Hello,Cargo

使用cargo创建项目

```
cargo new hello_cargo
```

![](https://johnstar666.oss-cn-guangzhou.aliyuncs.com/20241022210358.png)

![](https://johnstar666.oss-cn-guangzhou.aliyuncs.com/20241022210432.png)

![](https://johnstar666.oss-cn-guangzhou.aliyuncs.com/20241022210632.png)

![](https://johnstar666.oss-cn-guangzhou.aliyuncs.com/20241022210743.png)

为发布构建

```
cargo build --release
```

# 建议

尽量使用cargo