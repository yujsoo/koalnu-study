# 03월 24일 - 03월 25일 과제 기록
> 과제 수행 중 학습한 개념과 문제 발생 및 해결 과정을 기록합니다.   
> 해결 과정은 시도한 방법을 순서대로 정리했습니다.
<br /> 

## 가위바위보 게임 1~6

### ⚠️ Cannot read properties of undefined

### 문제
가위바위보 선택 값을 `state`로 관리하고, 해당 값에 따라 이미지를 보여주려고 했다.  
하지만 게임 시작 전에는 `state`의 초기값이 `null`이기 때문에,  
`choice[state]`가 `undefined`가 되는 상황이 발생했다.

이 상태에서 `.img`에 접근하려고 하면서 아래와 같은 에러가 발생했다.
```bash
Cannot read properties of undefined (reading 'img')
```  


### 원인
```jsx
img={choice[state].img}
```
`choice[state]` 이게 항상 존재한다는 보장이 없다. 반드시 ?. 또는 조건문 필요하다.

```jsx
img={state ? choice[state].img : '?'}
```
해당 코드는 `state`가 있냐 없냐만 본다. 그래서 `choice`에 있는 값이면 괜찮지만 없는 값이 들어갈 경우  
에러가 난다. 그래서 '?' 이렇게 처리를 하지 못한다.

### 해결
choice 객체 안에서 판단해야하므로, `choice[state]`가 있냐 없냐로 판단해야 한다.
- `choice[state]` 없으면 → `undefined`  
- `undefined?.img` → 에러 없이 `undefined` 반환

즉, `state`가 존재해도 `choice[state]`가 undefined일 수 있다.

#### Optional Chaining  
?. 의미 : "이 값이 있으면 접근하고, 없으면 그냥 멈춰"
```jsx
import person from '../assets/person.png';
import computer from '../assets/computer.png';

const Box = ({ title, item, result }) => {
  // 플레이어별 null img
  const nullImg = title === '나' ? person : computer;

  return <img src={item?.img || nullImg} alt={''} className={'img'}/>
}
```
```jsx
item?.img   // 안전하게 접근
|| nullImg  // 없으면 기본값
```

### ⚠️ 랜덤 선택 문제

### 문제

가위, 바위, 보 데이터를 객체(Object) 형태로 관리하고 있었고,
컴퓨터의 선택을 랜덤으로 구현하려고 할 때 문제가 발생했다.
- 객체는 배열처럼 index로 접근할 수 없음
- 랜덤으로 요소를 선택할 수 없음

```js
const choice = {
  scissors: { name: 'scissors', img: ... },
  rock: { name: 'rock', img: ... },
  paper: { name: 'paper', img: ... }
}
```

#### 문제 코드

```js
choice[0]
```

### 원인

#### 1. 객체(Object)의 특징

* key-value 구조
* 순서(index)가 없음
* 접근 방식: `choice.rock` 또는 `choice['rock']`

즉, 숫자 index로 접근 불가능

#### 2. 배열(Array)의 특징

* index 기반 구조
* `arr[0]`, `arr[1]` 접근 가능
* 랜덤 선택에 적합

랜덤 선택은 결국 "몇 번째 요소"를 고르는 것

### 해결

#### 1. 객체를 배열로 변환

```js
const keys = Object.keys(choice) // ['scissors', 'rock', 'paper']
```


#### 2. 랜덤 index 생성

```js
const randomIndex = Math.floor(Math.random() * keys.length)
```


#### 3. 랜덤 key 선택

```js
const randomKey = keys[randomIndex]
```

#### 4. 최종 값 반환

```js
return choice[randomKey]
```

#### 5. 최종 코드

```js
const randomChoice = () => {
  const keys = Object.keys(choice)
  const randomIndex = Math.floor(Math.random() * keys.length)
  const randomKey = keys[randomIndex]
  return choice[randomKey]
}
```

#### 💡 Object.values()를 활용하는 법

`Object.values()`를 활용하면 객체의 value만 배열로 반환하여   
더 간단하게 랜덤 값을 뽑을 수 있다.
```js
const values = Object.values(choice)

console.log(values);

0 : {name: 'scissors', img: '/rock-paper-scissors-game/src/assets/scissors_me.png'}
1 : {name: 'rock', img: '/rock-paper-scissors-game/src/assets/rock_me.png'}
2 : {name: 'paper', img: '/rock-paper-scissors-game/src/assets/paper_me.png'}

return values[Math.floor(Math.random() * values.length)]; //랜덤 값 최종 도출
```
 

