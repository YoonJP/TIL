# Sun Sep 10 2018

## TIL(Today I Learned)
---
### HTML

- `skip-nav`
```
<!-- bookmark function: link to other link in the same document, to avoid repetition of same thing(navigation) in terms of Web Accessibility -->

<a href="#main" class="skip-nav">본문 바로가기</a>
```

### CSS

-
```
/* hidden contents */
/* trick to be accessible but not visible (good for Web Accessibility and search engine optimization) */

.readable-hidden, .skip-nav{
    position: absolute;
    width: 1px;
    height: 1px;
    overflow: hidden;
    margin: -1px; 
    clip: rect(0,0,0,0);
    /* clip: rect(0,0,0,0); erase completely */
}
```
-
```
.skip-nav: focus{
    width: 100%;
    height: auto;
    padding: 1em;
    background-color: black;
    color: white;
    text-align: center;
    margin: 0;
    clip: rect(auto,auto,auto,auto);
    z-index: 100;
}
```
-
```
*, *::before, *::after{
    box-sizing: border-box;
}
```

- Inline & Inline-block Element
	- Inline Element
		- respect left & right margins and padding, but **not** top & bottom
		- **cannot** have a width and height set
		- allow other elements to sit to their left and right

	- Inline-block Element
		- allow other elements to sit to their left and right
		- respect top & bottom margins and padding
		- respect height and width


