--- 
title: "react에서 todoList 만들기 (1)"
layout: single
categories: "react 공부"
tags: [todo-list | react, 공부]
author_profile: false
sidebar: 
  nav: "main"
toc: true
toc_sticky: true
---

# 1. cra 설치
`npx create-react-app my-pages`

---

# 2. react-router-dom v6 설치
`npm install react-router-dom --save`

---

# 3. components 폴더 + 컴포넌트 생성
* Template.js
  * 뼈대 역할
* TodoList.js
  * 할일
* TodoInsert.js
  * Todo 입력
* TodoItem.js
  * 목록 하나의 아이템

---

# 4. 컴포넌트 기능 구현
## App.js

* 더미 데이터 생성 후 Template, TodoList에 넘겨주기

``` jsx
function App() {
  const [todos, setTodos] = useState([
    {
      id: 1,
      text: '할일 1',
      checked: true,
    },
    {
      id: 2,
      text: '할일 2',
      checked: false,
    },
    {
      id: 3,
      text: '할일 3',
      checked: true,
    },
  ]);

  return (
    <Template todoLength={todos.length}>
      <TodoList todos={todos} />
      <div className='add-todo-button'>
        <MdAddCircle />
      </div>
    </Template>
  );
}
```

---

## Template.js
* App.js에서 넘겨받은 `todoLength`로 총 **`todos` 개수 표시**

``` jsx
function Template({ children, todoLength }) {
  return (
    <>
      <div className="Template">
        <div className="title">오늘의 할 일 ({todoLength})</div>
        <div>{children}</div>
      </div>
    </>
  );
}
```

---

## TodoList.js
* App.js에서 받아온 **todos를 `map()`으로 반복해서 `TodoItem`으로 보내주기**
  * `todos` : 할 일 목록
  * `map()`함수를 사용하려면 고유한 key값도 보내야 하기 떄문에 더미데이터에서 설정한 id값 넘겨주기

``` jsx
function TodoList({ todos }) {
  return (
    <>
      <div>
        {todos.map((todo) => (
          <TodoItem todo={todo} key={todo.id} />
        ))}
      </div>
    </>
  );
}
```

---

## TodoItem.js
* TodoList에서 넘겨받은 **`todo`를 `text`로 표시**
  * todo에 있는 속성을 `구조분해 문법`을 이용해서 가져오기
* **checked**가 true면 체크 되어 있는 아이콘
false면 체크가 안되어 있는 아이콘 적용

``` jsx
import { MdCheckBox, MdCheckBoxOutlineBlank } from 'react-icons/md';

function TodoItem({ todo }) {
  const { text, id, checked } = todo;
  return (
    <>
      <div className="TodoItem">
        <div className={`content ${checked ? 'checked' : ''}`}>
          {checked ? <MdCheckBox /> : <MdCheckBoxOutlineBlank />}
          <div>{text}</div>
        </div>
      </div>
    </>
  );
}
```

---

# 5. css
## React Icons 설치 (svg)
`npm install react-icons --save`

---

![](./images/1.png)

## Template.js

``` css
.Template {
  padding-top: 20px;
  max-height: 100vh;
}

.title {
  width: 90vw;
  margin-left: auto;
  margin-right: auto;

  padding-bottom: 20px;
  font-size: 1.5rem;
  font-weight: bold;
  color: #6c567b;
}
```

## TodoList.js

``` css
.TodoList {
  width: 90vw;
  margin-left: auto;
  margin-right: auto;
  padding-bottom: 20px;
}
```

## TodoItem.css
``` css
.TodoItem {
  margin-left: auto;
  margin-right: auto;

  border-radius: 5px;
  box-shadow: 1px 2px 5px 1px #f67280;
  padding: 1rem;
  display: flex;
  align-items: center;
}

.TodoItem + .TodoItem {
  margin-top: 15px;
}

.content {
  cursor: pointer;
  flex: 1;
  display: flex;
  align-items: center;
}

.content svg {
  font-size: 1.5rem;
  color: #f67280;
}

.content .text {
  margin-left: 0.5rem;
  flex: 1;
} 

.content.checked .text {
  color: #6c567b;
  text-decoration: line-through;
  cursor: pointer;
  opacity: 0.6;
}
```

## App.js
``` css
.add-todo-button {
  position: fixed;
  right: -5px;
  bottom: 0;
  z-index: 100;
  width: 100px;
  height: 100px;
  cursor: pointer;
  font-size: 5rem;
  color: #f67280;
}
```