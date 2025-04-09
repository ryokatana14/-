<!DOCTYPE html>
<html lang="ja">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>満足度アンケート</title>
  <style>
    /* 全体の基本設定 */
    body {
      margin: 0;
      padding: 0;
      font-family: 'Arial', sans-serif;
      background: linear-gradient(135deg, #fce4ec, #f3e5f5);
      color: #333;
      text-align: center;
    }
    .page {
      display: none;
      padding: 20px;
      margin-top: 20px;
    }
    .page.active {
      display: block;
    }
    label {
      display: inline-block;
      font-weight: bold;
      margin-right: 10px;
    }
    input[type="date"] {
      padding: 8px;
      border-radius: 10px;
      border: 1px solid #ccc;
    }
    .options {
      display: flex;
      justify-content: center;
      gap: 10px;
      flex-wrap: wrap;
      margin-top: 10px;
    }
    .option-button, button.next-btn {
      background: #f06292;
      color: white;
      border: none;
      padding: 8px 15px;
      border-radius: 25px;
      cursor: pointer;
      box-shadow: 0 4px 10px rgba(0,0,0,0.2);
      font-size: 14px;
    }
    /* 中継ページ：進むボタンのみ */
    #transitionPage {
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
      background: linear-gradient(135deg, #e1f5fe, #ffe0b2);
    }
    /* お誕生日おめでとうございます！ページ */
    #birthdayPage {
      position: relative;
      width: 100%;
      height: 100vh;
      overflow: hidden;
      /* 洗練されたグラデーション背景 */
      background: linear-gradient(45deg, #ff9a9e, #fad0c4);
    }
    #birthday-message {
      font-size: 28px;
      font-weight: bold;
      color: #ffffff;
      white-space: nowrap;
      position: absolute;
      top: 40%;
      /* 左から右へ流れる */
      animation: slideIn 5s linear forwards;
    }
    @keyframes slideIn {
      0% { left: -100%; opacity: 0; }
      20% { opacity: 1; }
      100% { left: 100%; opacity: 1; }
    }
    /* confettiアニメーション（紙吹雪） */
    .confetti {
      position: absolute;
      width: 8px;
      height: 8px;
      background-color: #ffeb3b;
      opacity: 0.9;
      transform: rotate(0deg);
      animation: confettiFall 3s linear infinite;
    }
    @keyframes confettiFall {
      0% {
        transform: translateY(-10px) rotate(0deg);
        opacity: 1;
      }
      100% {
        transform: translateY(110vh) rotate(360deg);
        opacity: 0;
      }
    }
    /* 動画ページ */
    #video-section {
      margin-top: 20px;
    }
  </style>
</head>
<body>
  <!-- ページ1: 生年月日の入力 -->
  <div class="page active" id="page1">
    <label>生年月日を教えてください</label>
    <input type="date" id="birthday" required />
    <button class="next-btn" onclick="nextPage()">次へ</button>
  </div>

  <!-- ページ2: ご一緒に来た方 -->
  <div class="page" id="page2">
    <label>ご一緒に来ていただいたのはどなたですか？</label>
    <div class="options">
      <button class="option-button" onclick="selectOption('partner', '家族')">家族</button>
      <button class="option-button" onclick="selectOption('partner', '友人')">友人</button>
      <button class="option-button" onclick="selectOption('partner', '恋人')">恋人</button>
    </div>
  </div>

  <!-- ページ3: その人への想い -->
  <div class="page" id="page3">
    <label>その人のことはどれくらい好きですか？</label>
    <div class="options">
      <button class="option-button" onclick="selectOption('affection', '大好き')">大好き</button>
      <button class="option-button" onclick="selectOption('affection', 'まぁまぁ好き')">まぁまぁ好き</button>
      <button class="option-button" onclick="selectOption('affection', '普通')">普通</button>
      <button class="option-button" onclick="selectOption('affection', '苦手')">苦手</button>
    </div>
  </div>

  <!-- ページ4: 満足度 -->
  <div class="page" id="page4">
    <label>本日はご満足いただける時間を過ごせましたか？</label>
    <div class="options">
      <button class="option-button" onclick="selectOption('satisfaction', 'とても')">とても</button>
      <button class="option-button" onclick="selectOption('satisfaction', 'はい')">はい</button>
      <button class="option-button" onclick="selectOption('satisfaction', 'いいえ')">いいえ</button>
    </div>
  </div>

  <!-- ページ5: 中継ページ：進むボタンのみ -->
  <div class="page" id="transitionPage">
    <button class="next-btn" onclick="goToBirthdayPage()">進む</button>
  </div>

  <!-- ページ6: お誕生日おめでとうございます！ページ -->
  <div class="page" id="birthdayPage">
    <div id="birthday-message">お誕生日おめでとうございます！</div>
  </div>

  <!-- ページ7: 動画再生ページ -->
  <div class="page" id="video-section">
    <video width="300" controls>
      <source src="your-video.mp4" type="video/mp4">
      動画を再生できません
    </video>
  </div>

  <script>
    let currentPage = 1;
    let answers = {};

    // 各質問ページ（1～4）は「nextPage」や「selectOption」で移行します
    function nextPage() {
      document.getElementById(`page${currentPage}`).classList.remove('active');
      currentPage++;
      if (currentPage <= 4) {
        document.getElementById(`page${currentPage}`).classList.add('active');
      } else {
        // 質問終了後：移動先は中継ページ（ページ5）
        document.getElementById('transitionPage').classList.add('active');
      }
    }
    function selectOption(key, value) {
      answers[key] = value;
      nextPage();
    }
    // 中継ページから「お誕生日おめでとうございます！」ページへ
    function goToBirthdayPage() {
      document.getElementById('transitionPage').classList.remove('active');
      document.getElementById('birthdayPage').classList.add('active');
      // 発生させる紙吹雪（confetti）を追加（見栄えよく50個程度）
      for (let i = 0; i < 50; i++) {
        let confetti = document.createElement('div');
        confetti.className = 'confetti';
        confetti.style.left = Math.random() * 100 + 'vw';
        confetti.style.top = Math.random() * 50 + 'vh';
        confetti.style.backgroundColor = ['#ffeb3b', '#ffd700', '#87cefa'][i % 3];
        confetti.style.animationDelay = (Math.random() * 3) + 's';
        document.getElementById('birthdayPage').appendChild(confetti);
      }
      // 一定時間後に自動的に動画ページへ
      setTimeout(() => {
        document.getElementById('birthdayPage').classList.remove('active');
        document.getElementById('video-section').classList.add('active');
      }, 7000);
    }
  </script>
</body>
</html>
