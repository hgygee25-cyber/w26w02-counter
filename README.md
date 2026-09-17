# 뽀모도로 타이머

## 1. 프로젝트 소개

Vanilla JavaScript를 활용하여 제작한 뽀모도로 타이머입니다.

집중 시간과 휴식 시간을 설정하고,
타이머를 이용하여 효율적으로 시간을 관리할 수 있도록 제작했습니다.

## 2. 사용 기술

- HTML
- CSS
- Vanilla JavaScript

## 3. 주요 기능

- 타이머 시작
- 타이머 일시정지
- 타이머 초기화
- 집중 시간 설정
- 휴식 시간 설정
- 다음 세션으로 건너뛰기
- 집중 / 휴식 상태 표시

## 4. 기본 설정

- 집중 시간: 25분
- 휴식 시간: 5분

## 5. 파일 구성

- `index.html` : 웹페이지 구조
- `style.css` : 디자인 및 화면 스타일
- `script.js` : 타이머 기능 구현

## 6. 코드

<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>뽀모도로 타이머</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <main class="app">
    <section class="timer-card">
      <p class="eyebrow">FOCUS TIMER</p>
      <h1>뽀모도로 타이머</h1>
      <p id="modeLabel" class="mode">집중 시간</p>

      <div class="timer-circle">
        <div class="timer-content">
          <span id="time">25:00</span>
          <span id="sessionInfo">1번째 세션</span>
        </div>
      </div>

      <div class="progress-track">
        <div id="progressBar" class="progress-bar"></div>
      </div>

      <div class="controls">
        <button id="startBtn" class="primary">시작</button>
        <button id="resetBtn">초기화</button>
        <button id="skipBtn">건너뛰기</button>
      </div>

      <div class="settings">
        <div class="setting">
          <label for="focusMinutes">집중</label>
          <div class="number-control">
            <button class="minus" data-target="focusMinutes">−</button>
            <span id="focusMinutes">25</span>
            <button class="plus" data-target="focusMinutes">+</button>
          </div>
          <small>분</small>
        </div>

        <div class="setting">
          <label for="breakMinutes">휴식</label>
          <div class="number-control">
            <button class="minus" data-target="breakMinutes">−</button>
            <span id="breakMinutes">5</span>
            <button class="plus" data-target="breakMinutes">+</button>
          </div>
          <small>분</small>
        </div>
      </div>
    </section>

    <aside class="tips-card">
      <h2>오늘도 천천히, 꾸준히 🌷</h2>
      <p>집중할 때는 한 가지 일에만 마음을 모아보세요.</p>
      <div class="session-dots" id="sessionDots">
        <span class="dot active"></span>
        <span class="dot"></span>
        <span class="dot"></span>
        <span class="dot"></span>
      </div>
      <p class="tip-small">25분 집중 → 5분 휴식</p>
    </aside>
  </main>

* {
  box-sizing: border-box;
}

:root {
  --cream: #fffaf5;
  --pink: #f7c9d4;
  --pink-dark: #d98da1;
  --lavender: #ddd4f4;
  --mint: #cce9df;
  --yellow: #f8e4a6;
  --text: #5e5963;
  --muted: #96919b;
  --white: #ffffff;
  --shadow: 0 20px 50px rgba(125, 105, 115, 0.12);
}

