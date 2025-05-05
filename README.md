# skintouch
Soft care, natural glow
# ניצור גרסה משודרגת של SkinTouch לנערות עם תזכורת יומית ולוח טיפוח יומי
import os
import zipfile

# יצירת תיקייה
project_name = "skintouch_plus"
base_path = f"/mnt/data/{project_name}"
os.makedirs(base_path, exist_ok=True)

# תוכן HTML כולל לוח יומי + תזכורת יומית פשוטה (שיחה מדומה)
html_content = """
<!DOCTYPE html>
<html lang="he" dir="rtl">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>SkinTouch ✨ לנערות</title>
  <style>
    body { font-family: "Varela Round", sans-serif; background: #fff0f5; max-width: 600px; margin: auto; padding: 1rem; direction: rtl; color: #333; }
    h1 { color: #da70d6; text-align: center; }
    .card { background: #ffffffcc; border-radius: 15px; padding: 1rem; margin: 1rem 0; box-shadow: 0 4px 6px #0001; }
    .buttons button { background: #da70d6; color: white; border: none; padding: 0.6rem 1.2rem; margin: 0.5rem; border-radius: 8px; cursor: pointer; font-size: 1rem; }
    .buttons button:hover { background: #c060c0; }
    .checklist input { margin-left: 0.5rem; }
    .reminder { background: #ffe4f1; padding: 1rem; border-radius: 10px; margin-top: 1rem; }
  </style>
</head>
<body>

<h1>ברוכה הבאה ל־SkinTouch 💖</h1>

<div class="card" id="question-box"></div>

<div class="card" id="checklist" style="display:none;">
  <h2>✨ לוח טיפוח יומי</h2>
  <div class="checklist">
    <label><input type="checkbox"> ניקוי פנים</label><br/>
    <label><input type="checkbox"> סרום לחות</label><br/>
    <label><input type="checkbox"> קרם פנים</label><br/>
    <label><input type="checkbox"> קרם הגנה (אם זה בוקר)</label><br/>
  </div>
</div>

<div class="card reminder" id="reminder" style="display:none;">
  📅 אל תשכחי את שגרת הטיפוח שלך היום! קבעת תזכורת לשעה: <span id="reminder-time">19:00</span>
</div>

<script>
const questions = [
  { id: "shine", text: "האם הפנים שלך מבריקות במהלך היום?" },
  { id: "dryness", text: "האם את מרגישה יובש בעור לעיתים קרובות?" },
  { id: "acne", text: "האם את נוטה לפצעונים?" },
  { id: "combo", text: "האם יש לך אזורים שומניים ואזורים יבשים?" }
];

const routine = {
  "עור יבש": ["ניקוי עדין בבוקר ובלילה", "סרום לחות (כמו חומצה היאלורונית)", "קרם לחות עשיר", "קרם הגנה בבוקר"],
  "עור שמן": ["ניקוי פנים פעמיים ביום", "סרום מאזֵן", "קרם לחות קליל ללא שומן", "קרם הגנה לא קומדוגני"],
  "עור רגיל": ["ניקוי עדין", "קרם לחות בסיסי", "קרם הגנה בבוקר"],
  "עור מעורב": ["ניקוי בוקר וערב", "טיפול שונה לפי אזור", "קרם הגנה לכל הפנים"]
};

let step = 0;
let answers = {};

function renderQuestion() {
  const q = questions[step];
  document.getElementById('question-box').innerHTML = `
    <h2>${q.text}</h2>
    <div class="buttons">
      <button onclick="answer('כן')">כן</button>
      <button onclick="answer('לא')">לא</button>
    </div>
  `;
}

function answer(value) {
  answers[questions[step].id] = value;
  step++;
  if (step < questions.length) {
    renderQuestion();
  } else {
    showResult();
  }
}

function determineSkinType(a) {
  if (a.combo === "כן") return "עור מעורב";
  else if (a.dryness === "כן") return "עור יבש";
  else if (a.shine === "כן") return "עור שמן";
  else return "עור רגיל";
}

function showResult() {
  const type = determineSkinType(answers);
  const items = routine[type].map(step => `<li>${step}</li>`).join("");
  document.getElementById('question-box').innerHTML = `
    <h2>סוג העור שלך: ${type}</h2>
    <h3>המלצות לשגרה:</h3>
    <ul>${items}</ul>
    <p>🎯 עכשיו אפשר לסמן כל שלב בלוח הטיפוח היומי למטה!</p>
  `;
  document.getElementById('checklist').style.display = "block";
  document.getElementById('reminder').style.display = "block";
}

renderQuestion();
</script>

</body>
</html>
"""

# כתיבת הקובץ
html_path = os.path.join(base_path, "index.html")
with open(html_path, "w", encoding="utf-8") as f:
    f.write(html_content)

# יצירת ZIP
zip_path = f"/mnt/data/{project_name}.zip"
with zipfile.ZipFile(zip_path, "w") as zipf:
    zipf.write(html_path, arcname="index.html")

zip_path
