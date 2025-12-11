<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Neon Arithmetic Calculator</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        body {
            transition: background-color 0.3s ease;
        }
        .calculator-grid {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 1rem;
        }
        .calc-container,
        .calc-display,
        .calculator-grid button {
            transition: all 0.3s ease;
        }

        .neon-shadow {
            box-shadow: 0 0 5px #00FFFF, 0 0 10px #00FFFF, 0 0 15px #00FFFF;
        }
        .neon-shadow:hover {
            box-shadow: 0 0 8px #38BDF8, 0 0 15px #38BDF8, 0 0 25px #38BDF8;
            transform: scale(1.02);
        }
        .btn-func {
            background-color: #6D28D9;
            color: white;
        }
        
        .max-w-md {
            max-width: 24rem;
        }

        .light-mode {
            background-color: #F3F4F6;
        }
        .light-mode .calc-container {
            background-color: #FFFFFF;
            border-color: #93C5FD;
            box-shadow: 0 20px 25px -5px rgba(0, 0, 0, 0.1), 0 10px 10px -5px rgba(0, 0, 0, 0.04);
        }
        .light-mode .calc-title {
            color: #1D4ED8;
        }
        .light-mode .calc-display {
            background-color: #E5E7EB;
            border-color: #93C5FD;
        }
        .light-mode #previous-operation {
            color: #6B7280;
        }
        .light-mode #current-operation {
            color: #1F2937;
        }

        .light-mode [data-number],
        .light-mode [data-action="decimal"] {
            background-color: #F3F4F6 !important;
            color: #1F2937 !important;
        }

        .light-mode [data-operator],
        .light-mode .btn-func {
            background-color: #3B82F6 !important;
            color: white !important;
        }
        .light-mode [data-action="equals"] {
            background-color: #4F46E5 !important;
        }
        .light-mode [data-action="clear"] {
            color: #EF4444 !important;
        }
        .light-mode [data-action="delete"] {
            color: #3B82F6 !important;
        }
        
        .light-mode .neon-shadow {
            box-shadow: 0 0 3px #93C5FD, 0 0 5px #93C5FD;
        }
        .light-mode .neon-shadow:hover {
            box-shadow: 0 0 5px #3B82F6, 0 0 10px #3B82F6;
        }
    </style>
