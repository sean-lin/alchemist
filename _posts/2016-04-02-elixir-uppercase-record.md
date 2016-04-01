---
title: Elixir 如何使用名字是大写字母开头的 Record
---

# 缘由
今天需要写一段处理公钥签名的代码，需要用到 Erlang 的 public_key 库，这个库定义了很多 Record，于是我随手写下

``` elixir
require Record
Record.extract_all(from_lib: "public_key/include/public_key.hrl") |> Enum.each(fn {k, v} ->
    Record.defrecord k, v
end)
```

结果后面想用`OTPCertificate`这个 Record 时报语法错误了。elixir 编译器看起来把`OTPCertificate`当作模块名处理了。

# Workaround
那么，正确的姿势是什么呢？当当当

``` elixir
    unquote(:OTPCertificate)(cert_record, :tbsCertificate) 
```

大写字母的 Record 需要用 `unquote` 包起来才能正确执行

# 讨论
这个问题，其实是可以修复的，但是 Elixir 的作者并不想用更好的办法来做，原因:

* Record 在 Elixir 里主要是用来和 erlang 通信的
* Erlang 的 Record tag，大部分情况下都是小写开头的，大写开头的，大部分都是机器生成的。

所以， 这个问题就随风去吧。

[google 讨论组上的讨论](https://groups.google.com/d/msg/elixir-lang-talk/2UNUVbM83kw/y_AJmNwd0WcJ)

