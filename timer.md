<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>8분할 타이머</title>
    <!-- Tailwind CSS CDN을 로드하여 반응형 디자인 및 유틸리티 클래스 사용 -->
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        /* 기본 바디 스타일: 검정색 배경, 전체 화면 중앙 정렬 */
        body {
            background-color: black;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh; /* 뷰포트 높이 전체를 차지하도록 설정 */
            margin: 0; /* 기본 마진 제거 */
            font-family: "Inter", sans-serif; /* Inter 폰트 사용 */
            overflow: hidden; /* 스크롤바 방지 */
        }
        /* 타이머 그리드 컨테이너 스타일 */
        .timer-grid-container {
            width: 90vw; /* 뷰포트 너비의 90%를 차지하여 반응형으로 만듦 */
            max-width: 1200px; /* 큰 화면에서 최대 너비 제한 */
            aspect-ratio: 5 / 3; /* 가로 5, 세로 3의 비율 유지 */
            display: grid;
            grid-template-columns: repeat(2, 1fr); /* 2개의 동일한 너비의 컬럼 */
            grid-template-rows: repeat(4, 1fr); /* 4개의 동일한 높이의 로우 */
            gap: 1rem; /* 타이머 박스 사이의 간격 */
            padding: 1rem; /* 내부 패딩 */
            box-sizing: border-box; /* 패딩을 너비/높이에 포함 */
        }
        /* 개별 타이머 박스 스타일 */
        .timer-box {
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            border-radius: 0.75rem; /* 둥근 모서리 */
            padding: 0.5rem; /* 내부 패딩 */
            background-color: rgba(0, 0, 0, 0.5); /* 약간 투명한 검정색 배경으로 구분 */
        }
        /* 타이머 숫자 표시 스타일 */
        .timer-display {
            font-size: 8vw; /* 뷰포트 너비에 비례하는 반응형 폰트 크기 */
            font-weight: 900; /* 매우 굵게 */
            line-height: 1; /* 줄 높이 조정하여 수직 중앙 정렬 개선 */
            text-shadow: 0 0 10px rgba(255, 255, 255, 0.5); /* 밝은 숫자에 은은한 그림자 효과 */
            flex-grow: 1; /* 남은 공간을 차지하도록 설정 */
            display: flex;
            align-items: center;
            justify-content: center;
        }
        /* 타이머 버튼 컨테이너 스타일 */
        .timer-buttons {
            display: flex;
            gap: 0.5rem; /* 버튼 사이의 간격 */
            margin-top: 0.5rem; /* 버튼 상단 마진 */
        }
        /* 개별 타이머 버튼 스타일 */
        .timer-button {
            padding: 0.25rem 0.75rem; /* 상하 0.25rem, 좌우 0.75rem 패딩 */
            font-size: 0.875rem; /* 작은 폰트 크기 */
            border-radius: 0.375rem; /* 둥근 모서리 */
            background-color: #374151; /* 어두운 회색 배경 */
            color: white; /* 흰색 글자 */
            font-weight: 600; /* 세미볼드 */
            cursor: pointer; /* 마우스 오버 시 포인터 변경 */
            transition: background-color 0.2s; /* 배경색 변경 시 부드러운 전환 효과 */
            border: none; /* 테두리 없음 */
        }
        /* 버튼 호버 효과 */
        .timer-button:hover {
            background-color: #4b5563; /* 호버 시 약간 더 밝은 회색 */
        }

        /* 반응형 폰트 크기 조정 (미디어 쿼리) */
        @media (min-width: 640px) { /* sm (small) breakpoint */
            .timer-display {
                font-size: 6vw;
            }
        }
        @media (min-width: 768px) { /* md (medium) breakpoint */
            .timer-display {
                font-size: 5vw;
            }
        }
        @media (min-width: 1024px) { /* lg (large) breakpoint */
            .timer-display {
                font-size: 4vw;
            }
        }
        @media (min-width: 1280px) { /* xl (extra large) breakpoint */
             .timer-display {
                font-size: 3.5vw;
            }
        }
    </style>
