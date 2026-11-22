## 프로젝트 소개
### 프로젝트 정보
- useEffect와 useRef를 이용해 타이머와 입력창 기능을 구현하는 React 실습 프로젝트이다.
- 4개의 Lab을 통해 타이머 실행, 중지, 초기화와 입력창의 Focus, Clear 기능을 확인한다.
- Starter Project : https://github.com/2026-2-OSS/week12_starter_project
- GitHub Classroom Repository : https://github.com/2026-2-OSS/assign12-c02-22500819

## 설치 및 실행 방법
### 설치 및 실행
프로젝트 폴더에서 다음 명령어를 실행한다.

```bash
npm install
npm run dev
```

터미널에 표시된 Local 주소를 브라우저에서 열면 실행 화면을 볼 수 있다.

```bash
npm run build
```

배포 전에는 위 명령어로 빌드가 정상적으로 완료되는지 확인한다.

### 실행 화면
- 화면 상단에는 프로젝트 제목과 설명이 표시되고, 아래에는 LAB 1부터 LAB 4까지 선택하는 버튼이 있다.
- 처음에는 LAB 1이 표시되며, 다른 Lab 버튼을 누르면 해당 실습 화면으로 전환된다.
- [실행 화면 캡처 추가 필요]

## 소스코드 분석
### 프로젝트 구조
- App.jsx : 실습을 선택하는 기능과 화면 전환 기능을 담당한다. Lab 1이 기본값이며 2, 3, 4 각 버튼을 누르면 해당 Lab으로 다시 랜더링하여 마운트한다.
- Lab1_EffectTimer.jsx : useEffect를 이용하여 타이머 기능을 구현하는 컴포넌트이다.
- Lab2_TimerControl.jsx : useEffect 기능을 이용한 타이머 기능에 start, stop, reset 기능까지 구현한 컴포넌트이다.
- Lab3_InputFocus.jsx : useRef를 이용해 Focus, Clear 기능을 지원하는 Input Focus 컴포넌트이다.
- Lab4_RefTimer.jsx : 타이머 기능을 useRef를 이용하여 구현한 컴포넌트이다.

### useEffect
#### Effect, Dependency Array 및 Cleanup의 동작 원리
Effect는 랜더링과는 별개의 작업일 때, 예를 들어 타이머처럼 랜더링 이후에도 지속적으로 값이 변화해야 하는 외부 시스템 작업에서 사용한다. Lab2에서 화면이 랜더링 된 후, Start 버튼을 누르면 useEffect로 타이머가 시작된다. 그리고 타이머가 진행되는 동안 지속적으로 새로운 값을 표시해야하므로 useState가 랜더링하지만, Effect는 변화하지 않는다. 그러다가 다른 Lab으로 넘어가거나 Stop을 누르면 언마운트 될 때 'cleanup' 작업을 수행해 작업을 종료한다. 
Dependency Array는 이 Effect가 어떤 값에 의존하는지 적어줄 때 사용한다. 

### useRef
#### .current 속성과 useState의 차이
.current는 useRef가 만든 객체 안에 값을 보관하는 속성이다. useState와 비슷하게 값을 읽거나 바꿀 때 사용하지만, 가장 큰 차이점은 값을 바꿨을 때 useState는 재랜더링을 요청하지만 .current는 재랜더링을 요청하지 않는다. 값만 기억하면 될 때 useRef의 .current 속성을 사용한다.

### 코드 분석
```jsx
useEffect(() => {
  const id = setInterval(() => {
    setCount(prev => prev + 1);
  }, 1000);
  return () => clearInterval(id);
}, []);
```

setInterval은 1초마다 이전 count에 1을 더하고, 상태 변경으로 화면이 다시 렌더링된다. Cleanup은 이전 타이머를 제거한다. 

```jsx
const inputRef = useRef(null);
const focusInput = () => {
  inputRef.current.focus();
};
const clearInput = () => {
  inputRef.current.value = '';
  inputRef.current.focus();
};
<input ref={inputRef} />
```

useRef는 input을 참조할 객체를 만든다. input이 마운트되면 React가 실제 DOM 요소를 inputRef.current에 저장한다. Focus 버튼에서 focusInput을 호출하면 입력창에 포커스가 이동하고, Clear 버튼에서 clearInput을 호출하면 내용을 지운 뒤 다시 포커스한다. ref를 통한 DOM 조작은 컴포넌트의 재렌더링을 요청하지 않는다.

## Lab 구현 내용 및 실행 결과
### LAB 1. useEffect Timer
- useEffect 안에서 setInterval을 만들어 1초마다 Count가 증가하도록 구현했다.
- setCount(prev => prev + 1)을 사용해 이전 값을 기준으로 Count를 증가시킨다.
- Dependency Array는 빈 배열로 설정하고, Cleanup에서 clearInterval을 호출한다.
- 실행 결과 : 화면을 열면 Count가 자동으로 증가하는 것을 확인했다.

### LAB 2. Timer Start / Stop
- running 상태가 true일 때만 Effect에서 타이머를 생성한다.
- Start는 running을 true로, Stop은 false로 변경한다. Reset은 타이머를 멈추고 Count를 0으로 초기화한다.
- 실행 중에는 Start 버튼을 비활성화하고, Effect의 Cleanup에서 기존 타이머를 정리한다.
- 실행 결과 : Start를 누르면 Count가 증가하고, Stop을 누르면 현재 값에서 멈추며, Reset을 누르면 0으로 돌아가는 것을 확인했다.

### LAB 3. Input Focus with useRef
- inputRef를 만들고 입력창의 ref 속성에 연결했다.
- Focus는 입력창으로 포커스를 이동하고, Clear는 입력 내용을 지운 뒤 다시 포커스를 이동한다.
- 실행 결과 : Focus를 누른 뒤 문자를 입력할 수 있었고, Clear를 누르면 내용이 지워지고 입력창에 포커스가 유지되는 것을 확인했다.

