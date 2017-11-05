# Hacker News 어플리케이션 개발 사례

실제 react-sketchapp 사용의 이해를 돕기 위하여 Hacker News 모바일 어플리케이션 개발을 위한 디자인 프로젝트를 개발해 보았습니다.

Hacker News 모바일 어플리케이션은 Hacker News API 서버(https://github.com/HackerNews/API) 를 기반으로 한 모바일 앱이며, react-sketchapp 기반의 디자인 프로젝트와 모바일 어플리케이션 프로젝트로 분리되어 github 에 공개(또는 예정) 되어 있습니다. 

 * Hacker News Design : https://github.com/NewsPlatform/HackerNews-design
 * Hacker News Mobile Application : https://github.com/NewsPlatform/HackerNews-design


### Hacker News Design

이 프로젝트 (https://github.com/NewsPlatform/HackerNews-design) 는 `react-sketchapp` 으로 디자인 가이드와 화면을 디자인 한 프로젝트 입니다. 

실제 프로젝트를 진행하다 보면, `react-sketchapp` 는 디자인을 React 코드로 개발하는데 훌륭한 도구이긴 하지만, 몇가지 제약사항들이 존재 합니다. 물론 향후 개발자들의 기여를 통해 개선 될 것으로 기대하고 있습니다.

제약사항중 중요한 두가지 정도만 요약하면 아래와 같습니다.

 * SVG 사용 - react-sketchapp 에서 제공하는 화면 그리기위한 Component 는 View, Text 그리고 Image 정도이고 SVG 를 그릴 수 있는 방법은 아직까지 없습니다. 모바일 어플리케이션 개발을 위해서는 아이콘을 SVG 로 그려 사용하는 경우가 많지만, react-sketchapp 에서는 아직까지 SVG를 위한 컴포넌트를 제공하지 않기 때문에 이미지 파일로 변환하여 Image 컴포넌트를 사용할 수 밖에 없습니다. 

 * Local 이미지 로딩 - react-sketchapp 의 Image 컴포넌트는 Local 디렉토리의 이미지를 로딩할 수 없고 uri로 이미지를 로딩해야만 합나다. Local 이미지를 사용하기 위해서는 localhost 로 접속할 수 있도록 웹서버를 실행해서 해결하기도 합니다.

이제 다운로드 받아 실행해보도록 하겠습니다. 

실행하기에 앞서 Sketch 어플리케이션을 실행한 후 New Document 를 열어두어야 합니다. 
```
$ git clone https://github.com/NewsPlatform/HackerNews-design
$ cd HackerNews-design
$ npm install
$ npm start
```

앞서 간단한 예제 실행에서는 `npm run render` 를 실행하였지만, 이 프로젝트는 로컬 폴더의 이미지를 사용하기 위하여 별도의 `start` 스크립트를 `package.json` 에 추가하여 사용하였습니다.

`package.json` 파일의 스크립트 부분을 보면, 아래와 같습니다. 
```
  "scripts": {
    "start": "concurrently \"npm run serve\" \"skpm build --watch --run\"",
    "serve": "serve resources -n --port 9000 --silent",
     . . . . . . .
  },
```
여러 명령어를 동시에 실행할 수 있는 `concurrently` 모듈로 로컬 웹서버와 skpm 으로 빌드 및 랜더링을 동시에 실행하도록 한 것을 확인 할 수 있습니다.


### Hacker News Mobile 어플리케이션

이 프로젝트(https://github.com/NewsPlatform/HackerNews-mobile) 는  React Native(https://facebook.github.io/react-native/) 를 기반으로한 모바일 어플리케이션입니다. 물론 앞서 설명한 디자인의 스타일 요소는 그대로 사용하여 동작하도록 되어 있습니다. 

expo(https://expo.io) 를 통해 쉽고 빠르게 빌드 및 테스트 가능합니다.

```
$ git clone https://github.com/NewsPlatform/HackerNews-mobile
$ cd HackerNews-mobile
$ npm install
$ npm start
```

화면의 스타일을 정의한 파일은 `styles/style.js` 입니다. 여기에 정의된 스타일은 react-sketchapp 기반의 Design System 에서 작성된 파일과 동일합니다. 

그리고 `screens` 폴더 안의 화면과, `components` 폴더 안의 재사용 가능한 공통 컴포넌트들 역시 Design System 의 파일과 서로 매칭되도록 개발되었습니다. 만약 디자인이 변경된다면, Design System 의 파일과 본 어플리케이션 프로젝트의 파일이 함께 변경되어 관리되어야 할 것입니다.


# 마지막으로

Design System 를 구성할 수 있는 수 많은 방법 중 하나로`react-sketchapp` 을 소개한 것이므로 당연히 더 좋은 방법이 있을 수 있고, 지금도 수많은 시도가 진행되고 있습니다. 

Design System 은 스타일 가이드나 UIKit 이상으로, Production 수준에 가까운 기획, 디자인 그리고 개발자간의 커뮤니케이션과 협업을 위하여 분명 중요한 역할을 할 것입니다. 

우려되는 점은, `react-sketchapp` 를 통한 Design System 구축을 위해 React 기반의 JavaScript 코딩 역량과 React 에 대한 기본적인 지식이 반드시 필요할 것이기 때문에, 디자이너에게는 어느정도의 부담이 될 수도 있겠습니다. 

마지막으로 강조하고 싶은 것은, Design System 은 단지 디자이너만이 관련되어야 하는 것이 아니란 점입니다. 실제 데이터를 사용해서 디자인에 적용해보거나, 개발시 컨포넌트화 하는 단위도 Design System과 동일하게 맞추기 위해서는 디자이너와 개발자 모두가 협의를 통해 함께 구축해야만 할 것입니다.