</head>
<body class="bg-gray-900 min-h-screen flex items-center justify-center p-4 font-mono" id="calculator-body">

    <div class="w-full max-w-md bg-gray-800 p-6 rounded-3xl shadow-2xl border-2 border-cyan-500 calc-container">
        
        <div class="flex justify-between items-center mb-6">
            <h1 class="text-2xl font-bold text-cyan-400 calc-title">Digi Calc</h1>
            <button id="theme-toggle" class="p-2 rounded-full text-cyan-400 hover:text-cyan-200 transition duration-300">
                <svg id="moon-icon" xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M20.354 15.354A9 9 0 018.646 3.646 9.003 9.003 0 0012 21a9.003 9.003 0 008.354-5.646z" />
                </svg>
                <svg id="sun-icon" xmlns="http://www.w3.org/2000/svg" class="h-6 w-6 hidden" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 3v1m0 16v1m9-9h-1M4 12H3m15.364 6.364l-.707-.707M6.343 6.343l-.707-.707m12.728 0l-.707.707M6.343 17.657l-.707.707M16 12a4 4 0 11-8 0 4 4 0 018 0z" />
                </svg>
            </button>
        </div>

        <div id="display-container" class="mb-6 bg-gray-900 p-4 rounded-xl border border-cyan-600 calc-display">
            <div id="previous-operation" class="text-cyan-200 text-sm h-5 overflow-hidden text-right"></div>
            <div id="current-operation" class="text-white text-4xl h-10 overflow-hidden text-right font-light">0</div>
        </div>

        <div class="calculator-grid">
            <button data-action="open-paren" class="p-4 text-xl font-bold rounded-xl btn-func neon-shadow">(</button>
            <button data-action="close-paren" class="p-4 text-xl font-bold rounded-xl btn-func neon-shadow">)</button>
            <button data-action="log" class="p-4 text-xl font-bold rounded-xl btn-func neon-shadow">log</button>
            <button data-action="sqrt" class="p-4 text-xl font-bold rounded-xl btn-func neon-shadow">&radic;</button>

            <button data-action="clear" class="p-4 text-2xl font-bold rounded-xl bg-gray-700 text-red-400 neon-shadow">AC</button>
            <button data-action="delete" class="p-4 text-2xl font-bold rounded-xl bg-gray-700 text-cyan-400 neon-shadow">&larr;</button>
            <button data-action="percent" class="p-4 text-xl font-bold rounded-xl btn-func neon-shadow">%</button>
            <button data-operator="power" class="p-4 text-2xl font-bold rounded-xl btn-func neon-shadow">xʸ</button>

            <button data-number="7" class="p-4 text-2xl font-bold rounded-xl bg-gray-700 text-white hover:bg-gray-600 neon-shadow">7</button>
            <button data-number="8" class="p-4 text-2xl font-bold rounded-xl bg-gray-700 text-white hover:bg-gray-600 neon-shadow">8</button>
            <button data-number="9" class="p-4 text-2xl font-bold rounded-xl bg-gray-700 text-white hover:bg-gray-600 neon-shadow">9</button>
            <button data-operator="divide" class="p-4 text-2xl font-bold rounded-xl bg-cyan-700 text-white neon-shadow">&divide;</button>

            <button data-number="4" class="p-4 text-2xl font-bold rounded-xl bg-gray-700 text-white hover:bg-gray-600 neon-shadow">4</button>
            <button data-number="5" class="p-4 text-2xl font-bold rounded-xl bg-gray-700 text-white hover:bg-gray-600 neon-shadow">5</button>
            <button data-number="6" class="p-4 text-2xl font-bold rounded-xl bg-gray-700 text-white hover:bg-gray-600 neon-shadow">6</button>
            <button data-operator="multiply" class="p-4 text-2xl font-bold rounded-xl bg-cyan-700 text-white neon-shadow">&times;</button>

            <button data-number="1" class="p-4 text-2xl font-bold rounded-xl bg-gray-700 text-white hover:bg-gray-600 neon-shadow">1</button>
            <button data-number="2" class="p-4 text-2xl font-bold rounded-xl bg-gray-700 text-white hover:bg-gray-600 neon-shadow">2</button>
            <button data-number="3" class="p-4 text-2xl font-bold rounded-xl bg-gray-700 text-white hover:bg-gray-600 neon-shadow">3</button>
            <button data-operator="subtract" class="p-4 text-2xl font-bold rounded-xl bg-cyan-700 text-white neon-shadow">-</button>

            <button data-number="0" class="p-4 text-2xl font-bold rounded-xl bg-gray-700 text-white hover:bg-gray-600 neon-shadow">0</button>
            <button data-action="decimal" class="p-4 text-2xl font-bold rounded-xl bg-gray-700 text-white hover:bg-gray-600 neon-shadow">.</button>
            <button data-action="equals" class="p-4 text-2xl font-bold rounded-xl bg-purple-600 text-white neon-shadow">=</button>
            <button data-operator="add" class="p-4 text-2xl font-bold rounded-xl bg-cyan-700 text-white neon-shadow">+</button>
        </div>

    </div>

    <script>
        const currentDisplay = document.getElementById('current-operation');
        const previousDisplay = document.getElementById('previous-operation');
        const buttons = document.querySelectorAll('button');
        const themeToggle = document.getElementById('theme-toggle');
        const body = document.getElementById('calculator-body');
        const moonIcon = document.getElementById('moon-icon');
        const sunIcon = document.getElementById('sun-icon');

        let expression = ''; 
        let isResult = false; 

        function updateDisplay() {
            if (expression.includes('Error')) {
                currentDisplay.innerText = expression;
                previousDisplay.innerText = '';
                return;
            } 
            
            currentDisplay.innerText = expression || '0';
            
            if (!isResult) {
                previousDisplay.innerText = expression.length > 30 ? '...' + expression.slice(-30) : expression;
            } else {
                previousDisplay.innerText = '';
            }
        }

        function getEvalExpression(exp) {
            return exp
                .replace(/×/g, '*')
                .replace(/÷/g, '/')
                .replace(/\^/g, '**');
        }

        function appendNumber(number) {
            if (isResult || expression.includes('Error')) {
                expression = number.toString();
                isResult = false;
            } else {
                expression += number.toString();
            }
        }

        function appendDecimal() {
            if (isResult || expression.includes('Error')) {
                expression = '0.';
                isResult = false;
                return;
            }

            const lastPartMatch = expression.match(/(\d+\.?\d*)$/);
            if (lastPartMatch && !lastPartMatch[0].includes('.')) {
                expression += '.';
            } else if (!expression.match(/[\d)]$/) && expression !== '') {
                expression += '0.';
            } else if (expression === '') {
                expression = '0.';
            }
        }

        function chooseOperation(op) {
            if (isResult) {
                expression = expression + op;
                isResult = false;
            } else if (expression.includes('Error')) {
                return;
            } else if (expression === '') {
                if (op === '-') {
                    expression = '-';
                }
                return;
            } else {
                const lastChar = expression.slice(-1);
                const isOperator = ['+', '-', '×', '÷', '^'].includes(lastChar);
                
                if (isOperator) {
                    expression = expression.slice(0, -1) + op;
                } else if (lastChar === '(') {
                    if (op === '-' || op === '+') {
                        expression += op;
                    }
                } else {
                    expression += op;
                }
            }
        }
        
        function handleParen(paren) {
            if (isResult || expression.includes('Error')) {
                expression = paren;
                isResult = false;
                return;
            }

            const lastChar = expression.slice(-1);
            
            if (paren === '(') {
                if (lastChar.match(/[\d)]$/)) {
                    expression += '×' + paren;
                } else {
                    expression += paren;
                }
            } else if (paren === ')') {
                const openCount = (expression.match(/\(/g) || []).length;
                const closeCount = (expression.match(/\)/g) || []).length;

                if (openCount > closeCount && lastChar.match(/[\d)]$/)) {
                    expression += paren;
                }
            }
        }

        function calculate() {
            if (expression === '' || expression.includes('Error')) return;
            
            previousDisplay.innerText = expression;
            let evalString = getEvalExpression(expression);

            try {
                const openCount = (evalString.match(/\(/g) || []).length;
                const closeCount = (evalString.match(/\)/g) || []).length;
                if (openCount !== closeCount) {
                     throw new Error("Syntax Error");
                }

                const result = new Function('return ' + evalString)();

                if (!isFinite(result)) {
                    throw new Error("Result Error");
                }
                
                expression = parseFloat(result.toFixed(10)).toString();

            } catch (e) {
                expression = 'Syntax Error';
            }
            
            isResult = true;
        }
        
        function handleSingleOperandAction(action) {
            if (!isResult) {
                calculate();
            }

            if (expression === '' || expression.includes('Error')) return;
            
            let num = parseFloat(expression);
            let result;

            try {
                switch (action) {
                    case 'log':
                        if (num <= 0) {
                            throw new Error("Domain Error");
                        }
                        result = Math.log10(num);
                        break;
                    case 'sqrt':
                        if (num < 0) {
                            throw new Error("Domain Error");
                        }
                        result = Math.sqrt(num);
                        break;
                    case 'percent':
                        result = num / 100;
                        break;
                    default:
                        return;
                }
                
                expression = parseFloat(result.toFixed(10)).toString();
            } catch (e) {
                expression = e.message;
            }

            isResult = true;
        }

        function clearAll() {
            expression = '';
            isResult = false;
            previousDisplay.innerText = '';
        }

        function deleteLast() {
            if (expression.includes('Error')) {
                clearAll();
                updateDisplay();
                return;
            }
            if (isResult) {
                clearAll();
                return;
            }
            expression = expression.slice(0, -1);
        }

        function toggleTheme() {
            const isLightMode = body.classList.toggle('light-mode');
            
            if (isLightMode) {
                moonIcon.classList.add('hidden');
                sunIcon.classList.remove('hidden');
                themeToggle.classList.remove('text-cyan-400', 'hover:text-cyan-200');
                themeToggle.classList.add('text-blue-500', 'hover:text-blue-700');
            } else {
                moonIcon.classList.remove('hidden');
                sunIcon.classList.add('hidden');
                themeToggle.classList.add('text-cyan-400', 'hover:text-cyan-200');
                themeToggle.classList.remove('text-blue-500', 'hover:text-blue-700');
            }
        }
        themeToggle.addEventListener('click', toggleTheme);

        function handleKeyboardInput(key) {
            if (key >= '0' && key <= '9') {
                appendNumber(key);
            } else if (key === '.') {
                appendDecimal();
            } else if (key === '+' || key === '-') {
                chooseOperation(key);
            } else if (key === '*' || key === 'x') {
                chooseOperation('×');
            } else if (key === '/' || key === '÷') {
                chooseOperation('÷');
            } else if (key === '^') {
                chooseOperation('^');
            } else if (key === '(') {
                handleParen('(');
            } else if (key === ')') {
                handleParen(')');
            } else if (key === 'Enter' || key === '=') {
                calculate();
            } else if (key === 'Backspace') {
                deleteLast();
            } else if (key === 'Delete') {
                clearAll();
            } else if (key === '%') {
                handleSingleOperandAction('percent');
            }
            updateDisplay();
        }

        document.addEventListener('keydown', (e) => {
            if (e.key === 'Enter') {
                e.preventDefault();
            }
            handleKeyboardInput(e.key);
        });

        buttons.forEach(button => {
            button.addEventListener('click', () => {
                const number = button.dataset.number;
                const operator = button.dataset.operator;
                const action = button.dataset.action;
                
                if (button.id === 'theme-toggle') return;

                if (number != null) {
                    appendNumber(number);
                } else if (action === 'open-paren') {
                    handleParen('(');
                } else if (action === 'close-paren') {
                    handleParen(')');
                } else if (operator != null) {
                    let opSymbol;
                    switch (operator) {
                        case 'add': opSymbol = '+'; break;
                        case 'subtract': opSymbol = '-'; break;
                        case 'multiply': opSymbol = '×'; break;
                        case 'divide': opSymbol = '÷'; break;
                        case 'power': opSymbol = '^'; break; 
                    }
                    chooseOperation(opSymbol);
                } else if (action === 'equals') {
                    calculate();
                } else if (action === 'clear') {
                    clearAll();
                } else if (action === 'delete') {
                    deleteLast();
                } else if (action === 'decimal') {
                    appendDecimal();
                } else if (action === 'log' || action === 'sqrt' || action === 'percent') {
                    handleSingleOperandAction(action);
                }

                updateDisplay();
