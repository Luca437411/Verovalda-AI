
<!DOCTYPE html>
<html lang="ro">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<title>Vera Valda AI</title>

<style>
body {
margin: 0;
font-family: Arial, sans-serif;
background: linear-gradient(135deg, #7b2cff, #d946ef, #f472b6);
min-height: 100vh;
display: flex;
justify-content: center;
align-items: center;
}

.app {
width: 90%;
max-width: 700px;
background: white;
border-radius: 25px;
padding: 25px;
box-shadow: 0 15px 40px rgba(0,0,0,0.25);
}

h1 {
text-align: center;
color: #7b2cff;
margin-bottom: 5px;
}

.subtitle {
text-align: center;
color: #666;
margin-bottom: 25px;
}

#chat {
height: 400px;
overflow-y: auto;
border: 2px solid #e5d5ff;
border-radius: 18px;
padding: 15px;
background: #faf7ff;
margin-bottom: 15px;
}

.message {
margin: 10px 0;
padding: 12px 15px;
border-radius: 15px;
max-width: 85%;
line-height: 1.5;
}

.user {
background: #eadcff;
margin-left: auto;
text-align: right;
}

.ai {
background: #f1f1f1;
margin-right: auto;
}

.input-area {
display: flex;
gap: 10px;
}

input {
flex: 1;
padding: 14px;
border-radius: 15px;
border: 2px solid #d8c1ff;
font-size: 16px;
outline: none;
}

button {
padding: 14px 20px;
border: none;
border-radius: 15px;
background: #7b2cff;
color: white;
font-weight: bold;
cursor: pointer;
}

button:hover {
background: #6420d6;
}

.examples {
margin-top: 15px;
color: #666;
font-size: 14px;
}
</style>
</head>

<body>

<div class="app">

<h1>Vera Valda AI 🤖</h1>

<div class="subtitle">
Asistentul tău pentru întrebări și matematică
</div>

<div id="chat">

<div class="message ai">
<b>Vera Valda AI:</b><br>
Bună! Ce faci? 😊
</div>

</div>

<div class="input-area">
<input
id="question"
type="text"
placeholder="Scrie întrebarea aici..."
onkeydown="handleEnter(event)"
>

<button onclick="askAI()">Trimite</button>
</div>

<div class="examples">
Exemple: „1+1”, „12 × 8”, „sqrt(144)”, „x + 5 = 12”, „De ce este cerul albastru?”
</div>

</div>


<script>

function addMessage(text, type) {

const chat = document.getElementById("chat");

const message = document.createElement("div");

message.className = "message " + type;

message.innerHTML = text;

chat.appendChild(message);

chat.scrollTop = chat.scrollHeight;
}


function handleEnter(event) {

if (event.key === "Enter") {
askAI();
}

}


function askAI() {

const input = document.getElementById("question");

const question = input.value.trim();

if (question === "") {
return;
}

addMessage("<b>Tu:</b><br>" + escapeHTML(question), "user");

input.value = "";

const answer = getAnswer(question);

setTimeout(function() {

addMessage("<b>Vera Valda AI:</b><br>" + answer, "ai");

}, 300);

}


function getAnswer(originalQuestion) {

const question = originalQuestion.toLowerCase().trim();


/* =========================
SALUTURI
========================= */

if (
question === "bună" ||
question === "buna" ||
question === "salut" ||
question === "hello" ||
question === "hei"
) {

return "Bună! Ce faci? 😊";

}


if (
question.includes("ce faci") ||
question.includes("cum ești") ||
question.includes("cum esti")
) {

return "Sunt bine! Sunt Vera Valda AI și sunt gata să te ajut. 🤖";

}


/* =========================
GLUMA CU „DE CE”
========================= */

if (
question.startsWith("de ce") ||
question.startsWith("de ce ")
) {

return "De aia, aia blondă cu părul negru. 😂";

}


/* =========================
MULȚUMIRI
========================= */

if (
question.includes("mulțumesc") ||
question.includes("multumesc") ||
question.includes("mersi")
) {

return "Cu plăcere! 😊";

}


/* =========================
MATEMATICĂ
========================= */

const mathAnswer = solveMath(question);

if (mathAnswer !== null) {

return "Răspunsul este: <b>" + mathAnswer + "</b> 🧮";

}


/* =========================
ÎNTREBĂRI GENERALE
========================= */

if (question.includes("ce este matematica")) {

return "Matematica este domeniul care studiază numerele, cantitățile, formele, relațiile și tiparele.";

}


if (question.includes("ce este un triunghi")) {

return "Un triunghi este o figură geometrică formată din trei laturi și trei unghiuri.";

}


if (question.includes("ce este apa")) {

return "Apa este un compus chimic format din hidrogen și oxigen: H₂O.";

}


if (question.includes("capitala româniei") ||
question.includes("capitala romaniei")) {

return "Capitala României este București.";

}


if (question.includes("ce este soarele")) {

return "Soarele este steaua aflată în centrul Sistemului Solar.";

}


if (question.includes("cine ești") ||
question.includes("cine esti")) {

return "Sunt Vera Valda AI, un mic asistent creat în HTML, CSS și JavaScript.";

}


/* =========================
RĂSPUNS IMPLICIT
========================= */

return "Nu știu încă răspunsul la această întrebare. Poți încerca o întrebare de matematică sau una dintre întrebările mele cunoscute. 🤖";

}


