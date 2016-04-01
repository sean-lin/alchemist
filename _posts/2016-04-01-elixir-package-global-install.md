---
title: Elixir 的包如何全局安装
---

<p class="lead"> Elixir 的包管理机制并没有全局安装(类似 NPM 的 install --global )的概念。但是在写一些实验性代码段或者 EXS 时，
为了写几行代码，就要 mix new 一个新 project， 相当麻烦，最好可以把一些常用的库当作系统默认库使用。今天折腾了一下，感觉这样做是最简单的。</p>

# 建立空项目
现在任意地方建立一个任意名字的空 mix 项目，我是建立在 `~/.mix/` 里，项目名 global。

# 加入希望全局安装包
编辑 `mix.exs`, 把希望全局安装的包设置成依赖项，然后运行

``` shell
mix deps.get
mix deps.compile
```
下载并编译包

# 设置加载路径
Elixir 是由 Erlang 的 VM 执行的，只要修改 erl 加载 beam 文件的路径，就可以让 VM 启动时，加载响应的包。 最简单修改 erl 加载路径的办法是设置环境变量 `ERL_LIBS`。我是使用 bash 的，修改了下 `.bashrc`(osx 是.bash_profile),加入

```
export ERL_LIBS=$HOME/.mix/global/_build/dev/lib/
```

重开一个shell，iex 就可以加载到这些包了。

# 添加和升级
后面如果需要增加别的库，可以直接在 mix.exs 中增加， 然后执行 `deps.get` 和 `deps.compile`。
如果依赖的库升级了，也可以执行 `deps.update` 和 `deps.compile` 方便的升级

