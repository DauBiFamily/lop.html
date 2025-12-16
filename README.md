<!DOCTYPE html>
<html lang="vi">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Ôn Tập Tiểu Học</title>
<script src="https://cdn.tailwindcss.com"></script>
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<style>
    @import url('https://fonts.googleapis.com/css2?family=Nunito:wght@400;600;700&display=swap');
    body { font-family: 'Nunito', sans-serif; background-color:#FDFBF7; color:#4A4A4A; }
    .tab-active { background-color:#ED8936; color:white; font-weight:700; }
    .tab-inactive { background-color:white; color:#718096; font-weight:600; }
    .tab-inactive:hover { background-color:#FEF3C7; color:#D97706; }
    .card-hover:hover { transform: translateY(-2px); box-shadow:0 4px 6px rgba(0,0,0,0.1); }
    .correct-answer { background-color:#C6F6D5; border:2px solid #48BB78; }
    .incorrect-answer { background-color:#FED7D7; border:2px solid #F56565; }
</style>
</head>
<body class="min-h-screen flex flex-col items-center py-6 px-4">

<div id="app" class="w-full max-w-5xl bg-white shadow-xl rounded-2xl overflow-hidden flex flex-col min-h-[900px]">

<!-- Header -->
<header class="bg-[#FFEDD5] p-6 text-center border-b border-orange-100">
  <h1 class="text-3xl md:text-4xl font-bold text-orange-800 mb-2">🎒 Ôn Tập Tiểu Học</h1>
  <p class="text-orange-700 font-medium">Chọn lớp để bắt đầu học</p>
  <div class="flex justify-center gap-4 mt-4">
    <button onclick="switchGrade('grade1')" id="btn-grade1" class="px-6 py-2 rounded-lg tab-active">Lớp 1</button>
    <button onclick="switchGrade('grade2')" id="btn-grade2" class="px-6 py-2 rounded-lg tab-inactive">Lớp 2</button>
  </div>
</header>

<!-- Navigation Tabs -->
<nav class="flex flex-wrap justify-center bg-orange-50 border-b border-orange-100 p-2 gap-2 mt-2" id="nav-subjects">
  <!-- Buttons sẽ được render bằng JS -->
</nav>

<!-- Content Area -->
<main id="content-area" class="flex-grow p-6 overflow-y-auto bg-[#FDFBF7]">
  <!-- Nội dung render bằng JS -->
</main>

<!-- Footer -->
<footer class="bg-gray-50 p-4 text-center text-gray-500 text-sm border-t">
  <p>Chúc các bạn học tốt! 🌟</p>
</footer>

</div>

<script>
// ======== DỮ LIỆU MẪU ========
const grade1Data = {
  name:"Lớp 1",
  subjects:{
    math:{
      summary: { strengths:["Đếm số chính xác"], weaknesses:["Cộng trừ có nhớ"], intro:"Lớp 1 Toán: ôn tập phép cộng, trừ."},
      concepts:[
        {label:"Cộng 1 chữ số", wrong:"2+3=4", right:"2+3=5", explanation:"2+3=5"},
        {label:"Trừ 1 chữ số", wrong:"5-2=4", right:"5-2=3", explanation:"5-2=3"}
      ],
      vocab:[
        {term:"Số", def:"Một chữ số hoặc số nhiều chữ số"},
        {term:"Tổng", def:"Kết quả của phép cộng"}
      ],
      quiz:[
        {q:"2 + 3 = ?", options:["4","5","6","3"], correct:1, explain:"2+3=5"}
      ]
    },
    viet:{
      summary:{ strengths:["Nhận diện chữ cái"], weaknesses:["Đọc từ đúng"], intro:"Lớp 1 Tiếng Việt: ôn luyện chữ cái và từ."},
      concepts:[
        {label:"Chữ cái đầu", right:"A,B,C", explanation:"Nhận diện chữ cái đầu"},
      ],
      vocab:[{term:"Chữ cái", def:"Một ký tự trong bảng chữ cái"}],
      quiz:[{q:"Chữ cái thứ nhất của từ 'Bàn' là?", options:["B","A","N","None"], correct:0, explain:"Chữ cái đầu là B"}]
    },
    english:{
      summary:{ strengths:["Nhận diện chữ"], weaknesses:["Đọc từ"], intro:"Lớp 1 Tiếng Anh: chữ cái và từ vựng."},
      concepts:[{label:"Letter recognition", right:"A,B,C", explanation:"Recognize letters"}],
      vocab:[{term:"Letter", def:"A symbol of alphabet"}],
      quiz:[{q:"First letter of 'Cat'?", options:["C","A","T","None"], correct:0, explain:"First letter is C"}]
    }
  }
};

const grade2Data = {
  name:"Lớp 2",
  subjects:{
    math:{
      summary: { strengths:["Số hạng, Tổng"], weaknesses:["Cộng trừ có nhớ"], intro:"Lớp 2 Toán: ôn tập phép cộng, trừ, hình học."},
      concepts:[
        {label:"Cộng có nhớ", wrong:"34+25=69", right:"34+25=59", explanation:"4+5=9,3+2=5 =>59"},
        {label:"Trừ có nhớ", wrong:"62-28=44", right:"62-28=34", explanation:"2 không trừ được 8, mượn 1"}
      ],
      vocab:[
        {term:"Số hạng", def:"Thành phần trong phép cộng"},
        {term:"Hiệu", def:"Kết quả phép trừ"}
      ],
      quiz:[
        {q:"47+35=?", options:["72","82","81","712"], correct:1, explain:"7+5=12, 4+3+1=8 => 82"},
        {q:"81-26=?", options:["55","54","65","64"], correct:0, explain:"81-26=55"}
      ]
    },
    viet:{
      summary:{ strengths:["Đọc hiểu từ"], weaknesses:["Viết đúng chính tả"], intro:"Lớp 2 Tiếng Việt: ôn tập đọc viết."},
      concepts:[{label:"Viết đúng từ", right:"Bạn", explanation:"Ví dụ viết đúng từ"}],
      vocab:[{term:"Từ", def:"Một nhóm chữ có nghĩa"}],
      quiz:[{q:"Viết đúng từ: 'Bạn'", options:["Ban","Bạn","Bán","Bàn"], correct:1, explain:"Từ đúng là 'Bạn'"}]
    },
    english:{
      summary:{ strengths:["Đọc hiểu từ"], weaknesses:["Viết đúng"], intro:"Lớp 2 Tiếng Anh: từ vựng cơ bản."},
      concepts:[{label:"Spell word", right:"Cat", explanation:"Viết đúng từ Cat"}],
      vocab:[{term:"Word", def:"Một từ"}],
      quiz:[{q:"Spell word 'Dog'", options:["Dog","Dag","Dug","Doog"], correct:0, explain:"Dog"}]
    }
  }
};

// ======== STATE ========
let currentGrade = grade1Data;
let currentSubject = "math";
const contentArea = document.getElementById('content-area');
const navSubjects = document.getElementById('nav-subjects');

// ======== FUNCTIONS ========
function switchGrade(grade){
  if(grade==="grade1"){currentGrade=grade1Data;}
  else if(grade==="grade2"){currentGrade=grade2Data;}
  // Update buttons
  document.getElementById('btn-grade1').classList.toggle('tab-active', grade==="grade1");
  document.getElementById('btn-grade2').classList.toggle('tab-active', grade==="grade2");
  document.getElementById('btn-grade1').classList.toggle('tab-inactive', grade!=="grade1");
  document.getElementById('btn-grade2').classList.toggle('tab-inactive', grade!=="grade2");
  renderSubjectTabs();
}

function renderSubjectTabs(){
  navSubjects.innerHTML="";
  for(let subj in currentGrade.subjects){
    const btn=document.createElement('button');
    btn.className='px-6 py-2 rounded-lg transition-all duration-300 tab-inactive';
    if(subj===currentSubject) btn.classList.replace('tab-inactive','tab-active');
    btn.innerText=subj==="math"?"Toán":subj==="viet"?"Tiếng Việt":"Tiếng Anh";
    btn.onclick=()=>{currentSubject=subj; renderSubject(currentGrade.subjects[subj]); renderSubjectTabs();}
    navSubjects.appendChild(btn);
  }
  renderSubject(currentGrade.subjects[currentSubject]);
}

function renderSubject(subject){
  contentArea.innerHTML="";
  // Summary
  const sumDiv=document.createElement('div');
  sumDiv.className="mb-6 bg-blue-50 p-6 rounded-xl border border-blue-100";
  sumDiv.innerHTML=`<h2 class="text-2xl font-bold text-blue-800 mb-2">👋 Tổng Quan</h2><p class="text-blue-700">${subject.summary.intro}</p>`;
  contentArea.appendChild(sumDiv);

  // Strengths & Weaknesses
  const grid=document.createElement('div');
  grid.className="grid grid-cols-1 md:grid-cols-2 gap-6 mb-6";

  const strengths=subject.summary.strengths.map(s=>`<li class="flex items-start"><span class="mr-2 text-green-500">✔</span>${s}</li>`).join('');
  const weaknesses=subject.summary.weaknesses.map(s=>`<li class="flex items-start"><span class="mr-2 text-red-500">⚠️</span>${s}</li>`).join('');

  const strDiv=document.createElement('div');
  strDiv.className="bg-green-50 p-5 rounded-xl border border-green-100";
  strDiv.innerHTML=`<h3 class="font-bold text-green-800 mb-3">🌟 Đã Làm Tốt</h3><ul class="space-y-2">${strengths}</ul>`;

  const weakDiv=document.createElement('div');
  weakDiv.className="bg-red-50 p-5 rounded-xl border border-red-100";
  weakDiv.innerHTML=`<h3 class="font-bold text-red-800 mb-3">🎯 Cần Cải Thiện</h3><ul class="space-y-2">${weaknesses}</ul>`;

  grid.appendChild(strDiv); grid.appendChild(weakDiv);
  contentArea.appendChild(grid);

  // Concepts
  const conceptDiv=document.createElement('div'); conceptDiv.className="mb-6";
  conceptDiv.innerHTML="<h2 class='text-2xl font-bold text-gray-800 mb-2'>📚 Bài Học & Concepts</h2>";
  subject.concepts.forEach(c=>{
    conceptDiv.innerHTML+=`<div class="bg-white p-4 rounded-xl border border-gray-200 mb-3"><h4 class="font-bold text-gray-700">${c.label}</h4>${c.wrong?`<p class='text-red-500'>Sai: ${c.wrong}</p>`:""}<p class="text-green-600">Đúng: ${c.right?c.right:""}</p><p class="text-gray-500 text-sm">💡 ${c.explanation}</p></div>`;
  });
  contentArea.appendChild(conceptDiv);

  // Vocabulary
  const vocabDiv=document.createElement('div'); vocabDiv.className="mb-6";
  vocabDiv.innerHTML="<h2 class='text-2xl font-bold text-gray-800 mb-2'>📖 Từ Vựng</h2>";
  subject.vocab.forEach(v=>{
    vocabDiv.innerHTML+=`<div class="bg-white p-3 rounded-xl border border-gray-200 mb-2"><strong>${v.term}</strong>: ${v.def}</div>`;
  });
  contentArea.appendChild(vocabDiv);

  // Practice
  const practiceDiv=document.createElement('div'); practiceDiv.className="mb-6";
  practiceDiv.innerHTML="<h2 class='text-2xl font-bold text-gray-800 mb-2'>✍️ Luyện Tập</h2>";
  subject.quiz.forEach((q,i)=>{
    let optionsHTML=`<div class='grid grid-cols-1 sm:grid-cols-2 gap-2 mt-2'>`;
    q.options.forEach((opt,j)=>{ optionsHTML+=`<button onclick="checkAnswer(${i},${j},${JSON.stringify(subject.quiz)})" class="w-full p-2 rounded-lg border border-gray-200 hover:bg-gray-50 text-left">${String.fromCharCode(65+j)}. ${opt}</button>`; });
    optionsHTML+="</div>";
    practiceDiv.innerHTML+=`<div class="bg-white p-4 rounded-xl border border-gray-200 mb-3"><h4 class="font-bold">${q.q}</h4>${optionsHTML}<div id="feedback-${i}" class="hidden mt-2 p-2 rounded bg-gray-50 text-sm"></div></div>`;
  });
  contentArea.appendChild(practiceDiv);
}

function checkAnswer(qIndex,optIndex,quizData){
  const q=quizData[qIndex]; const feedback=document.getElementById(`feedback-${qIndex}`); feedback.classList.remove('hidden');
  const buttons=feedback.parentElement.querySelectorAll('button'); buttons.forEach(b=>b.disabled=true);
  if(optIndex===q.correct){buttons[optIndex].classList.add('correct-answer'); feedback.innerHTML=`🎉 Chính xác! ${q.explain}`;}
  else{buttons[optIndex].classList.add('incorrect-answer'); buttons[q.correct].classList.add('correct-answer'); feedback.innerHTML=`😅 Chưa đúng. ${q.explain}`;}
}

// ======== INIT ========
renderSubjectTabs();
</script>

</body>
</html>
