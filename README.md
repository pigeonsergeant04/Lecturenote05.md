# Lecturenote05.md
# [오픈소스 SW] Lecture 5: Shell Commands 2 실습 및 요약 노트

## 1. 주요 Shell 명령어 정리

### 1.1 입출력 재지정 (I/O Redirection)
- **Standard Output (stdout)**: 기본적으로 터미널 화면에 출력되는 결과를 파일로 보냄[cite: 2].
- **Standard Input (stdin)**: 키보드 입력 대신 파일의 내용을 입력으로 전달함[cite: 4].

* `>` (Standard Output Overwrite)
  - 명령어 실행 결과를 파일에 저장 (기존 파일이 있으면 덮어씀)[cite: 2].
  - **사용 예시**:
    ```bash
    ls -lh > file_list.txt
    cat file_list.txt
    ```

* `>>` (Standard Output Append)
  - 명령어 실행 결과를 기존 파일의 끝에 덧붙임 (파일이 없으면 새로 생성)[cite: 3].
  - **사용 예시**:
    ```bash
    ls -lh >> file_list.txt
    cat file_list.txt
    ```

* `<` (Standard Input)
  - 파일의 내용을 명령어의 표준 입력으로 전달[cite: 4].
  - `<`와 `>`를 한 줄에 혼용 가능[cite: 4].
  - **사용 예시**:
    ```bash
    sort < words.txt > sorted_words.txt
    cat sorted_words.txt
    ```

---

### 1.2 파이프라인 (Pipelines) 및 관련 유틸리티

* `|` (Pipeline)
  - 앞 명령어의 표준 출력을 뒤 명령어의 표준 입력으로 연결함 (`command1 | command2`)[cite: 5].
  - **사용 예시**:
    ```bash
    ls -lh | less
    ls -1 | wc -l
    ```

* `less`
  - 긴 텍스트 출력을 한 화면씩 나누어 봄 (종료 시 `q` 키 입력)[cite: 5].
  - **사용 예시**:
    ```bash
    ls -lh | less
    ```

* `wc` (Word Count)
  - 텍스트의 단어, 줄 수, 바이트 수를 셈[cite: 5].
  - **주요 옵션**:
    - `-l` : 줄(line) 수만 카운트[cite: 5]
  - **사용 예시**:
    ```bash
    ls -1 | wc -l
    ```

---

### 1.3 특수문자 및 확장 (Expansion & Backslash)

* **Expansion (확장)**
  - 쉘이 명령어를 실행하기 전 특수문자를 원래 의미로 해석하여 확장[cite: 6].
  - `*` : 임의의 파일/문자열과 매칭[cite: 6]
  - `~` : 현재 사용자의 홈 디렉토리 경로로 확장[cite: 6]
  - **사용 예시**:
    ```bash
    echo print out the text
    echo *
    echo ~
    ```

