# TIL(Today I learned)

---

# 09/20 (목) (윤재, 지원)

## 1. Today I learned

### 반응형 웹디자인

- One source Multi use

- Flexible (반응형) VS Adaptive (적응형) Web Design
  - Flexible Web	Design(반응형 웹 디자인)
    - 미디어쿼리(Mediaqueri)fmf 사용해 기기의 화면 해상도 크기를 감지한 후, 화면 크기에 맞게 하나의 UI 템플릿을 변형
    - 기기 최적화와 상관없이 모든 콘텐츠가 다운로드되지만, 일부 콘텐츠만 사용됨
    - 기기에 상관없이 모든 리소스를 다운로드 받아 사용하기 때문에 로드 속도가 느림
    - 기존 사이트를 전면 리뉴얼하여 개발
  - Adaptive Web Design(적응형 웹 디자인)
    - 서버 또는 브라우저 기반 기기 감지 방법을 사용해 각 기기에 적합한 UI 템플릿을 각각 제공
    - 감지된 기기에 최적환된 준비된 리소스만 다운로드 받아 사용
    - 각 기기에 필요한 리소스만 다운로드 받아 사용하기 때문에 로드 속도가 빠름
    - 기존 사이트 변경없이 개발 가능하지만, 추가 비용이 발생

- Off Canvas Pattern 
  - 큰 화면에서 볼 수 있는 화면을 작은 기기에서 보고, 숨겨진 화면을 보는 레이아웃
  - ex: Facebook

- 반응형 웹디자인 Architecture
  - Flexible Layout
    - Target / Context = Result
    - ex: 900 / 960 = 0.9375
  - CSS Media Queries
    - CSS3 모듈 중 하나로 사이트에 접속하는 장치에 따라 특정한 CSS 스타일을 사용하도록 해 줌
    - ex: 
      ```
      @charset "utf-8"; 
      
      /* Tablet & Desktop Device */ 
      @media all and (min-width: 768px)
      
      /* Tablet Device */
      @media all and (min-width:768px) and (max-width:1024px) 
      
      /* Desktop Device */
      @media all and (min-width:1025px) 
      ```

  - Responsive Image
    - 이미지 원본 크기 이상 커지지 않게 함, 가로 크기에 따라 세로 크기도 비율을 살려 조절
    - ex: 
      ```
      img {
          max-width: 100% ;
          height : auto ;
      }
      ```

- Responsive Image Issue와 해결방안
  - SVG (Scalable Vector Graphics)
    - 이미지를 아무리 확대하거나 축소해도 원래의 깨끗한 상태 그대로 유지되는 이미지
  - 다양한 이미지 포맷 대응: SVG, WebP, JPEG-WR, FlashPix
  - srcset과 sizes 속성
    -  `<img>`안에 넣는 속성으로 요청 브라우저 크기별 다른 사이즈의 이미지를 제공
    - srcset: 이미지 파일 경로
    - sizes: 파일의 크기
  - `<picture>` element
    - 상활별로 다른 이미지 표시 가능
    - picture 태그는 source 태그의 이미지가 지원되지 않는 환경에 보여줄 이미지인 img 태그를 반드시 포함해야 함


### Grid

- 웹 사이트의 레이아웃을 정할 때 자주 사용하는 기준

- 그리드 속성을 이용해 가변 그리드 레이아웃(fluid grid layer)을 쓰면 사이트의 모든 요소들을 상대적 크기로 지정해 브라우저의 크기에 따라 탄력적으로 보열 줄 수 있음

- 사이트 전체의 디자인이나 일관성을 유지시키기 편리

- 화면을 단순하게 만들면서 규칙적으로 배열해서 레이아웃을 일관성 있게 유지함

- 실습 :

  ```
  /* 그리드 컨테이너 */
  .container{
    height: 100vh;
    display: grid;
    grid-template-areas: "header";
    grid-template-rows: 100px 1fr 100px;
    grid-template-columns: repeat(12, 60px);
    grid-column-gap: 20px;
    justify-content: center;
    position: relative;
  }
  .container::before{
    content: "";
    position: absolute;
    top: 0;
    left: 50%;
    transform: translateX(-50%);
    width: 940px;
    height: 100%;
    background: repeating-linear-gradient(90deg, rgba(0,0,255,0.2), rgba(0,0,255,0.2) 60px, transparent 60px, transparent 80px);
  } 
  ```


## 2. Today I found out

윤재

```

```

지원

```

```

## 3. References

- (Grid Layout Guidebook)[https://uid.gitbook.io/css-grid]
- (Grid Calculator)[http://gridcalculator.dk/]
- (Grid Areas Layout )[https://webdesign.tutsplus.com/tutorials/css-grid-layout-using-grid-areas--cms-27264]



