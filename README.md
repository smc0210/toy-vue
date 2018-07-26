# Vue.js

- [Vue.js](#vuejs)
  - [1. Prepare before](#1-prepare-before)
  - [2. Getting Start](#2-getting-start)
  - [3. Life Cycle](#3-life-cycle)
  - [4. Component](#4-component)
  - [5. Route](#5-route)
  - [6. Template](#6-template)


## 1. Prepare before

1. `Chrome broswer`
2. `Atom` or etc `IDE Tools`
3. `Node.js`
4. Vue.js devtools - Chrome extension

> 확장프로그램 세부설정에서 파일 URL에 대한 액세스 허용을 체크 해줘야 file:// 경로도 개발자도구에서 인식


`Atom` 추천 테마, 패키지
- `seti-ui`
- `atom-material-syntax-dark`
- `language-vue`

## 2. Getting Start

```xml
<html>
  <head>
    <title>Vue Sample</title>
  </head>
  <body>
    <div id="app">
      {{ message }}
    </div>

    <script src="https://cdn.jsdelivr.net/npm/vue@2.5.2/dist/vue.js"></script>
    <script>
      new Vue({
        el: '#app',
        data: {
          message: 'Hello Vue.js!'
        }
      });
    </script>
  </body>
</html>
```

## 3. Life Cycle

## 4. Component

## 5. Route

## 6. Template








