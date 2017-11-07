# 2.2. React-sketchapp 기본 사용법

Sketch (https://www.sketchapp.com) 는 Vector 기반의 그래픽 툴로서, 최근 Photoshop 이상으로 많이 사용되고 있습니다. 쉽고 직관적으로 벡터 드로잉을 할 수 있고, 제플린 (https://zeplin.io) 과 같은 서비스와 연동하여 디자인 가이드를 자동으로 만들기도 하는 등 다양한 기능을 사용할 수 있습니다. 특히 모바일 UI 디자인에 많이 사용되고 있으며, Sketch 용으로 다양한 모바일 디자인 UI Kit을 구할 수도 있습니다.

Sketch의 Symbol 기능은 UI 공통 컴포넌트의 개념으로서 재사용 가능한 UI 요소를 미리 만들어 놓고 다양한 화면 디자인에서 가져다 사용할 수 있도록 하고 있습니다.

Mac의 OSX 에서만 사용이 가능한 단점이 있지만, 기존의 무겁고 고가의 디자인 툴과 비교하여 상대적으로 가볍고 직관적이기 때문에 웹과 모바일 영역에서 최근 가장 많이 사용되고 있는 디자인 툴 중 하나입니다.


### 2.2.1. react-sketchapp 란?

`react-sketchapp` (https://github.com/airbnb/react-sketchapp) 은 리엑트 컴포넌트를 Sketch 에 랜더링 할 수 있도록 하는 기능을 제공합니다.
지금까지 일반적인 디자인 작업으로, Sketch 로 디자인한 뒤에 개발 코드로 옮겨 왔지만, `react-sketchapp` 은 정반대로 React 기반의 코드를 작성하여 Sketch 에 디자인하는 것입니다.

기존에는 디자인 결과물을 기반으로 일부 코드를 생성하여 개발자에게 제공되는 방식으로 문제를 해결했다면, `react-sketchapp` 은 디자인 작업을 디자인 도구로 그리는 것이 아니라, 코드로 디자인을 하는 것입니다. 이런 방식은 디자이너와 개발자가 공통의 언어로 커뮤니케이션할 수 있고, 개발이 변경될 때에도 디자인도 함께 변경하기에도 용이합니다. 

> **React-sketchapp 을 사용하면 Sketch 를 직접 수정할 일이 없을까?**
> 실제로는 그렇지 않습니다. 분명히 React 로 코딩한 것을 Sketch 로 랜더링하게 되지만, 커뮤니케이션 중에 일부 빠르게 변화된 UI를 일시적으로 확인하기 위해서는 분명히 Sketch 에서 직접 수정하는 것이 빠를 것입니다. 다만 Sketch 에서 디자인을 수정한다고 해서 코드에 반영이 되지는 않습니다. 
> 그러므로, UI 변경이 확정이 되면 그때 React 로 수정사항을 코딩해서 반영해야 할 것입니다.


### 2.2.2. react-sketchapp 실행

`react-sketchapp` 을 다운로드 받은 후 간단한 예제 코드를 확인하고, 실제로 실행해 보도록 합니다. 예제코드는 `react-sketchapp` 프로젝트의 `./examples` 폴더에 여러가지가 준비 되어 있습니다. 이 중에 가장 기본적인 색상팔랫트를 작성하는 예제를 보도록 합니다.

우선 `git clone` 으로 소스를 다운로드 받아 예제 프로젝트의 디렉토리 구조를 확인합니다. 
```
$ git clone https://github.com/airbnb/react-sketchapp.git
$ cd react-sketchapp/examples/basic-setup
$ tree
.
├── README.md
├── package.json
└── src
    ├── manifest.json
    └── my-command.js
```

그리고, `package.json` 를 열어보고, 예제 프로젝트가 사용하는 모듈들이 무엇이 있는지 보도록 하겠습니다.
```
  "devDependencies": {
    "skpm": "^0.10.2"
  },
  "dependencies": {
    "react": "^15.4.2",
    "react-sketchapp": "^0.12.1",
    "react-test-renderer": "^15.4.2"
  }
```

그리고, `react-sketchapp` 에서 제공하는 Component(https://github.com/airbnb/react-sketchapp/tree/master/src/components) 를 사용하여 화면을 그리는 코드를 작성할 수 있습니다. `react-sketchapp` 에서 제공하는 Component 는 Sketch 의 작업 화면 단위인 Page와 Artboard 가 있으며, 디자인에 사용되는 View, Text, Image 가 기본적으로 제공됩니다.  

`react-sketchapp` 는 Sketch의 플로그인으로 빌드되어 Sketch에 랜더링 하는 방식이므로, 플로그인을 빌드할 수 있는 `skpm` (https://github.com/skpm/skpm) 를 사용하고 있습니다. 

이제 `package.json` 에서 빌드하는 스크립트 설정을 보도록 합니다.
```
  "main": "basic-setup.sketchplugin",
  "manifest": "src/manifest.json",
  "scripts": {
    "build": "skpm build",
    "watch": "skpm build --watch",
    "render": "skpm build --watch --run",
    "render:once": "skpm build --run",
    "link-plugin": "skpm link"
  },
```

`skpm` 를 사용하여 Sketch 플러그인으로 빌드하게 되는데 빌드된 파일 이름은 `basic-setup.sketchplugin` 라고 이름을 지정하고 있습니다. 그리고 다양한 빌드 스크립트를 `npm` 으로 실행할 수 있도록 하였습니다. 
`npm run render` 를 실행하면 `skpm` 으로 빌드한 후 Sketch에 랜더링 할 것이고, 소스가 수정되면 자동으로 Sketch 가 리로드될 것입니다. 

마지막으로 `manifest` 로 지정된 파일을 통해서 실제로 어떤 파일이 실행 되어 랜더링 될 것인지 설정해야 할 것입니다. 
`src/manifest.json` 의 일부를 보면, 최초 `my-command.js` 파일이 실행되도록 되어 있습니다.
```
"commands": [{
  "name": "react-sketchapp: Basic Setup",
  "identifier": "main",
  "script": "./my-command.js"
}]
```

실제로 Sketch 에 랜더링 되는 부분은 `my-command.js` 파일이며, `react-sketchapp` 에서 제공하는 컴포넌트들을 사용하여 구현한 디자인이 Sketch에 랜더링 될 것입니다. 

이제, 실행해보도록 합니다. 

실행하기에 앞서 Sketch 어플리케이션을 실행한 후 New Document 를 열어두어야 합니다. 
```
$ npm install
$ npm run render
```

![example image](https://cloud.githubusercontent.com/assets/591643/24777148/e742cd0e-1ad8-11e7-8751-090f6b2db514.png)


그 외 다양한 예제 파일(https://github.com/airbnb/react-sketchapp/tree/master/examples)이 있으며, 하나씩 실행해 볼 것을 권장합니다.
