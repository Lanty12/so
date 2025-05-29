<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Quiz de Fútbol</title>
    <style>
        body {
            font-family: 'Arial', sans-serif;
            background-color: #f0f0f0;
            color: #333;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            margin: 0;
            padding: 20px;
            box-sizing: border-box;
        }

        .quiz-container {
            background-color: #fff;
            padding: 30px;
            border-radius: 10px;
            box-shadow: 0 0 15px rgba(0, 0, 0, 0.1);
            width: 100%;
            max-width: 600px;
            text-align: center;
        }

        h1 {
            color: #1a73e8; /* Un azul futbolero */
            margin-bottom: 20px;
        }

        #question-container {
            margin-bottom: 20px;
        }

        #question-text {
            font-size: 1.2em;
            margin-bottom: 20px;
        }

        .options-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 10px;
            margin-bottom: 20px;
        }

        .option-btn {
            background-color: #4CAF50; /* Verde césped */
            color: white;
            border: none;
            padding: 15px;
            text-align: center;
            text-decoration: none;
            display: inline-block;
            font-size: 1em;
            border-radius: 5px;
            cursor: pointer;
            transition: background-color 0.3s ease;
        }

        .option-btn:hover:not(:disabled) {
            background-color: #45a049;
        }

        .option-btn:disabled {
            cursor: not-allowed;
            opacity: 0.7;
        }

        .option-btn.correct {
            background-color: #28a745; /* Verde más brillante para correcto */
        }

        .option-btn.incorrect {
            background-color: #dc3545; /* Rojo para incorrecto */
        }

        #feedback {
            font-size: 1.1em;
            font-weight: bold;
            margin-bottom: 15px;
            min-height: 1.5em; /* Para evitar saltos de diseño */
        }

        #next-btn, #restart-btn {
            background-color: #007bff;
            color: white;
            border: none;
            padding: 10px 20px;
            font-size: 1em;
            border-radius: 5px;
            cursor: pointer;
            transition: background-color 0.3s ease;
            margin-top: 10px;
        }

        #next-btn:hover, #restart-btn:hover {
            background-color: #0056b3;
        }

        #score-container {
            font-size: 1.2em;
            margin-top: 20px;
        }

        .hidden {
            display: none;
        }
    </style>
</head>
<body>

    <div class="quiz-container">
        <h1>⚽ Quiz de Conocimiento Futbolístico ⚽</h1>

        <div id="question-container">
            <p id="question-text"></p>
            <div id="options-grid" class="options-grid">
                <!-- Las opciones se generarán con JavaScript -->
            </div>
        </div>

        <div id="feedback"></div>

        <button id="next-btn" class="hidden">Siguiente Pregunta</button>

        <div id="score-container" class="hidden">
            <h2>¡Juego Terminado!</h2>
            <p id="final-score"></p>
            <button id="restart-btn">Jugar de Nuevo</button>
        </div>
    </div>

    <script>
        const questions = [
            {
                question: "¿Qué país ganó la Copa Mundial de la FIFA 2010?",
                options: ["Brasil", "Argentina", "España", "Alemania"],
                correctAnswer: "España"
            },
            {
                question: "¿Quién es conocido como 'O Rei' (El Rey) del fútbol?",
                options: ["Diego Maradona", "Lionel Messi", "Pelé", "Cristiano Ronaldo"],
                correctAnswer: "Pelé"
            },
            {
                question: "¿En qué equipo juega actualmente Lionel Messi (a fecha de 2023)?",
                options: ["FC Barcelona", "Paris Saint-Germain", "Inter Miami CF", "Manchester City"],
                correctAnswer: "Inter Miami CF"
            },
            {
                question: "¿Cuántos Balones de Oro ha ganado Cristiano Ronaldo?",
                options: ["3", "5", "7", "6"],
                correctAnswer: "5"
            },
            {
                question: "¿Qué equipo ha ganado más veces la UEFA Champions League?",
                options: ["AC Milan", "Liverpool FC", "Bayern Munich", "Real Madrid"],
                correctAnswer: "Real Madrid"
            }
        ];

        let currentQuestionIndex = 0;
        let score = 0;

        const questionTextElement = document.getElementById('question-text');
        const optionsGridElement = document.getElementById('options-grid');
        const feedbackElement = document.getElementById('feedback');
        const nextButton = document.getElementById('next-btn');
        const scoreContainer = document.getElementById('score-container');
        const finalScoreElement = document.getElementById('final-score');
        const restartButton = document.getElementById('restart-btn');
        const questionContainer = document.getElementById('question-container');

        function startGame() {
            currentQuestionIndex = 0;
            score = 0;
            scoreContainer.classList.add('hidden');
            questionContainer.classList.remove('hidden');
            nextButton.classList.add('hidden'); // Ocultar al inicio
            feedbackElement.textContent = '';
            loadQuestion();
        }

        function loadQuestion() {
            feedbackElement.textContent = ''; // Limpiar feedback anterior
            nextButton.classList.add('hidden'); // Ocultar botón "Siguiente" al cargar nueva pregunta
            
            // Limpiar opciones anteriores
            optionsGridElement.innerHTML = '';

            const currentQuestion = questions[currentQuestionIndex];
            questionTextElement.textContent = currentQuestion.question;

            currentQuestion.options.forEach(option => {
                const button = document.createElement('button');
                button.textContent = option;
                button.classList.add('option-btn');
                button.addEventListener('click', () => selectAnswer(option, button, currentQuestion.correctAnswer));
                optionsGridElement.appendChild(button);
            });
        }

        function selectAnswer(selectedOption, button, correctAnswer) {
            // Deshabilitar todos los botones de opción
            const optionButtons = optionsGridElement.querySelectorAll('.option-btn');
            optionButtons.forEach(btn => btn.disabled = true);

            if (selectedOption === correctAnswer) {
                score++;
                feedbackElement.textContent = "¡Correcto! 🎉";
                feedbackElement.style.color = "green";
                button.classList.add('correct');
            } else {
                feedbackElement.textContent = `Incorrecto. La respuesta era: ${correctAnswer}`;
                feedbackElement.style.color = "red";
                button.classList.add('incorrect');
                // Resaltar la correcta
                optionButtons.forEach(btn => {
                    if (btn.textContent === correctAnswer) {
                        btn.classList.add('correct');
                    }
                });
            }
            nextButton.classList.remove('hidden'); // Mostrar botón "Siguiente"
        }

        function showNextQuestion() {
            currentQuestionIndex++;
            if (currentQuestionIndex < questions.length) {
                loadQuestion();
            } else {
                showResults();
            }
        }

        function showResults() {
            questionContainer.classList.add('hidden');
            nextButton.classList.add('hidden');
            feedbackElement.textContent = '';
            scoreContainer.classList.remove('hidden');
            finalScoreElement.textContent = `Tu puntuación final es: ${score} de ${questions.length}`;
        }

        nextButton.addEventListener('click', showNextQuestion);
        restartButton.addEventListener('click', startGame);

        // Iniciar el juego al cargar la página
        startGame();
    </script>

</body>
</html>
