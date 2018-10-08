# TIL(Today I learned)

------

# Monday, October 9, 2018

## 1. Today I learned

### Snake Game 

- 1st Snake Game Logic:
```js
import { ROWS, COLS } from './config';
import SnakeGame from './SnakeGame';

function SnakeGameLogic() {
    // 각 마디의 좌표를 저장하는 배열
    this.joints = [
        { x: 2, y: 0 },// index = 0 (head)
        { x: 1, y: 0 },// index = 1 
        { x: 0, y: 0 }// index = 2 (tail)
    ];
    // 화살표키를 계속 누르지 않아도 뱀이 이동할 수 있게 방향을 저장
    this.direction = ''
    // 먹이의 좌표
    this.fruit = { x: 3, y: 5 };
}

// 방향키를 눌렀을 때 방향이 바뀌도록 함
SnakeGameLogic.prototype.up = function () {
    this.direction = 'up'
}

SnakeGameLogic.prototype.down = function () {
    this.direction = 'down'
}

SnakeGameLogic.prototype.left = function () {
    this.direction = 'left'
}

SnakeGameLogic.prototype.right = function () {
    this.direction = 'right'
}

SnakeGameLogic.prototype.nextState = function () {
    let tailToHead = this.joints.pop() 
    // 떼어진 꼬리(붙여질 머리)의 위치를 바꿔줌
    if (this.direction === 'up') {
        tailToHead = { 
            x: this.joints[0].x, 
            y: this.joints[0].y - 1 };
    } else if (this.direction === 'down') {
        tailToHead = { 
            x: this.joints[0].x, 
            y: this.joints[0].y + 1 };
    } else if (this.direction === 'left') {
        tailToHead = { 
            x: this.joints[0].x - 1, 
            y: this.joints[0].y };
    } else if (this.direction === 'right') {
        tailToHead = { 
            x: this.joints[0].x + 1, 
            y: this.joints[0].y };
    }
    //  뱀의 머리가 벽에 부딪혔을 때나 자기 몸에 부딪혔을 때 게임이 끝남
    if (tailToHead.x >= COLS || tailToHead.x < 0 || tailToHead.y >= ROWS || tailToHead.y < 0) {
        return false;
    } else if (this.joints.some(joint => joint.x === tailToHead.x && joint.y === tailToHead.y)) {
        return false;
    } 
    // 먹이를 먹었을 때 뱀의 길이가 늘어남, 먹이가 뱀의 몸과 안 겹치게 생성됨
    if (tailToHead.x === this.fruit.x && tailToHead.y === this.fruit.y) {
        let fruitToTail = { x: this.fruit.x, y: this.fruit.y };
        this.joints.push(fruitToTail);
      do {
        this.fruit.x = Math.floor(Math.random() * COLS);
        this.fruit.y = Math.floor(Math.random() * ROWS);
      } while (
          this.fruit.x === tailToHead.x && this.fruit.y === tailToHead.y && 
          this.joints.some(joint => joint.x === this.fruit.x && joint.y === this.fruit.y));
    }
    // 떼어진 꼬리(붙여질 머리)를 붙여줌
    this.joints.unshift(tailToHead);
    return true;
}

export default SnakeGameLogic;
```

- 2nd Snake Game Logic:
```js
import { ROWS, COLS } from './config';
import SnakeGame from './SnakeGame';

function SnakeGameLogic() {
    // 각 마디의 좌표를 저장하는 배열
    this.joints = [
        { x: 2, y: 0 },// index = 0 (head)
        { x: 1, y: 0 },// index = 1 
        { x: 0, y: 0 }// index = 2 (tail)
    ];
    // 화살표키를 계속 누르지 않아도 뱀이 이동할 수 있게 방향을 저장
    this.direction = ''
    // 먹이의 좌표
    this.fruit = { x: 3, y: 5 };
}

// 방향키를 눌렀을 때 방향이 바뀌도록 함
SnakeGameLogic.prototype.up = function () {
    this.direction = 'up'
}

SnakeGameLogic.prototype.down = function () {
    this.direction = 'down'
}

SnakeGameLogic.prototype.left = function () {
    this.direction = 'left'
}

SnakeGameLogic.prototype.right = function () {
    this.direction = 'right'
}

SnakeGameLogic.prototype.nextState = function () {
    let tailToHead = {}
    // 먹이를 먹었을 때 뱀의 길이가 늘어남(꼬리를 떼지 않음), 먹이가 뱀의 몸과 안 겹치게 생성됨
    if (this.joints[0].x === this.fruit.x && this.joints[0].y === this.fruit.y) {
      do {
        this.fruit.x = Math.floor(Math.random() * COLS);
        this.fruit.y = Math.floor(Math.random() * ROWS);
      } while (this.fruit.x === this.joints[0].x && this.fruit.y === this.joints[0].y && this.joints.some(joint => joint.x === this.fruit.x && joint.y === this.fruit.y));
    } else {
      // 먹이를 먹는 경우가 아니면 꼬리를 뗌
      tailToHead = this.joints.pop();
    }
    // 떼어진 꼬리(붙여질 머리)의 위치를 바꿔줌
    if (this.direction === 'up') {
        tailToHead = {
            x: this.joints[0].x,
            y: this.joints[0].y - 1
        };
    } else if (this.direction === 'down') {
        tailToHead = {
            x: this.joints[0].x,
            y: this.joints[0].y + 1
        };
    } else if (this.direction === 'left') {
        tailToHead = {
            x: this.joints[0].x - 1,
            y: this.joints[0].y
        };
    } else if (this.direction === 'right') {
        tailToHead = {
            x: this.joints[0].x + 1,
            y: this.joints[0].y
        };
    }
    //  뱀의 머리가 벽에 부딪혔을 때나 자기 몸에 부딪혔을 때 게임이 끝남
    if (tailToHead.x >= COLS || tailToHead.x < 0 || tailToHead.y >= ROWS || tailToHead.y < 0) {
        return false;
    } else if (this.joints.some(joint => joint.x === tailToHead.x && joint.y === tailToHead.y)) {
        return false;
    }

    // 떼어진 꼬리(붙여질 머리)를 붙여줌
    this.joints.unshift(tailToHead);
    return true;
}

export default SnakeGameLogic;
```

## 2. References

- [1st Snake Game Logic - GitHub link](https://github.com/YoonJP/fds-snake-game/blob/master/src/SnakeGameLogic.js)
- [2nd Snake Game Logic - GitHub link](https://github.com/YoonJP/fds-snake-game/blob/master/src/SnakeGameLogic2.js)