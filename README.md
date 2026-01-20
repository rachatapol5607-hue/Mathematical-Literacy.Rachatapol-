<html lang="th" class="h-full">
 <head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>คณิตศาสตร์การเงิน ป.5</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <script src="/_sdk/element_sdk.js"></script>
  <link href="https://fonts.googleapis.com/css2?family=Kanit:wght@300;400;500;600;700&amp;display=swap" rel="stylesheet">
  <style>
    body {
      box-sizing: border-box;
    }
    * {
      font-family: 'Kanit', sans-serif;
    }
    .coin-spin {
      animation: spin 2s linear infinite;
    }
    @keyframes spin {
      from { transform: rotateY(0deg); }
      to { transform: rotateY(360deg); }
    }
    .float-up {
      animation: floatUp 0.5s ease-out forwards;
    }
    @keyframes floatUp {
      0% { opacity: 1; transform: translateY(0); }
      100% { opacity: 0; transform: translateY(-50px); }
    }
    .bounce-in {
      animation: bounceIn 0.5s ease-out;
    }
    @keyframes bounceIn {
      0% { transform: scale(0); }
      50% { transform: scale(1.2); }
      100% { transform: scale(1); }
    }
    .shake {
      animation: shake 0.5s ease-in-out;
    }
    @keyframes shake {
      0%, 100% { transform: translateX(0); }
      25% { transform: translateX(-10px); }
      75% { transform: translateX(10px); }
    }
    .progress-fill {
      transition: width 0.5s ease-out;
    }
    .card-hover:hover {
      transform: translateY(-5px);
      box-shadow: 0 10px 25px rgba(0,0,0,0.15);
    }
    .card-hover {
      transition: all 0.3s ease;
    }
  </style>
  <style>@view-transition { navigation: auto; }</style>
  <script src="/_sdk/data_sdk.js" type="text/javascript"></script>
 </head>
 <body class="h-full">
  <div id="app" class="h-full w-full overflow-auto bg-gradient-to-br from-emerald-400 via-teal-500 to-cyan-600"><!-- หน้าหลัก -->
   <div id="home-screen" class="min-h-full p-6 flex flex-col items-center justify-center">
    <div class="text-center mb-8">
     <div class="text-7xl mb-4">
      💰
     </div>
     <h1 id="main-title" class="text-4xl md:text-5xl font-bold text-white drop-shadow-lg mb-2">คณิตศาสตร์การเงิน</h1>
     <p id="welcome-text" class="text-xl text-emerald-100">เรียนรู้เรื่องเงินอย่างสนุก! 🎉</p>
    </div>
    <div class="grid grid-cols-1 md:grid-cols-2 gap-4 w-full max-w-2xl"><button onclick="startLesson('counting')" class="card-hover bg-white rounded-2xl p-6 text-left shadow-lg">
      <div class="text-4xl mb-3">
       🪙
      </div><h3 class="text-xl font-semibold text-gray-800">นับเงินเหรียญ</h3><p class="text-gray-500 text-sm">ฝึกนับเหรียญบาทและสตางค์</p></button> <button onclick="startLesson('banknotes')" class="card-hover bg-white rounded-2xl p-6 text-left shadow-lg">
      <div class="text-4xl mb-3">
       💵
      </div><h3 class="text-xl font-semibold text-gray-800">นับเงินธนบัตร</h3><p class="text-gray-500 text-sm">ฝึกนับธนบัตรไทย</p></button> <button onclick="startLesson('change')" class="card-hover bg-white rounded-2xl p-6 text-left shadow-lg">
      <div class="text-4xl mb-3">
       🛒
      </div><h3 class="text-xl font-semibold text-gray-800">การทอนเงิน</h3><p class="text-gray-500 text-sm">ฝึกคำนวณเงินทอน</p></button> <button onclick="startLesson('quiz')" class="card-hover bg-white rounded-2xl p-6 text-left shadow-lg">
      <div class="text-4xl mb-3">
       🎯
      </div><h3 class="text-xl font-semibold text-gray-800">ทดสอบความรู้</h3><p class="text-gray-500 text-sm">ทดสอบทักษะการคำนวณเงิน</p></button>
    </div>
    <div id="score-display" class="mt-8 bg-white/20 backdrop-blur rounded-xl px-6 py-3 text-white"><span class="text-lg">⭐ คะแนนรวม: <span id="total-score" class="font-bold">0</span> แต้ม</span>
    </div>
   </div><!-- หน้าเกม -->
   <div id="game-screen" class="min-h-full p-6 hidden">
    <div class="max-w-2xl mx-auto"><!-- Header -->
     <div class="flex justify-between items-center mb-6"><button onclick="goHome()" class="bg-white/20 hover:bg-white/30 text-white px-4 py-2 rounded-full transition"> ← กลับหน้าหลัก </button>
      <div class="bg-white/20 backdrop-blur rounded-full px-4 py-2 text-white">
       ⭐ <span id="game-score">0</span>
      </div>
     </div><!-- Progress Bar -->
     <div class="bg-white/20 rounded-full h-4 mb-6 overflow-hidden">
      <div id="progress-bar" class="progress-fill bg-yellow-400 h-full rounded-full" style="width: 0%"></div>
     </div><!-- Game Title -->
     <h2 id="game-title" class="text-2xl font-bold text-white text-center mb-6"></h2><!-- Question Area -->
     <div id="question-area" class="bg-white rounded-2xl p-6 shadow-xl mb-6">
      <div id="question-content" class="text-center"><!-- Question content will be inserted here -->
      </div>
     </div><!-- Answer Area -->
     <div id="answer-area" class="grid grid-cols-2 gap-4"><!-- Answer options will be inserted here -->
     </div><!-- Feedback -->
     <div id="feedback" class="mt-6 text-center hidden">
      <div id="feedback-content" class="bg-white rounded-xl p-4 inline-block"></div>
     </div>
    </div>
   </div><!-- หน้าผลลัพธ์ -->
   <div id="result-screen" class="min-h-full p-6 flex flex-col items-center justify-center hidden">
    <div class="bg-white rounded-3xl p-8 text-center shadow-2xl max-w-md w-full">
     <div class="text-6xl mb-4" id="result-emoji">
      🏆
     </div>
     <h2 class="text-3xl font-bold text-gray-800 mb-2">ยอดเยี่ยม!</h2>
     <p class="text-gray-600 mb-4">เก่งมากๆ เลย!</p>
     <div class="bg-gradient-to-r from-yellow-400 to-orange-400 rounded-xl p-4 mb-6">
      <p class="text-white text-lg">คะแนนที่ได้</p>
      <p id="final-score" class="text-4xl font-bold text-white">0</p>
     </div>
     <div class="flex gap-4"><button onclick="goHome()" class="flex-1 bg-gray-200 hover:bg-gray-300 text-gray-700 py-3 rounded-xl font-semibold transition"> 🏠 หน้าหลัก </button> <button onclick="playAgain()" class="flex-1 bg-emerald-500 hover:bg-emerald-600 text-white py-3 rounded-xl font-semibold transition"> 🔄 เล่นอีก </button>
     </div>
    </div>
   </div>
  </div>
  <script>
    // Configuration
    const defaultConfig = {
      app_title: 'คณิตศาสตร์การเงิน',
      welcome_message: 'เรียนรู้เรื่องเงินอย่างสนุก! 🎉',
      primary_color: '#10b981',
      secondary_color: '#ffffff',
      text_color: '#1f2937',
      accent_color: '#f59e0b',
      button_color: '#059669',
      font_family: 'Kanit',
      font_size: 16
    };

    let config = { ...defaultConfig };
    let totalScore = 0;
    let gameScore = 0;
    let currentLesson = '';
    let currentQuestion = 0;
    let questions = [];

    // Coin and banknote data
    const coins = [
      { value: 0.25, emoji: '🪙', label: '25 สตางค์' },
      { value: 0.50, emoji: '🪙', label: '50 สตางค์' },
      { value: 1, emoji: '🪙', label: '1 บาท' },
      { value: 2, emoji: '🪙', label: '2 บาท' },
      { value: 5, emoji: '🪙', label: '5 บาท' },
      { value: 10, emoji: '🪙', label: '10 บาท' }
    ];

    const banknotes = [
      { value: 20, emoji: '💵', color: '#2d8c44', label: '20 บาท' },
      { value: 50, emoji: '💵', color: '#2563eb', label: '50 บาท' },
      { value: 100, emoji: '💵', color: '#dc2626', label: '100 บาท' },
      { value: 500, emoji: '💵', color: '#7c3aed', label: '500 บาท' },
      { value: 1000, emoji: '💵', color: '#92400e', label: '1,000 บาท' }
    ];

    // Products for change calculation
    const products = [
      { name: 'ขนมปัง', price: 15, emoji: '🍞' },
      { name: 'น้ำผลไม้', price: 12, emoji: '🧃' },
      { name: 'ไอศกรีม', price: 20, emoji: '🍦' },
      { name: 'ขนมถุง', price: 10, emoji: '🍿' },
      { name: 'นม', price: 14, emoji: '🥛' },
      { name: 'มาม่า', price: 6, emoji: '🍜' },
      { name: 'ข้าวเหนียวหมูปิ้ง', price: 25, emoji: '🍖' },
      { name: 'น้ำเปล่า', price: 7, emoji: '💧' },
      { name: 'ลูกอม', price: 5, emoji: '🍬' },
      { name: 'แซนด์วิช', price: 35, emoji: '🥪' }
    ];

    // Initialize SDK
    function initializeApp() {
      if (window.elementSdk) {
        window.elementSdk.init({
          defaultConfig,
          onConfigChange: async (newConfig) => {
            config = { ...defaultConfig, ...newConfig };
            updateUI();
          },
          mapToCapabilities: (cfg) => ({
            recolorables: [
              {
                get: () => cfg.primary_color || defaultConfig.primary_color,
                set: (v) => { cfg.primary_color = v; window.elementSdk.setConfig({ primary_color: v }); }
              },
              {
                get: () => cfg.secondary_color || defaultConfig.secondary_color,
                set: (v) => { cfg.secondary_color = v; window.elementSdk.setConfig({ secondary_color: v }); }
              },
              {
                get: () => cfg.text_color || defaultConfig.text_color,
                set: (v) => { cfg.text_color = v; window.elementSdk.setConfig({ text_color: v }); }
              },
              {
                get: () => cfg.accent_color || defaultConfig.accent_color,
                set: (v) => { cfg.accent_color = v; window.elementSdk.setConfig({ accent_color: v }); }
              },
              {
                get: () => cfg.button_color || defaultConfig.button_color,
                set: (v) => { cfg.button_color = v; window.elementSdk.setConfig({ button_color: v }); }
              }
            ],
            borderables: [],
            fontEditable: {
              get: () => cfg.font_family || defaultConfig.font_family,
              set: (v) => { cfg.font_family = v; window.elementSdk.setConfig({ font_family: v }); }
            },
            fontSizeable: {
              get: () => cfg.font_size || defaultConfig.font_size,
              set: (v) => { cfg.font_size = v; window.elementSdk.setConfig({ font_size: v }); }
            }
          }),
          mapToEditPanelValues: (cfg) => new Map([
            ['app_title', cfg.app_title || defaultConfig.app_title],
            ['welcome_message', cfg.welcome_message || defaultConfig.welcome_message]
          ])
        });
        config = { ...defaultConfig, ...window.elementSdk.config };
      }
      updateUI();
    }

    function updateUI() {
      const title = document.getElementById('main-title');
      const welcome = document.getElementById('welcome-text');
      const app = document.getElementById('app');
      
      title.textContent = config.app_title || defaultConfig.app_title;
      welcome.textContent = config.welcome_message || defaultConfig.welcome_message;
      
      // Apply font
      const fontFamily = config.font_family || defaultConfig.font_family;
      document.body.style.fontFamily = `${fontFamily}, Kanit, sans-serif`;
      
      // Apply font size scaling
      const baseSize = config.font_size || defaultConfig.font_size;
      title.style.fontSize = `${baseSize * 2.5}px`;
      welcome.style.fontSize = `${baseSize * 1.25}px`;
      
      // Apply colors
      app.style.background = `linear-gradient(135deg, ${config.primary_color || defaultConfig.primary_color}, ${config.button_color || defaultConfig.button_color})`;
    }

    // Generate questions for each lesson type
    function generateCountingQuestions() {
      const questions = [];
      for (let i = 0; i < 5; i++) {
        const numCoins = Math.floor(Math.random() * 4) + 2;
        const selectedCoins = [];
        let total = 0;
        
        for (let j = 0; j < numCoins; j++) {
          const coin = coins[Math.floor(Math.random() * coins.length)];
          selectedCoins.push(coin);
          total += coin.value;
        }
        
        total = Math.round(total * 100) / 100;
        
        const wrongAnswers = generateWrongAnswers(total, 3);
        const allAnswers = shuffleArray([total, ...wrongAnswers]);
        
        questions.push({
          type: 'counting',
          coins: selectedCoins,
          correct: total,
          options: allAnswers
        });
      }
      return questions;
    }

    function generateBanknoteQuestions() {
      const questions = [];
      for (let i = 0; i < 5; i++) {
        const numNotes = Math.floor(Math.random() * 3) + 1;
        const selectedNotes = [];
        let total = 0;
        
        for (let j = 0; j < numNotes; j++) {
          const note = banknotes[Math.floor(Math.random() * banknotes.length)];
          selectedNotes.push(note);
          total += note.value;
        }
        
        const wrongAnswers = generateWrongAnswers(total, 3);
        const allAnswers = shuffleArray([total, ...wrongAnswers]);
        
        questions.push({
          type: 'banknotes',
          notes: selectedNotes,
          correct: total,
          options: allAnswers
        });
      }
      return questions;
    }

    function generateChangeQuestions() {
      const questions = [];
      for (let i = 0; i < 5; i++) {
        const product = products[Math.floor(Math.random() * products.length)];
        const paymentOptions = [20, 50, 100];
        const payment = paymentOptions.find(p => p >= product.price) || 100;
        const change = payment - product.price;
        
        const wrongAnswers = generateWrongAnswers(change, 3);
        const allAnswers = shuffleArray([change, ...wrongAnswers]);
        
        questions.push({
          type: 'change',
          product: product,
          payment: payment,
          correct: change,
          options: allAnswers
        });
      }
      return questions;
    }

    function generateQuizQuestions() {
      const allQuestions = [
        ...generateCountingQuestions().slice(0, 2),
        ...generateBanknoteQuestions().slice(0, 2),
        ...generateChangeQuestions().slice(0, 2)
      ];
      return shuffleArray(allQuestions);
    }

    function generateWrongAnswers(correct, count) {
      const wrong = [];
      while (wrong.length < count) {
        let offset = (Math.floor(Math.random() * 20) - 10);
        if (offset === 0) offset = 5;
        let wrongAnswer = correct + offset;
        if (wrongAnswer < 0) wrongAnswer = correct + Math.abs(offset);
        wrongAnswer = Math.round(wrongAnswer * 100) / 100;
        if (!wrong.includes(wrongAnswer) && wrongAnswer !== correct) {
          wrong.push(wrongAnswer);
        }
      }
      return wrong;
    }

    function shuffleArray(array) {
      const newArray = [...array];
      for (let i = newArray.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));
        [newArray[i], newArray[j]] = [newArray[j], newArray[i]];
      }
      return newArray;
    }

    // Game functions
    function startLesson(lesson) {
      currentLesson = lesson;
      currentQuestion = 0;
      gameScore = 0;
      
      switch (lesson) {
        case 'counting':
          questions = generateCountingQuestions();
          document.getElementById('game-title').textContent = '🪙 นับเงินเหรียญ';
          break;
        case 'banknotes':
          questions = generateBanknoteQuestions();
          document.getElementById('game-title').textContent = '💵 นับเงินธนบัตร';
          break;
        case 'change':
          questions = generateChangeQuestions();
          document.getElementById('game-title').textContent = '🛒 การทอนเงิน';
          break;
        case 'quiz':
          questions = generateQuizQuestions();
          document.getElementById('game-title').textContent = '🎯 ทดสอบความรู้';
          break;
      }
      
      document.getElementById('home-screen').classList.add('hidden');
      document.getElementById('game-screen').classList.remove('hidden');
      document.getElementById('result-screen').classList.add('hidden');
      document.getElementById('game-score').textContent = '0';
      
      showQuestion();
    }

    function showQuestion() {
      if (currentQuestion >= questions.length) {
        showResults();
        return;
      }
      
      const q = questions[currentQuestion];
      const progress = ((currentQuestion) / questions.length) * 100;
      document.getElementById('progress-bar').style.width = `${progress}%`;
      document.getElementById('feedback').classList.add('hidden');
      
      let questionHTML = '';
      
      if (q.type === 'counting') {
        questionHTML = `
          <p class="text-lg text-gray-600 mb-4">รวมเงินเหรียญทั้งหมดเท่าไร?</p>
          <div class="flex flex-wrap justify-center gap-2 mb-4">
            ${q.coins.map((coin, i) => `
              <div class="bg-yellow-100 rounded-full p-3 bounce-in" style="animation-delay: ${i * 0.1}s">
                <span class="text-3xl">${coin.emoji}</span>
                <p class="text-xs text-yellow-700 font-medium">${coin.label}</p>
              </div>
            `).join('')}
          </div>
        `;
      } else if (q.type === 'banknotes') {
        questionHTML = `
          <p class="text-lg text-gray-600 mb-4">รวมธนบัตรทั้งหมดเท่าไร?</p>
          <div class="flex flex-wrap justify-center gap-3 mb-4">
            ${q.notes.map((note, i) => `
              <div class="rounded-lg p-3 bounce-in text-white font-bold" style="background-color: ${note.color}; animation-delay: ${i * 0.1}s">
                <span class="text-2xl">${note.emoji}</span>
                <p class="text-sm">${note.label}</p>
              </div>
            `).join('')}
          </div>
        `;
      } else if (q.type === 'change') {
        questionHTML = `
          <p class="text-lg text-gray-600 mb-2">ซื้อ${q.product.name} ${q.product.emoji}</p>
          <p class="text-2xl font-bold text-emerald-600 mb-2">ราคา ${q.product.price} บาท</p>
          <p class="text-gray-500">จ่ายเงิน ${q.payment} บาท</p>
          <p class="text-lg text-gray-700 mt-4 font-semibold">ต้องได้เงินทอนเท่าไร?</p>
        `;
      }
      
      document.getElementById('question-content').innerHTML = questionHTML;
      
      // Generate answer buttons
      const answerArea = document.getElementById('answer-area');
      answerArea.innerHTML = q.options.map((option, i) => `
        <button onclick="checkAnswer(${option}, ${q.correct})" 
                class="bg-white hover:bg-gray-50 border-2 border-gray-200 hover:border-emerald-400 rounded-xl p-4 text-xl font-bold text-gray-700 transition transform hover:scale-105 active:scale-95">
          ${formatMoney(option)} บาท
        </button>
      `).join('');
    }

    function formatMoney(amount) {
      if (amount < 1) {
        return `${amount * 100} สตางค์`;
      }
      return amount.toLocaleString('th-TH');
    }

    function checkAnswer(selected, correct) {
      const feedback = document.getElementById('feedback');
      const feedbackContent = document.getElementById('feedback-content');
      
      if (selected === correct) {
        gameScore += 10;
        document.getElementById('game-score').textContent = gameScore;
        feedbackContent.innerHTML = `
          <div class="text-green-500 text-4xl mb-2">✅</div>
          <p class="text-green-600 font-bold text-lg">ถูกต้อง! +10 แต้ม</p>
        `;
        feedbackContent.className = 'bg-green-50 border-2 border-green-200 rounded-xl p-4 inline-block bounce-in';
      } else {
        feedbackContent.innerHTML = `
          <div class="text-red-500 text-4xl mb-2">❌</div>
          <p class="text-red-600 font-bold">ไม่ถูกต้อง</p>
          <p class="text-gray-600">คำตอบที่ถูกคือ <span class="font-bold text-emerald-600">${formatMoney(correct)} บาท</span></p>
        `;
        feedbackContent.className = 'bg-red-50 border-2 border-red-200 rounded-xl p-4 inline-block shake';
      }
      
      feedback.classList.remove('hidden');
      
      // Disable all answer buttons
      const buttons = document.querySelectorAll('#answer-area button');
      buttons.forEach(btn => {
        btn.disabled = true;
        btn.classList.add('opacity-50', 'cursor-not-allowed');
      });
      
      setTimeout(() => {
        currentQuestion++;
        showQuestion();
      }, 1500);
    }

    function showResults() {
      totalScore += gameScore;
      document.getElementById('total-score').textContent = totalScore;
      
      document.getElementById('game-screen').classList.add('hidden');
      document.getElementById('result-screen').classList.remove('hidden');
      document.getElementById('final-score').textContent = gameScore + ' แต้ม';
      
      const maxScore = questions.length * 10;
      const percentage = (gameScore / maxScore) * 100;
      
      let emoji = '🏆';
      if (percentage >= 80) emoji = '🏆';
      else if (percentage >= 60) emoji = '⭐';
      else if (percentage >= 40) emoji = '👍';
      else emoji = '💪';
      
      document.getElementById('result-emoji').textContent = emoji;
    }

    function goHome() {
      document.getElementById('home-screen').classList.remove('hidden');
      document.getElementById('game-screen').classList.add('hidden');
      document.getElementById('result-screen').classList.add('hidden');
    }

    function playAgain() {
      startLesson(currentLesson);
    }

    // Initialize
    initializeApp();
  </script>
 <script>(function(){function c(){var b=a.contentDocument||a.contentWindow.document;if(b){var d=b.createElement('script');d.innerHTML="window.__CF$cv$params={r:'9c0ad2b841e77336',t:'MTc2ODg3MjgwMC4wMDAwMDA='};var a=document.createElement('script');a.nonce='';a.src='/cdn-cgi/challenge-platform/scripts/jsd/main.js';document.getElementsByTagName('head')[0].appendChild(a);";b.getElementsByTagName('head')[0].appendChild(d)}}if(document.body){var a=document.createElement('iframe');a.height=1;a.width=1;a.style.position='absolute';a.style.top=0;a.style.left=0;a.style.border='none';a.style.visibility='hidden';document.body.appendChild(a);if('loading'!==document.readyState)c();else if(window.addEventListener)document.addEventListener('DOMContentLoaded',c);else{var e=document.onreadystatechange||function(){};document.onreadystatechange=function(b){e(b);'loading'!==document.readyState&&(document.onreadystatechange=e,c())}}}})();</script></body>
</html>
