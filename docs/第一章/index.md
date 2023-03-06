## 前言

本文章是面对完全不懂的小白所写, 如果有更多的技巧或者想要深入的理解, 欢迎提出Pull Request或Issue. 

### 插件组成

首先, 先让我们看看下面这段代码

```go
package plugin

import (
    zero "github.com/wdvxdr1123/ZeroBot"
)

func init() {
    zero.OnFullMatch("Hello").SetBlock(true).SetPriority(10).Handle(func(ctx *zero.Ctx){
        ctx.Send("Hello, I am ZeroBot")
    })
}
```

可以看到, 插件的代码由三个部分组成, 分别是`package ...` `import (...)` `func init() {...}`
接下来, 我将分别解释这三个是什么, 如果无法理解, 可以跳过, 直接照抄就行

`package`也就是包是`Golang`语言中的概念, 是源码的合集, 而一个或多个插件就可以作为一个包, 在`main.go`中导入

`import (...)`则是导入, 导入的是一个包, 包中定义了许多的函数和变量以及常量, 有公开的和非公开的, 其中公开的则可以在要导入的地方使用

`func init() {...}`则是初始化, 在`main.go`中以`_ ...`的形式导入, 就会执行init函数

更多解释可以看官方文档或者自行搜索

### 匹配器

接下来, 我将列出ZeroBot存在的匹配器, ZeroBot的匹配器基本涵盖了所有的需求, 它们定义在`https://github.com/wdvxdr1123/ZeroBot/engine.go`

```go
// On 添加新的指定消息类型的匹配器
On(typ string, rules ...Rule) 
// OnMessage 消息触发器
OnMessage(rules ...Rule) 
// OnRequest 请求消息触发器
OnRequest(rules ...Rule) 
// OnNotice 系统提示触发器
OnNotice(rules ...Rule) 
// OnMetaEvent 元事件触发器
OnMetaEvent(rules ...Rule) 
// OnFullMatch 完全匹配触发器 
OnFullMatch(src string, rules ...Rule) 
// OnFullMatchGroup 完全匹配触发器组
OnFullMatchGroup(src []string, rules ...Rule) 
// OnKeyword 关键词触发器
OnKeyword(keyword string, rules ...Rule) 
// OnKeywordGroup 关键词触发器组
OnKeywordGroup(keywords []string, rules ...Rule) 
// OnCommand 命令触发器
OnCommand(commands string, rules ...Rule) 
// OnCommandGroup 命令触发器组
OnCommandGroup(commands []string, rules ...Rule)
// OnPrefix 前缀触发器
OnPrefix(prefix string, rules ...Rule) 
// OnPrefixGroup 前缀触发器组
OnPrefixGroup(prefix []string, rules ...Rule) 
// OnSuffix 后缀触发器
OnSuffix(suffix string, rules ...Rule) 
// OnSuffixGroup 后缀触发器组
OnSuffixGroup(suffix []string, rules ...Rule) 
// OnRegex 正则触发器
OnRegex(regexPattern string, rules ...Rule) 
// OnShell shell命令触发器
OnShell(command string, model interface{}, rules ...Rule) 
```

然后, 我将分别介绍, 并给出示例


<details>
<summary>On</summary>

> ```go
> On(typ string, rules ...Rule) 
> ```

咕咕咕

```go
    On(typ string, rules ...Rule) 
```

[可能有的图片]

</details>

<details>
<summary>OnMessage</summary>

> ```go
> OnMessage(rules ...Rule) 
> ```

OnMessage将被所有的消息触发, 但不会被请求消息和系统提示以及元事件触发

```go
package plugin

import (
    zero "github.com/wdvxdr1123/ZeroBot"
)

func init() {
    zero.OnMessage().SetBlock(true).SetPriority(10).Handle(func(ctx *zero.Ctx){
        ctx.Send("接收到了一条消息")
    })
}
```

>**情景模拟** 
>Human: 消息 
>Bot: 接收到了一条消息 
>Human: 另一条消息 
>Bot: 接收到了一条消息 

[可能有的图片]

</details>

<details>
<summary>OnRequest</summary>

> ```go
> OnRequest(rules ...Rule) 
> ```

咕咕咕

```go
    OnRequest(rules ...Rule) 
```

[可能有的图片]

</details>

<details>
<summary>OnNotice</summary>

> ```go
> OnNotice(rules ...Rule) 
> ```

咕咕咕

```go
    OnNotice(rules ...Rule) 
```

[可能有的图片]

</details>

<details>
<summary>OnMetaEvent</summary>

> ```go
> OnMetaEvent(rules ...Rule) 
> ```

咕咕咕

```go
    OnMetaEvent(rules ...Rule) 
```

[可能有的图片]

</details>

<details>
<summary>OnFullMatch</summary>

> ```go
> OnFullMatch(src string, rules ...Rule) 
> ```

