# Rust로 만드는 터미널용 커맨트 라인 앱 만들기 연습 ver 0.2.0 설명서

## clap 크레이트 도입

`Cargo.toml` 파일에 다음 문장 추가

```toml
clap = { version = "3.0", features = ["derive"] }
```

`main.rs` 파일에 앞에서 도입한 `clap`에서 `Parser`을 사용한다고 하고 `#[derive(Parser)]`과 같이 설정해서 `derive`모드로 사용한다고 하고 이를 구조체를 도입하고 이를 `main()`에 적용해보자. 그런 다음 구조체로 구현한 것이 `main()` 함수를 이용해서 어떻게 작용하는 출력해보자. 그러면 `main.rs`이 다음과 같이 될 것이다.

```rust
use clap::Parser;

/// Search for a pattern in a file and display the lines that contain it.
#[derive(Parser)]
struct Cli {
    /// The pattern to look for
    pattern: String,
    /// The path to the file to read
    #[clap(parse(from_os_str))]
    path: std::path::PathBuf,
}

fn main() {
    let args = Cli::parse();
    println!("파일 경로 및 파일 이름:{:?}, 찾을 내용: {}", &args.path, &args.pattern);
}
```

이를 터미널에서 다음과 같이 컴파일하고 실행하면 다음과 같은 결과를 확인할 수 있다.

```console
➜ cargo run hello hello.txt
   Compiling command_line_app_ex v0.1.0 (/Users/jaehwan/git/command_line_app_ex)
    Finished dev [unoptimized + debuginfo] target(s) in 0.33s
     Running `target/debug/command_line_app_ex hello hello.txt`
파일 경로 및 파일 이름:"hello.txt", 찾을 내용: hello
➜ cargo run aa --help 
    Finished dev [unoptimized + debuginfo] target(s) in 0.01s
     Running `target/debug/command_line_app_ex aa --help`
command_line_app_ex 
Search for a pattern in a file and display the lines that contain it

USAGE:
    command_line_app_ex <PATTERN> <PATH>

ARGS:
    <PATTERN>    The pattern to look for
    <PATH>       The path to the file to read

OPTIONS:
    -h, --help    Print help information
```

 별로 한 것이 없는데 터미널 앱 형태를 이미 갖춘 것이다. 첫 번째 입력한 인자인 `hello`은 찾을 내용으로 표시되며, 두 번째 입력한 인자인 `hello.txt`은 '파일 경로 및 파일 이름'이라고 표시되고 있다. 이를 통해 알 수 있는 것이 특정 `txt`에 특정 내용을 찾는 커멘드 앱을 만들고 있는 것인지 추측할 수 있겠다.

### 지금까지 한 코드

- [ver 0.2.0 코드 확인](https://github.com/dialektike/command_line_app_ex/tree/0.2.0)
- [v.0.2.0 코드 다운](https://github.com/dialektike/command_line_app_ex/releases/tag/v.0.2.0)

- [ver 0.1.0 코드와 ver 0.2.0 코드 비교](https://github.com/dialektike/command_line_app_ex/commit/e877921a2ac2b72aa739798258f357eb2cc9ccb7)