/* =====================================================
SISTEM DE MATEMATICĂ
===================================================== */

function solveMath(question) {

let expression = question;

/* Transformăm simbolurile matematice */

expression = expression
.replace(/×/g, "*")
.replace(/÷/g, "/")
.replace(/−/g, "-")
.replace(/,/g, ".")
.replace(/\^/g, "**");


/* -------------------------
1 + 1
------------------------- */

if (
/^[0-9+\-*/().\s%^]+$/.test(expression)
) {

try {

const result = Function(
'"use strict"; return (' + expression + ')'
)();

if (
typeof result === "number" &&
Number.isFinite(result)
) {

return formatNumber(result);

}

} catch (error) {

return null;

}

}


/* -------------------------
sqrt(144)
------------------------- */

const sqrtMatch = expression.match(
/^sqrt\s*\(\s*(-?[0-9.]+)\s*\)$/
);

if (sqrtMatch) {

const number = Number(sqrtMatch[1]);

if (number >= 0) {

return formatNumber(Math.sqrt(number));

}

}


/* -------------------------
rădăcină pătrată
------------------------- */

const radicalMatch = question.match(
/radical(?:a)?\s+(?:din|de)\s+([0-9.]+)/
);

if (radicalMatch) {

const number = Number(radicalMatch[1]);

return formatNumber(Math.sqrt(number));

}


/* -------------------------
x + 5 = 12
------------------------- */

const equation = question.match(
/^x\s*([+\-])\s*(-?[0-9.]+)\s*=\s*(-?[0-9.]+)$/
);

if (equation) {

const operation = equation[1];

const number = Number(equation[2]);

const result = Number(equation[3]);

let x;

if (operation === "+") {

x = result - number;

} else {

x = result + number;

}

return "x = " + formatNumber(x);

}


/* -------------------------
2x + 5 = 15
------------------------- */

const linear = question.match(
/^(-?[0-9.]+)x\s*([+\-])\s*(-?[0-9.]+)\s*=\s*(-?[0-9.]+)$/
);

if (linear) {

const a = Number(linear[1]);

const operation = linear[2];

const b = Number(linear[3]);

const c = Number(linear[4]);

let x;

if (operation === "+") {

x = (c - b) / a;

} else {

x = (c + b) / a;

}

return "x = " + formatNumber(x);

}


/* -------------------------
procente
------------------------- */

const percent = question.match(
/^([0-9.]+)\s*%\s*(?:din|din)\s*([0-9.]+)$/
);

if (percent) {

const p = Number(percent[1]);

const total = Number(percent[2]);

const result = p / 100 * total;

return formatNumber(result);

}


return null;

}


/* =====================================================
FORMAT NUMERE
===================================================== */

function formatNumber(number) {

if (Number.isInteger(number)) {

return number.toString();

}

return Number(number.toFixed(10)).toString();

}


/* =====================================================
PROTECȚIE PENTRU TEXT AFIȘAT
===================================================== */

function escapeHTML(text) {

return text
.replace(/&/g, "&amp;")
.replace(/</g, "&lt;")
.replace(/>/g, "&gt;")
.replace(/"/g, "&quot;")
.replace(/'/g, "&#039;");

}

</script>

</body>
</html>
