# Rust로 만드는 터미널용 커맨트 라인 앱 만들기 연습

## 처음 시작

1. github에서 `command_line_app_ex`를 만듬
2. 위에서 만든 프로젝트를 가져옴:`gh repo clone dialektike/command_line_app_ex`
3. `cargo`를 이용한 프로젝트 초기화: `cargo init command_line_app_ex`
4. `cargo run` 실행

2번째 프로젝트를 가져오는 방법은 다양합니다. 전 [gh](https://cli.github.com)이라는 것을 이용했습니다.여기서 3번째가 헛갈릴 수도 있습니다. 원래는 `cargo new command_line_app_ex`라고 프로젝트를 만들지만, 전 이미 만든 프로젝트를 가지오고 있기 때문에 위와 같이 했습니다. 그리고 또 유의할 점은 `ls`나 `dir`를 사용했을 때 `command_line_app_ex`이라는 프로젝트 이름이 보이는 상태에서 3번을 실행해야 합니다. 4번은 지금까지 작업한 것을 확인하기 위한 것입니다.

### 지금까지 한 코드 0.1.0

- [ver 0.1.0 코드 확인](https://github.com/dialektike/command_line_app_ex/tree/0.1.0)

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

### 지금까지 한 코드: 0.2.0

- [ver 0.2.0 코드 확인](https://github.com/dialektike/command_line_app_ex/tree/0.2.0)

- [ver 0.1.0 코드와 ver 0.2.0 코드 비교](https://github.com/dialektike/command_line_app_ex/commit/e877921a2ac2b72aa739798258f357eb2cc9ccb7)

## txt 파일 내용을 읽어오는 기능 구현

앞에서 살펴본 것처럼 현재 이 앱에서 구현하고자 하는 기능은 `txt` 형식의 파일을 읽어서 특정 내용을 찾는 기능이다. 이 기능을 테스트하기 위해 `txt` 형식의 파일을 하나 만들자. 터미널에서 만들려면 다음과 같이 하면 된다.

```console
touch hello.txt
echo "Hello World\!" >> hello.txt
echo "Hello World, again\!" >> hello.txt
```

위에서 만든 파일을 검사해보자.

```console
➜ cat hello.txt
Hello World!
Hello World, again!
```

앞에서 입력한 내용이 잘 들어가 있다.

이제 `args.path`으로 파일 이름을 받아 그 `txt` 파일을 문자열로 읽고, `args.pattern`으로 파일 내용에서 찾을 내용을 받아서 그 내용이 들어 있는 줄을 출력하는 코드를 작성해보자.

```rust
fn main() {
    let args = Cli::parse();
    let content = std::fs::read_to_string(&args.path).expect("could not read file");

    for line in content.lines() {
        if line.contains(&args.pattern) {
            println!("{}", line);
        }
    }
}
```

`content`로 해당 `txt` 파일의 내용을 가져온 다음, `for` 문으로 그 내용을 한 줄씩 돌리면서 만약 우리가 찾을 내용이 있으면 그 내용이 있는 줄을 출력하게 된다. 실행 결과는 다음과 같다.

```console
➜ cargo run Hello hello.txt
    Finished dev [unoptimized + debuginfo] target(s) in 0.01s
     Running `target/debug/command_line_app_ex Hello hello.txt`
Hello World!
Hello World, again!
➜ cargo run again! hello.txt
    Finished dev [unoptimized + debuginfo] target(s) in 0.01s
     Running `target/debug/command_line_app_ex 'again'\!'' hello.txt`
Hello World, again!
```

위의 결과에서 보면 `Hello`가 들어 있는 두 줄을 잘 출력하고 있으며, `again!`은 한 줄에만 들어있기 때문에 한 줄만 출력하고 있는 것을 확인할 수 있다.

```console
➜ cargo run Hello helloo.txt    
    Finished dev [unoptimized + debuginfo] target(s) in 0.01s
     Running `target/debug/command_line_app_ex Hello helloo.txt`
thread 'main' panicked at 'could not read file: Os { code: 2, kind: NotFound, message: "No such file or directory" }', src/main.rs:15:55
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
➜ cargo run hello hello.txt 
    Finished dev [unoptimized + debuginfo] target(s) in 0.01s
     Running `target/debug/command_line_app_ex hello hello.txt`
```

첫 번째는 없는 파일 이름을 입력한 경우를 실행해봤다. 에러가 발생했다. 두 번째는 첫 글자를 소문자로 바꿔 `hello`을 찾아봤는데, 이것은 해당 파일에 들어 있지 않은 내용이 아니기 때문에 아무 것도 출력되지 않았다.

참고로 컴파일된 실행 파일을 실행하려면 다음과 같이 하면 된다.

```console
➜ target/debug/command_line_app_ex again! hello.txt
Hello World, again!
➜ target/debug/command_line_app_ex hello hello.txt
➜ target/debug/command_line_app_ex again! helloo.txt
thread 'main' panicked at 'could not read file: Os { code: 2, kind: NotFound, message: "No such file or directory" }', src/main.rs:15:55
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
➜ target/debug/command_line_app_ex
error: The following required arguments were not provided:
    <PATTERN>
    <PATH>

USAGE:
    command_line_app_ex <PATTERN> <PATH>

For more information try --help
```

이렇게 실행해보니 `Hello` `helloo.txt`와 같은 인자 없이 실행하는 경우 발생하는 에러를 처리하지 못하는 것을 확인할 수 있다.
