# Grupo10criptografia-clasica
Grupo 10 - Criptografía Clásica Página web.
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <title>Criptografía Clásica</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>

  <h1>Herramientas de Cifrado Clásico</h1>

  <label>Seleccione algoritmo:</label>
  <select id="algorithm">
    <option value="caesar">Cifrado César</option>
    <option value="vigenere">Cifrado Vigenère</option>
    <option value="transposition">Transposición Columnar</option>
    <option value="atbash">Atbash</option>
  </select>

  <label>Texto:</label>
  <textarea id="text" placeholder="Ingrese el texto aquí"></textarea>

  <label>Clave / Parámetro:</label>
  <input type="text" id="key" placeholder="Ej: 3 o CLAVE">

  <div class="buttons">
    <button onclick="encrypt()">Cifrar</button>
    <button onclick="decrypt()">Descifrar</button>
  </div>

  <label>Resultado:</label>
  <textarea id="result" readonly></textarea>

  <script src="script.js"></script>
</body>
</html>


body {
  font-family: Arial, sans-serif;
  background: linear-gradient(to right, #1E90FF, #87CEFA); /* degradado azul */
  max-width: 700px;
  margin: auto;
  padding: 20px;
}

h1 {
  text-align: center;
}

label {
  font-weight: bold;
  margin-top: 10px;
  display: block;
}

textarea, input, select {
  width: 100%;
  padding: 10px;
  margin-top: 5px;
}

.buttons {
  margin: 15px 0;
  display: flex;
  justify-content: space-between;
}

button {
  width: 48%;
  padding: 10px;
  font-weight: bold;
  cursor: pointer;
}



function encrypt() {
  const alg = algorithm.value;
  const text = document.getElementById("text").value;
  const key = document.getElementById("key").value;

  if (alg === "caesar") result.value = caesarEncrypt(text, parseInt(key));
  if (alg === "vigenere") result.value = vigenereEncrypt(text, key);
  if (alg === "transposition") result.value = transpositionEncrypt(text, key);
  if (alg === "atbash") result.value = atbash(text);
}

function decrypt() {
  const alg = algorithm.value;
  const text = document.getElementById("text").value;
  const key = document.getElementById("key").value;

  if (alg === "caesar") result.value = caesarDecrypt(text, parseInt(key));
  if (alg === "vigenere") result.value = vigenereDecrypt(text, key);
  if (alg === "transposition") result.value = transpositionDecrypt(text, key);
  if (alg === "atbash") result.value = atbash(text);
}

/* ===== CÉSAR ===== */
function caesarEncrypt(text, shift) {
  return text.replace(/[a-z]/gi, c => {
    let base = c <= 'Z' ? 65 : 97;
    return String.fromCharCode((c.charCodeAt(0) - base + shift) % 26 + base);
  });
}
function caesarDecrypt(text, shift) {
  return caesarEncrypt(text, 26 - shift);
}

/* ===== VIGENÈRE ===== */
function vigenereEncrypt(text, key) {
  let res = "", j = 0;
  key = key.toLowerCase();
  for (let i = 0; i < text.length; i++) {
    let c = text[i];
    if (/[a-z]/i.test(c)) {
      let shift = key[j++ % key.length].charCodeAt(0) - 97;
      let base = c <= 'Z' ? 65 : 97;
      res += String.fromCharCode((c.charCodeAt(0) - base + shift) % 26 + base);
    } else res += c;
  }
  return res;
}
function vigenereDecrypt(text, key) {
  let res = "", j = 0;
  key = key.toLowerCase();
  for (let i = 0; i < text.length; i++) {
    let c = text[i];
    if (/[a-z]/i.test(c)) {
      let shift = key[j++ % key.length].charCodeAt(0) - 97;
      let base = c <= 'Z' ? 65 : 97;
      res += String.fromCharCode((c.charCodeAt(0) - base - shift + 26) % 26 + base);
    } else res += c;
  }
  return res;
}

/* ===== TRANSPOSICIÓN ===== */
function transpositionEncrypt(text, key) {
  let cols = key.length;
  let rows = Math.ceil(text.length / cols);
  let grid = Array.from({ length: rows }, () => Array(cols).fill(" "));
  let k = 0;

  for (let r = 0; r < rows; r++)
    for (let c = 0; c < cols; c++)
      if (k < text.length) grid[r][c] = text[k++];

  return key.split("")
    .map((_, i) => grid.map(r => r[i]).join(""))
    .join("");
}

function transpositionDecrypt(text, key) {
  let cols = key.length;
  let rows = Math.ceil(text.length / cols);
  let grid = Array.from({ length: rows }, () => Array(cols).fill(" "));
  let k = 0;

  for (let c = 0; c < cols; c++)
    for (let r = 0; r < rows; r++)
      if (k < text.length) grid[r][c] = text[k++];

  return grid.flat().join("").trim();
}

/* ===== ATBASH ===== */
function atbash(text) {
  return text.replace(/[a-z]/gi, c => {
    let base = c <= 'Z' ? 65 : 97;
    return String.fromCharCode(25 - (c.charCodeAt(0) - base) + base);
  });
}
