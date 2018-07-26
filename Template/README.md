# Template

- [Template](#template)
        - [1. Template?](#1-template)
        - [2. Data Binding](#2-data-binding)
        - [3. JavaScript expressions](#3-javascript-expressions)
       
### 1. Template?

뷰 인스턴스에서 정의한 데이터 및 로직들을 연결하여 사용자가 브라우저에서 볼 수 있는 형태의 HTML로 변환해 주는 속성

템플릿 속성을 사용하는 방법 두 가지
- 1. 뷰 인스턴스의 `template` 속성을 활용하는 방법
- 2. 싱글파일 컴포넌트 체계의 `<template>`코드를 활용하는 방법

1번 방법이 지금까지 예제에서 사용했던 방법이다.

템플릿에서 사용하는 뷰의 속성과 문법
- 데이터 바인딩
- 자바스크립트 표현식
- 디렉티브
- 이벤트 처리
- 고급 템플릿 기법

### 2. Data Binding

HTML 화면 요소를 뷰인스턴스의 데이터와 연결하는 것을 의미하며 주요 문법으로는 `{{}}`문법과 `v-bind`가 있다.


**`{{}}` - 콧수염 괄호**
```xml
<div id="app">
    {{ message }}
</div>

<script>
    new Vue({
        el: '#app',
        data: {
            message: 'Hello Vue.js'
        }
    });
</script>
```
상기 코드에서 만약 data 속성의 message 값이 바뀌면 뷰 반응성에 의해 화면이 자동으로 갱신된다.

뷰 데이터가 변경되어도 값을 바꾸고 싶지 않다면 `v-once`속성을 사용한다.

```xml
<div id="app" v-once>
    {{ message }}
</div>
```

**v-bind**

아이디,클래스,스타일 등의 HTML 속성(`attribute`)값에 뷰 데이터 갑사을 연결할 때 사용하는 데이터 연결 방식

```xml
<html>
  <head>
    <title>Vue Template - Data Binding</title>
  </head>
  <body>
    <div id="app">
      <p v-bind:id="idA">아이디 바인딩</p>
      <p v-bind:class="classA">클래스 바인딩</p>
      <p v-bind:style="styleA">스타일 바인딩</p>
    </div>
    <script src="https://cdn.jsdelivr.net/npm/vue@2.5.2/dist/vue.js"></script>
    <script>
      new Vue({
        el: '#app',
        data: {
          idA: 10,
          classA: 'container',
          styleA: 'color: blue'
        }
      });
    </script>
  </body>
</html>
```

### 3. JavaScript expressions 

