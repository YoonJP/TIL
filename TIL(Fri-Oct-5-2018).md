# TIL(Today I learned)

------

# 10/05 (금)

## 1. Today I learned

## Quiz 실습

### array type

### 문제 5

수 타입의 값으로만 이루어진 두 배열을 입력받아, 다음과 같이 동작하는 함수를 작성하세요.
- 두 배열의 같은 자리에 있는 요소를 더한 결과가 새 배열의 요소가 됩니다.
- 만약 입력받은 두 배열의 길이가 갖지 않다면, 긴 배열에 있는 요소를 새 배열의 같은 위치에 포함시키세요.

예:
```
addArray([1, 2, 3], [4, 5, 6, 7]) -> [5, 7, 9, 7]
```

풀이:
```js
function addTwoArray(arr1, arr2) {
  let twoArray = []
  for (let i = 0; i < arr2.length; i++) {
    if (i < arr1.length) {
      twoArray.push(arr1[i] + arr2[i])
    } else {
      twoArray.push(arr2[i])
    }
  }
  return twoArray
}

addTwoArray([1, 2, 3], [4, 5, 6, 7]) // [5, 7, 9, 7] 반환
```

강사님 풀이(1): 
```js
// (이 방법은 원본 배열이 바뀐다.)
function addArray(arr1, arr2) {
  let longer
  let shorter
  if (arr1.length > arr2.length) {
    longer = arr1
    shorter = arr2
  } else {
    longer = arr2
    shorter = arr1
  }
  for (let i = 0; i < shorter.length; i++) {
    longer[i] += shorter[i] // longer[i] = longer[i] + shorter[i]
  }
  return longer
}

addArray([1, 2, 3], [4, 5, 6, 7])
```

강사님 풀이(2): 
```js
// (이 방법은 원본 배열이 바꾸지 않는다.)
function addArray(arr1, arr2) {
  let longer
  let shorter
  if (arr1.length > arr2.length) {
    longer = arr1.slice() // slice 메소드는 배열을 복사할 때도 쓰인다. 
    shorter = arr2.slice() // 원본 배열을 변경하지 않기 위해 사본을 만들어준다. 
  } else {
    longer = arr2.slice()
    shorter = arr1.slice()
  }
  for (let i = 0; i < shorter.length; i++) {
    longer[i] += shorter[i] // longer[i] = longer[i] + shorter[i]
  }
  return longer
}

addArray([1, 2, 3], [4, 5, 6, 7])
```

### 문제 6

배열을 입력받아, 배열의 요소 중 두 개를 선택하는 조합을 모두 포함하는 배열을 작성하세요.

예:
```
combination([1, 2, 3]); -> [[1, 2], [1, 3], [2, 3]]
```

풀이:
```js
function combination(arr) {
  let memory = []
  for (let i = 0; i < arr.length - 1; i++) {
    for (let j = i + 1; j < arr.length; j++) {
      memory.push([arr[i], arr[j]])
    }
  }
  return memory
}

combination([1, 2, 3])
```


### 문제 7

'금액'과 '동전의 종류가 들어있는 배열'를 입력받아, 최소한의 동전을 사용해서 금액을 맞출 수 있는 방법을 출력하는 함수를 작성하세요.
(단, 동전의 종류가 들어있는 배열에는 큰 동전부터 순서대로 들어있다고 가정합니다.)

예:
```
coins(263, [100, 50, 10, 5, 1]);
// 출력
100
50
10
1
1
1
```

풀이:
```js
function coins(input, coin) {
  let currentIndex = 0
  let remain = input
  while (remain > 0) {
    if (remain >= coin[currentIndex]) {
      remain -= coin[currentIndex]
      console.log(coin[currentIndex])
    } else {
      currentIndex++
    }
  }
}

coins(263, [100, 50, 10, 5, 1])
// 출력: 
// 100
// 100
// 50
// 10
// 1
// 1
// 1
```

강사님 풀이1(방어적 코드: 만약, 남은 액수가 내가 지금 보고있는 동전보다 적을 경우):
```js
function coins(input, coinTypes) {
  // 남은 액수
  let remain = input
  // 현재 내가 보고있는 동전
  let currentIndex = 0
  while (remain > 0 && currentIndex < coinTypes.length) {
    // 남은 액수가 내가 지금 보고있는 동전보다 크거나 같으면 
    if (remain >= coinTypes[currentIndex]) {
      console.log(coinTypes[currentIndex])
      remain -= coinTypes[currentIndex]
    } else {
      // 다음 동전으로 넘어간다.
      currentIndex++
    }
  }
}

coins(263, [100, 50, 10, 5]) // 
```

강사님 풀이2(방어적 코드: 동전 타입이 내림차순이 아니게 입력된 경우):
```js
function coins(input, coinTypes) {
  // coinTypes를 내림차순 정렬
  coinTypes.sort((x, y) => y - x)
  // 남은 액수
  let remain = input
  // 현재 내가 보고있는 동전
  let currentIndex = 0
  while (remain > 0 && currentIndex < coinTypes.length) {
    // 남은 액수가 내가 지금 보고있는 동전보다 크거나 같으면 
    if (remain >= coinTypes[currentIndex]) {
      console.log(coinTypes[currentIndex])
      remain -= coinTypes[currentIndex]
    } else {
      // 다음 동전으로 넘어간다.
      currentIndex++
    }
  }
}

coins(263, [50, 100, 10, 5, 1])
```

