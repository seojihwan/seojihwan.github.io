---
title: "[JS] Modules"
date: 2020-05-24 12:00:00
category: 'JS'
draft: false
---

#### module: 재 사용 가능한 코드로 캡슐화하고 다른 코드에서 쉽게 사용할 수 있게한다.

ES6이전의 ES5에서는 자바스크립트에서 공식적으로 정의된 module이 없다고 한다.

대신에 다음과 같은 foramt을 사용하엿다.

- Asynchronous Module Definition (AMD)
- CommonJS
- Universal Module Definition (UMD)

- System.register: ES5에서 ES6모듈을 지원하기 위해 고안되었다.
- ES6 module format: export , import로 사용하고 있는 방식이다.

##### module loaders: runtime에 module을 해석하고, load한다.

대표적인 2가지 module loader로 다음과 같은 format을 지원한다.

- RequireJS: AMD format
- SystemJS: AMD,CommonJS,UMD, System.register format

##### module bundlers: build time에 여러 module들을 하나의 file로 통합하여 bundle file을 생성한다.

module loader가 runtime에 필요한 파일을 다운로드 하고 메인파일을 해석하는 것과 달리 module bundler는 bundle을 load한다. module bundler는 여러 module을 통합하여 불러오기 때문에 module loader을 대체할 수 있지만, 때때로 사이즈가 큰 bundle file을 불러올 때는 성능저하가 일어 날 수 있으므로, loader가 더 나은 성능을 보여줄 경우도 있다.

- Browerify: CommonJS modules
- Webpack: AMD, CommonJS, ES6 modules
