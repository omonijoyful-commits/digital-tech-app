<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Digital Technologies - Teaching App</title>
<style>
  *{box-sizing:border-box; font-family: 'Segoe UI', Arial, sans-serif;}
  body{margin:0; background:#EEF2FF; color:#111;}
  header{background:linear-gradient(135deg,#1E3A8A,#3B82F6); color:white; padding:20px 16px; text-align:center; position:sticky; top:0; z-index:10;}
  header h1{margin:0; font-size:23px;}
  header p{margin:5px 0 0; font-size:13px; opacity:.9;}
  .tabs{display:flex; gap:8px; padding:12px; overflow-x:auto; background:white; position:sticky; top:88px; z-index:9;}
  .tabs button{white-space:nowrap; border:1px solid #ddd; background:#F8FAFC; padding:8px 14px; border-radius:20px; cursor:pointer; font-size:13px;}
  .tabs button.active{background:#1E3A8A; color:white; border-color:#1E3A8A;}
  .search-box{padding:12px;}
  input{width:100%; padding:13px; border:2px solid #ddd; border-radius:12px; font-size:15px;}
  .topics{display:grid; grid-template-columns:1fr 1fr; gap:12px; padding:0 12px 12px;}
  .card{background:white; border-radius:16px; overflow:hidden; box-shadow:0 2px 10px rgba(0,0,0,.08); cursor:pointer;}
  .card img{width:100%; height:125px; object-fit:cover;}
  .card-body{padding:10px;}
  .card-body h3{margin:0; font-size:14px; color:#1E3A8A; line-height:1.3;}
  .card-body p{margin:6px 0 0; font-size:12px; color:#666; line-height:1.4;}
  .badge{display:inline-block; background:#DBEAFE; color:#1E40AF; font-size:11px; padding:3px 8px; border-radius:20px; margin-top:6px; font-weight:bold;}
  .badge.jss{background:#FEF3C7; color:#92400E;}
  .badge.sss{background:#DCFCE7; color:#166534;}
  /* Lesson View */
  #lessonView{display:none; padding-bottom:90px;}
  .lesson-hero{position:relative;}
  .lesson-hero img{width:100%; height:250px; object-fit:cover;}
  .lesson-hero span{position:absolute; bottom:12px; left:12px; background:rgba(0,0,0,.7); color:white; padding:6px 12px; border-radius:20px; font-size:12px;}
  .lesson-content{padding:16px; background:white; margin:12px; border-radius:16px; line-height:1.7; font-size:15px;}
  .lesson-content h2{color:#1E3A8A; margin-top:0; font-size:20px;}
  .lesson-content h3{color:#EA580C; margin-top:22px; border-bottom:2px solid #FFF7ED; padding-bottom:4px;}
  .verse{background:#EFF6FF; border-left:5px solid #2563EB; padding:12px; border-radius:8px; margin:12px 0;}
  .point{background:#FFFBEB; padding:12px; border-radius:10px; margin:10px 0; border:1px solid #FDE68A;}
  .point b{color:#92400E;}
  .example{background:#F0FDF4; padding:12px; border-radius:10px; margin:10px 0; border:1px solid #BBF7D0;}
  .img-row{display:grid; grid-template-columns:1fr 1fr; gap:8px; margin:12px 0;}
  .img-row img{width:100%; height:110px; object-fit:cover; border-radius:10px;}
  .back-btn{background:#1E3A8A; color:white; border:none; padding:12px 20px; border-radius:30px; font-size:15px; margin:12px; cursor:pointer;}
  .action-row{display:flex; gap:10px; padding:0 12px;}
  .action-row button{flex:1; padding:13px; border:none; border-radius:12px; font-size:14px; cursor:pointer; font-weight:bold;}
  .btn-quiz{background:#F97316; color:white;}
  .btn-share{background:#16A34A; color:white;}
  .btn-print{background:#111827; color:white; margin:12px; width:calc(100% - 24px); padding:13px; border:none; border-radius:12px;}
  footer{text-align:center; padding:15px; font-size:12px; color:#888;}
  @media(min-width:700px){ .topics{grid-template-columns:1fr 1fr 1fr 1fr;} .lesson-content{max-width:700px; margin:12px auto;} }
</style>
</head>
<body>

<header>
  <h1>💻 Digital Technologies</h1>
  <p>Teaching App for JSS & SSS Students • Notes + Pictures + Quiz</p>
</header>

<!-- HOME -->
<div id="homeView">
  <div class="tabs" id="classTabs">
    <button class="active" onclick="filterClass('All',this)">All Classes</button>
    <button onclick="filterClass('JSS',this)">JSS 1 - 3</button>
    <button onclick="filterClass('SSS',this)">SSS 1 - 3</button>
    <button onclick="filterClass('General',this)">General / Practical</button>
  </div>
  <div class="search-box">
    <input type="text" id="search" placeholder="🔍 Search e.g. Computer, Internet, Excel, Coding, AI..." onkeyup="filterTopics()">
  </div>
  <div class="topics" id="topicsGrid"></div>
  <footer>Digital Technologies Teaching App • Tap a topic to teach</footer>
</div>

<!-- LESSON -->
<div id="lessonView">
  <button class="back-btn" onclick="goHome()">← Back to All Topics</button>
  <div class="lesson-hero"><img id="lessonImg" src=""><span id="lessonTag"></span></div>
  <div class="lesson-content" id="lessonContent"></div>
  <div class="action-row">
    <button class="btn-quiz" onclick="showQuiz()">📝 Take Quiz</button>
    <button class="btn-share" onclick="shareLesson()">📤 Share on WhatsApp</button>
  </div>
  <div class="lesson-content" id="quizBox" style="display:none;"></div>
  <button class="btn-print" onclick="window.print()">🖨️ Print / Save as PDF Lesson Note</button>
</div>

<script>
// =========================================================
// 👇 LESSONS DATABASE - ADD YOUR OWN TOPICS HERE
// To change picture: replace the image link with your own picture link
// =========================================================
const LESSONS = [
  {
    id: 1,
    title: "Introduction to Computer",
    level: "JSS",
    classLabel: "JSS 1 • Term 1",
    image: "https://images.unsplash.com/photo-1517336714731-489689fd1ca8?w=800",
    img2: "https://images.unsplash.com/photo-1587831990711-23ca6441447b?w=800",
    img3: "https://images.unsplash.com/photo-1593640408182-31c70c8268f5?w=800",
    summary: "What is a computer? Parts, types and uses.",
    objective: "By the end of lesson, students should define computer, identify parts, and state uses.",
    content: `
      <h2>🖥️ Introduction to Computer</h2>
      <div class="verse"><b>Definition:</b> A computer is an electronic device that accepts data, processes it, and gives out information as output.</div>
      <h3>1. Parts of a Computer</h3>
      <div class="point"><b>Input Devices:</b> Keyboard, Mouse, Scanner, Microphone - they send data IN.<br><b>Output Devices:</b> Monitor, Printer, Speaker - they bring result OUT.<br><b>System Unit:</b> CPU - the brain. Memory, Motherboard inside.</div>
      <h3>2. Types of Computer</h3>
      <div class="point">Super Computer • Mainframe • Mini Computer • Micro Computer (Desktop, Laptop, Tablet, Phone)</div>
      <h3>3. Uses of Computer</h3>
      <div class="example">🏫 School: Typing, Research, CBT Exams<br>🏦 Bank: ATM, Transfer<br>🏥 Hospital: Patient records<br>🏠 Home: Movies, Games, Learning</div>
      <h3>Class Activity</h3><p>Ask students to point to the Monitor, Keyboard, Mouse, System Unit in the picture above. Let them name 3 things they use computer for at home.</p>
    `,
    quiz: "Which device is the brain of the computer? A) Monitor B) CPU C) Keyboard",
    answer: "B) CPU"
  },
  {
    id: 2,
    title: "Computer Hardware & Software",
    level: "JSS",
    classLabel: "JSS 2 • Term 1",
    image: "https://images.unsplash.com/photo-1558494949-ef010cbdcc31?w=800",
    img2: "https://images.unsplash.com/photo-1517694712202-14dd9538aa97?w=800",
    img3: "https://images.unsplash.com/photo-1555066931-4365d14bab8c?w=800",
    summary: "Difference between what you can touch and what you cannot touch.",
    objective: "Differentiate hardware and software with examples.",
    content: `
      <h2>🔧 Hardware vs Software</h2>
      <div class="verse"><b>Hardware</b> = Parts you can SEE and TOUCH. <br><b>Software</b> = Programs you CANNOT touch, but you can SEE them work.</div>
      <h3>Examples of Hardware</h3><div class="point">Keyboard, Mouse, Monitor, Printer, CPU, Hard Disk, RAM, Speaker, USB</div>
      <h3>Examples of Software</h3><div class="point"><b>System Software:</b> Windows 11, Android, iOS - controls the computer<br><b>Application Software:</b> MS Word, Excel, WhatsApp, Chrome, Games - does specific work</div>
      <h3>Simple Analogy for Students</h3><div class="example">Hardware = Your Body (hand, leg, head)<br>Software = Your Mind / Instructions that tell body what to do. Without mind, body cannot work!</div>
      <h3>Class Activity</h3><p>Show phone: Hardware is screen, battery. Software is WhatsApp, Camera app. Let students list 5 hardware and 5 software they know.</p>
    `,
    quiz: "Which is a software? A) Keyboard B) MS Word C) Printer",
    answer: "B) MS Word"
  },
  {
    id: 3,
    title: "The Internet & World Wide Web",
    level: "JSS",
    classLabel: "JSS 3 • Term 2",
    image: "https://images.unsplash.com/photo-1451187580459-43490279c0fa?w=800",
    img2: "https://images.unsplash.com/photo-1519389950473-47ba0277781c?w=800",
    img3: "https://images.unsplash.com/photo-1526374965328-7f61d4dc18c5?w=800",
    summary: "What is internet, browser, website, and how to stay safe online.",
    objective: "Explain internet, identify browsers, and state 5 safety rules.",
    content: `
      <h2>🌐 The Internet & WWW</h2>
      <div class="verse"><b>Internet</b> = Global network connecting millions of computers together.<br><b>WWW</b> = Collection of websites you access through the internet.</div>
      <h3>What You Need To Go Online</h3><div class="point">1. Device (Phone/Laptop) <br>2. Data / WiFi <br>3. Browser - Chrome, Edge, Firefox, Safari <br>4. Website Address e.g. www.google.com</div>
      <h3>Uses of Internet</h3><div class="example">Search for assignments • Watch educational videos • Send email • Online classes • Banking • Communication</div>
      <h3>⚠️ Internet Safety - VERY IMPORTANT</h3><div class="point"><b>1.</b> Don't share your password, address, school.<br><b>2.</b> Don't talk to strangers online.<br><b>3.</b> Don't click strange links.<br><b>4.</b> Tell teacher/parent if someone bullies you online.<br><b>5.</b> Don't believe everything you see online.</div>
    `,
    quiz: "Which is a Browser? A) Google Chrome B) Facebook C) WhatsApp",
    answer: "A) Google Chrome"
  },
  {
    id: 4,
    title: "Microsoft Word - Word Processing",
    level: "SSS",
    classLabel: "SSS 1 • Practical",
    image: "https://images.unsplash.com/photo-1586281380349-632531db7ed4?w=800",
    img2: "https://images.unsplash.com/photo-1544716278-ca5e3f4abd8c?w=800",
    img3: "https://images.unsplash.com/photo-1455390582262-044cdead277a?w=800",
    summary: "How to type, format, save and print documents.",
    objective: "Create a document, format text, save and print.",
    content: `
      <h2>📝 Microsoft Word</h2>
      <div class="verse"><b>Word Processing</b> = Using computer to type, edit, format and print documents like letters, assignments, CVs.</div>
      <h3>Steps To Create Document</h3><div class="point">1. Open MS Word → Click Blank Document<br>2. Type your work<br>3. Select text to make it <b>Bold (Ctrl+B)</b>, <i>Italic (Ctrl+I)</i>, <u>Underline (Ctrl+U)</u><br>4. Change Font Size, Colour, Alignment<br>5. Save: Ctrl+S → Name file → Save<br>6. Print: Ctrl+P</div>
      <h3>Keyboard Shortcuts Students Must Know</h3><div class="example">Ctrl + C = Copy &nbsp; Ctrl + V = Paste &nbsp; Ctrl + X = Cut<br>Ctrl + Z = Undo &nbsp; Ctrl + S = Save &nbsp; Ctrl + P = Print<br>Ctrl + A = Select All &nbsp; Ctrl + B = Bold</div>
      <h3>Class Practical</h3><p>Let each student type their name, school, and 3 lines about themselves. Format title in Bold, 16pt, Center. Save as "My Name.docx"</p>
    `,
    quiz: "What is Ctrl + S for? A) Print B) Save C) Copy",
    answer: "B) Save"
  },
  {
    id: 5,
    title: "Microsoft Excel - Spreadsheet",
    level: "SSS",
    classLabel: "SSS 2 • Practical",
    image: "https://images.unsplash.com/photo-1460925895917-afdab827c52f?w=800",
    img2: "https://images.unsplash.com/photo-1554224155-6726b3ff858f?w=800",
    img3: "https://images.unsplash.com/photo-1551288049-bebda4e38f71?w=800",
    summary: "Rows, Columns, Cells and Formulas for calculations.",
    objective: "Identify cell, row, column and use SUM, AVERAGE formulas.",
    content: `
      <h2>📊 Microsoft Excel</h2>
      <div class="verse"><b>Spreadsheet</b> = Table for storing numbers and calculating automatically. Made of Rows, Columns, Cells.</div>
      <h3>Basic Terms</h3><div class="point"><b>Row:</b> Horizontal line - 1,2,3...<br><b>Column:</b> Vertical line - A,B,C...<br><b>Cell:</b> Where Row + Column meet e.g. B2, C5<br><b>Workbook:</b> Whole Excel file<br><b>Worksheet:</b> One page inside workbook</div>
      <h3>Important Formulas</h3><div class="example">=SUM(B2:B10) = Add all numbers from B2 to B10<br>=AVERAGE(B2:B10) = Find average<br>=MAX(B2:B10) = Biggest number<br>=MIN(B2:B10) = Smallest number<br>=B2*C2 = Multiply</div>
      <h3>Class Practical</h3><p>Create a table: Name | Maths | English | Total. Enter 5 students' scores. Use =SUM to find Total. Use =AVERAGE to find average. Draw a Bar Chart.</p>
    `,
    quiz: "What is the address where Row 5 meets Column C? A) 5C B) C5 C) C-5",
    answer: "B) C5"
  },
  {
    id: 6,
    title: "Introduction to Coding & Scratch",
    level: "General",
    classLabel: "JSS 2 - SSS 1 • Coding",
    image: "https://images.unsplash.com/photo-1461749280684-dccba630e2f6?w=800",
    img2: "https://images.unsplash.com/photo-1515879218367-8466d910aaa4?w=800",
    img3: "https://images.unsplash.com/photo-1555066931-4365d14bab8c?w=800",
    summary: "What is coding? Learn to think like a computer with Scratch.",
    objective: "Explain coding and create a simple Scratch animation.",
    content: `
      <h2>💡 Introduction to Coding</h2>
      <div class="verse"><b>Coding</b> = Giving instructions to a computer in a language it understands. Like giving a recipe to a cook.</div>
      <h3>Popular Coding Languages</h3><div class="point">Scratch (for beginners) • Python (easy) • HTML/CSS (for websites) • JavaScript • Java</div>
      <h3>What Is Scratch?</h3><div class="example">Scratch uses BLOCKS like Lego. You drag blocks: "When Green Flag Clicked" → "Move 10 steps" → "Say Hello!" - No need to type hard code.</div>
      <h3>First Program in Scratch</h3><div class="point">1. Go to scratch.mit.edu<br>2. Pick a Cat character (Sprite)<br>3. Drag: When Flag Clicked<br>4. Drag: Say "Hello, My Name Is..." for 2 seconds<br>5. Drag: Move 10 Steps, Turn, Play Sound</div>
      <h3>Why Learn Coding?</h3><p>To create games, apps, websites. To solve problems. Jobs of future need coding.</p>
    `,
    quiz: "Which language is best for beginners with blocks? A) Python B) Scratch C) Java",
    answer: "B) Scratch"
  },
  {
    id: 7,
    title: "Artificial Intelligence (AI)",
    level: "SSS",
    classLabel: "SSS 2 - 3 • Trending",
    image: "https://images.unsplash.com/photo-1677442136019-21780ecad995?w=800",
    img2: "https://images.unsplash.com/photo-1620712943542-3c4586c6e9b5?w=800",
    img3: "https://images.unsplash.com/photo-1485827404703-89b55fcc595e?w=800",
    summary: "What is AI? How ChatGPT, Siri, Robots work. Good and bad.",
    objective: "Explain AI, give examples, and state how to use AI responsibly.",
    content: `
      <h2>🤖 Artificial Intelligence (AI)</h2>
      <div class="verse"><b>AI</b> = Making computers think and learn like humans. Computer learns from data.</div>
      <h3>Examples Students Know</h3><div class="point">ChatGPT / Meta AI - Answers questions<br>Siri / Google Assistant - Voice assistant<br>Face ID - Unlocks phone with face<br>Google Maps - Shows best road<br>Netflix / TikTok - Recommends videos you like<br>Robots, Self-driving cars</div>
      <h3>How Does AI Learn?</h3><div class="example">Like a child: Show it 1000 pictures of cats → It learns "This is a cat". More data = Smarter AI. This is called Machine Learning.</div>
      <h3>Good & Bad of AI</h3><div class="point"><b>GOOD:</b> Helps with assignments, Helps doctors, Helps farmers, Translates languages.<br><b>BAD:</b> Can give wrong answer, Can be used to cheat, Can create fake pictures/videos (Deepfake), Can take some jobs.</div>
      <h3>Golden Rule</h3><p>Use AI as a HELPER, not as a CHEATER. Ask AI to explain, not to do your assignment for you. Always verify.</p>
    `,
    quiz: "What is Machine Learning? A) Machine sleeping B) Computer learning from data C) Machine breaking",
    answer: "B) Computer learning from data"
  },
  {
    id: 8,
    title: "Cyber Security & Online Safety",
    level: "General",
    classLabel: "All Classes • Must Know",
    image: "https://images.unsplash.com/photo-1563013544-824ae1b704d3?w=800",
    img2: "https://images.unsplash.com/photo-1510511459019-1c8b18d5e7f8?w=800",
    img3: "https://images.unsplash.com/photo-1614064641938-3bbee52942c7?w=800",
    summary: "Passwords, Scams, Hacking, and how to protect yourself.",
    objective: "Create strong password and identify scams.",
    content: `
      <h2>🔐 Cyber Security</h2>
      <div class="verse"><b>Cyber Security</b> = Protecting your phone, account and information from thieves (Hackers) online.</div>
      <h3>How To Create Strong Password</h3><div class="point">Use at least 8 characters. Mix: Capital + Small + Number + Symbol. Example: <b>Sch00l@2026!</b> <br>Don't use: Your name, birthday, 12345</div>
      <h3>Common Scams Students Must Know</h3><div class="example">1. "You have won 500k, click link" - FAKE!<br>2. "Send me recharge card, I am your uncle" - FAKE!<br>3. Fake JAMB / WAEC result upgrade - FAKE!<br>4. Stranger asking for your OTP - NEVER share OTP!</div>
      <h3>5 Safety Rules</h3><div class="point">1. Lock your phone with PIN / Fingerprint<br>2. Turn on 2-Step Verification<br>3. Log out from school computer after use<br>4. Don't use free WiFi for banking<br>5. Update your apps</div>
    `,
    quiz: "Should you share your OTP with anyone? A) Yes B) No, Never",
    answer: "B) No, Never"
  },
  {
    id: 9,
    title: "Data, Database & PowerPoint",
    level: "SSS",
    classLabel: "SSS 3 • Practical",
    image: "https://images.unsplash.com/photo-1557804506-669a67965ba0?w=800",
    img2: "https://images.unsplash.com/photo-1544531585-9847b68c8c86?w=800",
    img3: "https://images.unsplash.com/photo-1517245386807-bb43f82c33c4?w=800",
    summary: "What is data? How to present with PowerPoint like a pro.",
    objective: "Define data and create a 5-slide presentation.",
    content: `
      <h2>📽️ Data & PowerPoint Presentation</h2>
      <div class="verse"><b>Data</b> = Raw facts like Name, Age, Score. <br><b>Information</b> = Data that has been processed to make sense. Example: "Average score of class is 72%"</div>
      <h3>How To Make Good PowerPoint</h3><div class="point">Slide 1: Title - Topic, Your Name<br>Slide 2: Introduction<br>Slide 3: Main Points with Pictures<br>Slide 4: Examples / Chart<br>Slide 5: Conclusion + Thank You + Questions<br><b>Rules:</b> Don't put too many words. Use big font (28+). Use pictures. Use 1 idea per slide.</div>
      <h3>Class Practical</h3><div class="example">Topic: "My School" - Make 5 slides. Add pictures, add animation (Transition), add your voice. Present to class for 2 minutes.</div>
      <h3>Where Is Data Stored?</h3><p>Database: Like school register stored in computer. Examples: MS Access, MySQL. Banks, Hospitals, INEC all use databases.</p>
    `,
    quiz: "What is processed data called? A) Data B) Information C) Database",
    answer: "B) Information"
  }
];

let currentLesson = null;
let currentFilter = 'All';
let searchText = '';

function renderTopics(){
  const grid = document.getElementById('topicsGrid');
  let list = LESSONS.filter(l=>{
    let matchClass = currentFilter==='All' || l.level===currentFilter || (currentFilter==='General' && l.level==='General');
    // JSS filter includes JSS only, SSS includes SSS only
    if(currentFilter==='JSS') matchClass = l.level==='JSS';
    if(currentFilter==='SSS') matchClass = l.level==='SSS';
    if(currentFilter==='General') matchClass = l.level==='General';
    let matchSearch = (l.title+l.summary+l.classLabel).toLowerCase().includes(searchText.toLowerCase());
    return matchClass && matchSearch;
  });
  if(list.length===0){ grid.innerHTML = '<p style="grid-column:1/-1; text-align:center; padding:20px;">No topic found. Try another search.</p>'; return; }
  grid.innerHTML = '';
  list.forEach(l=>{
    grid.innerHTML += `
      <div class="card" onclick="openLesson(${l.id})">
        <img src="${l.image}">
        <div class="card-body">
          <h3>${l.title}</h3>
          <p>${l.summary}</p>
          <span class="badge ${l.level==='JSS'?'jss':l.level==='SSS'?'sss':''}">${l.classLabel}</span>
        </div>
      </div>`;
  });
}

function filterClass(type, btn){
  currentFilter = type;
  document.querySelectorAll('.tabs button').forEach(b=>b.classList.remove('active'));
  btn.classList.add('active');
  renderTopics();
}

function filterTopics(){
  searchText = document.getElementById('search').value;
  renderTopics();
}

function openLesson(id){
  currentLesson = LESSONS.find(x=>x.id===id);
  document.getElementById('homeView').style.display='none';
  document.getElementById('lessonView').style.display='block';
  document.getElementById('lessonImg').src = currentLesson.image;
  document.getElementById('lessonTag').innerText = currentLesson.classLabel;
  document.getElementById('lessonContent').innerHTML =
    currentLesson.content +
    `<div class="img-row"><img src="${currentLesson.img2}"><img src="${currentLesson.img3}"></div>` +
    `<div class="verse"><b>🎯 Learning Objective:</b><br>${currentLesson.objective}</div>` +
    `<h3>📸 Lesson Pictures</h3><p>Use the pictures above to explain to students. Ask: "What do you see in this picture? What is it used for?"</p>`;
  document.getElementById('quizBox').style.display='none';
  window.scrollTo(0,0);
}

function goHome(){
  document.getElementById('lessonView').style.display='none';
  document.getElementById('homeView').style.display='block';
}

function showQuiz(){
  const box = document.getElementById('quizBox');
  box.style.display='block';
  box.innerHTML = `<h3>📝 Quick Quiz</h3><p><b>Q:</b> ${currentLesson.quiz}</p><button onclick="alert('✅ Answer: ${currentLesson.answer}')" style="background:#1E3A8A;color:white;border:none;padding:11px 20px;border-radius:20px;cursor:pointer;">Show Answer</button>`;
  box.scrollIntoView({behavior:'smooth'});
}

function shareLesson(){
  const text = `*DIGITAL TECHNOLOGIES - ${currentLesson.title}* \n${currentLesson.classLabel}\n\nObjective: ${currentLesson.objective}\n\nLearn with us!`;
  window.open(`https://wa.me/?text=${encodeURIComponent(text)}`);
}

// Start
renderTopics();
</script>
</body>
</html>
