<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no"/>
  <title>EarnifyBot – Monetag Edition</title>
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.1/css/all.min.css">
  <style>
    :root {
      --primary-color: #00ffd5;
      --primary-hover: #00c9ff;
      --secondary-color: #2a2a2e;
      --background-color: #0d0d0d;
      --text-color: #f0f0f0;
      --text-muted-color: #a0a0a0;
      --accent-color: #ff8c00;
      --card-bg: #121212;
    }
    * { box-sizing: border-box; -webkit-tap-highlight-color: transparent; }
    body {
      margin:0; padding:0; background:var(--background-color); color:var(--text-color);
      font-family:'Segoe UI','Roboto',sans-serif;
      height:100vh; display:flex; justify-content:center; align-items:center;
    }
    .container { width:100%; max-width:420px; height:100%; background:var(--background-color); display:flex; flex-direction:column; }
    main { flex-grow:1; overflow-y:auto; padding:20px; padding-bottom:80px; }
    .view { display:none; animation:fadeIn 0.4s ease-in-out; }
    .view.active { display:block; }
    @keyframes fadeIn { from{opacity:0; transform:translateY(10px);} to{opacity:1; transform:translateY(0);} }
    .user-header-card { background:var(--card-bg); border-radius:16px; padding:15px; margin-bottom:25px; display:flex; align-items:center; }
    .user-avatar { width:50px; height:50px; border-radius:50%; margin-right:15px; background:linear-gradient(135deg,var(--primary-color),#0077ff); display:flex; align-items:center; justify-content:center; font-weight:bold; font-size:20px; }
    .user-info .username { font-size:18px; font-weight:700; margin:0; }
    .user-info .title { font-size:14px; color:var(--text-muted-color); margin:0; }
    .balance-card { background:linear-gradient(135deg,#00ffd5,#0077ff); border-radius:16px; padding:20px; text-align:center; margin-bottom:25px; }
    .balance-card .label { font-size:16px; opacity:0.8; margin-bottom:5px; }
    .balance-card .balance-value { font-size:36px; font-weight:800; margin:0; }
    .section-title { font-size:20px; font-weight:700; margin-bottom:15px; padding-left:5px; border-left:4px solid var(--primary-color); }
    .task-card { background:var(--card-bg); border-radius:12px; padding:20px; margin-bottom:15px; display:flex; align-items:center; cursor:pointer; border:1px solid rgba(255,255,255,0.05); }
    .task-card:hover { background:var(--secondary-color); }
    .task-icon { font-size:24px; color:var(--primary-color); margin-right:20px; }
    .task-details h3 { margin:0; font-size:16px; font-weight:600; }
    .task-details p { margin:3px 0 0; font-size:13px; color:var(--text-muted-color); }
    .task-arrow { margin-left:auto; font-size:18px; color:var(--text-muted-color); }
    .footer-nav { position:fixed; bottom:0; left:0; right:0; display:flex; justify-content:space-around; background:var(--secondary-color); padding:10px 0; }
    .nav-button { display:flex; flex-direction:column; align-items:center; color:var(--text-muted-color); background:none; border:none; font-size:12px; cursor:pointer; }
    .nav-button .icon { font-size:20px; margin-bottom:4px; }
    .nav-button.active { color:var(--primary-color); }
    .list-item { display:flex; align-items:center; background:var(--card-bg); padding:12px 15px; border-radius:10px; margin-bottom:10px; }
    .list-item .info { flex-grow:1; }
    .list-item .info .name { font-weight:600; font-size:15px; }
    .list-item .info .detail { font-size:13px; color:var(--text-muted-color); }
    .history-icon { color:var(--primary-color); margin-right:15px; font-size:18px; }
  </style>
</head>
<body>

<div class="container">
  <main>
    <!-- HOME VIEW -->
    <div id="home-view" class="view active">
      <div class="user-header-card">
        <div class="user-avatar">EB</div>
        <div class="user-info">
          <h2 class="username" id="username">Guest</h2>
          <p class="title">Welcome to EarnifyBot</p>
        </div>
      </div>
      <div class="balance-card">
        <p class="label">Balance</p>
        <h1 class="balance-value" id="balance-value">৳0</h1>
      </div>
      <h2 class="section-title">Quick Actions</h2>
      <div class="task-card" onclick="showView('tasks-view')">
        <div class="task-icon"><i class="fa-solid fa-play"></i></div>
        <div class="task-details"><h3>Start Earning</h3><p>Watch ads and get paid instantly</p></div>
        <div class="task-arrow"><i class="fa-solid fa-chevron-right"></i></div>
      </div>
      <div class="task-card" onclick="openWithdrawModal()">
        <div class="task-icon"><i class="fa-solid fa-wallet"></i></div>
        <div class="task-details"><h3>Withdraw</h3><p>Request payout to bKash or Nagad</p></div>
        <div class="task-arrow"><i class="fa-solid fa-chevron-right"></i></div>
      </div>
    </div>

    <!-- TASKS VIEW -->
    <div id="tasks-view" class="view">
      <h2 class="section-title">Earning Tasks</h2>
      <div class="task-card" onclick="showRewardedInterstitial()">
        <div class="task-icon"><i class="fa-solid fa-rectangle-ad"></i></div>
        <div class="task-details"><h3>Rewarded Interstitial</h3><p>Watch full ad to earn TK</p></div>
        <div class="task-arrow"><i class="fa-solid fa-play"></i></div>
      </div>
      <div class="task-card" onclick="showRewardedPopup()">
        <div class="task-icon"><i class="fa-solid fa-rectangle-ad"></i></div>
        <div class="task-details"><h3>Rewarded Popup</h3><p>Popup ad gives instant reward</p></div>
        <div class="task-arrow"><i class="fa-solid fa-play"></i></div>
      </div>
    </div>

    <!-- HISTORY VIEW -->
    <div id="history-view" class="view">
      <h2 class="section-title">Activity History</h2>
      <div id="history-list"></div>
    </div>
  </main>

  <nav class="footer-nav">
    <button class="nav-button active" onclick="showView('home-view')"><i class="icon fa-solid fa-house"></i>Home</button>
    <button class="nav-button" onclick="showView('tasks-view')"><i class="icon fa-solid fa-list-check"></i>Tasks</button>
    <button class="nav-button" onclick="showView('history-view')"><i class="icon fa-solid fa-clock-rotate-left"></i>History</button>
  </nav>
</div>

<!-- ✅ Monetag SDK -->
<script src='//libtl.com/sdk.js' data-zone='9962737' data-sdk='show_9962737'></script>

<script>
  let balance = 0;
  let historyLog = [];

  function saveData(){
    localStorage.setItem("earnifyData", JSON.stringify({balance, historyLog}));
  }
  function loadData(){
    let d = localStorage.getItem("earnifyData");
    if(d){ let parsed = JSON.parse(d); balance = parsed.balance||0; historyLog=parsed.historyLog||[]; }
    updateDisplay();
  }
  function updateDisplay(){
    document.getElementById("balance-value").textContent = `৳${balance}`;
  }
  function addToHistory(detail){
    historyLog.unshift({detail, time:new Date().toLocaleString()});
    saveData(); renderHistory();
  }
  function renderHistory(){
    const list=document.getElementById("history-list");
    if(historyLog.length===0){
      list.innerHTML="<div class='list-item'><div class='info'><div class='name'>No activity yet</div></div></div>";
      return;
    }
    list.innerHTML = historyLog.map(item=>`<div class='list-item'><div class='history-icon'><i class='fa-solid fa-coins'></i></div><div class='info'><div class='name'>${item.detail}</div><div class='detail'>${item.time}</div></div></div>`).join('');
  }

  function rewardUser(amount,source){
    balance+=amount; updateDisplay(); addToHistory(`+${amount} TK from ${source}`); alert(`✅ You earned ${amount} TK!`);
  }

  // --- Monetag Ad Functions ---
  // Rewarded Interstitial
  function showRewardedInterstitial(){
    show_9962737().then(()=>{
      rewardUser(1,"Rewarded Interstitial");
    });
  }

  // Rewarded Popup
  function showRewardedPopup(){
    show_9962737('pop').then(()=>{
      rewardUser(1,"Rewarded Popup");
    }).catch(e=>console.error(e));
  }

  // In-App Interstitial (auto ads)
  function initInAppInterstitial(){
    show_9962737({
      type:'inApp',
      inAppSettings:{
        frequency:2,
        capping:0.1,
        interval:30,
        timeout:5,
        everyPage:false
      }
    });
  }

  // --- Navigation ---
  function showView(id){
    document.querySelectorAll('.view').forEach(v=>v.classList.remove('active'));
    document.getElementById(id).classList.add('active');
    document.querySelectorAll('.nav-button').forEach(b=>b.classList.remove('active'));
    const btn=document.querySelector(`.nav-button[onclick="showView('${id}')"]`);
    if(btn)btn.classList.add('active');
    if(id==='history-view') renderHistory();
  }

  function openWithdrawModal(){
    if(balance<300){alert('❌ Minimum withdraw is 300 TK'); return;}
    alert(`✅ Withdrawal requested: ৳${balance}. Please wait for admin approval.`);
    addToHistory(`Withdrawal request ৳${balance}`);
    balance=0; updateDisplay(); saveData();
  }

  // Init
  loadData();
  initInAppInterstitial();
</script>
</body>
</html>