OnFullMatch只有收到与设置的触发词完全相同的消息时, 才会被触发

```go
package plugin

import (
    zero "github.com/wdvxdr1123/ZeroBot"
)

func init() {
    zero.OnFullMatch("测试").SetBlock(true).SetPriority(10).Handle(func(ctx *zero.Ctx){
        ctx.Send("收到")
    })
}
```

>**情景模拟** 
>Human: 测试 
>Bot: 收到 
>Human: 测试试 
>Human: 测试 
>Bot: 收到 

[可能有的图片]

</details>

<details>
<summary>OnFullMatchGroup</summary>

> ```go
> OnFullMatchGroup(src []string, rules ...Rule) 
> ```

OnFullMatchGroup与OnFullMatch相同, 但可以设置多个触发词

```go
package plugin

import (
    zero "github.com/wdvxdr1123/ZeroBot"
)

func init() {
    zero.OnFullMatchGroup([]string{"测试","test"}).SetBlock(true).SetPriority(10).Handle(func(ctx *zero.Ctx){
        ctx.Send("收到")
    })
}
```

>**情景模拟** 
>Human: 测试 
>Bot: 收到 
>Human: test 
>Bot: 收到 
>Human: 测试试 
>Human: testt 
>Human: 测试 
>Bot: 收到 

[可能有的图片]

</details>

<details>
<summary>OnKeyword</summary>

> ```go
> OnKeyword(keyword string, rules ...Rule) 
> ```

OnKeyword将在收到消息中含有触发词时被触发

```go
package plugin

import (
    zero "github.com/wdvxdr1123/ZeroBot"
)

func init() {
    zero.OnKeyword("测试").SetBlock(true).SetPriority(10).Handle(func(ctx *zero.Ctx){
        ctx.Send("收到")
    })
}
```

>**情景模拟** 
>Human: 测试 
>Bot: 收到 
>Human: 测试试 
>Bot: 收到 
>Human: 测测试 
>Bot: 收到 
>Human: 测1123试 
>Human: 11测试23 
>Bot: 收到 

[可能有的图片]

</details>

<details>
<summary>OnKeywordGroup</summary>

> ```go
> OnKeywordGroup(keywords []string, rules ...Rule) 
> ```

OnKeywordGroup与OnKeyword相同, 但可以设置多个触发词

```go
package plugin

import (
    zero "github.com/wdvxdr1123/ZeroBot"
)

func init() {
    zero.OnKeywordGroup([]string{"测试","test"}).SetBlock(true).SetPriority(10).Handle(func(ctx *zero.Ctx){
        ctx.Send("收到")
    })
}
```

>**情景模拟** 
>Human: 测试 
>Bot: 收到 
>Human: 测试试 
>Bot: 收到 
>Human: 测测试 
>Bot: 收到 
>Human: 测1123试 
>Human: 11测试23 
>Bot: 收到 
>Human: test 
>Bot: 收到 
>Human: testt 
>Bot: 收到 
>Human: ttest 
>Bot: 收到 
>Human: te1123st 
>Human: 11test23 
>Bot: 收到 

[可能有的图片]

</details>

<details>
<summary>OnCommand</summary>

> ```go
> OnCommand(commands string, rules ...Rule) 
> ```

OnCommand与其它匹配器不同的是, 它只有在消息以`Config`中设置的`CommandPrefix`为开头并且后续为触发词时才能触发
为了方便演示, 后续教程都将`/`设置为`CommandPrefix`

```go
package plugin

import (
    zero "github.com/wdvxdr1123/ZeroBot"
)

func init() {
    zero.OnCommand("测试").SetBlock(true).SetPriority(10).Handle(func(ctx *zero.Ctx){
        ctx.Send("收到")
    })
}
```

>**情景模拟** 
>Human: /测试 
>Bot: 收到 
>Human: /测试试 
>Bot: 收到 
>Human: /测测试 
>Human: /测试 
>Bot: 收到 
>Human: /测试 试 
>Bot: 收到 

[可能有的图片]

</details>

<details>
<summary>OnCommandGroup</summary>

> ```go
> OnCommandGroup(commands []string, rules ...Rule)
> ```

OnCommandGroup与OnCommand相同, 但可以设置多个触发词

```go
package plugin

import (
    zero "github.com/wdvxdr1123/ZeroBot"
)

func init() {
    zero.OnCommandGroup([]string{"测试","test"}).SetBlock(true).SetPriority(10).Handle(func(ctx *zero.Ctx){
        ctx.Send("收到")
    })
}
```

>**情景模拟** 
>Human: /测试 
>Bot: 收到 
>Human: /测试试 
>Bot: 收到 
>Human: /测测试 
>Human: /测试 
>Bot: 收到 
>Human: /测试 试 
>Bot: 收到 
>Human: /test 
>Bot: 收到 
>Human: /testt 
>Bot: 收到 
>Human: /ttest 
>Human: /test 
>Bot: 收到 
>Human: /test t 
>Bot: 收到 