</head>
<body>
    <!-- 8개의 타이머를 담는 그리드 컨테이너 -->
    <div class="timer-grid-container">
        <!-- 타이머 1 (1행, 왼쪽) - 3분 -->
        <div class="timer-box">
            <div class="timer-display text-yellow-400" id="timer-display-0">03:00</div>
            <div class="timer-buttons">
                <button class="timer-button" onclick="startTimer(0)">시작</button>
                <button class="timer-button" onclick="stopTimer(0)">정지</button>
                <button class="timer-button" onclick="resetTimer(0)">초기화</button>
            </div>
        </div>

        <!-- 타이머 2 (1행, 오른쪽) - 3분 -->
        <div class="timer-box">
            <div class="timer-display text-green-400" id="timer-display-1">03:00</div>
            <div class="timer-buttons">
                <button class="timer-button" onclick="startTimer(1)">시작</button>
                <button class="timer-button" onclick="stopTimer(1)">정지</button>
                <button class="timer-button" onclick="resetTimer(1)">초기화</button>
            </div>
        </div>

        <!-- 타이머 3 (2행, 왼쪽) - 4분 -->
        <div class="timer-box">
            <div class="timer-display text-yellow-400" id="timer-display-2">04:00</div>
            <div class="timer-buttons">
                <button class="timer-button" onclick="startTimer(2)">시작</button>
                <button class="timer-button" onclick="stopTimer(2)">정지</button>
                <button class="timer-button" onclick="resetTimer(2)">초기화</button>
            </div>
        </div>

        <!-- 타이머 4 (2행, 오른쪽) - 4분 -->
        <div class="timer-box">
            <div class="timer-display text-green-400" id="timer-display-3">04:00</div>
            <div class="timer-buttons">
                <button class="timer-button" onclick="startTimer(3)">시작</button>
                <button class="timer-button" onclick="stopTimer(3)">정지</button>
                <button class="timer-button" onclick="resetTimer(3)">초기화</button>
            </div>
        </div>

        <!-- 타이머 5 (3행, 왼쪽) - 2분 -->
        <div class="timer-box">
            <div class="timer-display text-yellow-400" id="timer-display-4">02:00</div>
            <div class="timer-buttons">
                <button class="timer-button" onclick="startTimer(4)">시작</button>
                <button class="timer-button" onclick="stopTimer(4)">정지</button>
                <button class="timer-button" onclick="resetTimer(4)">초기화</button>
            </div>
        </div>

        <!-- 타이머 6 (3행, 오른쪽) - 2분 -->
        <div class="timer-box">
            <div class="timer-display text-green-400" id="timer-display-5">02:00</div>
            <div class="timer-buttons">
                <button class="timer-button" onclick="startTimer(5)">시작</button>
                <button class="timer-button" onclick="stopTimer(5)">정지</button>
                <button class="timer-button" onclick="resetTimer(5)">초기화</button>
            </div>
        </div>

        <!-- 타이머 7 (4행, 왼쪽) - 3분 -->
        <div class="timer-box">
            <div class="timer-display text-yellow-400" id="timer-display-6">03:00</div>
            <div class="timer-buttons">
                <button class="timer-button" onclick="startTimer(6)">시작</button>
                <button class="timer-button" onclick="stopTimer(6)">정지</button>
                <button class="timer-button" onclick="resetTimer(6)">초기화</button>
            </div>
        </div>

        <!-- 타이머 8 (4행, 오른쪽) - 3분 -->
        <div class="timer-box">
            <div class="timer-display text-green-400" id="timer-display-7">03:00</div>
            <div class="timer-buttons">
                <button class="timer-button" onclick="startTimer(7)">시작</button>
                <button class="timer-button" onclick="stopTimer(7)">정지</button>
                <button class="timer-button" onclick="resetTimer(7)">초기화</button>
            </div>
        </div>
    </div>

    <script>
        // 각 타이머의 상태와 설정을 저장하는 배열
        const timers = [
            { initialTime: 3 * 60, currentTime: 3 * 60, isRunning: false, intervalId: null, displayElement: null }, // 타이머 1 (3분)
            { initialTime: 3 * 60, currentTime: 3 * 60, isRunning: false, intervalId: null, displayElement: null }, // 타이머 2 (3분)
            { initialTime: 4 * 60, currentTime: 4 * 60, isRunning: false, intervalId: null, displayElement: null }, // 타이머 3 (4분)
            { initialTime: 4 * 60, currentTime: 4 * 60, isRunning: false, intervalId: null, displayElement: null }, // 타이머 4 (4분)
            { initialTime: 2 * 60, currentTime: 2 * 60, isRunning: false, intervalId: null, displayElement: null }, // 타이머 5 (2분)
            { initialTime: 2 * 60, currentTime: 2 * 60, isRunning: false, intervalId: null, displayElement: null }, // 타이머 6 (2분)
            { initialTime: 3 * 60, currentTime: 3 * 60, isRunning: false, intervalId: null, displayElement: null }, // 타이머 7 (3분)
            { initialTime: 3 * 60, currentTime: 3 * 60, isRunning: false, intervalId: null, displayElement: null }  // 타이머 8 (3분)
        ];

        // 초를 MM:SS 형식으로 변환하는 함수
        function formatTime(seconds) {
            const minutes = Math.floor(seconds / 60);
            const remainingSeconds = seconds % 60;
            return `${String(minutes).padStart(2, '0')}:${String(remainingSeconds).padStart(2, '0')}`;
        }

        // 특정 타이머의 화면 표시를 업데이트하는 함수
        function updateDisplay(index) {
            const timer = timers[index];
            if (timer.displayElement) {
                timer.displayElement.textContent = formatTime(timer.currentTime);
            }
        }

        // 특정 타이머를 시작하는 함수
        function startTimer(index) {
            const timer = timers[index];
            if (!timer.isRunning) {
                timer.isRunning = true;
                timer.intervalId = setInterval(() => {
                    if (timer.currentTime > 0) {
                        timer.currentTime--;
                        updateDisplay(index);
                    } else {
                        stopTimer(index); // 시간이 0이 되면 타이머 정지
                    }
                }, 1000); // 1초마다 업데이트
            }
        }

        // 특정 타이머를 정지하는 함수
        function stopTimer(index) {
            const timer = timers[index];
            if (timer.isRunning) {
                timer.isRunning = false;
                clearInterval(timer.intervalId); // setInterval 중지
            }
        }

        // 특정 타이머를 초기화하는 함수
        function resetTimer(index) {
            const timer = timers[index];
            stopTimer(index); // 실행 중이면 정지
            timer.currentTime = timer.initialTime; // 초기 시간으로 재설정
            updateDisplay(index); // 화면 업데이트
        }

        // 창이 로드될 때 모든 타이머를 초기화하고 초기 화면을 설정
        window.onload = function() {
            timers.forEach((timer, index) => {
                // 각 타이머의 displayElement를 해당 HTML 요소에 연결
                timer.displayElement = document.getElementById(`timer-display-${index}`);
                updateDisplay(index); // 초기 시간으로 화면 표시
            });
        };
    </script>
</body>
</html>
