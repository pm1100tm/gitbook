# Express

## Node.js Framework Express

Node.js 의 대표적인 웹프레임워크인 **Express** 를 활용하여 간단한 서버를 만들어본다.

## 프로젝트 폴더 만들고, 해당 폴더에 진입하기

```shell
mkdir express-demo-app
cd mkdir express-demo-app
```

## npm 초기화

```shell
npm init -y
```

## .gitignore 파일 생성

```shell
touch .gitignore
```

> 파일 생성 후 .gitignore 페이지에서 **VisualStudioCode**, **node** 키워드 입력 후
> copy&paste 한다. node_modules, dist 가 반드시 존재해야 한다.

## 아래의 모듈 설치

```shell
npm i -D typescript
npm i -D ts-node
npm i -D nodemon
npm i eslint
npm i express
npm i @types/express
```

> nodemon 은 개발 편의상 설치. ts-node 에 대하여 의존성을 가지고 있기 때문에
> ts-node 가 반드시 설치되어있어야 한다.

## 각종 초기화

```shell
# tsconfig.json 파일 생성
npx tsc --init
```

```shell
# lint 설정. .eslintrc.js 파일 생성
npx eslint --init
```

![lint-init-for-node.png](/asset/lint-init-for-node.png)

### Prettier 가 extension 으로 설치되어 있는 경우, lint 설정 후 충돌 피하기

```shell
npm install --save-dev eslint-config-prettier
```

```javascript
// .eslintrc.js
...
overrides: [
  {
    env: {
      node: true,
    },
    files: ['.eslintrc.{js,cjs}'],
    parserOptions: {
      sourceType: 'script',
    },
  },
  {
    extends: ['xo-typescript', 'prettier'], // <- Add here!
    files: ['*.ts', '*.tsx'],
  },
],
```

### ts-node, nodemon 실행 방법

```shell
npx ts-node
npx nodemon <path>
```

## package.json 수정

```json
...
"main": "app.ts",
"scripts": {
  "start": "nodemon app.ts",
  "lint": "eslint --fix .",
  "test": "echo \"Error: no test specified\" && exit 1"
},
```

## app.ts 파일 생성

루트 패스에 app.ts 파일 생성 후 아래와 같이 작성

```javascript
import express from 'express';
import cors from 'cors';

const port = 3000;
const app = express();

app.use(cors());
app.get('/', (req, res) => res.send('Hello, World!'));

app.listen(port, () => {
  console.log(`Server running at http://localhost:${port}`);
});
```

## REST URL 기본 규칙

```plain
GET /products
GET /products/{id}
POST /products
PATCH /product/{id}
DELETE /product/{id}
```

## 실행

```shell
npm run start
```

## TO BE ADDED

- How to set cors for other origin
- How to set url path and divide api
