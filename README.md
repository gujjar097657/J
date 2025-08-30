<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>AI App with Login</title>
  <style>
    body { font-family: Arial, sans-serif; margin: 20px; }
    #chatBox, #authBox { border: 1px solid #ccc; padding: 15px; border-radius: 10px; }
    #chat { border: 1px solid #ccc; padding: 10px; height: 250px; overflow-y: auto; margin-bottom: 10px; }
    .msg { margin: 5px 0; }
    .user { color: blue; }
    .bot { color: green; }
    input { padding: 8px; margin: 5px 0; width: 90%; }
    button { padding: 8px 12px; margin-top: 5px; cursor: pointer; }
    #chatBox { display: none; }
  </style>
</head>
<body>
  <h1>🤖 AI App with Login</h1>

  <!-- 🔹 Login/Signup Form -->
  <div id="authBox">
    <h2>Login / Signup</h2>
    <input id="username" placeholder="Enter username" /><br>
    <input id="password" type="password" placeholder="Enter password" /><br>
    <button onclick="signup()">Signup</button>
    <button onclick="login()">Login</button>
  </div>

  <!-- 🔹 Chat Box -->
  <div id="chatBox">
    <h2>AI Chat</h2>
    <div id="chat"></div>
    <input id="userInput" placeholder="Type a message..." onkeydown="if(event.key==='Enter') sendMessage()" />
    <button onclick="sendMessage()">Send</button>
    <br><br>
    <button onclick="logout()">Logout</button>
  </div>

  <script>
    // Save users in localStorage (just demo)
    function signup() {
      let user = document.getElementById("username").value;
      let pass = document.getElementById("password").value;
      if (!user || !pass) return alert("Enter username & password");
      if (localStorage.getItem(user)) return alert("User already exists!");
      localStorage.setItem(user, pass);
      alert("Signup successful! Please login.");
    }

    function login() {
      let user = document.getElementById("username").value;
      let pass = document.getElementById("password").value;
      let savedPass = localStorage.getItem(user);
      if (savedPass && savedPass === pass) {
        localStorage.setItem("loggedInUser", user);
        document.getElementById("authBox").style.display = "none";
        document.getElementById("chatBox").style.display = "block";
        document.getElementById("chat").innerHTML = `<div class='msg bot'>Welcome, ${user}! You are logged in.</div>`;
      } else {
        alert("Invalid username or password");
      }
    }

    function logout() {
      localStorage.removeItem("loggedInUser");
      document.getElementById("chatBox").style.display = "none";
      document.getElementById("authBox").style.display = "block";
    }

    function sendMessage() {
      let input = document.getElementById("userInput");
      let msg = input.value.trim();
      if (!msg) return;
      let chat = document.getElementById("chat");
      chat.innerHTML += `<div class='msg user'>👤 ${msg}</div>`;
      input.value = "";
      chat.scrollTop = chat.scrollHeight;

      // Fake AI reply
      setTimeout(() => {
        let reply = "🤖 AI: " + (msg.includes("?") ? "That's a great question!" : "I heard you say: " + msg);
        chat.innerHTML += `<div class='msg bot'>${reply}</div>`;
        chat.scrollTop = chat.scrollHeight;
      }, 600);
    }

    // Auto-login if user was logged in
    window.onload = () => {
      let loggedUser = localStorage.getItem("loggedInUser");
      if (loggedUser) {
        document.getElementById("authBox").style.display = "none";
        document.getElementById("chatBox").style.display = "block";
        document.getElementById("chat").innerHTML = `<div class='msg bot'>Welcome back, ${loggedUser}!</div>`;
      }
    }
  </script>
</body>
</html>
