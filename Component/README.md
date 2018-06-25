# Component

## TOC
- [1. `beforeCreate`](#1-beforecreate)


## Contents

조합하여 화면을 구성할 수 있는 블록(화면의 특정영역)을 의미
화면을 구조화 하여 일괄적인 패턴으로 개발

### 1. Global & Local Component

- 전역 컴포넌트는 뷰로 접근 가능한 모든 범위에서 사용가능하다.
- 지역 컴포넌트는 특정 인스턴스에서만 유효한 범위를 갖는다.

#### 컴포넌트 등록

```xml
<html>
  <head>
    <title>Vue Local and Global Components</title>
  </head>
  <body>
    <div id="app">
      <h3>첫 번째 인스턴스 영역</h3>
      <my-global-component></my-global-component>
      <my-local-component></my-local-component>
    </div>
    <hr>
    <div id="app2">
      <h3>두 번째 인스턴스 영역</h3>
      <my-global-component></my-global-component>
      <my-local-component></my-local-component>
    </div>

    <script src="https://cdn.jsdelivr.net/npm/vue@2.5.2/dist/vue.js"></script>
    <script>
      // 전역 컴포넌트 등록
      Vue.component('my-global-component', {
        template: '<div>전역 컴포넌트 입니다.</div>'
      });
      // 지역 컴포넌트 내용
      var cmp = {
        template: '<div>지역 컴포넌트 입니다.</div>'
      };
      new Vue({
        el: '#app',
        // 지역 컴포넌트 등록
        components: {
          'my-local-component': cmp
        }
      });
      // 두 번째 인스턴스
      new Vue({
        el: '#app2'
      });
    </script>
  </body>
</html>

```

### 2. Global & Local Component