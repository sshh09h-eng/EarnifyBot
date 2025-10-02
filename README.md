if(email==="nixus390@gmail.com" && pass==="Hamid_017"){
      currentUser = "admin";
      document.getElementById("loginPage").classList.add("hidden");
      document.getElementById("adminPage").classList.remove("hidden");
      loadAdmin();
      return;
    }

    if(users[email] && users[email].pass === pass){
      currentUser = email;
      balance = users[email].balance;
      document.getElementById("bal").innerText = balance;
      document.getElementById("loginPage").classList.add("hidden");
      document.getElementById("dashboardPage").classList.remove("hidden");
    } else {
      document.getElementById("loginStatus").innerText = "❌ Invalid login!";
    }
  }

  function signup(){
    let email = document.getElementById("signupEmail").value;
    let pass = document.getElementById("signupPass").value;

    let storedUsers = JSON.parse(localStorage.getItem("users")  "{}");
    if(storedUsers[email]){
      document.getElementById("signupStatus").innerText = "⚠️ User already exists!";
      return;
    }

    storedUsers[email] = {pass: pass, balance: 0};
    localStorage.setItem("users", JSON.stringify(storedUsers));
    users = storedUsers;

    document.getElementById("signupStatus").innerText = "✅ Registered! Please login.";
  }

  function logout(){
    currentUser = null;
    document.querySelectorAll(".card").forEach(c=>c.classList.add("hidden"));
    document.getElementById("loginPage").classList.remove("hidden");
  }

  function rewardUser(amount){
    balance += amount;
    users[currentUser].balance = balance;
    document.getElementById("bal").innerText = balance;
    document.getElementById("status").innerText = `+${amount} TK added! 🎉`;
    saveData();
  }

  // Monetag Ads
  function watchInterstitial(){
    document.getElementById("status").innerText = "Loading ad...";
    show_9962737().then(()=>{ rewardUser(1); });
  }
  function watchPopup(){
    document.getElementById("status").innerText = "Loading popup ad...";
    show_9962737('pop').then(()=>{ rewardUser(1); });
  }

  function withdraw(){
    let amt = parseInt(document.getElementById("withdrawAmount").value);
    let method = document.getElementById("method").value;
    if(amt < 300  amt > 500){
      alert("❌ Withdraw must be between 300–500 TK");
      return;
    }
    if(balance < amt){
      alert("❌ Insufficient balance");
      return;
    }
    withdraws.push({user: currentUser, amount: amt, method, status:"pending"});
    balance -= amt;
    users[currentUser].balance = balance;
    document.getElementById("bal").innerText = balance;
    saveData();
    alert("✅ Withdrawal requested!");
  }

  function loadAdmin(){
    let box = document.getElementById("withdrawRequests");
    box.innerHTML = "";
    withdraws.forEach((req,i)=>{
      let div = document.createElement("div");
      div.innerHTML = `${req.user} → ${req.amount} TK (${req.method}) [${req.status}] 
        <button onclick="approve(${i})">✔️</button> 
        <button onclick="reject(${i})">✖️</button>`;
      box.appendChild(div);
    });
  }
  function approve(i){
    withdraws[i].status="approved"; saveData(); loadAdmin();
  }
  function reject(i){
    withdraws[i].status="rejected"; saveData(); loadAdmin();
  }
</script>
</body>
</html>
