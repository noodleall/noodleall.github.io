<!DOCTYPE html>
<html lang="ko">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>이미지 코드 공유</title>
<style>
body {
  font-family: Arial, sans-serif;
  background: #f0f4f8;
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 20px;
}
h1 { color: #333; }
.card {
  background: white;
  padding: 20px;
  border-radius: 16px;
  box-shadow: 0 10px 20px rgba(0,0,0,0.1);
  margin-bottom: 20px;
  width: 90%;
  max-width: 450px;
  text-align: center;
}
textarea { width: 100%; margin-top: 10px; }
button {
  margin-top: 10px;
  padding: 10px 20px;
  border: none;
  border-radius: 999px;
  background: #667eea;
  color: white;
  cursor: pointer;
}
button:hover { background: #5563d6; }
canvas { border:1px solid #333; margin-top:10px; }
</style>
</head>
<body>

<h1>이미지 코드 공유 사이트</h1>

<div class="card">
  <h2>1️⃣ 이미지 업로드 & 코드 생성</h2>
  <input type="file" id="fileInput"><br>
  <button onclick="generateCode()">코드 만들기</button>
  <textarea id="codeOutput" rows="4" readonly placeholder="여기에 코드가 생성됩니다."></textarea>
</div>

<div class="card">
  <h2>2️⃣ 코드 입력 → 이미지 보기</h2>
  <textarea id="codeInput" rows="4" placeholder="여기에 코드를 붙여넣기"></textarea>
  <button onclick="drawFromCode()">이미지 그리기</button>
  <canvas id="canvas" width="50" height="50"></canvas>
</div>

<script>
function generateCode() {
  const file = document.getElementById("fileInput").files[0];
  if(!file) return alert("이미지를 선택하세요!");

  const reader = new FileReader();
  reader.onload = function(e) {
    const img = new Image();
    img.src = e.target.result;
    img.onload = function() {
      const canvas = document.createElement("canvas");
      canvas.width = 50;
      canvas.height = 50;
      const ctx = canvas.getContext("2d");
      ctx.drawImage(img, 0, 0, 50, 50);
      const pixels = ctx.getImageData(0,0,50,50).data;
      let code = "";
      for(let i=0;i<pixels.length;i+=4){
        const avg = Math.floor((pixels[i]+pixels[i+1]+pixels[i+2])/3);
        code += avg.toString(36).padStart(2,'0');
      }
      document.getElementById("codeOutput").value = code;
      alert("코드 생성 완료!");
    }
  }
  reader.readAsDataURL(file);
}

function drawFromCode() {
  const code = document.getElementById("codeInput").value;
  if(!code) return alert("코드를 입력하세요!");

  const canvas = document.getElementById("canvas");
  const ctx = canvas.getContext("2d");
  const imgData = ctx.createImageData(50,50);

  for(let i=0,j=0;i<code.length;i+=2,j+=4){
    const val = parseInt(code.substr(i,2),36);
    imgData.data[j] = val;
    imgData.data[j+1] = val;
    imgData.data[j+2] = val;
    imgData.data[j+3] = 255;
  }
  ctx.putImageData(imgData,0,0);
}
</script>

</body>
</html>
<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>페이지 제목</title>
</head>
<body>

  <!-- 여기에 웹페이지 내용 넣기 -->
  <h1>안녕하세요!</h1>

</body>
</html>
