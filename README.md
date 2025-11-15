<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Modern Calculator</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }
        
        body {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 20px;
        }
        
        .calculator {
            background: #2d3748;
            border-radius: 20px;
            box-shadow: 0 20px 50px rgba(0, 0, 0, 0.3);
            width: 100%;
            max-width: 400px;
            overflow: hidden;
        }
        
        .display {
            background: #1a202c;
            color: white;
            padding: 30px 20px;
            text-align: right;
            min-height: 120px;
            display: flex;
            flex-direction: column;
            justify-content: flex-end;
        }
        
        .previous-operand {
            color: #a0aec0;
            font-size: 1.2rem;
            margin-bottom: 10px;
            min-height: 1.5rem;
            word-wrap: break-word;
            word-break: break-all;
        }
        
        .current-operand {
            color: white;
            font-size: 2.5rem;
            font-weight: 300;
            word-wrap: break-word;
            word-break: break-all;
        }
        
        .buttons {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 1px;
            background: #4a5568;
        }
        
        button {
            border: none;
            background: #2d3748;
            color: white;
            font-size: 1.5rem;
            padding: 20px;
            cursor: pointer;
            transition: all 0.2s ease;
            outline: none;
        }
        
        button:hover {
            background: #4a5568;
        }
        
        button:active {
            background: #1a202c;
            transform: scale(0.95);
        }
        
        .operator {
            background: #3182ce;
            color: white;
        }
        
        .operator:hover {
            background: #4299e1;
        }
        
        .equals {
            background: #38a169;
            grid-column: span 2;
        }
        
        .equals:hover {
            background: #48bb78;
        }
        
        .clear, .delete {
            background: #e53e3e;
        }
        
        .clear:hover, .delete:hover {
            background: #f56565;
        }
        
        .zero {
            grid-column: span 2;
        }
        
        .header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 15px 20px;
            background: #1a202c;
            color: #a0aec0;
        }
        
        .title {
            font-size: 1.2rem;
            font-weight: 500;
        }
        
        .theme-toggle {
            background: none;
            border: none;
            color: #a0aec0;
            font-size: 1.2rem;
            cursor: pointer;
            padding: 5px;
            border-radius: 5px;
            transition: color 0.3s;
        }
        
        .theme-toggle:hover {
            color: white;
        }
        
        .history-panel {
            position: absolute;
            top: 0;
            right: 0;
            width: 300px;
            height: 100%;
            background: #2d3748;
            transform: translateX(100%);
            transition: transform 0.3s ease;
            padding: 20px;
            overflow-y: auto;
            box-shadow: -5px 0 15px rgba(0, 0, 0, 0.2);
        }
        
        .history-panel.active {
            transform: translateX(0);
        }
        
        .history-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 20px;
            padding-bottom: 10px;
            border-bottom: 1px solid #4a5568;
        }
        
        .history-title {
            font-size: 1.2rem;
            color: white;
        }
        
        .close-history {
            background: none;
            border: none;
            color: #a0aec0;
            font-size: 1.2rem;
            cursor: pointer;
        }
        
        .history-list {
            list-style: none;
        }
        
        .history-item {
            padding: 10px 0;
            border-bottom: 1px solid #4a5568;
            color: #a0aec0;
            font-size: 1rem;
        }
        
        .history-expression {
            color: white;
            font-size: 1.1rem;
            margin-bottom: 5px;
        }
        
        .history-result {
            color: #38a169;
            font-weight: 500;
        }
        
        .clear-history {
            background: #e53e3e;
            color: white;
            border: none;
            padding: 10px 15px;
            border-radius: 5px;
            cursor: pointer;
            margin-top: 20px;
            width: 100%;
            font-size: 1rem;
        }
        
        .clear-history:hover {
            background: #f56565;
        }
        
        @media (max-width: 500px) {
            .calculator {
                max-width: 100%;
            }
            
            .history-panel {
                width: 100%;
            }
            
            button {
                padding: 15px;
                font-size: 1.2rem;
            }
            
            .current-operand {
                font-size: 2rem;
            }
        }
    </style>
