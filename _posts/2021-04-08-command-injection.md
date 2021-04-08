---
title: "Command injection"
categories:
  - Web Hacking
tags:
  - Web
  - Command
  - Hacking
last_modified_at: 2021-04-08
author_profile: true
sitemap:
  changefreq: daily
  priority: 1.0
---

Command injection에 대해서 알아봅시다.<br/>

Command injection은 취약점이 있는 서버에서 
임의의 커맨드를 실행시키는 것이 목적인 공격 방법입니다.
대부분 이 공격은 서버에서 커맨드 실행 시,
유저의 입력에 대한 확인 절차가 충분하지 않을 경우에 발생합니다.
Command injection은 직접 코드를 삽입하지 않고,
`원래 있는 코드 상의 취약점`을 이용해 커맨드를 추가로 실행한다는 점에서
Code injection과는 차이가 있습니다.

## 커맨드?

커맨드(command)는 컴퓨터 내부 시스템을 실행하는데 사용하는 명령어들을 가리키는 말입니다.
보통 유닉스(Unix)나 리눅스(Linux) 기반으로 돌아가는 서버가 많습니다.
그래서 간단한 커맨드 몇 개를 알아두면 여러 워게임을 풀 때 편합니다.
간단한 리눅스 커맨드는 여기에: [til/Linux](https://github.com/tula3and/til/blob/master/System%20Programming/Linux.md)

- `;` 또는 `&&`를 통해 여러 커맨드를 한 줄 내에서 실행할 수 있습니다.
- 커맨드가 `"`로 묶일 경우 하나의 커맨드로 취급합니다.
  ```
  root@tula3and:/workspace/wargame# ls
  README.md  index.py
  root@tula3and:/workspace/wargame# "ls"
  README.md  index.py
  root@tula3and:/workspace/wargame# echo hello
  hello
  root@tula3and:/workspace/wargame# "echo hello"
  bash: echo hello: command not found
  ```

## 공격 예시

리눅스 커맨드 중 `echo`와 `ls`를 이용해서 예시를 들어보겠습니다.

```python
text = request.form.get('input_text')
cmd = f'echo "{text}"'
out = subprocess.check_output(['/bin/sh', '-c', cmd])
print(out)
```

위의 파이썬 코드가 돌아가는 서버에서 유저로부터 `hello`라는 입력 값을 받으면,
`echo` 때문에 그 입력 값을 단순 출력해주는 결과가 나올 것입니다. 아래와 같이 말이죠!

```
hello
```

이때 입력 값에 대한 확인 절차가 없으므로, Command injection을 통해 임의의 커맨드를 넣어줄 수 있습니다.
입력 값을 `hello"; "ls`로 주게 되면, `cmd`는 `'echo "hello"; "ls"'`와 같게 될 것입니다.
따라서 해당 `cmd`가 실행된다면, `hello`가 출력될 뿐만 아니라 `ls`가 실행된 결과인 해당 위치의 모든 파일이 출력되게 됩니다.
굳이 실행 예를 들자면, 아래와 같은 결과가 나오겠네요.

```
hello
README.md index.py
```

---

위의 코드 중 `['/bin/sh', '-c', cmd]`에서 `-c`의 의미는 커맨드가 `"`로 묶여 있을 때, 해당하는 프로그램에 맞게 해석할 수 있도록 하는 플래그입니다.
자세한 내용은 [이 링크](https://askubuntu.com/questions/831847/what-is-the-sh-c-command)를 참고해주세요.

## References

- [OWASP Command Injection](https://owasp.org/www-community/attacks/Command_Injection)
- [Dreamhack: command-injection-1](https://dreamhack.io/wargame/challenges/44/)
- [Command injection 꿀팁](https://m.blog.naver.com/yjw_sz/221609691465)

---

💬 _Any comments and suggestions will be appreciated._
