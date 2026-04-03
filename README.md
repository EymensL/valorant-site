[index.html](https://github.com/user-attachments/files/26471233/index.html)
<!DOCTYPE html>
<html>
<head>
  <title>Valorant Duo</title>
</head>
<body style="background:black;color:white;font-family:sans-serif">

<h1>🎮 Valorant Duo Bul</h1>

<input id="username" placeholder="Kullanıcı adı">
<input id="password" type="password" placeholder="Şifre">

<br><br>

<button onclick="register()">Kayıt</button>
<button onclick="login()">Giriş</button>

<p id="info"></p>

<hr>

<h2>Sohbet</h2>

<div id="chat" style="height:200px;overflow:auto;border:1px solid white"></div>

<br>

<input id="msg" placeholder="Mesaj yaz">
<button onclick="send()">Gönder</button>

<script src="https://cdn.jsdelivr.net/npm/@supabase/supabase-js"></script>

<script>
// 🔑 BURAYI DEĞİŞTİR
const supabaseClient = supabase.createClient(
  "https://ixvuxfmfpnifwbidvfic.supabase.co",
  "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6Iml4dnV4Zm1mcG5pZndiaWR2ZmljIiwicm9sZSI6ImFub24iLCJpYXQiOjE3NzUyMTQ4NDYsImV4cCI6MjA5MDc5MDg0Nn0.oEB-6nnJPtWDIKiEJLbIwxMw7V2XLgoUJFQL-Ou4IS4"
);

let currentUser = "";

// 🧾 KAYIT
async function register() {
  let username = document.getElementById("username").value;
  let password = document.getElementById("password").value;

  if (!username || !password) {
    alert("Boş bırakma");
    return;
  }

  await supabaseClient.from("users").insert([
    { username, password }
  ]);

  document.getElementById("info").innerText = "Kayıt başarılı";
}

// 🔐 GİRİŞ
async function login() {
  let username = document.getElementById("username").value;
  let password = document.getElementById("password").value;

  let { data } = await supabaseClient
    .from("users")
    .select("*")
    .eq("username", username)
    .eq("password", password);

  if (data.length > 0) {
    currentUser = username;
    document.getElementById("info").innerText = "Giriş başarılı";
    loadMessages();
  } else {
    document.getElementById("info").innerText = "Hatalı giriş";
  }
}

// 💬 MESAJ GÖNDER
async function send() {
  let text = document.getElementById("msg").value;

  if (!currentUser) {
    alert("Önce giriş yap");
    return;
  }

  await supabaseClient.from("messages").insert([
    { username: currentUser, text }
  ]);

  document.getElementById("msg").value = "";
  loadMessages();
}

// 📥 MESAJLARI ÇEK
async function loadMessages() {
  let { data } = await supabaseClient
    .from("messages")
    .select("*");

  let chat = document.getElementById("chat");
  chat.innerHTML = "";

  data.forEach(m => {
    chat.innerHTML += `<p><b>${m.username}:</b> ${m.text}</p>`;
  });
}

// 🔄 OTOMATİK YENİLE
setInterval(loadMessages, 2000);
</script>

</body>
</html>