</head>
<body>
    <div class="calculator">
        <div class="header">
            <div class="title">Calculator</div>
            <div>
                <button class="theme-toggle" id="historyToggle">
                    <i class="fas fa-history"></i>
                </button>
            </div>
        </div>
        
        <div class="display">
            <div class="previous-operand" id="previousOperand"></div>
            <div class="current-operand" id="currentOperand">0</div>
        </div>
        
        <div class="buttons">
            <button class="clear" id="clear">C</button>
            <button class="delete" id="delete">⌫</button>
            <button class="operator" data-operation="÷">÷</button>
            <button class="operator" data-operation="*">×</button>
            
            <button class="number" data-number="7">7</button>
            <button class="number" data-number="8">8</button>
            <button class="number" data-number="9">9</button>
            <button class="operator" data-operation="-">-</button>
            
            <button class="number" data-number="4">4</button>
            <button class="number" data-number="5">5</button>
            <button class="number" data-number="6">6</button>
            <button class="operator" data-operation="+">+</button>
            
            <button class="number" data-number="1">1</button>
            <button class="number" data-number="2">2</button>
            <button class="number" data-number="3">3</button>
            <button class="equals" id="equals">=</button>
            
            <button class="number zero" data-number="0">0</button>
            <button class="number" data-number=".">.</button>
        </div>
    </div>
    
    <div class="history-panel" id="historyPanel">
        <div class="history-header">
            <div class="history-title">Calculation History</div>
            <button class="close-history" id="closeHistory">
                <i class="fas fa-times"></i>
            </button>
        </div>
        <ul class="history-list" id="historyList">
            <!-- History items will be added here dynamically -->
        </ul>
        <button class="clear-history" id="clearHistory">Clear History</button>
    </div>

    <script>
        document.addEventListener('DOMContentLoaded', function() {
            const previousOperandElement = document.getElementById('previousOperand');
            const currentOperandElement = document.getElementById('currentOperand');
            const clearButton = document.getElementById('clear');
            const deleteButton = document.getElementById('delete');
            const equalsButton = document.getElementById('equals');
            const numberButtons = document.querySelectorAll('.number');
            const operationButtons = document.querySelectorAll('.operator');
            const historyToggle = document.getElementById('historyToggle');
            const historyPanel = document.getElementById('historyPanel');
            const closeHistory = document.getElementById('closeHistory');
            const historyList = document.getElementById('historyList');
            const clearHistoryButton = document.getElementById('clearHistory');
          
            let currentOperand = '0';
            let previousOperand = '';
            let operation = undefined;
            let calculationHistory = JSON.parse(localStorage.getItem('calculatorHistory')) || [];
            
            function init() {
                updateDisplay();
                renderHistory();
                numberButtons.forEach(button => {
                    button.addEventListener('click', () => {
                        appendNumber(button.getAttribute('data-number'));
                        updateDisplay();
                    });
                });
                
                operationButtons.forEach(button => {
                    button.addEventListener('click', () => {
                        chooseOperation(button.getAttribute('data-operation'));
                        updateDisplay();
                    });
                });
                
                equalsButton.addEventListener('click', () => {
                    compute();
                    updateDisplay();
                });
                
                clearButton.addEventListener('click', () => {
                    clear();
                    updateDisplay();
                });
                
                deleteButton.addEventListener('click', () => {
                    deleteNumber();
                    updateDisplay();
                });
                
                historyToggle.addEventListener('click', toggleHistory);
                closeHistory.addEventListener('click', toggleHistory);
                clearHistoryButton.addEventListener('click', clearHistory);
                
                document.addEventListener('keydown', handleKeyboardInput);
            }
            
            function appendNumber(number) {
                if (number === '.' && currentOperand.includes('.')) return;
                
                if (currentOperand === '0' && number !== '.') {
                    currentOperand = number;
                } else {
                    currentOperand += number;
                }
            }
            
            function chooseOperation(selectedOperation) {
                if (currentOperand === '') return;
                
                if (previousOperand !== '') {
                    compute();
                }
                
                operation = selectedOperation;
                previousOperand = currentOperand;
                currentOperand = '';
            }
            
            function compute() {
                let computation;
                const prev = parseFloat(previousOperand);
                const current = parseFloat(currentOperand);
                
                if (isNaN(prev) || isNaN(current)) return;
                
                switch (operation) {
                    case '+':
                        computation = prev + current;
                        break;
                    case '-':
                        computation = prev - current;
                        break;
                    case '*':
                        computation = prev * current;
                        break;
                    case '÷':
                        computation = prev / current;
                        break;
                    default:
                        return;
                }
                
                const historyItem = {
                    expression: `${previousOperand} ${operation} ${currentOperand}`,
                    result: computation.toString()
                };
                
                calculationHistory.unshift(historyItem);
                if (calculationHistory.length > 10) {
                    calculationHistory.pop();
                }
                
                localStorage.setItem('calculatorHistory', JSON.stringify(calculationHistory));
                renderHistory();
                
                currentOperand = computation.toString();
                operation = undefined;
                previousOperand = '';
            }
            
            function clear() {
                currentOperand = '0';
                previousOperand = '';
                operation = undefined;
            }
            
            function deleteNumber() {
                if (currentOperand.length === 1) {
                    currentOperand = '0';
                } else {
                    currentOperand = currentOperand.slice(0, -1);
                }
            }
            
            function updateDisplay() {
                currentOperandElement.innerText = currentOperand;
                
                if (operation != null) {
                    previousOperandElement.innerText = `${previousOperand} ${operation}`;
                } else {
                    previousOperandElement.innerText = previousOperand;
                }
            }
            
            function handleKeyboardInput(e) {
                if ((e.key >= '0' && e.key <= '9') || e.key === '.') {
                    appendNumber(e.key);
                } else if (e.key === '+' || e.key === '-' || e.key === '*' || e.key === '/') {
                    let operation;
                    if (e.key === '/') operation = '÷';
                    else if (e.key === '*') operation = '*';
                    else operation = e.key;
                    chooseOperation(operation);
                } else if (e.key === 'Enter' || e.key === '=') {
                    compute();
                } else if (e.key === 'Escape') {
                    clear();
                } else if (e.key === 'Backspace') {
                    deleteNumber();
                }
                updateDisplay();
            }
            
            function toggleHistory() {
                historyPanel.classList.toggle('active');
            }
            
            function renderHistory() {
                historyList.innerHTML = '';
                
                if (calculationHistory.length === 0) {
                    historyList.innerHTML = '<li class="history-item">No calculations yet</li>';
                    return;
                }
                
                calculationHistory.forEach(item => {
                    const li = document.createElement('li');
                    li.className = 'history-item';
                    li.innerHTML = `
                        <div class="history-expression">${item.expression}</div>
                        <div class="history-result">= ${item.result}</div>
                    `;
                    historyList.appendChild(li);
                });
            }
            
            function clearHistory() {
                calculationHistory = [];
                localStorage.setItem('calculatorHistory', JSON.stringify(calculationHistory));
                renderHistory();
            }
            
            init();
        });
    </script>
</body>
</html>
