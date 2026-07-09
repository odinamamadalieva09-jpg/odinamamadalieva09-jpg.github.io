# odinamamadalieva09-jpg.github.io
квиз для 7 класса по алгебре
<!DOCTYPE html>
<html lang="ru">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Своя игра: Путь в 7 класс</title>
<style>
    body {
        font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        background: linear-gradient(135deg, #0f2027, #203a43, #2c5364);
        color: white;
        margin: 0;
        padding: 0;
        min-height: 100vh;
    }
    .container { max-width: 1200px; margin: 0 auto; padding: 20px; }
    
    /* Стартовый экран */
    #start-screen {
        display: flex; flex-direction: column; align-items: center; justify-content: center; min-height: 100vh;
    }
    h1 {
        font-size: 3rem; text-align: center; margin-bottom: 20px;
        text-shadow: 2px 2px 4px rgba(0,0,0,0.5); color: #FFD700;
    }
    .team-setup {
        background: rgba(255, 255, 255, 0.1); padding: 30px; border-radius: 15px;
        width: 100%; max-width: 500px; backdrop-filter: blur(10px);
    }
    input, button {
        width: 100%; padding: 15px; margin: 10px 0; border: none; border-radius: 8px;
        font-size: 1.2rem; box-sizing: border-box;
    }
    input {
        background: rgba(255, 255, 255, 0.2); color: white; border: 1px solid rgba(255, 255, 255, 0.3);
    }
    input::placeholder { color: rgba(255, 255, 255, 0.7); }
    button {
        background: #e94560; color: white; cursor: pointer; transition: background 0.3s; font-weight: bold;
    }
    button:hover { background: #ff5e7e; }
    .btn-secondary { background: #0f3460; }
    .btn-secondary:hover { background: #1a4a80; }
    
    .team-item {
        display: flex; justify-content: space-between; align-items: center;
        background: rgba(255,255,255,0.1); padding: 10px; border-radius: 8px; margin-bottom: 10px;
    }
    .btn-remove {
        background: #e74c3c; width: auto; padding: 5px 15px; margin: 0; font-size: 1rem;
    }

    /* Игровой экран */
    #game-screen { display: flex; flex-direction: column; min-height: 100vh; }
    .scoreboard {
        display: flex; flex-wrap: wrap; gap: 15px; justify-content: center; margin-bottom: 20px;
    }
    .team-card {
        background: rgba(255, 255, 255, 0.1); padding: 15px 25px; border-radius: 10px;
        text-align: center; min-width: 150px;
    }
    .team-name { font-size: 1.2rem; font-weight: bold; }
    .team-score { font-size: 2rem; color: #FFD700; margin: 5px 0; }

    /* Игровое поле */
    .board {
        display: grid; grid-template-columns: repeat(6, 1fr); gap: 10px; margin-bottom: 20px;
    }
    .category-header {
        background: #0f3460; padding: 20px 10px; text-align: center; font-weight: bold;
        font-size: 1.1rem; border-radius: 8px; min-height: 80px;
        display: flex; align-items: center; justify-content: center;
    }
    .question-cell {
        background: #0000CD; color: #FFD700; font-size: 2rem; font-weight: bold;
        text-align: center; padding: 30px 10px; border-radius: 8px; cursor: pointer;
        transition: transform 0.2s, background 0.2s;
        display: flex; align-items: center; justify-content: center; min-height: 100px;
    }
    .question-cell:hover { background: #0000FF; transform: scale(1.05); }
    .question-cell.answered { visibility: hidden; }

    /* Модальное окно */
    .modal-overlay {
        position: fixed; top: 0; left: 0; right: 0; bottom: 0;
        background: rgba(0, 0, 0, 0.8); display: flex; align-items: center; justify-content: center;
        z-index: 1000; padding: 20px;
    }
    .modal-content {
        background: linear-gradient(135deg, #1a2a6c, #b21f1f, #fdbb2d);
        background-size: 400% 400%; animation: gradientBG 15s ease infinite;
        padding: 40px; border-radius: 20px; width: 100%; max-width: 800px;
        text-align: center; box-shadow: 0 10px 30px rgba(0,0,0,0.5);
        display: flex; flex-direction: column; max-height: 90vh; overflow-y: auto;
    }
    @keyframes gradientBG {
        0% { background-position: 0% 50%; } 50% { background-position: 100% 50%; } 100% { background-position: 0% 50%; }
    }
    .modal-category { font-size: 1.5rem; opacity: 0.8; }
    .modal-value { font-size: 3rem; font-weight: bold; color: #FFD700; margin: 10px 0; }
    .modal-question { font-size: 1.8rem; margin: 30px 0; line-height: 1.4; }
    .modal-answer {
        font-size: 2rem; font-weight: bold; color: #00FF00;
        background: rgba(0,0,0,0.5); padding: 20px; border-radius: 10px; display: none;
    }
    .modal-controls { margin-top: 30px; display: flex; flex-direction: column; gap: 15px; }
    .team-controls {
        display: flex; justify-content: space-between; align-items: center;
        background: rgba(255,255,255,0.1); padding: 15px; border-radius: 10px;
    }
    .team-controls .name { font-size: 1.2rem; font-weight: bold; }
    .score-btns { display: flex; gap: 10px; }
    .score-btn {
        padding: 10px 20px; font-size: 1.2rem; border-radius: 5px; width: auto; margin: 0;
    }
    .score-btn.plus { background: #2ecc71; }
    .score-btn.plus:hover { background: #27ae60; }
    .score-btn.minus { background: #e74c3c; }
    .score-btn.minus:hover { background: #c0392b; }
    .close-btn { background: #95a5a6; margin-top: 20px; }
    .close-btn:hover { background: #7f8c8d; }

    /* Адаптивность */
    @media (max-width: 768px) {
        .board { grid-template-columns: repeat(6, 1fr); gap: 5px; }
        .category-header { font-size: 0.8rem; padding: 10px 5px; min-height: 60px; }
        .question-cell { font-size: 1.2rem; padding: 15px 5px; min-height: 60px; }
        .modal-question { font-size: 1.3rem; }
        .modal-value { font-size: 2rem; }
        h1 { font-size: 2rem; }
    }
</style>
</head>
<body>

<!-- Стартовый экран -->
<div id="start-screen" class="container">
    <h1>Своя игра: Путь в 7 класс</h1>
    <div class="team-setup">
        <input type="text" id="team-name" placeholder="Введите название команды" onkeypress="if(event.key==='Enter') addTeam()">
        <button onclick="addTeam()">Добавить команду</button>
        <div id="teams-list"></div>
        <button class="btn-secondary" onclick="initGame()">Начать игру</button>
    </div>
</div>

<!-- Игровой экран -->
<div id="game-screen" class="container" style="display:none;">
    <div id="scoreboard" class="scoreboard"></div>
    <div id="board" class="board"></div>
</div>

<!-- Модальное окно -->
<div id="modal" class="modal-overlay" style="display:none;">
    <div class="modal-content">
        <div class="modal-category" id="modal-category"></div>
        <div class="modal-value" id="modal-value"></div>
        <div class="modal-question" id="modal-question"></div>
        <button class="btn-secondary" onclick="showAnswer()">Показать ответ</button>
        <div class="modal-answer" id="modal-answer"></div>
        <div class="modal-controls" id="modal-controls"></div>
        <button class="close-btn" onclick="closeQuestion()">Закрыть вопрос</button>
    </div>
</div>

<script>
const questionsData = [
    // Категория 1: Рациональные числа
    { cat: "Рациональные числа", val: 100, q: "Вычислите: -5 + 8.", a: "3" },
    { cat: "Рациональные числа", val: 200, q: "Найдите значение выражения: (-2) * (-3) - 4.", a: "2" },
    { cat: "Рациональные числа", val: 300, q: "Какой знак будет у произведения, если перемножить 15 отрицательных и 10 положительных чисел?", a: "Отрицательный (так как отрицательных чисел нечётное количество)" },
    { cat: "Рациональные числа", val: 400, q: "Расположите числа в порядке убывания: 0,37; -3/7; 0,375; -0,37.", a: "0,375; 0,37; -0,37; -3/7 (так как -3/7 ≈ -0,428)" },
    { cat: "Рациональные числа", val: 500, q: "Представьте в виде обыкновенной дроби периодическую дробь 0,(4).", a: "4/9" },

    // Категория 2: Проценты
    { cat: "Проценты", val: 100, q: "Чему равны 25% от 100?", a: "25" },
    { cat: "Проценты", val: 200, q: "Цена товара выросла на 10%. На сколько процентов она стала больше первоначальной?", a: "На 10%" },
    { cat: "Проценты", val: 300, q: "В классе 30 учеников. 40% из них — отличники. Сколько отличников в классе?", a: "12" },
    { cat: "Проценты", val: 400, q: "Арбуз массой 10 кг содержал 99% воды. После усушки он стал содержать 98% воды. Какова стала масса арбуза?", a: "5 кг (Сухое вещество весило 0,1 кг. 0,1 / (1 - 0,98) = 5)" },
    { cat: "Проценты", val: 500, q: "Цену товара повысили на 15%, а затем понизили на 15%. Как изменилась цена по сравнению с первоначальной?", a: "Понизилась на 2,25%" },

    // Категория 3: Выражения
    { cat: "Выражения", val: 100, q: "Найдите значение выражения 2x + 5 при x = 3.", a: "11" },
    { cat: "Выражения", val: 200, q: "При каком значении переменной выражение 1 / (x - 2) не имеет смысла?", a: "При x = 2" },
    { cat: "Выражения", val: 300, q: "Упростите выражение: 3(a + 2) - 3a.", a: "6" },
    { cat: "Выражения", val: 400, q: "Известно, что a - b = 4. Чему равно значение выражения (1/2)(b - a)?", a: "-2" },
    { cat: "Выражения", val: 500, q: "Составьте выражение для решения задачи: «Один рабочий делает 7 деталей в час, другой 9. Сколько они сделают за 4 часа?» и найдите ответ.", a: "(7 + 9) * 4 = 64" },

    // Категория 4: Тождества
    { cat: "Тождества", val: 100, q: "Раскройте скобки: -(x - 5).", a: "-x + 5" },
    { cat: "Тождества", val: 200, q: "Приведите подобные слагаемые: 5x + 2x - 3x.", a: "4x" },
    { cat: "Тождества", val: 300, q: "Является ли равенство (a + b)^2 = a^2 + 2ab + b^2 тождеством?", a: "Да" },
    { cat: "Тождества", val: 400, q: "Докажите, что при любом a значение выражения 3(a + 2) - 3a равно 6.", a: "3a + 6 - 3a = 6" },
    { cat: "Тождества", val: 500, q: "Упростите выражение: 2(3x - 4) - 3(2x - 1).", a: "-5" },

    // Категория 5: Уравнения
    { cat: "Уравнения", val: 100, q: "Решите уравнение: x + 5 = 12.", a: "7" },
    { cat: "Уравнения", val: 200, q: "Решите уравнение: 2x - 3 = 7.", a: "5" },
    { cat: "Уравнения", val: 300, q: "Решите уравнение: 3(x - 2) = x + 4.", a: "5" },
    { cat: "Уравнения", val: 400, q: "Какое уравнение не имеет корней: 2x + 3 = 2x + 8 или 33x = 18x?", a: "2x + 3 = 2x + 8 (так как 0x = 5)" },
    { cat: "Уравнения", val: 500, q: "Решите уравнение: |x - 2| = 5.", a: "7 и -3" },

    // Категория 6: Задачи
    { cat: "Задачи", val: 100, q: "У Игоря 5 яблок, у Пети на 2 больше. Сколько яблок у Пети?", a: "7" },
    { cat: "Задачи", val: 200, q: "В корзине было x яблок, в ящике в 2 раза больше. Сколько всего яблок?", a: "3x" },
    { cat: "Задачи", val: 300, q: "В одной кассе продали на 36 билетов больше, чем в другой. Всего продали 392 билета. Сколько билетов продали в каждой кассе?", a: "178 и 214" },
    { cat: "Задачи", val: 400, q: "Старинная задача. Из четырех жертвователей второй дал вдвое больше первого, третий — втрое больше второго, четвертый — вчетверо больше третьего, а все вместе дали 132 рупии. Сколько дал первый?", a: "4 рупии (x + 2x + 6x + 24x = 33x = 132 => x=4)" },
    { cat: "Задачи", val: 500, q: "Из 36 учащихся каждый изучает английский или немецкий. 25 — английский, 20 — немецкий. Сколько учеников изучают оба языка?", a: "9 (25 + 20 - 36 = 9)" }
];

let teams = [];
let currentQuestionIndex = -1;

function initGame() {
    if (teams.length === 0) {
        alert("Добавьте хотя бы одну команду!");
        return;
    }
    document.getElementById('start-screen').style.display = 'none';
    document.getElementById('game-screen').style.display = 'flex';
    renderBoard();
    renderScoreboard();
}

function addTeam() {
    const input = document.getElementById('team-name');
    const name = input.value.trim();
    if (name) {
        teams.push({ name: name, score: 0 });
        input.value = '';
        renderTeamsList();
    }
}

function removeTeam(index) {
    teams.splice(index, 1);
    renderTeamsList();
}

function renderTeamsList() {
    const list = document.getElementById('teams-list');
    list.innerHTML = '';
    teams.forEach((team, index) => {
        const div = document.createElement('div');
        div.className = 'team-item';
        div.innerHTML = `<span>${team.name}</span><button class="btn-remove" onclick="removeTeam(${index})">Удалить</button>`;
        list.appendChild(div);
    });
}

function renderBoard() {
    const board = document.getElementById('board');
    board.innerHTML = '';
    const categories = [...new Set(questionsData.map(q => q.cat))];
    
    categories.forEach(cat => {
        const header = document.createElement('div');
        header.className = 'category-header';
        header.textContent = cat;
        board.appendChild(header);
    });
    
    for (let val = 100; val <= 500; val += 100) {
        categories.forEach(cat => {
            const qIndex = questionsData.findIndex(q => q.cat === cat && q.val === val);
            const cell = document.createElement('div');
            cell.className = 'question-cell';
            cell.textContent = val;
            cell.onclick = () => openQuestion(qIndex);
            cell.id = `cell-${qIndex}`;
            board.appendChild(cell);
        });
    }
}

function renderScoreboard() {
    const scoreboard = document.getElementById('scoreboard');
    scoreboard.innerHTML = '';
    teams.forEach(team => {
        const card = document.createElement('div');
        card.className = 'team-card';
        card.innerHTML = `<div class="team-name">${team.name}</div><div class="team-score">${team.score}</div>`;
        scoreboard.appendChild(card);
    });
}

function openQuestion(index) {
    currentQuestionIndex = index;
    const q = questionsData[index];
    document.getElementById('modal-category').textContent = q.cat;
    document.getElementById('modal-value').textContent = q.val;
    document.getElementById('modal-question').textContent = q.q;
    document.getElementById('modal-answer').textContent = q.a;
    document.getElementById('modal-answer').style.display = 'none';
    document.getElementById('modal').style.display = 'flex';
    renderModalControls();
}

function renderModalControls() {
    const controls = document.getElementById('modal-controls');
    controls.innerHTML = '';
    teams.forEach((team, index) => {
        const div = document.createElement('div');
        div.className = 'team-controls';
        div.innerHTML = `
            <div class="name">${team.name}</div>
            <div class="score-btns">
                <button class="score-btn plus" onclick="updateScore(${index}, 1)">+${questionsData[currentQuestionIndex].val}</button>
                <button class="score-btn minus" onclick="updateScore(${index}, -1)">-${questionsData[currentQuestionIndex].val}</button>
            </div>
        `;
        controls.appendChild(div);
    });
}

function showAnswer() {
    document.getElementById('modal-answer').style.display = 'block';
}

function updateScore(teamIndex, multiplier) {
    const q = questionsData[currentQuestionIndex];
    teams[teamIndex].score += q.val * multiplier;
    renderScoreboard();
}

function closeQuestion() {
    document.getElementById('modal').style.display = 'none';
    if (currentQuestionIndex !== -1) {
        document.getElementById(`cell-${currentQuestionIndex}`).classList.add('answered');
    }
}
</script>
</body>
</html>