body {
  margin: 0;
  min-height: 100vh;

  font-family:
    "Pretendard",
    "Apple SD Gothic Neo",
    "Malgun Gothic",
    sans-serif;

  color: var(--text);

  background:
    radial-gradient(
      circle at 10% 15%,
      rgba(247, 201, 212, 0.65) 0 90px,
      transparent 91px
    ),
    radial-gradient(
      circle at 90% 80%,
      rgba(204, 233, 223, 0.7) 0 120px,
      transparent 121px
    ),
    linear-gradient(135deg, #fffaf5, #f9f5fb);

  display: flex;
  align-items: center;
  justify-content: center;

  padding: 30px 18px;
}

/* 전체 영역 */

.app {
  width: min(900px, 100%);

  display: grid;
  grid-template-columns:
    minmax(0, 1.5fr)
    minmax(240px, 0.8fr);

  gap: 22px;
}

/* 카드 */

.timer-card,
.tips-card {
  background: rgba(255, 255, 255, 0.82);

  border: 1px solid rgba(255, 255, 255, 0.95);

  border-radius: 32px;

  box-shadow: var(--shadow);

  backdrop-filter: blur(12px);
}

/* 타이머 카드 */

.timer-card {
  padding: 42px;

  text-align: center;
}

/* 상단 작은 글씨 */

.eyebrow {
  margin: 0 0 8px;

  color: var(--pink-dark);

  font-size: 12px;

  font-weight: 800;

  letter-spacing: 3px;
}

/* 제목 */

h1 {
  margin: 0;

  font-size: clamp(28px, 5vw, 40px);

  letter-spacing: -1.5px;
}

/* 집중 시간 / 휴식 시간 */

.mode {
  margin: 10px 0 24px;

  color: var(--muted);

  font-weight: 600;
}

/* 타이머 원 */

.timer-circle {
  width: min(300px, 70vw);

  aspect-ratio: 1;

  margin: 0 auto 22px;

  border-radius: 50%;

  background:
    conic-gradient(
      var(--pink) 0deg,
      var(--pink) 360deg
    );

  padding: 12px;

  display: flex;

  align-items: center;
  justify-content: center;

  transition: background 0.3s ease;
}

/* 원 안쪽 */

.timer-content {
  width: 100%;
  height: 100%;

  border-radius: 50%;

  background: var(--cream);

  display: flex;

  flex-direction: column;

  justify-content: center;

  align-items: center;
}

/* 시간 */

#time {
  font-size: clamp(48px, 10vw, 70px);

  font-weight: 800;

  letter-spacing: -3px;

  font-variant-numeric: tabular-nums;
}

/* 세션 정보 */

#sessionInfo {
  margin-top: 4px;

  color: var(--muted);

  font-size: 13px;
}

/* 진행바 */

.progress-track {
  width: 80%;

  height: 8px;

  margin: 0 auto 26px;

  overflow: hidden;

  border-radius: 99px;

  background: #eee9ed;
}

.progress-bar {
  width: 0%;

  height: 100%;

  border-radius: inherit;

  background: var(--pink);

  transition: width 0.2s linear;
}

/* 버튼 */

.controls {
  display: flex;

  justify-content: center;

  gap: 10px;

  flex-wrap: wrap;
}

button {
  border: 0;

  border-radius: 14px;

  padding: 12px 20px;

  background: #f2eef2;

  color: var(--text);

  font: inherit;

  font-weight: 700;

  cursor: pointer;

  transition:
    transform 0.15s ease,
    opacity 0.15s ease,
    background 0.15s ease;
}

button:hover {
  transform: translateY(-2px);

  opacity: 0.9;
}

button:active {
  transform: translateY(0);
}

/* 시작 버튼 */

button.primary {
  min-width: 100px;

  background: var(--pink);

  color: #6f4652;
}

/* 설정 */

.settings {
  display: flex;

  justify-content: center;

  gap: 28px;

  margin-top: 28px;

  padding-top: 24px;

  border-top: 1px solid #f0ebef;
}

.setting {
  display: grid;

  grid-template-columns: auto;

  justify-items: center;

  gap: 7px;
}

.setting label {
  font-size: 13px;

  font-weight: 700;

  color: var(--muted);
}

/* + / - */

.number-control {
  display: flex;

  align-items: center;

  gap: 10px;
}

.number-control button {
  width: 32px;
  height: 32px;

  padding: 0;

  border-radius: 50%;

  background: var(--lavender);
}

.number-control span {
  width: 30px;

  font-weight: 800;
}

