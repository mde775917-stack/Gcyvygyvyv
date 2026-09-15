<!DOCTYPE html>
<html lang="bn">
<head>
  <meta charset="UTF-8">

  <meta name="viewport"
        content="width=device-width,
        initial-scale=1.0,
        maximum-scale=1.0,
        user-scalable=no">

  <title>Gky Reward</title>

  <!-- Telegram Mini App -->
  <script src="https://telegram.org/js/telegram-web-app.js"></script>

  <!-- AdsGram -->
  <script src="https://sad.adsgram.ai/js/sad.min.js"></script>

  <style>
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
      font-family: Arial, sans-serif;
    }

    body {
      min-height: 100vh;
      background: #f3f6fb;
      color: #17212b;
    }

    .header {
      background: #1684ff;
      color: white;
      padding: 28px 20px;
      border-radius: 0 0 28px 28px;
    }

    .header h1 {
      font-size: 26px;
      margin-bottom: 8px;
    }

    .header p {
      opacity: .9;
      font-size: 14px;
    }

    .container {
      padding: 20px;
    }

    .card {
      background: white;
      border-radius: 20px;
      padding: 22px;
      margin-bottom: 18px;
      box-shadow: 0 5px 20px rgba(0,0,0,.06);
    }

    .balance-title {
      color: #777;
      font-size: 14px;
    }

    .balance {
      font-size: 34px;
      font-weight: bold;
      margin-top: 8px;
    }

    .ad-card {
      text-align: center;
    }

    .ad-icon {
      font-size: 50px;
      margin-bottom: 12px;
    }

    .ad-card h2 {
      font-size: 22px;
      margin-bottom: 10px;
    }

    .ad-card p {
      color: #777;
      line-height: 1.5;
      margin-bottom: 20px;
    }

    #watchAd {
      width: 100%;
      border: none;
      border-radius: 14px;
      padding: 16px;
      background: #1684ff;
      color: white;
      font-size: 17px;
      font-weight: bold;
      cursor: pointer;
    }

    #watchAd:disabled {
      opacity: .6;
    }

    .status {
      text-align: center;
      margin-top: 14px;
      color: #777;
      font-size: 14px;
    }

    .user-card {
      font-size: 15px;
    }

    .user-name {
      margin-top: 8px;
      color: #1684ff;
      font-weight: bold;
    }
  </style>
</head>

<body>

  <!-- Header -->
  <div class="header">
    <h1>🎁 Gky Reward</h1>
    <p>Telegram Mini App</p>
  </div>

  <div class="container">

    <!-- Balance -->
    <div class="card">
      <div class="balance-title">
        বর্তমান ব্যালেন্স
      </div>

      <div class="balance">
        ৳ <span id="balance">0.00</span>
      </div>
    </div>


    <!-- Advertisement -->
    <div class="card ad-card">

      <div class="ad-icon">
        📺
      </div>

      <h2>
        বিজ্ঞাপন দেখুন
      </h2>

      <p>
        নিচের বাটনে চাপ দিলে বিজ্ঞাপন চালু হবে।
      </p>

      <button id="watchAd">
        ▶️ বিজ্ঞাপন দেখুন
      </button>

      <div class="status" id="status">
        প্রস্তুত
      </div>

    </div>


    <!-- Telegram User -->
    <div class="card user-card">

      <b>👤 Telegram Account</b>

      <div class="user-name" id="username">
        Loading...
      </div>

    </div>

  </div>


<script>

  // ==========================================
  // TELEGRAM MINI APP
  // ==========================================

  const tg = window.Telegram.WebApp;

  tg.ready();
  tg.expand();


  // Telegram user information
  const telegramUser =
    tg.initDataUnsafe &&
    tg.initDataUnsafe.user;


  if (telegramUser) {

    let name = telegramUser.first_name || "";

    if (telegramUser.last_name) {
      name += " " + telegramUser.last_name;
    }

    if (telegramUser.username) {
      name += "  @" + telegramUser.username;
    }

    document.getElementById("username").innerText = name;

  } else {

    document.getElementById("username").innerText =
      "Telegram user পাওয়া যায়নি";

  }


  // ==========================================
  // ADSGRAM
  // ==========================================

  const BLOCK_ID = "int-48073";

  let AdController = null;


  function initializeAds() {

    try {

      if (!window.Adsgram) {

        document.getElementById("status").innerText =
          "❌ AdsGram SDK লোড হয়নি";

        return false;
      }


      AdController =
        window.Adsgram.init({
          blockId: BLOCK_ID
        });


      return true;

    } catch (error) {

      console.error("AdsGram initialization error:", error);

      document.getElementById("status").innerText =
        "❌ AdsGram চালু করা যায়নি";

      return false;
    }
  }


  // Initialize
  initializeAds();


  // ==========================================
  // SHOW AD
  // ==========================================

  const button =
    document.getElementById("watchAd");

  const status =
    document.getElementById("status");


  button.addEventListener("click", async function () {

    if (!AdController) {

      const initialized = initializeAds();

      if (!initialized) {
        return;
      }
    }


    button.disabled = true;

    status.innerText =
      "⏳ বিজ্ঞাপন লোড হচ্ছে...";


    try {

      await AdController.show();


      status.innerText =
        "✅ বিজ্ঞাপন শেষ হয়েছে";


      /*
       * IMPORTANT:
       *
       * তোমার Block ID হলো:
       *
       * int-48073
       *
       * এটি Interstitial block।
       *
       * তাই এখানে টাকা/Reward যোগ করা হচ্ছে না।
       */


    } catch (error) {

      console.error("AdsGram show error:", error);

      status.innerText =
        "❌ বিজ্ঞাপন দেখানো যায়নি";

    }


    button.disabled = false;

  });


</script>

</body>
</html>
