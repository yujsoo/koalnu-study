# 03월 23일 과제 기록
> 과제 수행 중 학습한 개념과 문제 발생 및 해결 과정을 기록합니다.   
> 해결 과정은 시도한 방법을 순서대로 정리했습니다.
<br /> 

##  📚 리액트 기초 1 - 5


#### 1. 리액트란?
상태 기반으로 UI를 자동으로 그려주는 컴포넌트 라이브러리
#### 2. 컴포넌트  
- UI를 쪼개서 재사용한다. 
- 어떤걸 컴포넌트로? 반복, 기능별로, 하나에 한 기능만  
- 컴포넌트를 이렇게 나누어라 저렇게 나누어라 사실 절대적 기준은 없다. 시스템에 따라, 회사에 따라 그 규칙은 바뀔수 있다.  
- 컴포넌트 이름은 반드시 **대문자**로 시작해야한다.

#### 3. 상태 (State)  

- #### 데이터가 바뀌면 화면도 바뀐다.
    ```js
    const [count, setCount] = useState(0)  
    ```  


- #### 왜 `state`는 `const`로 선언할까?  
    `const`라서 값이 '안 바뀐다'가 아니라 변수 재할당을 막기 위해서이다.  
     JavaScript에서 `const`는 **변수 자체를 다시 할당하지 못하게 하는 것**이다. React에서는 이렇게 하면 모른다.  

    ```js
    count = 10
    ``` 

    React는 변수 변경을 감지하지 못하기 때문에 반드시 `setState`를 통해서만 변경해야 한다.
  ```js
  setCount(10)
  ```

- #### 동작 흐름
  `state`값 변경 > React에게 변경 알림 > 컴포넌트 다시 실행 > UI 업데이트


- #### 왜 일반 변수는 값을 기억 못할까?

    ```js
    function Counter() {
      let count = 0
      return <div>{count}</div>
    }
    ```
    컴포넌트는 함수라서 매번 다시 실행된다

    ```js
    Counter()
    Counter()
    ```
    함수가 실행될 때마다 다음 렌더링에서도 다시 0부터 시작

  - count 생성 (0)
  - 사용
  - 함수 종료 → count 삭제  

#### 4. props
- 부모 컴포넌트가 자식 컴포넌트에게 데이터를 전달할 때 사용하는 값
- 읽기 전용(Read-only)이며, 자식 컴포넌트에서 직접 수정할 수 없다. 변경은 부모를 통해서만 가능하다.
- 데이터는 항상 부모 → 자식 방향으로만 전달

    ```js
    function Parent() {
      return <Child name="지수" />
    }
    
    function Child(props) {
      return <div>{props.name}</div>
    }
    ```
  <br />

## ⚠️  Node 버전 불일치로 인한 오류 및 해결 과정

### 문제
`npm run dev` 실행 후 아래 오류 발생 

```bash
You are using Node.js 20.17.0. Vite requires Node.js version 20.19+ or 22.12+. 
Please upgrade your Node.js version.
```

### 원인
현재 노드 버전이 vite 요구사항 보다 낮았음.


### 해결

1. [[참조](https://codecoco.tistory.com/103)] 해당 글을 참고하여 아래 코드를 실행시킴.
    ```bash
   sudo npm install
    ```  
   아래와 같은 오류가 다시 발생한다.
    ```bash
   EACCES: permission denied
    ```
    
   `sudo npm install`을 하면 `npm install`을 내 계정이 아니라 root(관리자) 계정으로 실행한 것이다. 쉽게 설명하자면,  
   권한 상태가 관리자 파일이니까 일반 사용자는 건드리지마! 가 된다. 이후에 `npm install`을 하면 일반 사용자가 실행하려고 했는데 root라서 건드리지 못한다.


2. Node 버전 업그레이드하기
   ```bash
   sudo n latest
    ```
3. npm 권한 복구
   ```bash
   sudo chown -R $(whoami) ~/.npm
    ```
4. npm 권한 복구
   ```bash
   rm -rf node_modules package-lock.json  
   npm cache clean -f
    ```
5. 패키지 재설치
    ```bash
    npm install
    ```
   
