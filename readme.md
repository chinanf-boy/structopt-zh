# TeXitoi/structopt [![explain]][source] [![translate-svg]][translate-list]

<!-- [![size-img]][size] -->

[explain]: http://llever.com/explain.svg
[source]: https://github.com/chinanf-boy/Source-Explain
[translate-svg]: http://llever.com/translate.svg
[translate-list]: https://github.com/chinanf-boy/chinese-translate-list
[size-img]: https://packagephobia.now.sh/badge?p=Name
[size]: https://packagephobia.now.sh/result?p=Name

「 通过定义结构来解析命令行参数。它结合了[clap](https://crates.io/crates/clap)的自定义派生(derive). 」

[中文](./readme.md) | [english](https://github.com/TeXitoi/structopt)

---

## 校对 ✅

<!-- doc-templite START generated -->
<!-- repo = 'TeXitoi/structopt' -->
<!-- commit = 'b4cdc47686b1f6a6fa9134eff447cfa7c00cab36' -->
<!-- time = '2018-12-22' -->

| 翻译的原文 | 与日期        | 最新更新 | 更多                       |
| ---------- | ------------- | -------- | -------------------------- |
| [commit]   | ⏰ 2018-12-22 | ![last]  | [中文翻译][translate-list] |

[last]: https://img.shields.io/github/last-commit/TeXitoi/structopt.svg
[commit]: https://github.com/TeXitoi/structopt/tree/b4cdc47686b1f6a6fa9134eff447cfa7c00cab36

<!-- doc-templite END generated -->

### 贡献

欢迎 👏 勘误/校对/更新贡献 😊 [具体贡献请看](https://github.com/chinanf-boy/chinese-translate-list#贡献)

## 生活

[help me live , live need money 💰](https://github.com/chinanf-boy/live-need-money)

---

# StructOpt[![Build status](https://travis-ci.org/TeXitoi/structopt.svg?branch=master)](https://travis-ci.org/TeXitoi/structopt) [![](https://img.shields.io/crates/v/structopt.svg)](https://crates.io/crates/structopt) [![](https://docs.rs/structopt/badge.svg)](https://docs.rs/structopt)

通过定义结构来解析命令行参数。它结合了[clap](https://crates.io/crates/clap)的自定义派生(derive).

## 文档

可在[Docs.rs](https://docs.rs/structopt)找到。你也可以检查一下[示例](https://github.com/TeXitoi/structopt/tree/master/examples)和[changelog](https://github.com/TeXitoi/structopt/blob/master/CHANGELOG.md).

## 例

加`structopt`，到你的`Cargo.toml`依赖项:

```toml
[dependencies]
structopt = "0.2"
```

然后,在你的 Rust 文件中:

```rust
#[macro_use]
extern crate structopt;

use std::path::PathBuf;
use structopt::StructOpt;

/// 一个基础示例
#[derive(StructOpt, Debug)]
#[structopt(name = "basic")]
struct Opt {
    // 一个 flag, 给命令行使用. 
    // 注意，三注释作为 命令的帮助信息
    /// 应用 debug 模式
    #[structopt(short = "d", long = "debug")]
    debug: bool,

    // `v/verbose` 标志 的 数量
    /// Verbose 模式 (-v, -vv, -vvv, etc.)
    #[structopt(short = "v", long = "verbose", parse(from_occurrences))]
    verbose: u8,

    /// 设置 speed
    #[structopt(short = "s", long = "speed", default_value = "42")]
    speed: f64,

    /// Output 文件
    #[structopt(short = "o", long = "output", parse(from_os_str))]
    output: PathBuf,

    /// 多少 车
    #[structopt(short = "c", long = "nb-cars")]
    nb_cars: Option<i32>,

    /// 考虑 的 管理 等级
    #[structopt(short = "l", long = "level")]
    level: Vec<String>,

    /// 需求的文件
    #[structopt(name = "FILE", parse(from_os_str))]
    files: Vec<PathBuf>,
}

fn main() {
    let opt = Opt::from_args();
    println!("{:?}", opt);
}
```

使用此示例:

```
$ ./basic
error: The following required arguments were not provided:
    --output <output>

USAGE:
    basic --output <output> --speed <speed>

For more information try --help
$ ./basic --help
basic 0.2.0
Guillaume Pinot <texitoi@texitoi.eu>
A basic example

USAGE:
    basic [FLAGS] [OPTIONS] --output <output> [--] [FILE]...

FLAGS:
    -d, --debug      Activate debug mode
    -h, --help       Prints help information
    -V, --version    Prints version information
    -v, --verbose    Verbose mode

OPTIONS:
    -c, --nb-cars <nb_cars>   Number of cars
    -l, --level <level>...    admin_level to consider
    -o, --output <output>     Output file
    -s, --speed <speed>       Set speed [default: 42]

ARGS:
    <FILE>...    Files to process
$ ./basic -o foo.txt
Opt { debug: false, verbose: 0, speed: 42, output: "foo.txt", car: None, level: [], files: [] }
$ ./basic -o foo.txt -dvvvs 1337 -l alice -l bob --nb-cars 4 bar.txt baz.txt
Opt { debug: true, verbose: 3, speed: 1337, output: "foo.txt", nb_cars: Some(4), level: ["alice", "bob"], files: ["bar.txt", "baz.txt"] }
```

## StructOpt rustc 版本策略

- 必须在[changelog](https://github.com/TeXitoi/structopt/blob/master/CHANGELOG.md)和[travis 配置](https://github.com/TeXitoi/structopt/blob/master/.travis.yml)中指定最小的 rustc 版本修改.
- 如果 StructOpt 的某个依赖项的最新版本需要新版本,贡献者可以在没有任何理由的情况下，增加最小的 rustc 版本(`cargo update`不会在 StructOpt 上失败).
- 如果库用户体验得到改善，贡献者可以增加最小的 rustc 版本.

## 为什么

我用[docopt](https://crates.io/crates/docopt)很长一段时间(Rust 1.0 前)。我真的很喜欢你有一个带有解析参数的结构: 不需要转换`String`至`f64`,没用`unwrap`。但另一方面,我不喜欢手工编写使用字符串。感觉就像回到 WYSIWYG 编辑的黄金时代。字段命名也需要点人工.

今天,在 Rust 中读取命令行参数的新标准是[clap](https://crates.io/crates/clap)。这个箱功能齐全! 但我认为有一个缺点:即使您可以验证参数并表达需要参数，您仍然需要将看起来像字符串向量(string vectors)的 hashmap 的内容转换为，对您的应用程序有用的东西.

现在,有稳定的自定义派生(derive)。因此,我可以添加 clap ，用于我想念的自动转换，这是结果。

## 执照

根据任何一种许可

- Apache 许可证,2.0 版([LICENSE-APACHE](LICENSE-APACHE)要么<https://www.apache.org/licenses/LICENSE-2.0>)
- MIT 许可证([LICENSE-MIT](LICENSE-MIT)要么<https://opensource.org/licenses/MIT>)

根据你的选择.

### 贡献

除非您另有明确说明，否则您在 Apache-2.0 许可中，定义的任何有意提交(包含在您工作中的贡献)应按上述方式进行双重许可,不附加任何其他条款或条件.
