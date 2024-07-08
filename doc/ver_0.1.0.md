# Rust로 만드는 터미널용 커맨트 라인 앱 만들기 연습 ver 0.1.0 설명서

## 지금까지의 과정

1. github에서 `command_line_app_ex`를 만듬
2. 위에서 만든 프로젝트를 가져옴:`gh repo clone dialektike/command_line_app_ex`
3. `cargo`를 이용한 프로젝트 초기화: `cargo init command_line_app_ex`
4. `cargo run` 실행

## 참고

2번에서 프로젝트를 가져오는 방법은 다양한 방법이 있습니다. 전 **[gh](https://cli.github.com)**이라는 것을 이용했습니다. 여기서 3번 과정이 헛갈릴 수도 있습니다. 원래는 `cargo new command_line_app_ex`라고 입력하여 프로젝트를 만들 수도 있지만, 전 이미 github에서 만든 프로젝트를 가지오고 있기 때문에 위와 같이 `cargo init command_line_app_ex`을 입력했습니다.

그리고 `cargo init command_line_app_ex`을 사용할 때 유의할 점은 `ls`나 `dir`를 사용했을 때 `command_line_app_ex`이라는 프로젝트 이름이 보이는 상태에서 3번 과정을 실행해야 합니다. 4번 과정까지 진행하고 나면 아래 ver 0.1.0 코드까지 진행된 상태가 됩니다.

자세한 내용은 아래 링크를 참고하세요.

### 지금까지 한 코드

- [v.0.1.0 코드 확인](https://github.com/dialektike/command_line_app_ex/tree/0.1.0)
- [v.0.1.0 코드 다운](https://github.com/dialektike/command_line_app_ex/releases/tag/v.0.1.0)
