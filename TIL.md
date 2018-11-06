# TIL (Today I Learned)

# Tue Nov 6 2018



# 1. Today I Learned



# 실습 

## json-server를 활용해 Single Page App 만들기



# 1. Login form 만들기

## JS

- await api.delete('/todos/' + todoItem.id)
  - 삭제

- localStorage.getItem('token')
  - local storage에 있는 token 가져오기
- localStorage.removeItem('token')
  - local storage에 있는 token 삭제하기
- 수정할 경우 PATCH

```JSON
// post할 때 처럼 수정할 내용을 적는다. 
{
	"complete": true
}

// --------------------------------

await api.patch('/todos/' + todoItem.id, {
        complete: true
      })

// ----------------------------------

await api.patch('/todos/' + todoItem.id, {
        complete: !todoItem.complete // true면 false로 false면 true로 바꿈
      })


```

- completeEl.setAttribute('checked', '') 

  - boolean attribute: (ex: checked, disabled) attribute 이름만 있고 값은 없음

- 업데이트 방식 (웹사이트 → 서버)

  - ex: 체크 박스클릭 시 

  - 1. 비관적 업데이트(pessimistic update):

       사용자 입력 → 수정 요청 → 성공 시 화면 갱신

       - 사용자는 불편할 수 있지만 개발자 입장에선 개발하기 쉬움

    2. 낙관적 업데이트(optimistic update):

       - 사용자 입력 → 화면 갱신 → 수정 요청 → (성공 or 실패)
         - 성공 시: 화면 갱신
         - 실패 시: 원상복구

       - 사용자는 편리하지만 개발자 입장에서는 코드를 짜기가 까다로울 수 있음

- Loading indicator

  - (or progress indicator)
  - A **progress indicator** is an element of a [command line interface](https://en.wikipedia.org/wiki/Command_line_interface), a [textual user interface](https://en.wikipedia.org/wiki/Text_user_interface), or a [graphical user interface](https://en.wikipedia.org/wiki/Graphical_user_interface) that is intended to inform the user that an operation is in progress, to reassure that the system is not hung or waiting for user input, and often to provide the user with an estimate of how far through a task the system has progressed. (from *Wikipedia*)

- **await** drawTodoList()

  - 비동기 함수를 호출했을 때 반환하는 promise에는 비동기 함수 내부에서 반환는 값이 채워진다. 

- 화면을 그리는 방식
  - 화면을 모두 다시 그리는 코드 
    - 개발자에게는 Good
    - 기계에게는 Bad
  - 필요한 부분만 갱신하는 코드
    - 개발자에게는 Bad
    - 기계에는 Good

  (**→ React는 화면을 모두 다시 그리는 코드처럼 짤 수 있고 기계입장에서는 필요한 부분만 갱신되서 개발자와 기계 모두에게 좋다.**) 



## html

- <input type="checkbox">
  - input - checkbox



## CSS(SCSS)

- Loading indicator
  - body에 class를 붙이는 방법

```css
// loading indicator
body.loading:after {
  display: block;
  position: fixed;
  content: '';
  background-color: rgba(0,0,0,0.5);
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
}
```





# 2. 게시판 만들기

- [Parcel에서 환경변수 사용하기](https://en.parceljs.org/env.html)
  - `.env`: 개발 서버 & 운영(실제 사용) 서버의 분리를 위함(?) check [ ]

## JSON server

- [json-server에 내장된 여러 기능](https://github.com/typicode/json-server#routes)


## Routes

Based on the previous `db.json` file, here are all the default routes. You can also add [other routes](https://github.com/typicode/json-server#add-custom-routes) using `--routes`.



### Plural routes

```
GET    /posts
GET    /posts/1
POST   /posts
PUT    /posts/1
PATCH  /posts/1
DELETE /posts/1
```



### Filter

Use `.` to access deep properties

```json
GET /posts?title=json-server&author=typicode
GET /posts?id=1&id=2
GET /comments?author.name=typicode
```



### Paginate

Use `_page` and optionally `_limit` to paginate returned data.

In the `Link` header you'll get `first`, `prev`, `next` and `last` links.

```json
GET /posts?_page=7
GET /posts?_page=7&_limit=20
```

*10 items are returned by default*



### Sort

Add `_sort` and `_order` (ascending order by default)

```json
GET /posts?_sort=views&_order=asc
GET /posts/1/comments?_sort=votes&_order=asc
```

For multiple fields, use the following format:

```json
GET /posts?_sort=user,views&_order=desc,asc
```



### Operators

Add `_gte` or `_lte` for getting a range

```json
GET /posts?views_gte=10&views_lte=20
```

Add `_ne` to exclude a value

```json
GET /posts?id_ne=1
```

Add `_like` to filter (RegExp supported)

```json
GET /posts?title_like=server
```

like(문자열에 포함 여부 검사)

**※ ex: 텍스트로 상품 검색하기 기능에 사용가능**



※ developer acronym

gte (greater than or equal)

lte (less than or equal)

ne (not equal)



### Full-text search

Add `q`

```json
GET /posts?q=internet
```

**※ ex: 텍스트로 상품 검색하기 기능에 사용가능**



### Relationships

To include children resources, add `_embed`

```json
GET /posts?_embed=comments
GET /posts/1?_embed=comments
```

To include parent resource, add `_expand`

```json
GET /comments?_expand=post
GET /comments/1?_expand=post
```

To get or create nested resources (by default one level, [add custom routes](https://github.com/typicode/json-server#add-custom-routes) for more)

```
GET  /posts/1/comments
POST /posts/1/comments // when posting comments
```

※ children resource (ex: 게시물의 댓글)



# Axios에서URLSearchParams 사용하기

- `params` are the URL parameters to be sent with the request
- Must be a plain object or a URLSearchParams object

```json
  params: {
    ID: 12345
  },
```

- [URLSearchParams](https://developer.mozilla.org/ko/docs/Web/API/URLSearchParams)
  - 같은 이름의 query string을 반복해서 써야 하는 경우 객체쓰지 않고 params를 쓴다



# 2. 실습 Link

- [Login form for To-do List - Netlify](https://frosty-einstein-35035e.netlify.com/)

- [Bulletin Board - Glitch](https://glitch.com/edit/#!/mesquite-professor?path=README.md:12:115)



