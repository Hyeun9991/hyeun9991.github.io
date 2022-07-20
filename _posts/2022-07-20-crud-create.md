--- 
title: "React CRUD 기능 구현하기 [Create]"
layout: single
categories: "CRUD"
tags: [react-crud, Create]
author_profile: false
sidebar: 
  nav: "main"
toc: true
toc_sticky: true
---

# Create 기능 구현

# 로직
## Create 페이지로 넘어가고, 사용자의 입력값 구해서 전달하기
### [App.js]
1. Create 링크 클릭 시 Create 모드로 넘어가야 함 
    * `onClick()` 이벤트 생성 후 리로드 막고 모드를 `CREATE` 모드로 변경
    * `Create` 컴포넌트 생성 후 `CREATE` 모드일 때 보여주기
<br/>

### [Create.js]
2. 사용자의 입력값(props) 구하기
    * `e.target.title.value`
    * `target` : form 태그
    * `title`이라는 이름을 가진 태그의 `value`
<br/>

3. 사용자가 입력한 props를 submit했을 때 전달하기
    * 전달받은 `onCreate(title, body)`함수 호출해서 전달하기
<br/>

## 전달받은 데이터를 topics에 추가하기
### [App.js]
1. `nextId` state 생성
    * 다음에 생성할 글의 id를 구하기 위해서 생성
<br/>

2. `topics`를 state로 변경
<br/>

3. `newTopic`이라는 객체를 만들어서 전달받은 데이터 (id, title, body) 넣기
    * id는 `nextId`
<br/>

4. `newTopics`라는 객체에 `topics`를 복제
    * **범객체의 state를 변경하기 위해선 객체 또는 배열을 복제한 후 복제본을 변경해야 state가 변경되면서 컴포넌트가 다시 실행이 됨**
<br/>

5. 변경된 복제본을 `setTopics()`에 넣어서 state 변경
<br/>

6. 글 작성 후 추가된 글로 바로 가기
    * `READ`모드로 변경

# Source Code

## App.js
``` jsx
function App() {
  const [mode, setMode] = useState('MAIN');
  const [id, setId] = useState(null);
  const [nextId, setNextId] = useState(4); // 추가
  const [topics, setTopics] = useState([ // state로 변경
    { id: 1, title: 'Html', body: 'Html is...' },
    { id: 2, title: 'Css', body: 'Css is...' },
    { id: 3, title: 'JavaScript', body: 'JavaScript is...' },
  ]);

  let content = null;
  if (mode === 'MAIN') {
    // 생략
  } else if (mode === 'READ') {
    // 생략
  } else if (mode === 'CREATE') { // create 모드 생성
    content = (
      <Create
        onCreate={(_title, _body) => {
          const newTopic = { id: nextId, title: _title, body: _body };
          const newTopics = [...topics];
          newTopics.push(newTopic);
          setTopics(newTopics);
          setMode('READ');
        }}
      />
    );
  }

  return (
    <div className="App">
      // 생략
      <a
        href="/create"
        onClick={(e) => {
          e.preventDefault();
          setMode('CREATE');
        }}
      >
        Create
      </a>
    </div>
  );
}
```

## Create.js
``` jsx
const Create = ({ onCreate }) => {
  return (
    <article>
      <h2>Create</h2>
      <form onSubmit={(e) => {
        e.preventDefault();
        const title = e.target.title.value;
        const body = e.target.body.value;
        onCreate(title, body);
      }}>
        <p><input type="text" name='title' placeholder='제목을 입력하세요'/></p>
        <p><textarea name="body" placeholder='내용을 입력하세요'></textarea></p>
        <input type="submit" value="Create" />
      </form>
    </article>
  )
}
```
<br/>

[맨위로 가기](#create-기능-구현)