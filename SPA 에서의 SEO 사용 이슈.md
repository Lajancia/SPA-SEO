
# **SPA 에서의 SEO 사용 이슈** #

## **Single - SPA 에서 SEO 사용이 가능한가?** ##

***결론적으로 불가능하지 않다. 다만 사용이 어렵다.***

</br>

## **SPA와 SEO** ##

> 두 개를 동시에 사용하는 것이 왜 어려운지에 관하여 이해하기 위해서는, 먼저 핵심이 되는 CSR과 SSR에 대하여 이해할 필요가 있다.

</br>

### **CSR(Client Side Rendering)** ###
![CSR 동작](https://media.vlpt.us/images/hsngju/post/dc611718-4ca5-4cc8-80fd-09b58aba365b/image.png)
* 일반적인 경우, SPA는 CSR을 기반으로 동작한다.
* 동적으로 만들어진 곳이 Client side 인 형태일 경우를 CSR이라고 한다.
* Static한 파일들을 먼저 받아오고, 데이터가 없는 빈 html을 받아온 뒤, 이후에 데이터를 요청해서 받아오는 방식이다. 이로 인해 동적으로 빠르게 랜더링 할 수 있어 UX가 뛰어나다.
* 서버에 요청하는 횟수가 훨씬 적어 서버 부담이 덜하다.

</br>

### **CSR의 단점** ###
* 모든 static파일이 로드될 때 까지 기다려야 한다.
* 검색 엔진이 크롤링을 하는 것에 어려움을 겪어, SEO 문제가 발생할 수 있다. (구글 검색엔진 제외)

</br>

### **SSR(Server Side Rendering)** ###
![SSR 동작](https://media.vlpt.us/images/hsngju/post/3f88af15-c76d-4609-a6e0-af8b30e5f473/image.png)
* 서버에서 메인페이지를 요청하면, 완성된 html을 응답하는 형태이다.
* 초기 렌더링 속도가 빠르다.
* JS를 이용한 렌더링이 아니고, html에 모든 컨텐츠가 저장되어 있어 SEO(검색 엔진 최적화)가 가능하다. 

</br>

### **SSR의 단점** ###
* 새로운 페이지를 요청할 때 마다 새고로침이 발생한다.
* 데이터 수정을 위해서는 서버를 거쳐야한다.
* 매번 서버에 요청을 하는 행위는 서버에 과부하의 원인이 될 수 있다.

</br>

>기본적으로 CSR을 활용한 자바스크립트 베이스의 SPA에서는 SEO가 원활히 동작하지 못한다.

>예외적으로 구글 검색 엔진은 자바스크립트를 크롤링할 수 있지만, 대부분의 검색 엔진은 불가능하다.

</br>

## **SSR 기반 SPA** ##

SSR에서 SPA를 동작시키기 위해서는 SPA가 virtual DOM에서 동작할 필요가 있다. 이 가상 DOM은 html string으로 변환된다. 이것은 엔드 유저에 보내지기 전에 페이지에 주입된다. 페이지가 엔드 유저에 다다르면, SPA가 자바스크립트를 실행시키고 매끄럽게 기존의 존재하는 컨텐츠를 대체할 것이다. SSR은 크롤러가 자바스크립트를 이용하냐의 여부에 관계없이 SEO를 사용하기 좋게 해준다.

문제는 SSR의 몇가지 단점이다.
* 사용하는 spa는 서버 기반 자바스크립트 환경이어야 한다.
* SSR 적용 자체가 복잡하다.
* SPA가 서버에서 더 많은 요청을 수행해야 한다. 조금 더 느려질 가능성이 있다. 캐싱을 사용하여 조금 완화시킬 수 있는 부분이다.
SSR은 node.js 백엔드에서만 동작한다. 
* php 확장을 통해 Node.js가 아닌 백엔드를 활용할 수 있지만, 매우 제한적이다.

</br>

## **Vue.js기반 SPA에서의 SSR 적용 방법** ##


### **spa에 바로 ssr 적용하기** ###
SSR을 적용시킨다는 것은 새로운 서비스 레이어를 프론트 엔드 클러스터에 추가하는 것과 같다. 이것의 위치는 프록시 서버의 바로 뒤에 위치할 것이며, REST API 바로 앞에 위치한다. 이 때문에 아키텍쳐가 비교적 복잡해지고 관리에 있어서 시간을 잡아먹게 한다.

보통 SSR은 node.js를 이용한다. 이는 SSR의 자체적인 기능에 복잡한 시나리오에 대한 계산 퍼포먼스가 부족하기 때문이다. 때문에 공평한 UI를 제공하고 싶다면, 그에 맞춰 추가적인 비용이 발생한다.

그러나 이 문제를 해결한다 해도, 우리에게는 여전히 의문이 남아있다. 바로 3rd party libraries를 handling하는 것이다. 이것은 개발에 있어 지옥이 될 수도 있다. 조금의 실수가 ux 전반에 큰 데미지를 줄 수 있기 때문이다.

</br>

### **SSR 사용을 위한 Vue 커뮤니티 라이브러리** ### 
* nuxt.js: 꽤 유명한 풀스텍 프레임워크이다. 리액트의 next.js와 유사하다. ismorphic 아키텍쳐를 클라이언트와 서버 사이드에 지원한다. 

* vapper.js : ssr에 포커싱한다.

* ream: 넉스트의 대체가 될 수 있지만, 더 많은 커스터마이징이 가능하다.
* vueniverse: 또하나의 풀스텍 프레임워크이다.
* ves: 풀 스택 프레임워크이다. egg.js를 베이스로 하는 ssr이다.

>SSR을 이용한 새로운 프로젝트를 시작한다면, nuxt.js 혹은 next.js를 이용하는 것을 권장한다. 왜냐하면 더 많은 커뮤니티가 형성되어 있고, 더 오랫동안 개발되어왔기 때문이다. 

>결론적으로 ssr은 spa의 결점을 다룰 수 있게 한다. 이를 적용시키기 전에, 적용을 반드시 해야하는가에 대한 여부를 신중히 하도록 해야 한다.

</br>


### **ragu server를 이용한 SSR 적용** ###
공식 홈페이지: https://ragu-framework.github.io/#!

사용 방법 참고: https://maniero.medium.com/transforming-your-vue-js-project-into-micro-frontends-with-server-side-rendering-9cdaab16f5e9
![ragu server](https://miro.medium.com/max/2000/1*Xkz-wp3OEsTTBrVwzHfMfw.png)

* Vue.js 기반으로 개발 가능하다.
* 기존의 Vue.js프로젝트를 SSR로 변환 가능하다.
* Single-SPA를 이용하지 않고 SSR 기반의 Micro-Frontend 프레임워크를 제공한다.

</br>

**이슈사항**
>기존의 프로젝트 진행을 위해 사용한 Single-SPA를 활용할 이유가 사라짐 

</br>

### **Nuxt.js Universal 모드를 이용한 SSR 적용** ###
![nuxt.js universal mode](https://miro.medium.com/max/1400/1*mLL-1t7_0Ne3JN6On0t_vA.png)
* nuxtServerInit -> middleware -> validate -> asyncData -> fetch 과정을 거쳐서 렌더링한 뒤 완성된 페이지를 리스폰스로 보낸다.
* 이후에는 페이지 이동은 갱신 없이 클리아언트에서 nuxt-link를 통해 필요한 리소스만 ajax 통신으로 받아서 렌더링한다.
* nuxt.js에서 제공하는 universal 모드를 사용하여 Isomorphic application (Server Side Rendering + Client Side Navigation)을 사용할 수 있다.
* 가장 보편적인 방법이다.



**이슈 사항**
>nuxt에서의 single-spa가 universal mode와 호환 가능한지 불분명하다.

</br>



## **Single-SPA에서의 SSR 적용** ##

### **ssr implement 목적** ###
가장 첫번째 목적은 퍼포먼스를 개선하는 것에 있다. 서버 렌더링에 있어서 기존의 방식보다 훨씬 빨리 유저에게 컨텐츠를 디스플레이 할 수 있으며, 또 다른 이유로는 ssr이 개선된 seo를 포함하고 있다는 것이다.

>ssr은 기본적으로 개발하는 것과 유지 하는 것이 어렵다. 클라이언트와 서버 두 사이드에서 동작해야 하기 때문이다. 추가적으로 ssr은 당신의 어플리케이션을 동작하는데 기반이 되는 것들에 혼선을 줄 수 있다. 왜냐하면 기존의 csr spa에서 요구하지 않던  node js를 많은 spa+ssr 솔루션이 필요로 하기 때문이다.


</br>

### **Single-SPA에서 SSR implement를 위해 해야 하는 것** ###
* layout - 주로 route를 기반으로 한다. 어떤 http request를 렌더링할지 구별하고 어떤 html에 배치할지 구별하는 역할을 수행한다.
* fetch -각각의 html을 렌더링한다.
* headers - 마이크로프론트엔드에서 http request를 추출한다. http 응답 헤더의 형태로 병합하여 결과를 브라우저로 보낸다.
* body - http 응답 body를 브라우저로 보낸다. 다음 html을 수행할 때 까지의 웨이팅을 불러일으킬 수 있다.
* hydrate - 브라우저 내에서 모든 필요한 자바스크립트를 다운로드 하고 html을 state로 읽어들인다.

</br>


## **Single-SPA Ismorphic Microfrontedn app 예제** ##

Github: https://github.com/isomorphic-microfrontends

프로젝트 동작: https://isomorphic.microfrontends.app/

* REACT를 기반으로 한 Single-SPA 예제이다.

</br>

### **root-config 코드 분석** ###

![root-config](https://user-images.githubusercontent.com/50996139/116437952-ed964900-a888-11eb-8b2c-feb61df819b0.jpg)



</br>

**/server**

* server.js : 포트 번호 지정
* index.html : module loading 사용
* static.js : application을 static으로 등록
* app.js : express()으로 app 설정

</br>

**/views**
* index.html : 화면에 나타날 어플리케이션 등록

</br>

**/browser**
* root-config.js : single-layout 등록

</br>

**root-config**
* webpack.config.cjs : 다른 파일의 webpack.config.js와 다른 파일. 웹팩 파일로 변경할 시 오류 발생
* node-loader.config.js : @node-loader/import-maps 와 @node-loader/http 사용. nodejs에서의 fetch 단계의 동적 모듈 로딩을 가능하게 하기 위한 것이다.

</br>


### **Single-SPA의 SSR Implementation 핵심 코드** ##

**index.html**
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>Isomorphic Microfrontends</title>
    <meta
      name="importmap-type"
      content="systemjs-importmap"
      server-cookie
      server-only
    />
    <script src="https://cdn.jsdelivr.net/npm/import-map-overrides@2.0.0/dist/import-map-overrides.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/systemjs@6.6.1/dist/system.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/systemjs@6.6.1/dist/extras/amd.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/systemjs@6.6.1/dist/extras/named-exports.min.js"></script>
    <link
      rel="stylesheet"
      href="https://fonts.googleapis.com/css?family=Roboto:300,400,500,700&display=swap"
    />
    <assets></assets>
  </head>
  <body>
    <template id="single-spa-layout">
      <single-spa-router>
        <application name="@isomorphic-mf/navbar" props="user"></application>
        <route path="pokemons">
          <application
            name="@isomorphic-mf/pokemons"
            props="user"
          ></application>
        </route>
      </single-spa-router>
    </template>
    <fragment name="importmap"></fragment>
    <script>
      System.import("@isomorphic-mf/root-config");
    </script>
    <import-map-overrides-full
      show-when-local-storage="devtools"
      dev-libs
    ></import-map-overrides-full>
  </body>
</html>

````

1. Layout : /server/views/index.html에 존재. single-spa-layout을 통해 모든 경로를 핸들링할 하나의 템플릿을 정의한다. 이때 사용되는 single-spa-layout은 single-spa 공식 레이아웃 엔진이다.

</br>

```javascript

import { app } from "./app.js";
import {
  constructServerLayout,
  sendLayoutHTTPResponse,
} from "single-spa-layout/server";
import _ from "lodash";
import { getImportMaps } from "single-spa-web-server-utils";

const serverLayout = constructServerLayout({
  filePath: "server/views/index.html",
});


app.use("*", (req, res, next) => {
  const developmentMode = process.env.NODE_ENV === "development";
  const importSuffix = developmentMode ? `?ts=${Date.now()}` : "";

  const importMapsPromise = getImportMaps({
    url:
      "https://storage.googleapis.com/isomorphic.microfrontends.app/importmap.json",
    nodeKeyFilter(importMapKey) {
      return importMapKey.startsWith("@isomorphic-mf");
    },
    req,
    allowOverrides: true,
  }).then(({ nodeImportMap, browserImportMap }) => {
    global.nodeLoader.setImportMapPromise(Promise.resolve(nodeImportMap));
    if (developmentMode) {
      browserImportMap.imports["@isomorphic-mf/root-config"] =
        "http://localhost:9876/isomorphic-mf-root-config.js";
      browserImportMap.imports["@isomorphic-mf/root-config/"] =
        "http://localhost:9876/";
    }
    return { nodeImportMap, browserImportMap };
  });

  const props = {
    user: {
      id: 1,
      name: "Test User",
    },
  };

  const fragments = {
    importmap: async () => {
      const { browserImportMap } = await importMapsPromise;
      return `<script type="systemjs-importmap">${JSON.stringify(
        browserImportMap,
        null,
        2
      )}</script>`;
    },
  };

  const renderFragment = (name) => fragments[name]();
////
///이 아래부터 동일
  sendLayoutHTTPResponse({
    serverLayout,
    urlPath: req.originalUrl,
    res,
    renderFragment,
    async renderApplication({ appName, propsPromise }) {
      await importMapsPromise;
      const [app, props] = await Promise.all([
        import(appName + `/server.mjs${importSuffix}`),
        propsPromise,
      ]);
      return app.serverRender(props);
    },
    async retrieveApplicationHeaders({ appName, propsPromise }) {///
      await importMapsPromise;
      const [app, props] = await Promise.all([
        import(appName + `/server.mjs${importSuffix}`),
        propsPromise,
      ]);
      return app.getResponseHeaders(props);
    },
    async retrieveProp(propName) {/////
      return props[propName];
    },
    assembleFinalHeaders(allHeaders) {/////
      return Object.assign({}, Object.values(allHeaders));
    },
  })
    .then(next)
    .catch((err) => {
      console.error(err);
      res.status(500).send("A server error occurred");
    });
});

````


2. Fetch : /server/index-html.js 파일에 존재한다. single-spa-layout을 사용할 때, renderApplication 함수가 마이크로프론트엔드를 패치한다. 이는 renderServerResponseBody에 제공된다. module loading 방식과 http request 방식 두 가지가 존재하는데, 보통의 경우에는 적은 인프라가 필요하고 더 나은 성능을 가진 module loading 방식을 선호한다.

3. HTTP Response Headers : 브라우저로 전송되는 HTTP 응답 헤더는 각각의 마이크로프론트엔드에서 추출된 디폴트 헤더와 헤더의 결합이다.  Single-spa-layoyt에서는 assemvleFinalHeaders 옵션으로 통해 맞춤 병합을 허용한다.

4. HTTP Response Body : 성능을 극대화 하기 위해서, 웹 서버에서 브라우저로 전송된 HTTP 응답을 바이트 단위로 스트리밍해야 한다. 언급된 모든 마이크로 프론트 엔드 레이아웃 미들웨어는 HTML 응답 본문을 브라우저로 스트리밍한다. single-spa-layout의 경우, sendLayoutHTTPResponse를 불러 동작한다.

</br>

```javascript
import "./set-public-path";
import React from "react";
import ReactDOM from "react-dom";
import singleSpaReact from "single-spa-react";
import Root from "./root.component";

const lifecycles = singleSpaReact({
  React,
  ReactDOM,
  rootComponent: Root,
  renderType: "hydrate"
});

export const bootstrap = lifecycles.bootstrap;
export const mount = lifecycles.mount;
export const unmount = lifecycles.unmount;
````

5. Hydrate : Hydration은 자바스크립트 초기화와 서버에서 전달받은 이벤트 리스너를 HTML에 적용하는 것을 말한다. single-spa-layout의 역할은 어떤 마이크로 프론트엔드가 어떤 부분의 DOM을 hydrate 할 지를 결정하는 것이다. 이는 contructLayoutEngine 과 singleSpa.start()를 통해 자동으로 수행된다. 위의 코드는 react의 hydrate 옵션을 적용한 것이다.

</br>

**이슈 사항**
>현재까지로는 React를 기반으로 한 예제밖에 제공하지 않는다. 따라서 vue.js를 기반으로 하여 SSR을 적용하기 어렵다.

>root-config 파일은 다른 navbar와 pokemons 파일이 존재하지 않아도 서버가 동작할 시 화면이 정상적으로 출력된다. 로컬에서 파일을 받아 올리는 것이 아니라, 외부의 파일을 받아오는 것으로 추측한다.

>vue에서는 hydrate가 어떻게 정의되는지에 대한 언급이 없다.


</br>






</br>

### **참고** ##
> CSR VS SSR https://velog.io/@junghyunhao/SPA-MPA-CSR-SSR-%EC%95%8C%EC%95%84%EB%B3%B4%EA%B8%B0

> SPA SSR https://velog.io/@namezin/CSR-SSR

> vue.js 기반 ssr 사용법 https://medium.com/weekly-webtips/server-side-rendering-ssr-for-an-spa-project-a3b4e846abcd

>nuxt.js spa 사용 https://medium.com/aha-official/%EC%95%84%ED%95%98-%ED%94%84%EB%A1%A0%ED%8A%B8-%EA%B0%9C%EB%B0%9C%EA%B8%B0-1-spa%EC%99%80-ssr%EC%9D%98-%EC%9E%A5%EB%8B%A8%EC%A0%90-%EA%B7%B8%EB%A6%AC%EA%B3%A0-nuxt-js-cafdc3ac2053

