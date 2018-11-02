# TIL

# Fri Nov 2 2018



# 1. Today I Learned

## 쿠키(cookie) VS 토큰(token)

![](/Users/yoonjaepark/Desktop/Fast Campus Front-End School/9W 36D (Fri, Nov 2, 2018)/TIL(Fri-Nov-3-2018)/img/cookie-token-auth.png)

###인증 토큰(Authentication Token) 저장 방식

- 인증 토큰 저장소로 쿠키를 쓰는 경우
  - cookie

- 인증 토큰 저장소를 직접 관리하는 경우
  - Local Storage



## JWT (JSON Web Token)

- 최근 널리 사용되고 있는 [토큰 형식의 표준](https://tools.ietf.org/html/rfc7519)

- 토큰 안에 **JSON 형식**으로 정보를 저장함

- 보안을 위해 서명 또는 암호화를 사용할 수 있음



## JAVASCRIPT 심화 2

##비동기 프로그래밍

### Promise

Promise는 **'언젠가 끝나는 작업'의 결과값**을 담는 통과 같은 객체입니다. 

Promise 객체가 만들어지는 시점에는 그 통 안에 무엇이 들어갈지 모를 수도 있습니다. 대신 `then` 메소드를 통해 콜백을 등록해서, 작업이 끝났을 때 결과값을 가지고 추가 작업을 할 수 있습니다.

비동기 작업을 하는 Promise 객체는 `Promise` 생성자를 통해 만들 수 있습니다.

```
const p = new Promise((resolve, reject) => {
  setTimeout(() => {
    // console.log('2초가 지났습니다.');
    resolve('hello');
  }, 2000);
});

p.then(msg => {
  console.log(msg); // hello
});
```



Promise 객체의 **결과값을 사용해 추가 작업**을 하려면 `then` 메소드를 호출해야 합니다. `then` 메소드에 콜백을 넘겨서, 첫 번째 인수로 들어온 결과값을 가지고 추가 작업을 할 수 있습니다.

```js
p.then(msg => {
  console.log(msg); // hello
});
```



`then` 메소드에는 아주 중요한 특징이 있는데, 바로 **then 메소드 자체도 Promise 객체를 반환한다**는 것입니다. 이 때, 콜백에서 반환한 값이 곧 Promise의 결과값이 됩니다.

```js
const p2 = p.then(msg => {
  return msg + ' world';
});

p2.then(msg => {
  console.log(msg); // hello world
});
```

위 코드는 아래와 같이 줄여 쓸 수 있습니다.

```js
p.then(msg => {
  return msg + ' world';
}).then(msg => {
  console.log(msg);
});
```

또한, `then` 메소드에 넘겨준 콜백에서 Promise 객체를 반환하면, `then` 메소드가 반환한 Promise 객체는 앞의 Promise 객체의 결과를 따르게 됩니다. 아래 예제를 직접 실행하고, 어떻게 출력이 되는지 확인해보세요.

```js
// Promise 객체를 반환하는 함수
function delay(ms) {
  return new Promise(resolve => {
    setTimeout(() => {
      console.log(`${ms} 밀리초가 지났습니다.`);
      resolve();
    }, ms);
  });
}

delay(1000)
  .then(() => delay(2000))
  .then(() => Promise.resolve('끝'))
  .then(console.log);

console.log('시작');
```



```js
※ 15번 예제 대체 코드
// 1초 뒤에 'hello'가 채워지는 통(promise)가 반환 되는 함수
function delay(ms, value) {
  return new Promise(resolve => {
    setTimeout(() => {
      // console.log(`${ms} 밀리초가 지났습니다.`);
      resolve(value);
    }, ms);
  });
}

delay(1000, 'hello')
  .then(str => {
    return str + ' world'
  })
  .then(str => {
    console.log(str)
  })

// 3초 뒤
delay(1000, 'hello')
  .then(str => {
    return delay(2000, str + ' world')
  })
  .then(str => {
    console.log(str)
  })

```



####Promise의 작동 원리 설명 image by Googling



1.

![](/Users/yoonjaepark/Desktop/Fast Campus Front-End School/9W 36D (Fri, Nov 2, 2018)/TIL(Fri-Nov-3-2018)/img/Screen Shot 2017-01-26 at 8.28.19 pm.png)



---



2.

![](/Users/yoonjaepark/Desktop/Fast Campus Front-End School/9W 36D (Fri, Nov 2, 2018)/TIL(Fri-Nov-3-2018)/img/js-promise.svg)



### 비동기 함수 (Async Function)

ES2017에서 도입된 **비동기 함수(async function)**를 사용하면, 동기식 코드와 거의 같은 구조를 갖는 비동기식 코드를 짤 수 있습니다.

함수 앞에 **async 키워드**를 붙이면, 이 함수는 비동기 함수가 됩니다.

```js
// 비동기 함수
async function func1() {
  // ...
}

// 비동기 화살표 함수
const func2 = async () => {
  // ...
}

// 비동기 메소드
class MyClass {
  async myMethod() {
    // ...
  }
}
```

비동기 함수는 **항상 Promise 객체를 반환한다**는 특징을 갖습니다. 이 Promise의 결과값은 비동기 함수 내에서 무엇을 반환하느냐에 따라 결정되며, **then 메소드와 똑같은 방식으로 동작합니다.**

```js
async function func1() {
  return 1;
}

async function func2() {
  return Promise.resolve(2);
} // 2가 채워진 통(promise)를 바로 반환하는 함수

func1().then(console.log); // 1
func2().then(console.log); // 2
```



**await 키워드**

1. `await`는 Promise의 `then` 메소드와 유사한 기능을 하는데, **await 키워드 뒤에 오는 Promise가 결과값을 가질 때까지 비동기 함수의 실행을 중단시킵니다.** 여기서의 '중단'은 비동기식이며, 브라우저는 Promise가 완료될 때까지 다른 작업을 처리할 수 있습니다.

2. `await`는 연산자이기도 하며, **await 연산의 결과값은 뒤에 오는 Promise 객체의 결과값**이 됩니다.

```js
// Promise 객체를 반환하는 함수.
function delay(ms) {
  return new Promise(resolve => {
    setTimeout(() => {
      console.log(`${ms} 밀리초가 지났습니다.`);
      resolve()
    }, ms);
  });
}

async function main() {
  await delay(1000);
  await delay(2000);
  const result = await Promise.resolve('끝');
  console.log(result);
}

main();
```

비동기 함수의 가장 큰 장점은 **동기식 코드를 짜듯이 비동기식 코드를 짤 수 있다**는 것입니다. (아래 예제는 Github 데이터를 불러오는 예제를 비동기 함수를 사용해 다시 작성한 것입니다.)

```js
const axios = require('axios');
const API_URL = 'https://api.github.com';

async function fetchStarCount() {
  const starCount = {};

  // 1. Github에 공개되어있는 저장소 중, 언어가 JavaScript이고 별표를 가장 많이 받은 저장소를 불러온다.
  const topRepoRes = await axios.get(`${API_URL}/search/repositories?q=language:javascript&sort=stars&per_page=1`);

  // 2. 위 저장소에 가장 많이 기여한 기여자 5명의 정보를 불러온다.
  const topMemberRes = await axios.get(`${API_URL}/repos/${topRepoRes.data.items[0].full_name}/contributors?per_page=5`);

  // 3. 해당 기여자들이 최근에 Github에서 별표를 한 저장소를 각각 10개씩 불러온다.
  const ps = topMemberRes.data.map(user => axios.get(`${API_URL}/users/${user.login}/starred?per_page=10`));
  const starredReposRess = await Promise.all(ps);
  const starredReposData = starredReposRess.map(r => r.data)

  // 4. 불러온 저장소를 모두 모아, 개수를 센 후 저장소의 이름을 개수와 함께 출력한다.
  for (let repoArr of starredReposData) {
    for (let repo of repoArr) {
      if (repo.full_name in starCount) {
        starCount[repo.full_name]++;
      } else {
        starCount[repo.full_name] = 1;
      }
    }
  }
  return starCount;
}

fetchStarCount().then(console.log);
```

`then` 메소드를 사용한 버전과 비교했을 때, 비동기 작업을 위해 콜백을 사용하는 부분이 모두 사라졌습니다.

`await` 키워드는 `for`, `if`와 같은 제어 구문 안에서도 쓰일 수 있기 때문에, `then` 메소드를 사용할 때보다 **복잡한 비동기 데이터 흐름을 아주 쉽게 표현할 수 있다**는 장점이 있습니다. 

다만, 비동기 함수 역시 Promise를 사용하기 때문에, 비동기 함수를 잘 쓰기 위해서는 여전히 Promise에 대해 잘 알고 있어야 합니다. 



### Generator

generator 함수는 **'함수를 잠시 멈춰둘 수 있다'**는 특징을 갖고 있습니다. 이 특징으로 인해 generator가 비동기 프로그래밍을 위해 사용되기도 합니다.

비동기 함수를 사용한 예제와 비교해서 보면, 코드의 구조가 굉장히 비슷합니다. 실제로, ES2017에서 비동기 함수가 도입되기 전에는 generator가 비동기 프로그래밍을 위해 널리 사용되었습니다. 최근에는 언어에 내장되어 있고 더 쉬운 비동기 함수를 많이 사용하는 편입니다.

다만 generator는 **함수의 재개를 프로그래머가 직접 제어할 수 있다**는 장점을 갖고 있기 때문에, 일부러 비동기 함수 대신 generator를 사용하는 경우도 있습니다. React에서 비동기 프로그래밍을 하기 위해 널리 사용되는 라이브러리인 [redux-saga](https://redux-saga.js.org/) 역시 generator를 활용하고 있습니다. 



# Axios

- Promise based HTTP client for the browser and node.js ([axios - github](https://github.com/axios/axios))
- JavaScript를 통해 직접 요청을 보내기 위해 널리 사용되는 라이브러리

- GET 메소드로 요청을 보내기 위해 `axios.get()` 함수를 사용할 수 있는데, 이 때 **Promise 객체가 반환됩니다.**

###Features

- Make [XMLHttpRequests](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest) from the browser
- Make [http](http://nodejs.org/api/http.html) requests from node.js
- Supports the [Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise) API
- Intercept request and response
- Transform request and response data
- Cancel requests
- Automatic transforms for JSON data
- Client side support for protecting against [XSRF](http://en.wikipedia.org/wiki/Cross-site_request_forgery)





### ※ css 확장언어

- Sass - 가장 많이 쓰는 언어 [Sass - guide](https://sass-lang.com/guide) (스코프 기능도 지원)

- Less

- Stylus

- PostCSS



# 2. 실습 

(in Glitch)

- [JWT](https://wpsn-jwt-example.glitch.me/)

- [json-server + login기능](https://glitch.com/edit/#!/fds-json-server-todo)


(in Repl)

- [Axios - login 기능](https://repl.it/@YoonJP/Axios-login-gineung?language=javascript)



(in Codepen)

- [To-do List](https://codepen.io/yoonjp/pen/KrPqNZ?editors=1010)
  - **Single Source of Truth**: 상태 저장소가 여러개 있는 경우 **상태의 불일치**가 발생할 수 있다. (→ **상태 저장소는 1개만 두는게 좋다.**) 비록 비효율적으로 보일지라도 버그를 줄이려면 믿을 수 있는 저장소 1개만 있는게 좋다. (ex: To-do list 하나를 여려명에서 사용할 경우, 실시간 웹(realtime web)(ex: [Trello](https://trello.com/)))
  - **웹소켓(Web socket)**: 서버랑 연결이 계속 이어져 있어서 내가 요청을 보내지 않아도 상태가 실시간으로 업데이트 되도록 해준다. 

# 3. Reference

- (JWT) 참고 링크
  - <https://jwt.io/introduction/>
  - <https://stormpath.com/blog/where-to-store-your-jwts-cookies-vs-html5-web-storage>
  - <https://blog.outsider.ne.kr/1160>
  - <https://velopert.com/2448>
  - <https://auth0.com/blog/json-web-token-signing-algorithms-overview/>

- [axios - github](https://github.com/axios/axios)

- [json-server](https://github.com/typicode/json-server)
  - [json-server + login기능 - npm](https://www.npmjs.com/package/fds-json-server)