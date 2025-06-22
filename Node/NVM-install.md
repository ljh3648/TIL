# NVM
Node Version Manger
노드 버전 관리자로 여러 버전의 node.js를 관리해주는 도구.

[NVM GitHub](https://github.com/nvm-sh/nvm)

## NVM 설치 방법 

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.3/install.sh | bash

source ~/.bashrc
```

## NVM으로 Node.js 설치 방법

```bash
nvm install node # 또는 버전 지정 가능
```

## NVM 명령어 모음

```bash
nvm ls # 설치된 node 버전 출력
nvm use 22.16.0 # 사용할 node 버전 변경
nvm alias default 22.16.0 # default 버전 변경
```