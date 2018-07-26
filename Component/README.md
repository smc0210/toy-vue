# Component

## TOC
- [1. Global & Local Component](#1-global---local-component)
- [2. Component](#2-component)


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

### 2. Component

뷰는 기본적으로 단방향 데이터 흐름을 가지기때ㅔ문에 공식사이트에서도 하위에서 상위로 데이터를 전달하는 방법을 다루지 않지만
복잡한 뷰 애플리케이션을 구축할때 이벤트 버스(Event Bus)를 이용하여 하위 -> 상위 컴포넌트로 이벤트 인자를 통해 데이터를 전달하는 방법이 있긴하다.

#### `props`를 이용한 상위컴포넌트 -> 하위 컴포넌트로 데이터 전달

```xml
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vue Props Sample</title>
  </head>
  <body>
    <div id="app">
      <!-- 팁 : 오른쪽에서 왼쪽으로 속성을 읽으면 더 수월합니다. -->
      <!-- <child-component v-bind:propsdata="message"></child-component> -->
      <child-component v-bind:propsdata="message"></child-component>
    </div>

    <script src="https://cdn.jsdelivr.net/npm/vue@2.5.2/dist/vue.js"></script>
    <script>
      // Vue.component('child-component', {
      //   props: ['propsdata'],
      //   template: '<p>{{ propsdata }}</p>',
      // });
      Vue.component('child-component',{
          props: ['propsdata'],
          template: '<p>{{ propsdata }}</p>',
      });
      new Vue({
        el: '#app',
        data: {
          message: 'Hello Vue! passed from Parent Component'
        }
      });
    </script>
  </body>
</html>

```


#### 하위-> 상위 컴포넌트로 이벤트 전달하기

이벤트 발생과 수신은 `$emit()`과 `v-on`속성을 사용하여 구현한다.

**이벤트 발생**
```javascript
this.$emit('이벤트명');
```

`$emit('이벤트명')`을 호출하면 괄호안에 정의된 이벤트가 발생하며, 일반적으로 `$emit()`을 호출하는 위치는 하위 컴포넌트의 특정 메서드 내부이다.
따라서 위 코드의 `this`는 하위 컴포넌트를 가리킨다.

**이벤트 수신**
```xml
<chid-component v-on:이벤트명="상위컴포넌트의 메소드명"></child-component>
```
호출한 이벤트는 하위 컴포넌트를 등록하는 태그에서 `v-on:`으로 받는다.


```xml
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vue Event Emit Sample</title>
  </head>
  <body>
    <div id="app">
      <child-component v-on:show-log="printText"></child-component>
    </div>

    <script src="https://cdn.jsdelivr.net/npm/vue@2.5.2/dist/vue.js"></script>
    <script>
      Vue.component('child-component', {
        template: '<button v-on:click="showLog">show</button>',
        methods: {
          showLog: function() {
            this.$emit('show-log');
          }
        }
      });
      new Vue({
        el: '#app',
        data: {
          message: 'Hello Vue! passed from Parent Component'
        },
        methods: {
          printText: function() {
            console.log("received an event");
          }
        }
      });
    </script>
  </body>
</html>
```


#### 같은 레벨의 컴포넌트 간 통신 - 이벤트 버스

뷰는 상위에서 하위로만 데이터를 전달해야 하는 기본적인 통신 규칙을 따르기 때문에 형제 관계의 컴포넌트에 값을 전달하려면,
공통 상위 컴포넌트로 이벤트를 전달한 후, 상위 컴포넌트에서 2개의 하위 컴포넌트에 `props`를 내려 보내야 한다.

이런 통신 구조를 유지하려면 상위컴포넌트가 필요없음에도 같은 레벨간 통신을 위해 상위 컴포넌트를 두어야 하는데
이를 해결할 수 있는 방법이 **이벤트 버스**다

```xml
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vue Event Bus Sample</title>
  </head>
  <body>
    <div id="app">
      <child-component></child-component>
    </div>

    <script src="https://cdn.jsdelivr.net/npm/vue@2.5.2/dist/vue.js"></script>
    <script>
      //로직을 담는 인스턴스와는 별개로 이벤트 버스를 위한 추가 인스턴스 생성
      var eventBus = new Vue();

      Vue.component('child-component', {
        template: '<div>하위 컴포넌트 영역입니다.<button v-on:click="showLog">show</button></div>',
        // 이벤트를 보내는 컴포넌트
        methods: {
          showLog: function() {
            eventBus.$emit('triggerEventBus', 100);
          }
        }
      });
      var app = new Vue({
        el: '#app',
        // 이벤트를 받는 컴포넌트
        created: function() {
          eventBus.$on('triggerEventBus', function(value){
            console.log("이벤트를 전달 받음. 전달 받은 값 : ", value);
          });
        }
      });
    </script>
  </body>
</html>

```
