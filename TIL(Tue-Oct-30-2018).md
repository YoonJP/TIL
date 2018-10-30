# TIL (Today I Learned)

# Tue, Oct 30, 2018



# FDS Node.js + HTTP

## 1. Express

- Node.js 생태계에서 **가장 널리 쓰이는 웹 프레임워크**

- 내장하고 있는 기능은 매우 적으나, **미들웨어**(like plug-in)를 주입하는 방식으로 기능을 확장하는 **생태계**를 가지고 있음

- [공식 매뉴얼 한국어 번역](https://expressjs.com/ko/)

- Express 앱의 기본 구조

```js
// Express 인스턴스 생성
const app = express()

// 미들웨어 주입
app.use(sessionMiddleware())
app.use(authenticationMiddleware())

// 라우트 핸들러 등록
app.get('/', (request, response) => {
  response.send('Hello express!')
})
// event Listener랑 비슷한 느낌

// 서버 구동
app.listen(3000, () => {
  console.log('Example app listening on port 3000!')
})
```

- Routing

```js
// HTTP 요청 메소드(GET, POST, ...)와 같은 이름의 메소드를 사용
app.get('/articles', (req, res) => {
  res.send('Hello Routing!')
})
// 특정 경로에만 미들웨어를 주입하는 것도 가능
app.post('/articles', bodyParserMiddleware(), (req, res) => {
  database.articles.create(req.body)
    .then(() => {
      res.send({ok: true})
    })
})
// 경로의 특정 부분을 함수의 인자처럼 입력받을 수 있음
app.get('/articles/:id', (req, res) => {
  database.articles.find(req.params.id) // `req.params`에 저장됨
    .then(article => {
      res.send(article)
    })
})
```

- Request 객체

  - req.body

    요청 바디를 적절한 형태의 자바스크립트 객체로 변환하여 이곳에 저장 (**body-parser 미들웨어**에 의해 처리됨)

  - req.ip

    요청한 쪽의 IP

  - req.params

    route parameter

  - req.query

    query string이 객체로 저장됨

  - res.status(...)

    응답의 상태 코드를 지정하는 메소드

  - res.append(...)

    응답의 헤더를 지정하는 메소드

  - res.send(...)

    응답의 바디를 지정하는 메소드 
    인자가 **텍스트**면 **text/html**, **객체**면 **application/json** 타입으로 응답

※ 요청(request)의 구성 요소

- 메소드
- 주소
- 헤더
- 바디

※ 응답(response)의 구성 요소

- 상태코드

- 헤더

- 바디


## 2. Template Language

- 웹 초창기 - CGI (Common Gate Interface)

- Template Engine

  - **템플릿과 데이터를 결합**해 문서를 생성하는 프로그램, 혹은 라이브러리
  - 템플릿을 작성할 때 사용하는 언어를 **템플릿 언어**라고 함

  - ex: JSP, ASP, PHP, EJS
  - template engine은 server에서 HTML을 만지는 것(전통적 웹 개발 방식)

- EJS (Embedded JavaScript Template)
  - [#](http://ejs.co/)
  - Node.js 생태계에서 가장 많이 사용되는 템플릿 엔진
  - JavaScript 코드를 템플릿 안에서 그대로 쓸 수 있음
  - [EJS VSCode Extension](https://marketplace.visualstudio.com/items?itemName=DigitalBrainstem.javascript-ejs-support)
  - [EJS에서 Emmet 사용하기](http://blog.daum.net/_blog/BlogTypeView.do?blogid=0Mst5&articleno=7691304&categoryId=807820&regdt=20171113095204)
  - Express랑 연동되어서 사용되는 템플릿 엔진

- EJS 예제
  - 템플릿 태그
    - `<% ... %>`: 템플릿의 구조를 제어하기 위해 사용하며, 문자열을 내놓지 않습니다.
    - `<%= ... %>`: 내부의 식을 문자열로 변환해 HTML 문서 안에 삽입합니다.
    - `<%# ... %>`: EJS 주석입니다. HTML 주석과는 다르게 아예 HTML 문서에 포함되지 않습니다.

- 파일을 그대로 제공하기

```js
// `public` 폴더에 있는 파일을 `/static` 경로 아래에서 제공
app.use('/static', express.static('public'))
```

```html
<!-- 템플릿 파일에서 참조할 수 있음 -->
<link rel="stylesheet" href="/static/index.css">
<script type="text/javascript" src="/static/index.js"></script>
```



## 3. Web Form

- HTML form의 기본 동작

  - HTML form을 전송하면, 입력된 정보가 기본적으로 

    percent encoding

     되어 요청됨

    - GET method

      ```
      GET /search?query=%EA%B0%9C&sort=latest HTTP/1.1
      ...
      ```

    - POST method

      ```
      POST /form HTTP/1.1
      Content-Type: application/x-www-form-urlencoded 
      ...
      
      home=Cosby&favorite+flavor=flies
      ```

  ※ Content-Type: application/x-www-form-urlencoded → 자주 쓴다


### UUID (Universally unique identifier)

인터넷 상의 수많은 자료를 구분하기 위해 각 자료에 식별자(identifier)를 부여하는 일은 아주 중요합니다. 식별자를 부여하는 가장 쉬운 방법은 자료가 생성된 순서대로 번호를 붙이는 것입니다. 실제로 많은 데이터베이스에서 이런 방법을 사용하고 있습니다. 하지만 환경에 따라 자료가 생성되는 순서를 알 수 없는 경우도 있습니다.

UUID는 식별자로 사용하기 위해 고안된 수 형식이며, 아래와 같은 형식으로 표현됩니다.

```
424e19f5-f330-4be1-889f-4a9f7da75b69
```

UUID는 표현할 수 있는 경우의 수가 무지무지무지무지 많습니다. (128bit = 2의 128제곱) UUID 난수를 생성하는 표준적인 방법(UUID version 4)을 사용하면, 언제 어디서 UUID를 생성해도 정확히 같은 UUID가 생성될 수 있는 확률이 매우매우매우매우 작기 때문에 안심하고 식별자로 사용할 수 있습니다.

이 프로젝트에서는 UUID를 생성하기 위해 `uuid` npm 패키지를 사용했습니다.



### Redirect after submission

순수한 HTML form을 이용해 **POST 메소드로 자료를 전송한 후에는 꼭 리디렉션을 통해 응답**해야 합니다. 특히 302 상태 코드를 사용해 응답해야 합니다.

POST 메소드 요청에 일반적인 응답(2xx)을 하게 되면, 해당 페이지를 새로고침을 했을 때 이전에 보냈던 요청을 그대로 다시 보내게 되기 때문에, 자료가 이중으로 전송되게 됩니다. `server.js`에서 주석을 해제해서 테스트해볼 수 있습니다. 단, 이는 순수 HTML form을 사용했을 때만 해당되며, Ajax를 통해 자료를 전송하는 방식이라면 2xx 상태코드의 일반적인 응답을 해도 괜찮습니다. (사용자가 Ajax를 새로고침할 수 있는 방법은 없기 때문입니다.)

301 상태코드(Moved Permanently)를 사용하면 안되는 이유는 브라우저 캐시 때문입니다. 브라우저가 한 번 301 응답을 받게 되면, 그 결과를 저장해두었다가 사용자가 같은 요청을 보내려고 할 때 서버에 요청을 보내지 않고 미리 저장해둔 응답을 대신 보여줍니다. 만약 사용자의 폼 전송에 대해 한 번 301 상태코드로 응답하게 되면, 사용자가 나중에 같은 내용으로 폼을 전송하려고 했을 때 제대로 전송되지 않을 것입니다.



**∴ 사용자들은 browser 내장 UI(Refresh, Go back, Go forward)를 사용하기 때문에 그 점을 고려해서 설계해야 될 것!**



---

# JAVASCRIPT 심화 2

# 클래스

## ES2015 class

ES2015 이전까지는 비슷한 종류의 객체를 많이 만들어내기 위해 **생성자**를 사용해왔습니다.

ES2015에서 도입된 **클래스**는 생성자의 기능을 대체합니다. `class` 표현식을 사용하면, 생성자와 같은 기능을 하는 함수를 훨씬 더 깔끔한 문법으로 정의할 수 있습니다.

```js
// 클래스
class Person {
  // 이전에서 사용하던 생성자 함수는 클래스 안에 `constructor`라는 이름으로 정의합니다.
  constructor({name, age}) {
    this.name = name;
    this.age = age;
  }

  // 객체에서 메소드를 정의할 때 사용하던 문법을 그대로 사용하면, 메소드가 자동으로 `Person.prototype`에 저장됩니다.
  introduce() {
    return `안녕하세요, 제 이름은 ${this.name}입니다.`;
  }
}

const person = new Person({name: '윤아준', age: 19});
console.log(person.introduce()); // 안녕하세요, 제 이름은 윤아준입니다.
console.log(typeof Person); // function
console.log(typeof Person.prototype.constructor); // function
console.log(typeof Person.prototype.introduce); // function
console.log(person instanceof Person); // true
```

`class` 블록에서는 JavaScript의 다른 곳에서는 사용되지 않는 **별도의 문법**으로 코드를 작성해야 합니다. 함수 혹은 객체의 내부에서 사용하는 문법과 혼동하지 않도록 주의하세요.

```js
// 클래스는 함수가 아닙니다!
class Person {
  console.log('hello');
}
// 에러: Unexpected token
// 클래스는 객체가 아닙니다!
class Person {
  prop1: 1,
  prop2: 2
}
// 에러: Unexpected token
```

※ 객체에서 메소드 문법을 통해서 작성할 때는 메소드 작성후 ','를 꼭 적어 줘야 했지만 class에서는 메소드를 만들고 ','를 적으면 안된다. 

```js
// 클래스는 객체가 아닙니다!
class Person {
  prop1: 1,
  prop2: 2
}
// 에러: Unexpected token
```

문법이 아니라 동작방식의 측면에서 보면, ES2015 이전의 생성자와 ES2015의 클래스는 다음과 같은 차이점이 있습니다.

- 클래스는 **함수로 호출될 수 없습니다.**

- 클래스 선언은 `let`과 `const`처럼 **블록 스코프**에 선언되며, **호이스팅(hoisting)**이 일어나지 않습니다.

- 클래스의 메소드 안에서 **super 키워드**를 사용할 수 있습니다.


## 메소드 정의하기

클래스의 메소드를 정의할 때는 객체 리터럴에서 사용하던 문법과 유사한 문법을 사용합니다.

인스턴스 메소드(instance method)는 다음과 같은 문법을 통해 정의합니다.

```js
class Calculator {
  add(x, y) {
    return x + y;
  }
  subtract(x, y) {
    return x - y;
  }
}
```

객체 리터럴의 문법과 마찬가지로, 임의의 표현식을 **대괄호**로 둘러싸서 메소드의 이름으로 사용할 수도 있습니다.

```js
const methodName = 'introduce';
class Person {
  constructor({name, age}) {
    this.name = name;
    this.age = age;
  }
  // 아래 메소드의 이름은 `introduce`가 됩니다.
  [methodName]() {
    return `안녕하세요, 제 이름은 ${this.name}입니다.`;
  }
}

console.log(new Person({name: '윤아준', age: 19}).introduce()); // 안녕하세요, 제 이름은 윤아준입니다.
```

**Getter 혹은 setter**를 정의하고 싶을 때는 메소드 이름 앞에 `get` 또는 `set`을 붙여주면 됩니다.

```js
class Account {
  constructor() {
    this._balance = 0;
  }
  get balance() {
    return this._balance;
  }
  set balance(newBalance) {
    this._balance = newBalance;
  }
}

const account = new Account();
account.balance = 10000;
account.balance; // 10000
```

`static` 키워드를 메소드 이름 앞에 붙여주면 해당 메소드는 **정적 메소드**가 됩니다.

```js
class Person {
  constructor({name, age}) {
    this.name = name;
    this.age = age;
  }
  // 이 메소드는 정적 메소드입니다.
  static sumAge(...people) {
    return people.reduce((acc, person) => acc + person.age, 0);
  }
}

const person1 = new Person({name: '윤아준', age: 19});
const person2 = new Person({name: '신하경', age: 20});

Person.sumAge(person1, person2); // 39
```



```js
class Person {
  constructor({name, age}) {
    this.name = name;
    this.age = age;
  }
  // 이 메소드는 정적 메소드입니다.
  static sumAge(...people) {
    return people.reduce((acc, person) => acc + person.age, 0);
  }
}

// function Person({name, age}) {
//   this.name = name;
//   this.age = age;
// }

// Person.sumAge = function(...people) {
//     return people.reduce((acc, person) => acc + person.age, 0);
// }

// Person.prototpye.introduce = function () {
//   console.log(`안녕하세요, ${this.name}`)
// }

const person1 = new Person({name: '윤아준', age: 19});
const person2 = new Person({name: '신하경', age: 20});

person1.introduce() // 정적 메소드
person1.sumAge()
Person.sumAge(person1, person2); // 39
```



## 클래스 필드 (Class Field)

클래스 블록 안에서 할당 연산자(`=`)를 이용해 인스턴스 속성을 지정할 수 있는 문법을 **클래스 필드(class field)**라고 합니다.

```js
class Counter {
  static initial = 0; // static class field
  count = Counter.initial; // class field
  inc() {
    return this.count++;
  }
}

const counter = new Counter();
console.log(counter.inc()); // 0
console.log(counter.inc()); // 1

Counter.initial = 10;
console.log(new Counter().count); // 10
```

```js
class Counter {
  static initial = 0; // static class field
  count = Counter.initial; // class field
  inc() {
    return this.count++;
  }
}

// 생성자 version(위의 코드와 99% 똑같은 코드)
function Counter () {
  this.count = Counter.initial  // 인스턴스 속성
}

Counter.initial = 0

Counter.prototype.inc = function () {
  return this.count++
}
```

※ 클래스 필드는 아직 정식 표준으로 채택된 기능은 아닙니다.[1](https://helloworldjavascript.net/pages/270-class.html#fn_1) (하지만 자주 쓰인다.) 아직 이 기능을 구현한 브라우저는 없는 상태이고, Babel, TypeScript 등의 트랜스파일러를 통해 일부 기능을 사용할 수 있습니다. 

### 클래스 필드와 this

`class` 블록은 새로운 블록 스코프를 형성하고, 이 내부에서 사용된 `this`는 인스턴스 객체를 가리키게 됩니다.

```js
class MyClass {
  a = 1;
  b = this.a;
}

new MyClass().b; // 1
```

이 성질을 이용하면, **화살표 함수를 통해서 메소드를 정의할 수 있습니다.** (화살표 함수 안에서의 `this` 키워드는 바로 바깥쪽 스코프에 존재하는 `this`와 같은 객체를 가리킨다는 사실을 떠올려보세요.)

```js
// 화살표 function로 만들어진 this는 함수가 정의되는 시점에서 this가 결정된다.  
class MyClass {
  a = 1;
  getA = () => {
    return this.a;
  }
}

new MyClass().getA(); // 1

------------------------------------------
    
// function 키워드로 만들어진 this는 어떻게 호출되느냐에 따라 this가 결정된다.     
class MyClass {
  a = 1;
  getA() {
    return this.a;
  }
}

new MyClass().getA(); // 1
```

이렇게만 보면 일반적인 메소드와 별로 차이가 없어 보이지만, 사실 동작방식 측면에서 굉장히 큰 차이점이 있습니다.

1. 일반적인 메소드는 클래스의 `prototype` 속성에 저장되는 반면, **클래스 필드는 인스턴스 객체에 저장됩니다.**
2. 화살표 함수의 `this`는 호출 형태에 관계없이 항상 인스턴스 객체를 가리키게 됩니다.

2번 성질때문에, **메소드를 값으로 다루어야 할 경우(다른 곳에서 사용해야 할 경우)**에는 일반적인 메소드 대신 화살표 함수를 사용하는 것이 좋다. (다만, 일반적인 메소드와 달리, 클래스 필드를 통해 정의한 메소드는 **인스턴스를 생성할 때마다 새로 생성되기 때문에** 메모리를 더 차지하게 되므로 주의해서 사용해야 합니다.)



---

# 실습 (in Glitch)

- [Glitch 튜토리얼](https://glitch.com/~yoonjae-glitch)

- [Express 실습](https://glitch.com/~ash-novel)

- Template Language
  - [EJS 실습](https://glitch.com/~tangible-basement)
  - [EJS 예제](https://glitch.com/~four-mascara)

- Web Form

  - [Form 예제](https://glitch.com/~spicy-match)

---

# 2. Reference

- [EJS](http://ejs.co/)
- [MIME type](https://developer.mozilla.org/ko/docs/Web/HTTP/Basics_of_HTTP/MIME_types)
- [MIME 타입의 전체 목록](https://developer.mozilla.org/en-US/docs/Web/HTTP/Basics_of_HTTP/MIME_types/Complete_list_of_MIME_types)
