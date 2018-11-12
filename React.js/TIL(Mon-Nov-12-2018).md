# TIL

# Mon Nov 12 2018

# React

- [React - 교재](https://reactjs-org-ko.netlify.com/tutorial/tutorial.html)

# Tutorial: Intro To React

## 개요

### React가 무엇인가요?

**React는 선언적**이고, 효율적이며, 유연한 JavaScript 라이브러리입니다. React는 UI를 제작할 때 사용하기 위해 만들어졌습니다. React를 사용하면, “컴포넌트”라 불리는 여러 격리된 코드 조각을 조합해서, 복잡한 UI를 쉽게 만들 수 있습니다.



-  프로그래밍의 큰 두 방식: 
  - 명령형(like JS):
    - 코드를 보고 바로 결과물을 예측하기 어렵다
  - 선언적 프로그래밍(like HTML)
    - 코드가 결과물의 구조를 잘 반영해서 보고 예측하기 쉽다 

  → **React는 선언적 프로그래밍 언어**



- 화면을 그리는 방식
  - 새로 그리기:
    - good for 사람
    - bad for 기계
  - 필요한 부분만 고치기:
    - bad for 사람
    - good for 기계

  → React는 good for 사람 & good for 기계



###**React 컴포넌트 클래스 VS React 엘리먼트**

React의 컴포넌트에는 두 가지 종류가 있습니다. 일단은 `React.Component`의 서브클래스부터 봅시다:

```react
class ShoppingList extends React.Component {
  render() {
    return (
      <div className="shopping-list">
        <h1>Shopping List for {this.props.name}</h1> // HTML이 아니다!(JSX이다), {}사이의 값은 어떤 표현식도 올 수 있다. 
        <ul>
          <li>Instagram</li>
          <li>WhatsApp</li>
          <li>Oculus</li>
        </ul>
      </div>
    );
  }
}

// 사용 예제: <ShoppingList name="Mark" />
```

XML과 비슷하게 생긴 위 태그의 사용법을 곧 살펴볼 것입니다. 우리는 화면을 어떻게 그릴지를 React에게 알려주기 위해 컴포넌트를 사용합니다. 데이터가 변경되면, React는 컴포넌트를 효율적으로 갱신합니다. (즉, 다시 그립니다.)

위에서 본 ShoppingList는 **React 컴포넌트 클래스**입니다. 컴포넌트는 props (“properties”의 줄임말)이라 불리는 매개변수를 받아서, `render`메소드에서 뷰의 계층 구조를 반환합니다.



`render` 메소드는 무엇을 그릴지에 대한 *설명*을 반환합니다. 그러면 React는 그것을 받아 화면에 그려줍니다. 여기서 `render`가 반환하는 것은 **React 엘리먼트**로, ‘무엇을 그릴지’에 대한 정보를 담고있는 객체입니다. 대부분의 React 개발자들은 이러한 구조를 쉽게 표현할 수 있는 JSX라는 특별한 문법을 사용합니다. `<div />`라는 JSX 코드는, 빌드 과정에서 `React.createElement('div')`로 변환됩니다. 위 예제는 사실 아래 코드와 같습니다: 

```React
return React.createElement('div', {className: 'shopping-list'},
  React.createElement('h1', /* ... h1 children ... */),
  React.createElement('ul', /* ... ul children ... */)
);
```

JSX 안에서는 JavaScript를 자유롭게 활용할 수 있습니다. JSX 중괄호 안에는 어떤 JavaScript 표현식(React 표현식도)도 넣을 수 있습니다. 그리고 React 엘리먼트는 JavaScript 객체로, 변수에 담거나 프로그램의 다른 부분으로 넘기는 것이 가능합니다.



위 예제의 ShoppingList 컴포넌트는 브라우저에 내장된 DOM 컴포넌트(`<div />`, `<li />`)만 그려주고 있습니다. 하지만 React 컴포넌트를 조합해서 그리는 것도 가능합니다. 예를 들어, 우리는 전체 쇼핑 목록을 그리기 위해 `<ShoppingList />`와 같이 쓸 수 있습니다. 각각의 React 컴포넌트는 독립적이며 캡슐화되어 있습니다. 이 성질은 우리가 단순한 컴포넌트로부터 복잡한 UI를 만드는 일을 가능하게 해 줍니다.



### 시작 코드 살펴보기

- [시작 코드](https://codepen.io/gaearon/pen/oWWQNa?editors=0010)



### Passing Data Through Props

: prop은 부모로부터 값을 내려받는 통로라고 볼 수 있음



### 상호작용을 하는 컴포넌트 만들기

무언가를 “기억”하기 위해, 컴포넌트는 **state**를 사용합니다.

React 컴포넌트의 생성자에서 `this.state` 속성을 넣어주면, 이 컴포넌트는 **상태를 갖게 됩니다.** 이 상태를 갖고 있는 컴포넌트만이 상태를 변경할 수 있습니다. 이제 사각형에 표시될 값을 state에 저장하고, 클릭되었을 때 그 값이 변경되게 만들어봅시다.

```React
class Square extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      value: null,
    };
  }

  render() {
    return (
      <button className="square" onClick={() => alert('click')}>
        {this.props.value}
      </button>
    );
  }
}
```

[JavaScript 클래스](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes)를 사용할 때, 서브클래스의 생성자를 정의할 때는 **반드시 `super`를 호출해주어야 합니다.** 생성자를 갖는 모든 React 컴포넌트 클래스는 그 생성자가 반드시 `super(props)`로 시작해야 합니다.



### ★★★ React의 가장 핵심★★★ 

★★★`this.setState()`의 2가지 기능★★★

- **상태 변경**
- **화면 갱신**

**→ React에서 화면을 다시 그리는 방법은 `this.setState()`를 이용해 상태를 바꾸는 방법 하나 뿐이다.** 

→ React는 화면을 다시 그려야 할 때마다 `render()`메소드를 계속 호출한다. 



컴포넌트 안에서 `setState`를 호출하면, React는 해당 컴포넌트가 품고 있는 자식 컴포넌트까지 모두 새로 그려줍니다.



※ React 동작 방식: 

1. 화면
2. 상태 
3. 상태로부터 화면을 그림
4. 상태 변경(setState)



### 상태 끌어올리기

※ React에서 자식컴포넌트에 저장된 상태를 부모컴포넌트에서 읽어오는 경우는 없다.

**여러 자식 컴포넌트에 저장되어 있는 데이터를 읽어와야 할 때, 혹은 자식 컴포넌트끼리 통신을 해야 할 필요가 있을 때는, 부모 컴포넌트에서 상태를 공유하세요.** **부모 컴포넌트에서는 prop을 통해 자식 컴포넌트에게 상태를 내려줄 수 있습니다. 이 방법을 통해 부모 컴포넌트와 자식 컴포넌트가 따로 놀지 않게 만들 수 있습니다.**



※ 상태를 공유해야하는 컴포넌트들의 가장 가까운 공통 조상에게 상태를 저장하는게 원칙이다. (상태의 불일치를 피하기위해서 하나의 상태저장소를 갖는게 좋다: 'single source of truth')



컴포넌트의 상태에는 자기 자신만 접근할 수 있으므로, Square 컴포넌트에서 Board 컴포넌트의 상태를 **직접 변경**할 수 있는 방법은 없습니다. 이런 경우, 부모 컴포넌트인 Board에서 **상태를 바꾸는 함수를 만들어 Square에 내려줌**으로써 문제를 해결할 수 있습니다. (간접적으로 부모의 상태를 바꾸게 되는것)



### 함수형 컴포넌트

- 클래스형 컴포넌트
- 함수형 컴포넌트

**함수형 컴포넌트**는, 상태를 갖지 않고, `render` 메소드만 있는 컴포넌트를 좀 더 편하게 작성할 수 있는 방법입니다. `React.Component`를 상속받는 클래스를 만드는 대신, `props`를 입력받아서 무엇을 그려야 할지를 반환하는 함수를 만드세요. 함수형 컴포넌트는 클래스에 비해 빨리 작성할 수 있으며, 많은 컴포넌트들이 함수형 컴포넌트로 작성될 수 있습니다.

```React
function Square(props) {
  return (
    <button className="square" onClick={props.onClick}>
      {props.value}
    </button>
  );
}
```

`onClick={() => this.props.onClick()}`을 `onClick={props.onClick}`과 같이 작성한 부분을 보면`<button>`내장 컴포넌트의 `onClick` prop에는 이렇게 함수를 직접 넘겨줄 수도 있습니다. 부모 컴포넌트로부터 받은 함수를 넘겨줄 때는 이렇게 해도 문제가 없어서 이런 코드가 많이 사용됩니다. (하지만, (특히 클래스 컴포넌트에서) `this` 때문에 문제가 생길 수도 있으니 주의!)



### 

※ render 메소드 안에는 화면을 그리는 작업과 상관없는 코드가 들어 있다면 안된다.(render를 계속 그리는 꼴이 되므로)

※ React는 엘리먼트 하나만 반환 할 수 있다. 

※ `          <input type="text" name="body" /> ` 닫는 태그가 없는 태그들은 꼭 `/`를 추가해 줘야된다.



# React Practice 

- [Tic Tac Toe - Codepen(함수형 컴포넌트)](https://codepen.io/yoonjp/pen/xQEMJy?editors=0010)

- [Tic Tac Toe - Codepen(클래스형 컴포넌트)](https://codepen.io/yoonjp/pen/vQXPXL?editors=0010)
- [React 기초 실습(up and down buttons) - Codepen](https://codepen.io/yoonjp/pen/dQpBEO?editors=0010)
- [React 기초 실습(menu selector) - Codepen](https://codepen.io/yoonjp/pen/BGLXLO?editors=0010) 

- [React 기초 실습(To-do List) - Codepen](https://codepen.io/yoonjp/pen/qQqWZY?editors=0010)



※ [React - Babel](https://babeljs.io/repl/#?presets=react&code_lz=DwEwlgbgBAxgNgQwM5IHIILYFMC8AiJACwHsAHUsAOwHMBaOMJAFzwD4AoKKYQgRg65cAyiXJVqUADKMmUAGbEATlADepRWSQA6SpiwBfTtwD0fAdwCucc12ANWASUrME1RZmDH7R2_YDqhAhMSACC5J7egtz2APIwVhZIEWDmnlYcnuAQrADc7EA)