.setting small {
  color: var(--muted);
}

/* 오른쪽 안내 카드 */

.tips-card {
  padding: 30px;

  align-self: center;

  background:
    linear-gradient(
      145deg,
      rgba(255, 255, 255, 0.9),
      rgba(255, 248, 250, 0.8)
    );
}

.tips-card h2 {
  margin: 0 0 12px;

  font-size: 20px;

  letter-spacing: -0.5px;
}

.tips-card p {
  margin: 0;

  line-height: 1.7;

  color: var(--muted);
}

/* 세션 점 */

.session-dots {
  display: flex;

  gap: 9px;

  margin: 28px 0 15px;
}

.dot {
  width: 13px;
  height: 13px;

  border-radius: 50%;

  background: #ebe6ed;
}

.dot.active {
  background: var(--pink);
}

.dot.completed {
  background: var(--mint);
}

.tip-small {
  font-size: 12px;
}

/* 모바일 */

@media (max-width: 720px) {
  .app {
    grid-template-columns: 1fr;
  }

  .tips-card {
    text-align: center;
  }

  .session-dots {
    justify-content: center;
  }

  .timer-card {
    padding: 30px 20px;
  }
}
// HTML 요소 가져오기

const timeDisplay = document.getElementById("time");
const modeLabel = document.getElementById("modeLabel");
const sessionInfo = document.getElementById("sessionInfo");

const progressBar = document.getElementById("progressBar");
const timerCircle = document.querySelector(".timer-circle");

const startBtn = document.getElementById("startBtn");
const resetBtn = document.getElementById("resetBtn");
const skipBtn = document.getElementById("skipBtn");

const dots = [
  ...document.querySelectorAll(".dot")
];


// 기본 시간

let focusMinutes = 25;
let breakMinutes = 5;


// 현재 상태

let mode = "focus";

let session = 1;

let remainingSeconds =
  focusMinutes * 60;

let totalSeconds =
  focusMinutes * 60;


// 타이머 상태

let timerId = null;

let running = false;


// 화면 업데이트

function updateDisplay() {

  // 분 계산

  const minutes =
    Math.floor(remainingSeconds / 60);


  // 초 계산

  const seconds =
    remainingSeconds % 60;


  // 25:00 형태로 표시

  timeDisplay.textContent =
    String(minutes).padStart(2, "0")
    + ":"
    + String(seconds).padStart(2, "0");


  // 현재 모드 표시

  if (mode === "focus") {

    modeLabel.textContent =
      "집중 시간";

  } else {

    modeLabel.textContent =
      "휴식 시간";
  }


  // 세션 표시

  sessionInfo.textContent =
    `${session}번째 세션`;


  // 진행률 계산

  const elapsed =
    totalSeconds - remainingSeconds;


  const progress =
    totalSeconds === 0
      ? 0
      : (elapsed / totalSeconds) * 100;


  // 진행바

  progressBar.style.width =
    `${progress}%`;


  // 타이머 원 색상

  if (mode === "focus") {

    timerCircle.style.background =
      `conic-gradient(
        var(--pink) ${progress * 3.6}deg,
        #f0e8ec 0deg
      )`;

    progressBar.style.background =
      "var(--pink)";

  } else {

    timerCircle.style.background =
      `conic-gradient(
        var(--mint) ${progress * 3.6}deg,
        #e8efec 0deg
      )`;

    progressBar.style.background =
      "var(--mint)";
  }


  // 세션 점 업데이트

  updateDots();
}


// 세션 점 업데이트

function updateDots() {

  dots.forEach((dot, index) => {

    // 기존 클래스 제거

    dot.classList.remove(
      "active",
      "completed"
    );


    // 이미 끝난 세션

    if (index < session - 1) {

      dot.classList.add(
        "completed"
      );


    // 현재 세션

    } else if (
      index === session - 1 &&
      mode === "focus"
    ) {

      dot.classList.add(
        "active"
      );
    }

  });
}


