# Angular에서 Lazy-Loading 사용하기

Lazy-Loading은 비동기식 라우팅이 가능하도록 앵귤러에서 제공하는 기능이다. 
Lazy-Loading을 통해 초기 로딩 속도를 개선할 수 있다. 특히 앱에 많은 컴포넌트가 있어서 복잡한 라우팅 작업을 있을 경우에!
Lazy-Loading을 적용하는 간단한 앱을 만들어보았다.
https://angularfirebase.com/lessons/how-to-lazy-load-components-in-angular-4-in-three-steps/ 을 참고했다.

<br>
## 라우팅 기능이 있는 앱을 하나 만들자

root URL에서 기본적으로 AppComponent를 로드하는데, 사용자가 lazy/load-me 로 이동했을 때 lazy module이 비동기로 로딩되도록 하려고 한다. 

```
ng new lazyDemo --routing
```

app component의 버튼에 lazy/load-me로 가는 링크를 달아준다.

```bash
<button routerLink="/lazy/load-me"></button>
<router-outlet></router-outlet>
```
<br>
## 그리고, Lazy Module을 만들자.

```
ng g module lazy --flat
ng g component lazy-parent --module lazy
ng g component lazy-child --module lazy
```

`--flat`이라는 flag를 붙이면 새로운 폴더가 생성되지 않고 바로 root 위치에 생성되도록 한다.
그리고 컴포넌트를 만들 때, `--module lazy` flag를 붙이면 자동으로 lazy module에 선언된다.

Lazy Module에 `RouterModule`을 import를 하고
1. route path를 `path: 'load-me'`
2. RouterModule.forChild(routes)를 @NgModule({})의 메타데이터로 추가한다.

<br>
## 앱 라우터에게 LazyModule을 로딩하라고 알려주자

RoutingModule에 route path에 `loadChildren` 프로퍼티를 추가한다.
\#를 사용해서 앵귤러에게 `/lazy` url에 진입했을 때 LazyModule을 로딩하라고 알려준다.

```bash
const routes: Routes = [
    { path: 'lazy', loadChildren: './lazy.module#LazyModule'}
];
```

<br>
## Lazy-Loading이 동작 확인하기

크롬 개발자 도구에서 network 탭을 클릭해서 확인할 수 있다.

lazy url에 접근했을 때, chunk.js파일이 새롭게 로딩되는 것을 확인할 수 있다.