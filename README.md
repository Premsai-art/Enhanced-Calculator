# Enhanced-Calculator
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Modern Calculator</title>
    <style>
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
        }

        body {
            background: linear-gradient(135deg, #1a1a2e 0%, #16213e 50%, #0f3460 100%);
            color: #fff;
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, sans-serif;
            min-height: 100vh;
            display: flex;
            align-items: center;
            justify-content: center;
            padding: 20px;
            font-smooth: always;
            -webkit-font-smoothing: antialiased;
        }

        .container {
            width: 100%;
            max-width: 420px;
            animation: slideUp 0.6s ease-out;
        }

        @keyframes slideUp {
            from {
                opacity: 0;
                transform: translateY(40px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }

        .calculator {
            background: linear-gradient(180deg, #1e293b 0%, #0f172a 100%);
            border-radius: 25px;
            padding: 24px;
            box-shadow: 
                0 20px 60px rgba(0, 0, 0, 0.6),
                0 0 40px rgba(59, 130, 246, 0.1);
            border: 1px solid rgba(59, 130, 246, 0.1);
            backdrop-filter: blur(20px);
            transition: all 0.3s ease;
        }

        .calculator:hover {
            box-shadow: 
                0 25px 70px rgba(0, 0, 0, 0.7),
                0 0 60px rgba(59, 130, 246, 0.15);
        }

        .header {
            text-align: center;
            margin-bottom: 24px;
        }

        .header h1 {
            font-size: 28px;
            font-weight: 700;
            background: linear-gradient(135deg, #60a5fa, #a78bfa);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
            letter-spacing: 0.5px;
        }

        .display {
            background: rgba(15, 23, 42, 0.9);
            border-radius: 16px;
            padding: 24px;
            margin-bottom: 20px;
            border: 2px solid rgba(59, 130, 246, 0.2);
            transition: all 0.3s ease;
            min-height: 100px;
            display: flex;
            flex-direction: column;
            justify-content: flex-end;
            gap: 8px;
        }

        .display:focus-within {
            border-color: #60a5fa;
            box-shadow: 0 0 20px rgba(96, 165, 250, 0.3);
        }

        .display .prev {
            font-size: 16px;
            color: #94a3b8;
            min-height: 24px;
            word-break: break-all;
            font-weight: 500;
        }

        .display .current {
            font-size: 48px;
            font-weight: 700;
            color: #f1f5f9;
            word-break: break-all;
            font-family: 'Monaco', 'Courier New', monospace;
            letter-spacing: 1px;
        }

        .display.error .current {
            color: #ff6b6b;
            font-size: 24px;
        }

        .buttons {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 12px;
            margin-bottom: 8px;
        }

        button {
            border: none;
            border-radius: 14px;
            padding: 20px 0;
            font-size: 18px;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.2s cubic-bezier(0.4, 0, 0.2, 1);
            color: #fff;
            position: relative;
            overflow: hidden;
            display: flex;
            align-items: center;
            justify-content: center;
            min-height: 60px;
            user-select: none;
            -webkit-tap-highlight-color: transparent;
        }

        /* Button ripple effect */
        button::before {
            content: '';
            position: absolute;
            top: 50%;
            left: 50%;
            width: 0;
            height: 0;
            border-radius: 50%;
            background: rgba(255, 255, 255, 0.3);
            transform: translate(-50%, -50%);
            pointer-events: none;
        }

        button:active::before {
            animation: ripple 0.6s ease-out;
        }

        @keyframes ripple {
            to {
                width: 300px;
                height: 300px;
                opacity: 0;
            }
        }

        button:active {
            transform: scale(0.96);
        }

        button:focus {
            outline: 2px solid #60a5fa;
            outline-offset: 2px;
        }

        /* Number buttons */
        .num {
            background: linear-gradient(135deg, #334155 0%, #475569 100%);
            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.2);
        }

        .num:hover {
            background: linear-gradient(135deg, #475569 0%, #64748b 100%);
            transform: translateY(-2px);
            box-shadow: 0 6px 20px rgba(0, 0, 0, 0.3);
        }

        .num:active {
            box-shadow: inset 0 2px 8px rgba(0, 0, 0, 0.3);
        }

        /* Operation buttons */
        .op {
            background: linear-gradient(135deg, #3b82f6 0%, #2563eb 100%);
            box-shadow: 0 4px 15px rgba(59, 130, 246, 0.3);
            font-weight: 700;
        }

        .op:hover {
            background: linear-gradient(135deg, #2563eb 0%, #1d4ed8 100%);
            transform: translateY(-2px);
            box-shadow: 0 6px 25px rgba(59, 130, 246, 0.4);
        }

        .op:active {
            box-shadow: inset 0 2px 8px rgba(0, 0, 0, 0.2);
        }

        .op.active {
            background: linear-gradient(135deg, #8b5cf6 0%, #7c3aed 100%);
            box-shadow: 0 0 20px rgba(139, 92, 246, 0.6);
        }

        /* Function buttons */
        .func {
            background: linear-gradient(135deg, #ef4444 0%, #dc2626 100%);
            box-shadow: 0 4px 15px rgba(239, 68, 68, 0.3);
            font-weight: 700;
        }

        .func:hover {
            background: linear-gradient(135deg, #dc2626 0%, #b91c1c 100%);
            transform: translateY(-2px);
            box-shadow: 0 6px 25px rgba(239, 68, 68, 0.4);
        }

        .func:active {
            box-shadow: inset 0 2px 8px rgba(0, 0, 0, 0.2);
        }

        /* Equals button */
        .equals {
            background: linear-gradient(135deg, #10b981 0%, #059669 100%);
            box-shadow: 0 4px 15px rgba(16, 185, 129, 0.3);
            grid-row: span 2;
            font-weight: 700;
            min-height: auto;
            padding: 0;
        }

        .equals:hover {
            background: linear-gradient(135deg, #059669 0%, #047857 100%);
            transform: translateY(-2px);
            box-shadow: 0 6px 25px rgba(16, 185, 129, 0.4);
        }

        .equals:active {
            box-shadow: inset 0 2px 8px rgba(0, 0, 0, 0.2);
        }

        /* Zero button spans 2 columns */
        .zero {
            grid-column: span 2;
        }

        /* Special button styles */
        .delete-btn {
            background: linear-gradient(135deg, #f59e0b 0%, #d97706 100%);
            box-shadow: 0 4px 15px rgba(245, 158, 11, 0.3);
        }

        .delete-btn:hover {
            background: linear-gradient(135deg, #d97706 0%, #b45309 100%);
            transform: translateY(-2px);
            box-shadow: 0 6px 25px rgba(245, 158, 11, 0.4);
        }

        /* Footer info */
        .footer {
            text-align: center;
            font-size: 12px;
            color: #64748b;
            padding-top: 16px;
            border-top: 1px solid rgba(59, 130, 246, 0.1);
            margin-top: 16px;
        }

        .footer p {
            margin: 8px 0 0 0;
        }

        .footer small {
            display: block;
            margin-top: 4px;
            color: #475569;
        }

        /* Responsive Design */
        @media (max-width: 480px) {
            .calculator {
                padding: 20px;
                border-radius: 20px;
            }

            .header h1 {
                font-size: 24px;
            }

            .display {
                padding: 20px;
                min-height: 90px;
                border-radius: 14px;
            }

            .display .current {
                font-size: 40px;
            }

            .display .prev {
                font-size: 14px;
            }

            .buttons {
                gap: 10px;
            }

            button {
                font-size: 16px;
                padding: 16px 0;
                min-height: 56px;
                border-radius: 12px;
            }
        }

        @media (max-width: 360px) {
            .calculator {
                padding: 16px;
            }

            .display {
                padding: 16px;
                min-height: 80px;
            }

            .display .current {
                font-size: 32px;
            }

            .buttons {
                gap: 8px;
            }

            button {
                font-size: 14px;
                padding: 12px 0;
                min-height: 48px;
                border-radius: 10px;
            }
        }

        /* Keyboard feedback */
        button:focus-visible {
            outline: 3px solid #60a5fa;
            outline-offset: 2px;
        }

        /* Success/Error animations */
        @keyframes shake {
            0%, 100% { transform: translateX(0); }
            25% { transform: translateX(-5px); }
            75% { transform: translateX(5px); }
        }

        .display.shake {
            animation: shake 0.4s ease-in-out;
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="calculator">
            <div class="header">
                <h1>🔢 Calculator</h1>
            </div>

            <div class="display">
                <div class="prev" id="prevDisplay"></div>
                <div class="current" id="currentDisplay">0</div>
            </div>

            <div class="buttons">
                <!-- Row 1: Functions -->
                <button class="func" id="clearBtn" title="Clear (ESC)">C</button>
                <button class="func delete-btn" id="signBtn" title="Toggle sign (+/-)">+/−</button>
                <button class="func" id="percentBtn" title="Percentage">%</button>
                <button class="op" id="divideBtn" title="Divide">÷</button>

                <!-- Row 2 -->
                <button class="num">7</button>
                <button class="num">8</button>
                <button class="num">9</button>
                <button class="op" id="multiplyBtn" title="Multiply">×</button>

                <!-- Row 3 -->
                <button class="num">4</button>
                <button class="num">5</button>
                <button class="num">6</button>
                <button class="op" id="subtractBtn" title="Subtract">−</button>

                <!-- Row 4 -->
                <button class="num">1</button>
                <button class="num">2</button>
                <button class="num">3</button>
                <button class="op" id="addBtn" title="Add">+</button>

                <!-- Row 5 -->
                <button class="num zero">0</button>
                <button class="num" id="decimalBtn" title="Decimal point">.</button>
                <button class="equals" id="equalsBtn" title="Calculate (ENTER)"> = </button>
            </div>

            <div class="footer">
                <p>Keyboard Support: <strong>0-9</strong> | <strong>+−×÷</strong> | <strong>ENTER</strong> | <strong>ESC</strong></p>
                <small>Press <strong>BACKSPACE</strong> to delete</small>
            </div>
        </div>
    </div>

    <script>
        class Calculator {
            constructor() {
                this.currentDisplay = document.getElementById("currentDisplay");
                this.prevDisplay = document.getElementById("prevDisplay");
                this.display = this.currentDisplay.parentElement;

                this.num1 = null;
                this.operation = null;
                this.currentInput = "0";
                this.resultShown = false;

                this.setupEventListeners();
            }

            setupEventListeners() {
                // Number buttons
                document.querySelectorAll(".num").forEach(btn => {
                    if (btn.id !== "decimalBtn") {
                        btn.addEventListener("click", () => this.appendDigit(btn.textContent));
                    }
                });

                // Decimal button
                document.getElementById("decimalBtn").addEventListener("click", () => this.appendDecimal());

                // Operation buttons
                document.getElementById("addBtn").addEventListener("click", () => this.chooseOperation("+"));
                document.getElementById("subtractBtn").addEventListener("click", () => this.chooseOperation("−"));
                document.getElementById("multiplyBtn").addEventListener("click", () => this.chooseOperation("×"));
                document.getElementById("divideBtn").addEventListener("click", () => this.chooseOperation("÷"));

                // Function buttons
                document.getElementById("clearBtn").addEventListener("click", () => this.clear());
                document.getElementById("signBtn").addEventListener("click", () => this.toggleSign());
                document.getElementById("percentBtn").addEventListener("click", () => this.percent());
                document.getElementById("equalsBtn").addEventListener("click", () => this.calculate());

                // Keyboard support
                document.addEventListener("keydown", (e) => this.handleKeyboard(e));
            }

            appendDigit(digit) {
                if (this.resultShown) {
                    this.currentInput = "0";
                    this.resultShown = false;
                    this.display.classList.remove("error");
                }

                if (this.currentInput === "0" && digit !== "0") {
                    this.currentInput = digit;
                } else if (this.currentInput !== "0" || digit === "0") {
                    this.currentInput += digit;
                }

                // Limit input length
                if (this.currentInput.length > 12) {
                    this.currentInput = this.currentInput.slice(0, -1);
                }

                this.updateDisplay();
            }

            appendDecimal() {
                if (this.resultShown) {
                    this.currentInput = "0";
                    this.resultShown = false;
                }
                if (!this.currentInput.includes(".")) {
                    this.currentInput += ".";
                }
                this.updateDisplay();
            }

            chooseOperation(op) {
                if (this.currentInput === "" || this.currentInput === ".") return;

                if (this.num1 === null) {
                    this.num1 = parseFloat(this.currentInput);
                } else if (!this.resultShown) {
                    this.num1 = this.compute();
                    if (this.num1 === null) return;
                    this.currentInput = String(this.num1);
                }

                this.operation = op;
                this.currentInput = "";
                this.resultShown = false;
                this.updateDisplay();
                this.highlightOperation(op);
            }

            compute() {
                const num2 = parseFloat(this.currentInput);
                if (this.operation === null || this.num1 === null) return null;

                let result;
                const operationSymbol = {
                    "+": "+",
                    "−": "-",
                    "×": "*",
                    "÷": "/"
                }[this.operation];

                try {
                    switch (operationSymbol) {
                        case "+":
                            result = this.num1 + num2;
                            break;
                        case "-":
                            result = this.num1 - num2;
                            break;
                        case "*":
                            result = this.num1 * num2;
                            break;
                        case "/":
                            if (num2 === 0) {
                                return null;
                            }
                            result = this.num1 / num2;
                            break;
                        default:
                            return null;
                    }

                    return Math.round(result * 100000000) / 100000000;
                } catch (e) {
                    return null;
                }
            }

            calculate() {
                if (this.operation === null || this.num1 === null || this.currentInput === "") return;

                const result = this.compute();

                if (result === null) {
                    this.showError("Cannot divide by 0");
                    return;
                }

                this.currentInput = String(result);
                this.num1 = null;
                this.operation = null;
                this.resultShown = true;
                this.removeOperationHighlight();
                this.updateDisplay();
            }

            toggleSign() {
                if (this.currentInput !== "0") {
                    const value = parseFloat(this.currentInput);
                    this.currentInput = String(-value);
                    this.updateDisplay();
                }
            }

            percent() {
                const value = parseFloat(this.currentInput);
                if (!isNaN(value)) {
                    this.currentInput = String(value / 100);
                    this.updateDisplay();
                }
            }

            deleteLastDigit() {
                if (this.currentInput.length <= 1) {
                    this.currentInput = "0";
                } else {
                    this.currentInput = this.currentInput.slice(0, -1);
                }
                this.updateDisplay();
            }

            clear() {
                this.num1 = null;
                this.operation = null;
                this.currentInput = "0";
                this.resultShown = false;
                this.display.classList.remove("error");
                this.removeOperationHighlight();
                this.updateDisplay();
            }

            updateDisplay() {
                this.currentDisplay.textContent = this.formatDisplay(this.currentInput);

                if (this.operation && this.num1 !== null) {
                    this.prevDisplay.textContent = `${this.formatDisplay(String(this.num1))} ${this.operation}`;
                } else {
                    this.prevDisplay.textContent = "";
                }
            }

            formatDisplay(str) {
                if (str === "" || str === ".") return "0";

                const parts = str.split(".");
                const integerPart = parseInt(parts[0]).toLocaleString("en-US");

                if (parts.length === 2) {
                    return `${integerPart}.${parts[1]}`;
                }
                return integerPart;
            }

            showError(message) {
                this.display.classList.add("shake", "error");
                this.currentDisplay.textContent = message;

                setTimeout(() => {
                    this.display.classList.remove("shake");
                    this.clear();
                }, 2000);
            }

            highlightOperation(op) {
                this.removeOperationHighlight();
                const btnMap = {
                    "+": "addBtn",
                    "−": "subtractBtn",
                    "×": "multiplyBtn",
                    "÷": "divideBtn"
                };
                if (btnMap[op]) {
                    document.getElementById(btnMap[op]).classList.add("active");
                }
            }

            removeOperationHighlight() {
                document.querySelectorAll(".op").forEach(btn => btn.classList.remove("active"));
            }

            handleKeyboard(e) {
                if (e.key >= "0" && e.key <= "9") {
                    e.preventDefault();
                    this.appendDigit(e.key);
                } else if (e.key === ".") {
                    e.preventDefault();
                    this.appendDecimal();
                } else if (e.key === "+") {
                    e.preventDefault();
                    this.chooseOperation("+");
                } else if (e.key === "-") {
                    e.preventDefault();
                    this.chooseOperation("−");
                } else if (e.key === "*") {
                    e.preventDefault();
                    this.chooseOperation("×");
                } else if (e.key === "/") {
                    e.preventDefault();
                    this.chooseOperation("÷");
                } else if (e.key === "%") {
                    e.preventDefault();
                    this.percent();
                } else if (e.key === "Enter" || e.key === "=") {
                    e.preventDefault();
                    this.calculate();
                } else if (e.key === "Backspace") {
                    e.preventDefault();
                    this.deleteLastDigit();
                } else if (e.key === "Escape") {
                    e.preventDefault();
                    this.clear();
                }
            }
        }

        // Initialize calculator
        document.addEventListener("DOMContentLoaded", () => {
            new Calculator();
        });
    </script>
</body>
</html>