### array type (추가 문제)

문제 1. 배열을 입력받아, 해당 배열에 들어있는 요소들 중 최대값을 찾는 함수를 작성하세요. (루프를 이용하세요)

예:

```js
max([3, 1, 4, 5, 2]) // -> 5
```

풀이:
```js
function max(arr) {
  let maxNumber = arr[0]
  for (let i = 1; i < arr.length; i++) {
    if (maxNumber - arr[i] < 0) {
      maxNumber = arr[i]
    }
  }
  return maxNumber
}

max([3, 1, 4, 5, 2]) // 5 반환
```

강사님 풀이(reduce 이용):
```js
function max(arr) {
  // reduce를 쓸 때
  // '누적값의 역할'이 무엇인지를 잘 정하는 것이 중요하다.

  // 누적값: 지금까지 봤던 숫자 중에 제일 큰 수 
  return arr.reduce((acc, item) => {
    // 안에 들어있는 함수의 반환값이, 다음 단계의 누적값이 된다.
    if (acc > item) {
      return acc
    } else {
      return item
    }
  }, 0)
}

max([3, 1, 4, 5, 2]) // 5 반환
```

강사님 풀이(삼항 연산자 이용):
```js
function max(arr) {
  return arr.reduce((acc, item) => acc > item ? acc : item, 0)
}

max([3, 1, 4, 5, 2]) // 5 반환
```

강사님 풀이(배열의 요소에 음수가 올 경우):
```js
function max(arr) {
  return arr.reduce((acc, item) => acc > item ? acc : item, -Infinity) // 음수중 가장 작은 수 '-Infinity', 초기 누적값(acc)를 생략하면 의도치 않은 결과가 나올 수 있으므로 주의!!
}

max([-3, -1, -4, -5, -2]) // 5 반환
```

※ Math.max()메소드를 쓰면 입력받은 배열의 요소중 최대값을 바로 구할 수 있다. 


### math type

### 문제 7

수 타입의 값으로만 이루어진 배열을 입력받아, 그 값들의 표준편차를 구하는 함수를 작성하세요.

강사님 풀이:
```js
function stdDev(arr) {
  // arr의 평균 구하기
  const sum = arr.reduce((acc, item) => acc + item, 0)
  const mean = sum / arr.length
  // 각 요소에 대한 편차 구하기 (평균 = 값 - 평균)
  const ps = arr.map(item => item - mean) // (map 메소드는 배열의 각 요소에 함수를 적용해서, 그 반환값을 요소로 갖는 새로운 배열을 만듦
  // 각 요소에 대해 제곱하기)
  const powers = ps.map(item => item ** 2) // item ** 2 =item * item
  // 위 제곱한 배열의 평균 구하기
  const vv = powers.reduce((acc, item) => acc + item, 0) / powers.length
  // 루트 씌우기
  return Math.sqrt(vv)
}

stdDev([1, 2, 3, 4, 5]) // 1.4142135623730951 반환
```

### Snake Game (in progress)

```js
import {ROWS, COLS} from './config';
import SnakeGame from './SnakeGame';

// NOTE: ROWS, COLS에는 행의 개수, 열의 개수가 저장되어 있습니다.
// 이 변수를 활용해서 코드를 작성하세요!

function SnakeGameLogic() {
  // 각 마디의 좌표를 저장하는 배열
  this.joints = [
    {x: 5, y: 7},
    {x: 4, y: 7},
    {x: 3, y: 7},
    {x: 2, y: 7},
    {x: 1, y: 7},
    {x: 0, y: 7},
  ]
  ;

  // 먹이의 좌표
  this.fruit = {x: 3, y: 5};
}

SnakeGameLogic.prototype.up = function() {
  // 위쪽 화살표 키를 누르면 실행되는 함수
  
  console.log('up');
}

SnakeGameLogic.prototype.down = function() {
  // 아래쪽 화살표 키를 누르면 실행되는 함수
  console.log('down');
}

SnakeGameLogic.prototype.left = function() {
  // 왼쪽 화살표 키를 누르면 실행되는 함수
  console.log('left');
  if ('right') {
    this.joints[1].x -= 1
    this.joints[0].x -= 1
    this.joints[2].x -= 1
    this.joints[3].x -= 1
    this.joints[4].x -= 1
    this.joints[5].x -= 1
  }
}

SnakeGameLogic.prototype.right = function() {
  // 오른쪽 화살표 키를 누르면 실행되는 함수
  // SnakeGameLogic.joints[0].slice()
  console.log('right');
  if ('right') {
    this.joints[1].x += 1
    this.joints[0].x += 1
    this.joints[2].x += 1
    this.joints[3].x += 1
    this.joints[4].x += 1
    this.joints[5].x += 1
  }
}

SnakeGameLogic.prototype.nextState = function() {
  // 한 번 움직여야 할 타이밍마다 실행되는 함수
  // 게임이 아직 끝나지 않았으면 `true`를 반환
  // 게임이 끝났으면 `false`를 반환
  console.log(`nextState`);
  return true;
}

export default SnakeGameLogic;
```

## 2. References

- [선택 정렬 - Wikipedia](https://ko.wikipedia.org/wiki/%EC%84%A0%ED%83%9D_%EC%A0%95%EB%A0%AC)