# Module
---
앱에서 모듈은 앱을 구성하는 개별적 요소로서 재사용 가능한 코드 조각을 말한다. 이때 모듈이 성립하려면 모듈은 자신만의 파일 스코프(모듈 스코프)를 가질 수 있어야 한다.
모듈의 재사용이 가능하려면 다른 모듈에 의해 재사용되어야 한다. 이때 공개가 필요한데 export를 통해 공개하고, 공개된 모듈을 import를 통해 불러들여 재사용이 가능해진다.

따라서 모듈은 자신만의 스코프를 가지고 개별적으로 존재하다가 다른 모듈에 의해 재사용이 되는 특성을 가진다.
이렇게 모듈을 잘 분리해야 재사용성이 좋아지고 개발 효율성과 유지보수성을 높일 수 있다.

## 자바스크립트에서 모듈

자바스크립트 자체는 모듈을 지원하지 않는다. 그래서 여러 개의 자바스크립트 파일을 script를 통해 분리하여 로드해도 하나의 전역을 공유한다.

이러한 자바스크립트의 한계를 위해 나온 모듈 시스템이 CommonJS와 AMD 진영으로 나뉘게 되었고 Node.js는 CommonJS를 채택하여 CommonJS 사양을 따르고 있다. 
그래서 Node.js 환경에서는 파일별로 독립적인 파일 스코프를 갖는다.

## ES6 모듈(ESM)

ES6에서는 클라이언트 사이드 자바스크립트에서도 동작하는 모듈 기능이 추가됐다.
script 태그에 type="module" 어트리뷰트를 추가하면 로드된 자바스크립트 파일은 모듈로서 동작한다.
ESM의 파일 확장자는 mjs를 사용할 것을 권장한다.

# Babel, Webpack
---
ES6+에서 ESM을 통해서 모듈 시스템을 지원하지만 브라우저에 따라 지원율이 제각각으로 대부분의 프로젝트에서는 모듈 로더가 필요하다.
여기서 다양하고 오래된 버전의 브라우저에서도 실행이 가능하고 또 모듈을 지원하려면 추가로 트랜스파일링과 모듈 번들러가 필요하다.

## Babel

바벨(Babel)은 트랜스파일러로 최신 ES6+환경의 코드를 ES5 사양의 코드로 트랜스파일링하여 구형 브라우저에서도 문제없이 실행될 수 있도록 한다.
바벨의 주목적은 **브라우저 호환성**을 보장하기 위함이다.

### Babel 설정
바벨을 사용하려면 바벨 설치와 플러그인 설치 그리고 설정이 필요하다.
```json
{
	"devDependencies": {
		"@babel/cli": "",
		"@babel/core": "",
		"@bael/preset-env": ""
	}
}
```

그리고 프리셋(preset)이 필요한데 프리셋은 바벨 플러그인을 모아둔 것으로 Babel 프리셋이라고 부른다.
`@babel/preset-env`
`@babel/preset-react`

babel.config.json 설정 파일을 생성하고 설치하고 활용할 `@babel/preset-env`을 명시하면 된다.
```json
{
	"presets": ["@babel/preset-env"]
}
```

```json
	"scripts" : {
		"build": "babel src/js -w -d dist/js"
	}
```
src/js 폴더에 있는 모든 자바스크립트 파일들을 트랜스 파일링한 후 결과물을 dist/js폴더에 저장한다.

추가로 필요한 바벨 플러그인은 Babel 홈페이지에서 찾아서 필요에 맞게 설치하여 적용하면 된다.
### Babel 모듈 로딩
바벨로 트랜스파일링을 하게되면 설정에 따라 달라지지만 빌드 타임에 실행되며, ESM이 미비햇던 과거 관행을 따라 자동적으로 Commonjs방식의 모듈로 변환한다. 따라서 브라우저는 require함수를 지원하지 않으므로 에러가 발생한다.

더 자세히 보면
@babel/preset-env는 자동적으로 modules옵션이 auto로 설정이되어 있어 Commonjs로 변환하게 된다.
그리고 런타임이 아니라 빌드 타임을 기준으로 한다.

물론 설정을 적용하면 ESM을 유지할 수 있다.
```json
{
  "presets": [
    ["@babel/preset-env", { "modules": false }]
  ]
}

```

정리하면 바벨은 주목적이 브라우저 호환성을 위해 트랜스파일링 하며, ESM을 지원하기 때문에 모듈 기능이 가능하지만 부족하기 때문에 웹팩을 통하여 모듈 번들러를 적용한다.

## Webpack

웹팩(Webpack)은 모듈 번들러로 의존 관계에 있는 자바스크립트, CSS, 이미지 등의 리소스들을 하나의 파일로 번들링 한다.
의존 모듈이 하나의 파일로 번들링되므로 별도의 모듈 로더가 필요없다. 따라서 여러개의 script태그로 로드할 필요도 없다.

### Webpack 설정

웹팩을 설치하고 웹팩이 모듈을 번들링할 때 바벨을 사용하여 ES6+사양의 코드를 ES5사양의 코드로 트랜스파일링 하도록 babel-loader를 설치한다.
```json
{
	devDpendencies: {
		"@babel/cli": "",
		"@babel/core": "",
		"@bael/preset-env": "",
		"@babel-loader": "" ,
		"webpack": ""
	}
}
```

webpack.config.js는 웹팩이 실행될 때 참조하는 설정 파일이다. 
```js
module.exports = {
	entry: "src/js/main.js",
	outut: {
		path: path.resolve(__dirname, 'dis'),
		filename: 'js/bundle.js'
	},
	module: {
		rules: [
			{
				test: ^.js%/,
				include: [
					path.resolve(__dirname, 'src/js')
				],
				exclude: /node_modules/,
				use: {
					loader: 'babel-loader',
					options: {
						presets: ['@babel/preset-env'],
						plugins: ['@babel/plugin-proposal-class-proeries']
					}
				}
			}
		]
	}

}
```

이렇게 실행하면 mian.js, lib.js 모듈이 하나로 번들링된 bundle.js가 빌드된다.

### babel-polyfill
바벨을 사용하면 문법만 변환한다 따라서 Promise, Object.assign, Array.from 등 런타임에 활용되는 ES6에 추가된 기능은 대체할 수 없기 때문에 트랜스파일링 되지않고 그대로 남는다.

그래서 babel-polyfill이 필요하다. 폴리필은 개발환경 뿐만 아니라 런타임 환경에도 영향을 준다.
entry에 추가하여 적용한다.
```json
module.exports = {
	entry: ['@babel/polyfill', 'src/js/main.js']
}
```