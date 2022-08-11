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

`main.rs` 파일에 다음 구조체 도입하고 정리

```rust
struct Cli {
    /// The pattern to look for
    pattern: String,
    /// The path to the file to read
    #[clap(parse(from_os_str))]
    path: std::path::PathBuf,
}
```

### 지금까지 한 코드: 0.2.0

- [ver 0.2.0 코드 확인](https://github.com/dialektike/command_line_app_ex/tree/0.2.0)

- [ver 0.1.0 코드와 ver 0.2.0 코드 비교](https://github.com/dialektike/command_line_app_ex/commit/e09edc1ad87771ab20f41756422f5c0b3670e8c3)
