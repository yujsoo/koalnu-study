# 03월 27일 - 03월 28일 과제 기록
> 과제 수행 중 학습한 개념과 문제 발생 및 해결 과정을 기록합니다.   
> 해결 과정은 시도한 방법을 순서대로 정리했습니다.
<br /> 

## 날씨앱 만들기 1~9

### 학습 내용 정리  

### 1. 현재 폴더에서 프로젝트 생성
기본 npm 프로젝트 생성 : 현재 폴더에서 바로 시작하려면?

```bash
npm create vite@latest .
```

### 2. HTML Geolocation API
사용자의 현재 위치(위도, 경도)를 가져와 날씨 API에 활용 [[참고](https://www.w3schools.com/html/html5_geolocation.asp)] 
```jsx
navigator.geolocation.getCurrentPosition((position) => {
 const lat = position.coords.latitude; 
 const lon = position.coords.longitude; 
});
```

### 3. OpenWeather API 연동
위도, 경도를 이용해 날씨 데이터 호출  

```jsx
  const [weather, setWeather] = useState(null); // 날씨 담을 state

  const getWeatherByCurrentLocation = async (lati, long) => {
    try {
      // OpenWeather API 사용
      let url = `https://api.openweathermap.org/data/2.5/weather?lat=${lati}&lon=${long}&appid=API_KEY&units=metric`;
      
      // fetch로 API 요청 (비동기 처리)
      // await: 응답이 올 때까지 기다림
      let response = await fetch(url);
      
      // 응답 데이터를 JSON 형태로 변환
      let data = await response.json();
      
      // 받아온 날씨 데이터를 state에 저장
      setWeather(data);
    }catch (error) {
      // API 요청 실패 시 (네트워크 오류 등)
      console.error(error);
    }
  }
```

### 4. OpenWeather API : 날씨 아이콘 표시
API에서 받은 icon 값을 활용해 이미지 출력하기  
나중에는 description에 따라 커스텀 아이콘으로 적용하였다.

```jsx
const icon = item.weather[0].icon; 

<img src={`https://openweathermap.org/img/wn/${icon}@2x.png`} />
```

---

### ⚠️ Cannot read properties of null
### 문제
```bash
Cannot read properties of null
```
### 원인
```jsx
{item.main.temp}
```
데이터가 없을 때 접근해서 생기는 오류이다.  
React는 먼저 렌더링 , 나중에 데이터가 들어오는 순서이다
### 해결
```jsx
if (!item) return null;

return (
    <b className='name'>{item.weather[0].description}</b>
    <b className='temp'>{item.main.temp}°C</b>
);
```