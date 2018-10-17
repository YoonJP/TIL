# TIL (Today I Learned)

# 10/17 (Wed) 



---

# 1. Today I Learned

# Quiz(Morning Activity) Review

# Lecture Note (RGB Challenge)

- **Javascript에서 동작을 만드는 2가지 방법**

1. DOM 객체를 이용해서

   - (ex: addClass or removeClass)

   - HTML을 몇개 추가할지 모를 경우(코드 작성시점에서 요소가 몇개 필요한지 모를 경우 )

2. style을 다르게 만들어서 

   - (ex: display: none; or block;)

   - HTML을 몇개 추가할지 알 경우(코드 작성시점에서 요소가 몇개 필요한지 알 경우 )
   - style은 이 방법을 하는게 좋다 (in CSS)



- **게임을 만들때 사고방식**

  - game loop(game cycle): 사용자의 입력을 받거나, 시간의 경과가 생기면 화면을 다시 그려줄 때, 다시 그리기 전의 현재 화면(**상태(state)**)를 기반해서 화면을 다시 그려준다. (그 때, 상태에 기반해서 어떻게 변화를 줘야 할 지 잘 생각해 볼 것.) 프론트엔드가 프로그래밍하는 방법이 이와 거의 유사하다. 



  ※ 상태(state)란 어떤 값을 기억하고 있는 모든 것이라고 볼 수 있다. (ex: 'let score = 0')

  ※ React의 작동방식: 상태(state)를 바꾸기만하면 화면을 다시 그려줌(최대한 기존의 것을 유지한 채로 최소한의 다시 그릴 부분만 선별해 그려줌) 



- 운영체제별 개행문자
  - 맥: LF (/n)
  - 윈도우: CRLF (/r/n)
  - Git 내장기능
    - autocrlf: 이 기능을 bash에서 켜면 윈도우 사용자는 LF로만 저장되는 Git 저장소의 파일을 CRLF로 자동으로 바꿔서 작업영역으로 가져오고 다시 Git 저장소로 commit을 할 때는 CRLF를 LF로 바꿔서 commit해준다. 



**※ 오늘의 '개발자 격언': 중복된 코드를 피하라!**

※ JS에서도 HTML 태그 선택자를 쓰는 것은 좋지 않다. 



# 실습

- [RGB Challenge - Logic completed](https://codepen.io/yoonjp/pen/EdQVdR)

---

# 3. Reference

- [mouseenter - devdocs](http://devdocs.io/dom_events/mouseenter)



---

# 4. Assignment

- [Tic Tac Toe](https://www.google.co.kr/search?q=tictactoe&oq=tictactoe&aqs=chrome..69i57j0l5.2269j0j1&sourceid=chrome&ie=UTF-8) - 완성후 간단한 리뷰 예정

- RGB Challenge (logic 완성된) - 100% 똑같이 만들기 

















