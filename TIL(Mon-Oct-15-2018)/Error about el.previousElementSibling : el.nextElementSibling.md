# I. el.previousElementSibling 오류

### Node.previousSibling

- The `Node.previousSibling` read-only property returns the node immediately preceding the specified one in its parent's [`childNodes`](https://devdocs.io/dom/node/childnodes) list, or `null` if the specified node is the first in that list.
- `Node.previousSibling`읽기 전용 속성은 부모의 자식 노드 목록에서 지정된 노드 바로 앞에 오는 노드를 반환하거나, 지정된 노드가 그 자식 목록의 첫 번째라면 `null`을 반환한다. 



### el.insertBefore

- 강사님 설명:

- - el.insertBefore(newChild, beforeWhat) - 원하는 위치에 자식 요소를 삽입하기 (두 번째 인수(beforeWhat)에 null 값을 넣으면 appendChild랑 같은 기능을 한다.)



- Syntax(in ‘devdocs’):

- ```js
  var *insertedNode* = *parentNode*.insertBefore(*newNode*, *referenceNode*);
  ```

- - If *referenceNode* is null, the *newNode* is inserted at the end of the list of child nodes.
  - 만약, <u>참조 노드(referenceNode)가 null이면, 새로운 노드(또는 삽입하려는 노드, the newNode)는 자식 노드들의 항목 마지막에 추가</u>된다. 



따라서 `todoItemEl`이 자식 노드들의 목록의 제일 위에 위치하고 있을 때, `todoListEl.insertBefore(todoItemEl, todoItemEl.previousElementSibling)`처럼 `el.insertBefore`를 이용해 todoItemEl.previousElementSibling 앞에 `todoItemEl`를 넣으려고 할 때 `todoItemEl`은 자식 노드들의 목록 제일 위에 위치하고 있으므로todoItemEl.previousElementSibling이 없다. 이로 인해 2번째 인수(the referenceNode)에 null값이 오게되고, 2번째 인수(the referenceNode)에 null값이 오면 `todoItemEl`(newNode)는 문법에 따라 `parentNode`의 자식 노드들 항목 마지막에 추가된다. 



# II. el.NextElementSibling 오류

### Node.nextSibling

- The `Node.nextSibling` read-only property returns the node immediately following the specified one in its parent's [`childNodes`](https://devdocs.io/dom/node/childnodes) list, or `null` if the specified node is the last node in that list.
- `Node.nextSibling`읽기 전용 속성은 부모의 자식 노드 목록에서 지정된 노드 바로 뒤따라 오는 노드를 반환하거나, 지정된 노드가 자식 노드 목록의 마지막 노드일 경우 `null`을 반환한다. 



### el.insertBefore

- comment below Example 2:

- ```js
  parentDiv.insertBefore(sp1, sp2.nextSibling);
  ```

- - If sp2 does not have a next sibling, then it must be the last child — sp2.nextSibling returns null, and sp1 is inserted at the end of the child node list (immediately after sp2).
  - 만약 sp2가 다음 형제가 없다면, 이것은 마지막 자식(노드)여야만 한다—<u>sp2.nextSibling은 null을 반환하고, sp1은 자식 노드 목록의 마지막에 삽입된다(sp2 바로 뒤에).</u>
