### LAB 4. Timer with useRef
- timerRef.current에 타이머 ID를 저장하고, 이미 ID가 있으면 Start 함수를 바로 종료해 중복 실행을 막는다.
- Stop은 타이머를 해제하고 ref를 null로 변경한다. Reset은 Stop을 실행한 뒤 Count를 0으로 만든다.
- 다른 Lab으로 이동할 때는 Cleanup에서 타이머를 정리한다.
- 실행 결과 : Start, Stop, Reset 동작을 확인했다. 추가 기능으로 Count가 분과 초로 표시되도록 수정했다.
- 분/초 표시를 추가한 뒤 npm run build가 정상적으로 완료되었다. Start 후 60초가 지나면 '0분 59초'에서 '1분 0초'로 넘어가는 것을 확인했다.

## lab 비교 분석
### useEffect의 Dependency Array가 필요한 이유는?
- Dependency Array을 이용함으로써 useEffect에 재실행 신호를 줄 수 있다. 이 프로그램에서는 타이머가 작동하면 useEffect로 타이머를 종료시킬 신호를 받아야 하는데, 이 때 Dependency Array를 이용해 신호를 받는다.

### Cleanup을 사용하지 않으면 어떤 문제가 발생할 수 있는가?
- 타이머가 중첩 실행될 수 있다. 같은 lab에서 다시 타이머가 시작되면 Count가 더 빠르게 증가하며 다른 Lab으로 이동하더라도 자원 누수가 발생한다.

### useState와 useRef의 가장 중요한 차이는?
- useState는 값이 바뀔 때 재랜더링을 진행하지만, useRef는 진행하지 않는다. 화면에 지속적으로 반영해야 하는 값은 state, 갱신 없이 기억만 하면 되는 값은 ref로 관리한다.

### LAB 2와 LAB 4의 Timer 관리 방식은 어떻게 다른가?
- LAB 2는 running 상태에 따라 Effect가 타이머를 관리하고, LAB 4는 이벤트 함수가 ref에 저장된 타이머 ID로 직접 관리한다. Lab2에서는 상태 변경이 타이머를 제어하고, Lab4에서는 타이머를 제어한 후 상태를 변경한다.

## Customizing
### 수정 내용 및 파일명
- src/App.jsx : 프로젝트 제목을 'Hook을 이용한 타이머'로, 설명을 '오픈소스 스튜디오 Hook 수업'으로 변경했다.
- src/App.css : 기본 버튼 색상을 초록색으로 변경하고, 입력창에 포커스가 있을 때 테두리 색상과 outline이 표시되도록 수정했다.
- src/components/Lab4_RefTimer.jsx : Count를 분과 초로 표시하는 선택 기능을 추가했다. Math.floor(count / 60)으로 분을 계산하고, count % 60으로 남은 초를 계산한다.
- LAB 2와 LAB 4의 Running / Stopped 상태 표시는 Starter Project에 있던 기능이며, 추가로 구현한 선택 기능은 LAB 4의 분/초 표시이다.

### 동작 및 Console 확인
- LAB 1 : 화면을 열면 Console에 `[LAB 1] rendered`가 출력되고, 이후 1초마다 Count가 바뀔 때마다 다시 출력된다. 개발 환경에서는 StrictMode 때문에 렌더링 로그가 두 번씩 보일 수 있다.
- LAB 1 Dependency Array 실험 : `[]`를 지우면 렌더링될 때마다 Cleanup → 새 setInterval이 반복 실행된다. Count는 계속 증가하지만, 매초 타이머를 새로 만들고 지우는 불필요한 작업이 생긴다. 확인 후 다시 `[]`로 되돌렸다.
- LAB 2 / LAB 4 : Start → Stop → Reset 순서로 눌러 상태 표시(Running / Stopped)와 Count가 맞게 바뀌는 것을 확인했다. 타이머 실행 중 다른 Lab으로 이동했다가 돌아오면 Count가 0부터 다시 시작해, 언마운트 시 Cleanup으로 타이머가 정리된 것을 확인했다.
- LAB 3 : Focus / Clear 버튼 클릭 후 입력창에 커서가 위치하며, 버튼 클릭으로는 재렌더링이 일어나지 않는다.

## Deployment
### Vercel 배포 주소
https://oss-week12.vercel.app/

## Weekly Review
### 새롭게 배운 내용 3가지
- useEffect는 렌더링과 별개로 타이머 같은 외부 작업을 처리하고, Cleanup으로 이전 작업을 정리해야 한다는 것
- setCount(prev => prev + 1)처럼 함수형 업데이트를 쓰면 Effect 안에서 오래된 State 값을 참조하는 문제를 막을 수 있다는 것
- useRef는 값이 바뀌어도 재렌더링되지 않아 DOM 참조나 타이머 ID처럼 화면에 보일 필요 없는 값을 보관하기에 적합하다는 것

### 어려웠던 점과 해결 방법
- 12주차기 때문에 앞의 내용 + 새로운 내용으로 누적되어 가느라 공부 부담이 조금 높아진 것 같다. 시간을 내어 AI와 공부해보며 천천히 공부하였다.

### AI 활용 내용 및 느낀 점
- AI를 활용하여 개념을 이해하는 데 적극 활용하였다. 자료 PDF를 이용해 개념 공부를 하고, 실습 코드를 함께 분석하였다. 이후 AI 없이 스스로 비슷하게 구현해보는 것으로
마무리하였다.
- AI랑 함께 공부하여 속도가 굉장히 빨랐던 것 같다.

