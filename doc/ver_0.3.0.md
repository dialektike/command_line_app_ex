# Rust로 만드는 터미널용 커맨트 라인 앱 만들기 연습 ver 0.3.0 설명서

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

### 지금까지 한 코드: 0.3.0

- [ver 0.3.0 코드 확인](https://github.com/dialektike/command_line_app_ex/tree/0.3.0)
- [v.0.3.0 코드 다운](https://github.com/dialektike/command_line_app_ex/releases/tag/v.0.3.0)
- [ver 0.2.0 코드와 ver 0.3.0 코드 비교](https://github.com/dialektike/command_line_app_ex/commit/07e19422d89802e2590d4a62d09dd5f8eb887ac0)
