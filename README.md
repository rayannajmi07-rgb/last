<!DOCTYPE html>
<html lang="ar">
<head>
<meta charset="UTF-8">
<title>المستشار الجامعي الذكي</title>
<link href="https://fonts.googleapis.com/css2?family=Cairo&display=swap" rel="stylesheet">
<style>
body {
  font-family: 'Cairo', sans-serif;
  background: linear-gradient(135deg, #74ebd5, #acb6e5);
  margin: 0;
  padding: 0;
  direction: rtl;
}
.container {
  background: #fff;
  max-width: 800px;
  margin: 40px auto;
  padding: 30px;
  border-radius: 15px;
  box-shadow: 0 10px 25px rgba(0,0,0,0.2);
}
h1 {
  text-align: center;
  color: #2c3e50;
  margin-bottom: 30px;
}
label {
  font-weight: bold;
  margin-top: 15px;
  display: block;
}
input, select, button {
  width: 100%;
  padding: 12px;
  margin: 8px 0;
  font-size: 16px;
  border-radius: 8px;
  border: 1px solid #ccc;
  box-sizing: border-box;
}
input:focus, select:focus {
  outline: none;
  border-color: #3498db;
  box-shadow: 0 0 5px rgba(52,152,219,0.5);
}
button {
  background: #3498db;
  color: #fff;
  border: none;
  cursor: pointer;
  font-size: 18px;
  transition: 0.3s;
  margin-top: 15px;
}
button:hover {
  background: #2980b9;
}
.result {
  background: #ecf0f1;
  padding: 20px;
  border-radius: 10px;
  margin-top: 25px;
  font-size: 16px;
  line-height: 1.5;
  color: #2c3e50;
}
.card {
  background: #f7f9fc;
  padding: 15px;
  border-radius: 10px;
  margin-bottom: 20px;
  box-shadow: 0 4px 10px rgba(0,0,0,0.1);
}
</style>
</head>

<body>

<div class="container">
<h1>🎓 المستشار الجامعي الذكي</h1>

<div class="card">
<label for="university">🏛️ اختر الجامعة:</label>
<select id="university">
  <option value="kingabd">جامعة الملك عبدالعزيز</option>
  <option value="kingkhalid">جامعة الملك خالد</option>
  <option value="imamu">جامعة الإمام محمد بن سعود</option>
  <option value="taibah">جامعة طيبة</option>
  <option value="kfupm">جامعة الملك فهد للبترول والمعادن</option>
</select>

<label for="gpa">📚 معدل الثانوية:</label>
<input id="gpa" type="number" placeholder="مثال: 95" min="0" max="100">

<label for="qudrat">🧠 درجة القدرات:</label>
<input id="qudrat" type="number" placeholder="مثال: 85" min="0" max="100">

<label for="tahsili">✏️ درجة التحصيلي:</label>
<input id="tahsili" type="number" placeholder="مثال: 90" min="0" max="100">

<label for="step">📑 درجة ستيب (STEP):</label>
<input id="step" type="number" placeholder="مثال: 10" min="0" max="100">

<button id="calculateBtn">🔍 تحليل فرص القبول</button>
</div>

<div class="result" id="result"></div>
</div>

<script>
document.getElementById("calculateBtn").addEventListener("click", function(){
  let gpa = Number(document.getElementById("gpa").value);
  let q = Number(document.getElementById("qudrat").value);
  let t = Number(document.getElementById("tahsili").value);
  let step = Number(document.getElementById("step").value) || 0;
  let uni = document.getElementById("university").value;

  if(isNaN(gpa) || isNaN(q) || isNaN(t)){
    alert("الرجاء إدخال جميع الدرجات بشكل صحيح!");
    return;
  }

  // تعيين الأوزان حسب الجامعة
  let weightGPA, weightQ, weightT, weightStep;
  switch(uni){
    case "kingabd":
      weightGPA = 0.3; weightQ = 0.3; weightT = 0.3; weightStep = 0.1; break;
    case "kingkhalid":
      weightGPA = 0.3; weightQ = 0.3; weightT = 0.35; weightStep = 0.05; break;
    case "imamu":
      weightGPA = 0.5; weightQ = 0.4; weightT = 0.1; weightStep = 0; break;
    case "taibah":
      weightGPA = 0.4; weightQ = 0.3; weightT = 0.3; weightStep = 0; break;
    case "kfupm":
      weightGPA = 0.1; weightQ = 0.5; weightT = 0.4; weightStep = 0; break;
    default:
      weightGPA = 0.3; weightQ = 0.3; weightT = 0.4; weightStep = 0;
  }

  // حساب النسبة الموزونة
  let weighted = (gpa*weightGPA) + (q*weightQ) + (t*weightT) + (step*weightStep);

  // اقتراح التخصصات
  let suggestions = [];
  if(weighted >= 95){ suggestions.push("الطب"); }
  if(weighted >= 92){ suggestions.push("الصيدلة"); }
  if(weighted >= 90){ suggestions.push("الهندسة"); }
  if(weighted >= 88){ suggestions.push("علوم الحاسوب"); }
  if(weighted >= 85){ suggestions.push("إدارة أعمال"); }
  if(weighted >= 80){ suggestions.push("علوم اجتماعية"); }
  if(weighted >= 70 && weighted < 80){ suggestions.push("تربية، فنون، حاسوب مبتدئ"); }
  if(weighted >= 60 && weighted < 70){ suggestions.push("أدبي عام، معهد مهني"); }

  let uniName = document.getElementById("university").selectedOptions[0].text;

  // إخراج النتيجة
  document.getElementById("result").innerHTML = `
  🏛️ <b>الجامعة:</b> ${uniName}<br>
  📊 <b>النسبة الموزونة التقريبية:</b> ${weighted.toFixed(2)}%<br><br>
  🤖 <b>تحليل الذكاء الاصطناعي:</b><br>
  - النسبة تعتبر <b>${weighted >= 85 ? "قوية للقبول" : "تحتاج تحسين"}</b><br>
  - ${weighted < 85 ? "ينصح برفع درجات القدرات والتحصيلي لزيادة فرصة القبول." : "يمكنك التفكير في التخصص المناسب."}<br><br>
  💡 <b>التخصصات المناسبة لك:</b> ${suggestions.length > 0 ? suggestions.join(", ") : "لا يوجد تخصص مطابق للنسبة الحالية"}<br><br>
  ⚠️ ملاحظة: النسب تقريبية وقد تختلف حسب الجامعة والتخصص والسنة.
  `;
});
</script>

</body>
</html>