// 타이머 시작

function startTimer() {

  // 이미 실행 중이라면 일시정지

  if (running) {

    pauseTimer();

    return;
  }


  running = true;

  startBtn.textContent =
    "일시정지";


  // 1초마다 실행

  timerId = setInterval(() => {

    // 시간이 남아 있다면 1초 감소

    if (remainingSeconds > 0) {

      remainingSeconds--;

      updateDisplay();


    // 시간이 0이면 모드 변경

    } else {

      switchMode();
    }

  }, 1000);
}


// 타이머 일시정지

function pauseTimer() {

  clearInterval(timerId);

  timerId = null;

  running = false;

  startBtn.textContent =
    "시작";
}


// 집중 ↔ 휴식 전환

function switchMode() {

  // 기존 타이머 정지

  clearInterval(timerId);

  timerId = null;

  running = false;


  // 집중 시간이었다면 휴식으로

  if (mode === "focus") {

    mode = "break";


  // 휴식이었다면 다음 집중 세션

  } else {

    mode = "focus";

    session++;


    // 4세션이 끝나면 다시 1세션

    if (session > 4) {

      session = 1;
    }
  }


  // 현재 모드에 맞는 시간 설정

  totalSeconds =
    (
      mode === "focus"
        ? focusMinutes
        : breakMinutes
    ) * 60;


  remainingSeconds =
    totalSeconds;


  // 화면 업데이트

  updateDisplay();


  // 브라우저 탭 제목 변경

  if (mode === "focus") {

    document.title =
      "집중할 시간 · 뽀모도로";

  } else {

    document.title =
      "잠깐 쉬어가요 · 뽀모도로";
  }


  startBtn.textContent =
    "시작";
}


// 타이머 초기화

function resetTimer() {

  pauseTimer();


  // 처음 상태로

  mode = "focus";

  session = 1;


  totalSeconds =
    focusMinutes * 60;


  remainingSeconds =
    totalSeconds;


  document.title =
    "뽀모도로 타이머";


  updateDisplay();
}


// 시간 설정 변경

function changeSetting(
  target,
  amount
) {

  // 집중 시간 변경

  if (target === "focusMinutes") {

    focusMinutes =
      Math.min(
        60,
        Math.max(
          1,
          focusMinutes + amount
        )
      );


    document.getElementById(
      "focusMinutes"
    ).textContent =
      focusMinutes;
  }


  // 휴식 시간 변경

  if (target === "breakMinutes") {

    breakMinutes =
      Math.min(
        30,
        Math.max(
          1,
          breakMinutes + amount
        )
      );


    document.getElementById(
      "breakMinutes"
    ).textContent =
      breakMinutes;
  }


  // 타이머가 실행 중이 아닐 때만
  // 현재 타이머에 바로 적용

  if (!running) {

    if (mode === "focus") {

      totalSeconds =
        focusMinutes * 60;

    } else {

      totalSeconds =
        breakMinutes * 60;
    }


    remainingSeconds =
      totalSeconds;


    updateDisplay();
  }
}


// 시작 버튼

startBtn.addEventListener(
  "click",
  startTimer
);


// 초기화 버튼

resetBtn.addEventListener(
  "click",
  resetTimer
);


// 건너뛰기 버튼

skipBtn.addEventListener(
  "click",
  switchMode
);


// + 버튼

document
  .querySelectorAll(".plus")
  .forEach(button => {

    button.addEventListener(
      "click",
      () => {

        changeSetting(
          button.dataset.target,
          1
        );

      }
    );

  });


// - 버튼

document
  .querySelectorAll(".minus")
  .forEach(button => {

    button.addEventListener(
      "click",
      () => {

        changeSetting(
          button.dataset.target,
          -1
        );

      }
    );

  });


// 처음 화면 표시

updateDisplay();

  <script src="scripts.js"></script>
</body>
</html>
