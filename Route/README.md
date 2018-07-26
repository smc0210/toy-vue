# Route

## TOC
- [1. `beforeCreate`](#1-beforecreate)


## Contents


### 1. Vue Router

뷰 라우터는 라우팅 기능을 위한 공식 라이브러리


- `<router-link to="URL">` : 페이지 이동 태그. 화면에는 `<a>`태그로 표시됨
- `<router-view>` : 페이지 표시 태그. URL에 따라 해당 컴포넌트를 뿌려주는 영역

```xml
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vue Router Sample</title>
  </head>
  <body>
    <div id="app">
      <h1>뷰 라우터 예제</h1>
      <p>
          <router-link to="/main">Main 컴포넌트로 이동</router-link>
          <router-link to="/login">Login 컴포넌트로 이동</router-link>
      </p>
      <router-view></router-view>
    </div>

    <script src="https://cdn.jsdelivr.net/npm/vue@2.5.2/dist/vue.js"></script>
    <script src="https://unpkg.com/vue-router@3.0.1/dist/vue-router.js"></script>
    <script>
        var Main = { template: '<div>main</div>' };
        var Login = { template: '<div>login</div>' };

        var routes = [
            { path: '/main', component: Main },
            { path: '/login', component: Login }
        ];

        var router = new VueRouter({
            routes
        });

        var app  = new Vue({
            router
        }).$mount('#app');
    </script>
  </body>
</html>

```


### 2. Nested Router
상위 컴포넌트 1개에 하위컴포넌트 1개를 포함하는 구조로 사용

```xml
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vue Nested Router</title>
  </head>
  <body>
    <div id="app">
      <router-view></router-view>
    </div>

    <script src="https://cdn.jsdelivr.net/npm/vue@2.5.2/dist/vue.js"></script>
    <script src="https://unpkg.com/vue-router@3.0.1/dist/vue-router.js"></script>
    <script>
      var User = {
        template: `
          <div>
            User Component
            <router-view></router-view>
          </div>
        `
      };
      var UserProfile = { template: '<p>User Profile Component</p>' };
      var UserPost = { template: '<p>User Post Component</p>' };
      var routes = [
        {
          path: '/user',
          component: User,
          children: [
            {
              path: 'posts',
              component: UserPost
            },
            {
              path: 'profile',
              component: UserProfile
            },
          ]
        }
      ];
      var router = new VueRouter({
        routes
      });
      var app = new Vue({
        router
      }).$mount('#app');
    </script>
  </body>
</html>

```

### 2. Named View

특정 페이지로 이동했을 대 여러 개의 컴포넌트를 동시에 표시하는 라우팅 방식
네스티드 라우터와의 차이점은 네스티드 라우터는 상위컴포넌트가 하위 컴포넌트를 포함하는 방식이지만
네임드 뷰는 같은 레벨의 여러 개의 컴포넌트를 한번에 표시한다 (예: Header - Body - Footer 구조)

```xml
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vue Named View Sample</title>
  </head>
  <body>
    <div id="app">
      <router-view name="header"></router-view>
      <router-view></router-view>
      <router-view name="footer"></router-view>
    </div>

    <script src="https://cdn.jsdelivr.net/npm/vue@2.5.2/dist/vue.js"></script>
    <script src="https://unpkg.com/vue-router@3.0.1/dist/vue-router.js"></script>
    <script>
      var Body = { template: '<div>This is Body</div>' };
      var Header = { template: '<div>This is Header</div>' };
      var Footer = { template: '<div>This is Footer</div>' };
      var router = new VueRouter({
        routes: [
          {
            path: '/',
            components: {
              default: Body,
              header: Header,
              footer: Footer
            }
          }
        ]
      })
      var app = new Vue({
        router
      }).$mount('#app');
    </script>
  </body>
</html>
```

### 3. Vue Resource

뷰 리소스는 초기에 코어팀에서 공식적으로 권하는 HTTP 통신 라이브러리 였으나 2016년 말에 공식적인 지원은 중단되었음

```xml
<html>
  <head>
    <title>Vue Resource Sample</title>
  </head>
  <body>
    <div id="app">
      <button v-on:click="getData">프레임워크 목록 가져오기</button>
    </div>

    <script src="https://cdn.jsdelivr.net/npm/vue@2.5.2/dist/vue.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/vue-resource@1.3.4"></script>
    <script>
      new Vue({
        el: '#app',
        methods: {
          getData: function() {
            this.$http.get(`https://raw.githubusercontent.com/joshua1988/doit-vuejs/master/data/demo.json`)
                .then(function(response) {
                  console.log(response);
                  console.log(JSON.parse(response.data));
                });
          }
        }
      });
    </script>
  </body>
</html>
```


### 4. Axios

액시오스는 현재 뷰 커뮤니티에서 가장 많이 사용되는 HTTP 통신 라이브러리로,
`Promise`기반의 API 형식이 다양하게 제공되어 별도의 로직을 구현할 필요없이 간단하게 원하는 로직을 구현 가능하다.

[액시오스 공식 Git repo](https://github.com/axios/axios)

> `Promise`란 서버에 데이터를 요청하여 받아오는 동작과 같은 비동기 로직 처리에 유용한 자바스크립트 객체로,
> 자바스크립트는 단일 쓰레드(thread)로 코드를 처리하기 때문에 특정 로직의 처리가 끝날 때까지 기다려주지 않는다.
> 따라서 데이터를 요청하고 받아올 때까지 기다렸다가 화면에 나타내는 로직을 실행해야 할 때 주로 Promise를 활용한다.

CDN을 사용하거나 NPM으로 설치하거나 둘중에 한가지 방법으로 사용한다.

**HTTP GET 요청**
```javascript
axios.get('URL 주소').then().catch();
```
서버에서 보낸 데이터를 정상적으로 받아오면 `then()`안에 정의한 로직이 실행되고, 오류가 발생하면 `catch()`에 정의한 로직이 실행된다.

**HTTP POST 요청**
```javascript
axios.get('URL 주소').then().catch();
```

**HTTP 요청에 대한 옵션 속성 정의**
```javascript
axios({
  method: 'get',
  url: 'URL 주소',
  ...
})
```
자세한 속성들을 직접 정의하여 보낼 수도 있다. 데이터 유형, 요청방식 등등...



```xml
<html>
	<head>
		<title>Vue with Axios Sample</title>
	</head>
	<body>
		<div id="app">
			<button v-on:click="getData">프레임워크 목록 가져오기</button>
		</div>

		<script src="https://cdn.jsdelivr.net/npm/vue@2.5.2/dist/vue.js"></script>
		<script src="https://unpkg.com/axios/dist/axios.min.js"></script>
		<script>
			new Vue({
				el: '#app',
				methods: {
					getData: function() {
						axios.get('https://raw.githubusercontent.com/joshua1988/doit-vuejs/master/data/demo.json')
							.then(function(response) {
								console.log(response);
							});
					}
				}
			});
		</script>
	</body>
</html>
```

