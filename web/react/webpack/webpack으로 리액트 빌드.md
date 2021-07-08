# react, typescript, webpack으로 세팅 및 빌드 하기

## 1. script

다음 프로세스를 순서대로 실행한다.

- yarn init // 초기 설정
- yarn add -D webpack webpack-cli // 웹팩 개발용 패키지 설치

touch webpack.config.js // 프로젝트의 루트 경로에 webpack 설정 파일을 만들어 준다.

// webpack 내용

```
    const path = require('path');
    module.exports = {
        entry: './src/index.tsx', // 빌드가 시작되는 경로
        output: {
            path: path.join(__dirname, '/dist'), // 빌드 후 번들링된 프로젝트 파일이 위치되는 경로 설정
            filename: 'bundle.min.js' // 번들린된 js 파일의 이름 설정
        }
    }
```

위 내용을 webpack.config.js에 작성해준후 다음 스크립트를 실행시킨다.

- yarn add -D typescript ts-loader html-webpack-plugin
// i) 1. webpack 모듈 번들러는 기본적으로 js 파일만 이해하고 번들링할 수 있다. 그러므로 loader를 사용하여 여러유형의 파일을 처리할 수 있도록해주며 타입스크립트 언어 또한 처리 할 수 있도록 패키지를 설치해주자
// i) 2. typescript 파일 번들링 모듈은 대표적으로 ts-loader와 awesome-typescript-loader가 있으며 해당 예제에서는 ts-loader를 사용한다.

touch tsconfig.json // 프로젝트의 루트 경로에 TS 환경 파일을 만들어 준다.

파일 생성후 아래 옵션을 내용으로 작성해준다.

```
{
    "compilerOptions": {
      "outDir": "build",
      "module": "esnext",
      "target": "es5",
      "lib": ["es6", "dom", "es2016", "es2017"],
      "sourceMap": true,
      "allowJs": false,
      "jsx": "react",
      "declaration": true,
      "declarationDir": "dist/types",
      "moduleResolution": "node",
      "esModuleInterop": true,
      "forceConsistentCasingInFileNames": true,
      "noImplicitReturns": true,
      "noImplicitThis": true,
      "noImplicitAny": true,
      "strictNullChecks": true,
      "suppressImplicitAnyIndexErrors": true,
      "noUnusedLocals": true,
      "noUnusedParameters": true
    },
    "include": ["src"],
    "exclude": ["node_modules", "build"]
  }
```

이후 다시 webpack.config.js 파일로 이동한 후 아래 내용추가

```
    const path = require('path');
    const HtmlWebpackPlugin = require('html-webpack-plugin');

    module.exports = {
    entry: './src/index.tsx',
    resolve: { // 인식 파일 형식 지정
        extensions: ['.ts', '.tsx', '.js']
    },
    output: {
        path: path.join(__dirname, '/dist'),
        filename: 'bundle.min.js'
    },
    module: { // webpack에서 loader를 사용할 때에는 아래와 같이 사용하며 loader는 webpack이 js파일이 아닌 형식의 파일도 이해할 수 있도록한다.
        rules: [
            { 
                test: /\.tsx?$/, 
                loader: 'ts-loader'
            }
        ]
    },
      plugins: [
        new HtmlWebpackPlugin({ // html파일을 webpack이 빌드할 수 있도록 플러그인을 설정한다.
            template: './src/index.html'
        })
    ]
}
```

- yarn add -D react react-dom @types/react @types/react-dom // react 패키지 설치

파일 구조 세팅

root
    | node_modules/
    | src
    |   - App.tsx
    |   - index.tsx
    |   - index.html
    | pakage.json
    | tsconfig.json
    | webpack.config.js
    
위와 같이 파일 및 폴더를 생성해 준후 아래 표시된 것과 같이 파일 내용을 작성해 준다.

App.tsx
```
    import * as React from 'react';

    const App = () => {
        return <>Hello, webpack/react</>
    }

    export default App;
```

index.tsx
```
    import * as React from 'react';
    import * as ReactDOM from 'react-dom';

    import App from './App';

    ReactDOM.render (<App />, document.getElementById("root"));
```

index.html

```
    <!DOCTYPE html> 
    <html lang="en"> 
    <head> 
        <meta charset="UTF-8"> 
        <meta name="viewport" content="width=device-width, initial-scale=1.0"> 
        <meta http-equiv="X-UA-Compatible" content="ie=edge"> 
        <title>TypeScript + React</title> 
    </head> 
    <body> 
        <div id="root"> 

        </div>
    </body>
    </html>
```

개발서버환경으로 프로젝트를 실행 시키기 위해 webpack-dev-server 설치

이후 pakage.json에 스크립트아래와 같이 수정

```
    {
    "name": "test-folder",
    "version": "1.0.0",
    "main": "index.js",
    "license": "MIT",
    "scripts": {
        "start": "webpack serve --mode development --open --hot",
        "build": "webpack --mode production"
    },
    "dependencies": {
        "react": "^17.0.2",
        "react-dom": "^17.0.2"
    },
    "devDependencies": {
        "@babel/preset-typescript": "^7.14.5",
        "@types/react": "^17.0.11",
        "@types/react-dom": "^17.0.8",
        "@types/webpack": "^5.28.0",
        "html-webpack-plugin": "^5.3.2",
        "ts-loader": "^9.2.3",
        "tslint": "^6.1.3",
        "tslint-react": "^5.0.0",
        "typescript": "^4.3.4",
        "webpack": "^5.40.0",
        "webpack-cli": "^4.7.2",
        "webpack-dev-server": "^3.11.2"
    }
    }

```

- yarn build
- yarn start

=> 확인