* `\` (Backslash)
  - 줄바꿈(Enter)을 무시하여 긴 명령어를 여러 줄로 나누어 입력할 수 있음[cite: 7].
  - **사용 예시**:
    ```bash
    ls -1 \
    > -h
    ```

---

### 1.4 파일 권한 관리 (Permissions & `chmod`)

* **권한 구조 (Permissions)**
  - 리눅스는 다중 사용자 시스템으로 소유자(Owner) / 그룹(Group) / 기타 사용자(Others) 별 권한이 다르게 지정됨[cite: 8].
  - **권한 종류**: Read (`r` = 4), Write (`w` = 2), Execute (`x` = 1)[cite: 8, 9]

* `chmod` (Change Mode)
  - 파일이나 디렉토리의 접근 권한을 변경[cite: 9].
  - **주요 권한 조합 (8진수)**:
    - `777` : 모든 사용자에게 전권 허용[cite: 9]
    - `755` : 소유자는 전권, 타인은 읽기/실행만 가능[cite: 9]
    - `700` : 소유자만 전권 허용 (비공개)[cite: 9]
    - `666` : 모든 사용자에게 읽기/쓰기 허용[cite: 9]
    - `644` : 소유자는 읽기/쓰기, 타인은 읽기만 가능 (일반 문서 기본값)[cite: 9]
    - `600` : 소유자만 읽기/쓰기 가능[cite: 9]
  - **사용 예시**:
    ```bash
    chmod 600 README.md
    chmod 644 words.txt
    ls -l words.txt
    ```

---

### 1.5 최고 관리자 권한 (Superuser)

* `sudo` (Superuser Do)
  - 시스템 관리자 권한이 필요한 명령어를 실행할 때 붙임[cite: 11].
  - 종료 시 `exit` 입력하여 세션 이탈[cite: 11].
  - **사용 예시**:
    ```bash
    sudo some_command
    sudo
    exit
    ```

---

### 1.6 텍스트 에디터 (Text Editors)

- **CLI 에디터**:
  - `vi` / `vim` : 유닉스 표준 에디터, 빠르고 강력함[cite: 12].
  - `nano` : 사용하기 가장 쉬운 입문자용 CLI 에디터[cite: 12].
  - `emacs` : 다양한 기능을 제공하는 고성능 에디터[cite: 12].
- **GUI 에디터**:
  - `gedit` : GNOME 데스크톱 환경의 기본 에디터[cite: 12].
  - `kwrite` : KDE 데스크톱 환경의 에디터 (구문 강조 기능 지원)[cite: 12].

---

### 1.7 쉘 스크립트 (Shell Script)

* `sh` / `.sh`
  - 여러 쉘 명령어를 하나의 파일에 저장하고 자동 실행함[cite: 13].
  - 파일 첫 줄에 Shebang(`#!/bin/bash`) 입력[cite: 13].
  - **사용 예시**:
    ```bash
    nano myscript.sh
    # 스크립트 작성:
    # #!/bin/bash
    # echo "Hello Shell Script!"
    
    sh myscript.sh
    ```
  - 💡 **Tip**: 윈도우 환경에서 nano 사용 시 문제가 있으면 메모장 등 다른 에디터로 편집 가능[cite: 13].

---

### 1.8 기타 유틸리티 및 다운로드 도구

* `history`
  - 이전 실행했던 명령어 히스토리를 확인[cite: 14].
  - **사용 예시**:
    ```bash
    history
    history > history_command.txt
    ```

* `wget`
  - 인터넷 URL로부터 파일을 직접 다운로드[cite: 15].
  - **사용 예시**:
    ```bash
    wget [https://opencv-tutorial.readthedocs.io/en/latest/_images/horse.jpg](https://opencv-tutorial.readthedocs.io/en/latest/_images/horse.jpg)
    ```

* `curl`
  - 인터넷을 통해 데이터를 가져오거나 전송[cite: 16].
  - **주요 옵션**: `-o` (저장할 파일 이름 지정)[cite: 16]
  - **사용 예시**:
    ```bash
    curl -o horse.jpg [https://opencv-tutorial.readthedocs.io/en/latest/_images/horse.jpg](https://opencv-tutorial.readthedocs.io/en/latest/_images/horse.jpg)
    ```

---

### 1.9 텍스트 검색 (Pattern Search - `grep`)

* `grep` (Global Regular Expression Print)
  - 파일 내에서 특정 문자열이나 정규식 패턴을 검색 및 출력[cite: 17].
  - **주요 옵션**:
    - `-i` : 대소문자 구별 없이 검색 (Case-insensitive)[cite: 17]
    - `-v` : 일치하지 않는 행만 출력 (Invert match)[cite: 17]
    - `-n` : 행 번호 표시[cite: 17]
    - `-r` : 디렉토리 내 모든 파일을 재귀적으로 검색[cite: 17]
  - **정규식 패턴**:
    - `^` : 줄의 시작[cite: 17]
    - `$` : 줄의 끝[cite: 17]
    - `[abc]` : 대괄호 안의 문자 중 하나[cite: 17]
    - `*` : 0개 이상의 문자 매칭[cite: 17]
  - **사용 예시**:
    ```bash
    grep "search_term" file.txt
    echo "This is a test sentence with some words." | grep -i "Test"
    echo "This is a test sentence with some words." | grep -n ^[A-Z]
    echo "This is a test sentence with some words." | grep -i ^t
    ```

---

## 2. 터미널 팁 및 AI 활용

1. **복잡한 명령어 문제 해결**:
   - 명령어가 기억나지 않거나 특정 조건(예: 특정 접두사/접미사를 가진 파일 삭제)을 만족하는 명령이 필요할 때 LLM(Claude, ChatGPT 등)에게 질의하여 알맞은 쉘 명령어를 추천받을 수 있음[cite: 18].
   - 예시: `rm abc*!` (abc로 시작하고 !로 끝나는 모든 파일 삭제)[cite: 18]