[可能有的图片]

</details>

<details>
<summary>OnPrefix</summary>

> ```go
> OnPrefix(prefix string, rules ...Rule) 
> ```

OnPrefix只有在消息开头为设置的前缀词时才能被触发
并且, 前缀后的文本将被存入`ctx.State["args"]`, 可以用`ctx.State["args"].(string)`来调用

```go
package plugin

import (
    zero "github.com/wdvxdr1123/ZeroBot"
)

func init() {
    zero.OnPrefix("前缀").SetBlock(true).SetPriority(10).Handle(func(ctx *zero.Ctx){
        ctx.Send("收到")
    })
}
```

>**情景模拟** 
>Human: 前缀测试 
>Bot: 收到 
>Human: 前缀测试试 
>Bot: 收到 
>Human: 前缀 
>Human: 前缀 测试 
>Bot: 收到 

[可能有的图片]

</details>

<details>
<summary>OnPrefixGroup</summary>

> ```go
> OnPrefixGroup(prefix []string, rules ...Rule) 
> ```

OnPrefixGroup与OnPrefix相同, 但可以设置多个前缀词

```go
package plugin

import (
    zero "github.com/wdvxdr1123/ZeroBot"
)

func init() {
    zero.OnPrefixGroup([]string{"前缀","prefix"}).SetBlock(true).SetPriority(10).Handle(func(ctx *zero.Ctx){
        ctx.Send("收到")
    })
}
```

>**情景模拟** 
>Human: 前缀测试 
>Bot: 收到 
>Human: 前缀测试试 
>Bot: 收到 
>Human: 前缀测测试 
>Bot: 收到 
>Human: 测试前缀 
>Human: 前缀 测试 
>Bot: 收到 
>Human: prefixtest 
>Bot: 收到 
>Human: prefixtestt 
>Bot: 收到 
>Human: prefixttest 
>Bot: 收到 
>Human: testprefix 
>Human: prefix test 
>Bot: 收到 

[可能有的图片]

</details>

<details>
<summary>OnSuffix</summary>

> ```go
> OnSuffix(suffix string, rules ...Rule) 
> ```

OnPrefix只有在消息结尾为设置的后缀词时才能被触发
并且, 后缀前的文本将被存入`ctx.State["args"]`, 可以用`ctx.State["args"].(string)`来调用

```go
package plugin

import (
    zero "github.com/wdvxdr1123/ZeroBot"
)

func init() {
    zero.OnSuffix("后缀").SetBlock(true).SetPriority(10).Handle(func(ctx *zero.Ctx){
        ctx.Send("收到")
    })
}
```

>**情景模拟** 
>Human: 测试后缀 
>Bot: 收到 
>Human: 测试试后缀 
>Bot: 收到 
>Human: 后缀 
>Human: 测试 后缀 
>Bot: 收到 

[可能有的图片]

</details>

<details>
<summary>OnSuffixGroup</summary>

> ```go
> OnSuffixGroup(suffix []string, rules ...Rule) 
> ```

OnSuffixGroup与OnSuffix相同, 但可以设置多个后缀词

```go
package plugin

import (
    zero "github.com/wdvxdr1123/ZeroBot"
)

func init() {
    zero.OnSuffixGroup([]string{"后缀","suffix"}).SetBlock(true).SetPriority(10).Handle(func(ctx *zero.Ctx){
        ctx.Send("收到")
    })
}
```

>**情景模拟** 
>Human: 测试后缀 
>Bot: 收到 
>Human: 测试试后缀 
>Bot: 收到 
>Human: 测测试后缀 
>Bot: 收到 
>Human: 后缀测试 
>Human: 测试 后缀 
>Bot: 收到 
>Human: testsuffix 
>Bot: 收到 
>Human: testtsuffix 
>Bot: 收到 
>Human: ttestsuffix 
>Bot: 收到 
>Human: suffixtest 
>Human: test suffix 
>Bot: 收到 

[可能有的图片]

</details>

<details>
<summary>OnRegex</summary>

> ```go
> OnRegex(regexPattern string, rules ...Rule) 
> ```

OnRegex将只有在消息符合你所设置的正则时才会被触发
并且, 正则匹配的文本将会被存入`ctx.State["regex_matched"]`, 可以用`ctx.State["regex_matched"].([]string)`来调用

由于正则复杂多变, 所以没有示例, 在此提供一个在线正则网站 https://regex101.com/ 以供练手

</details>

<details>
<summary>OnShell</summary>

> ```go
> OnShell(command string, model interface{}, rules ...Rule) 
> ```

咕咕咕

```go
    OnShell(command string, model interface{}, rules ...Rule) 
```

[可能有的图片]

</details>

