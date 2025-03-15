# quizforkja
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>세로 모드 퀴즈</title>
    <style>
        body {
            margin: 0;
            padding: 0;
            overflow: hidden;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            background-color: #000;
            position: relative;
        }
        img {
            width: 100vw;
            height: 100vh;
            object-fit: cover;
        }
        .error-message {
            display: none;
            color: white;
            font-size: 20px;
            text-align: center;
        }
        .answer-box {
            position: absolute;
            bottom: 20px;
            right: 20px;
            background-color: rgba(255, 255, 255, 0.8);
            padding: 10px;
            border-radius: 5px;
            z-index: 10;
            display: flex;
            gap: 5px;
        }
        .answer-box input {
            padding: 5px;
            width: 120px;
        }
        .answer-box button {
            padding: 5px 10px;
            background-color: #4CAF50;
            color: white;
            border: none;
            border-radius: 3px;
            cursor: pointer;
        }
        .answer-box button:hover {
            background-color: #45a049;
        }
        @media (orientation: landscape) {
            body::before {
                content: "화면을 세로로 돌려주세요!";
                position: absolute;
                top: 50%;
                left: 50%;
                transform: translate(-50%, -50%);
                color: white;
                font-size: 20px;
                text-align: center;
                z-index: 20;
            }
            img {
                display: none;
            }
        }
    </style>
</head>
<body>
    <img id="mainImage" src="https://gi.esmplus.com/varzz1/%EB%A9%94%EC%9D%B8%20%EC%9D%B4%EB%AF%B8%EC%A7%80.jpeg" alt="메인 이미지">
    <div class="error-message" id="errorMessage">이미지를 로드할 수 없습니다.</div>

    <script>
        const image = document.getElementById('mainImage');
        const errorMessage = document.getElementById('errorMessage');
        let currentQuestion = 0;
        let answerBox = null;

        const questions = [
            {
                background: 'https://gi.esmplus.com/varzz1/%ED%80%B4%EC%A6%88%201.jpeg',
                problem: 'https://gi.esmplus.com/varzz1/%EC%B2%AB%EB%B2%88%EC%A7%B8%20%EB%AC%B8%EC%A0%9C.jpeg',
                answer: '김진아'
            },
            {
                background: 'https://gi.esmplus.com/varzz1/%ED%80%B4%EC%A6%88%202.jpeg',
                problem: 'https://gi.esmplus.com/varzz1/%EB%91%90%EB%B2%88%EC%A7%B8%20%EB%AC%B8%EC%A0%9C.jpeg',
                answer: '3'
            },
            {
                background: 'https://gi.esmplus.com/varzz1/%ED%80%B4%EC%A6%88%203.jpeg',
                problem: 'https://gi.esmplus.com/varzz1/%EC%84%B8%EB%B2%88%EC%A7%B8%20%EB%AC%B8%EC%A0%9C.jpeg',
                answer: '고구마 고구마'
            },
            {
                background: 'https://gi.esmplus.com/varzz1/%ED%80%B4%EC%A6%88%204.jpeg',
                problem: 'https://gi.esmplus.com/varzz1/%EB%84%A4%EB%B2%88%EC%A7%B8%20%EB%AC%B8%EC%A0%9C.jpeg',
                answer: null
            }
        ];
        const successImage = 'https://gi.esmplus.com/varzz1/%EB%AC%B8%EC%A0%9C%ED%95%B4%EA%B2%B0.jpeg';
        const failImage = 'https://gi.esmplus.com/varzz1/%EC%8B%A4%ED%8C%A8.jpeg';

        image.addEventListener('touchstart', function() {
            if (image.src.includes('%EB%A9%94%EC%9D%B8%20%EC%9D%B4%EB%AF%B8%EC%A7%80.jpeg')) {
                image.src = questions[0].background;
                currentQuestion = 1;
            } 
            else if (questions.some(q => q.background === image.src)) {
                const questionIndex = questions.findIndex(q => q.background === image.src);
                image.src = questions[questionIndex].problem;
                currentQuestion = questionIndex + 1;
            } 
            else if (questions.some(q => q.problem === image.src)) {
                if (currentQuestion <= 3) {
                    showAnswerBox();
                }
            } 
            else if (image.src === successImage) {
                if (currentQuestion < 4) {
                    image.src = questions[currentQuestion].background;
                    currentQuestion++;
                } else {
                    image.src = 'https://gi.esmplus.com/varzz1/%EB%A9%94%EC%9D%B8%20%EC%9D%B4%EB%AF%B8%EC%A7%80.jpeg';
                    currentQuestion = 0;
                }
            } 
            else if (image.src === failImage) {
                image.src = questions[currentQuestion - 1].problem;
            }
        });

        function showAnswerBox() {
            if (answerBox) return; // 이미 입력창이 있으면 중복 생성 방지

            answerBox = document.createElement('div');
            answerBox.classList.add('answer-box');

            const input = document.createElement('input');
            input.type = 'text';
            input.placeholder = '정답을 입력하세요';
            input.id = 'answerInput';

            const button = document.createElement('button');
            button.textContent = '제출';
            button.onclick = checkAnswer;

            answerBox.appendChild(input);
            answerBox.appendChild(button);
            document.body.appendChild(answerBox);

            input.focus();
        }

        function checkAnswer() {
            if (currentQuestion > 3) return;
            
            const userAnswer = document.getElementById('answerInput').value.trim();
            const correctAnswer = questions[currentQuestion - 1].answer;

            if (userAnswer === correctAnswer) {
                image.src = successImage;
            } else {
                image.src = failImage;
            }

            removeAnswerBox();
        }

        function removeAnswerBox() {
            if (answerBox) {
                document.body.removeChild(answerBox);
                answerBox = null;
            }
        }
    </script>
</body>
</html>
