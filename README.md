



<html lang="en">
<head>
<meta charset="UTF-8">
<title>OAK UNITED PANEL</title>

<style>
body {
    background: #0f172a;
    color: white;
    font-family: Arial;
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
}
.container {
    width: 420px;
    background: #1e293b;
    padding: 20px;
    border-radius: 12px;
}
h1 {
    text-align: center;
    color: #64D44E;
}
.tabs {
    display: flex;
    margin-bottom: 10px;
}
.tab {
    flex: 1;
    padding: 8px;
    text-align: center;
    cursor: pointer;
    background: #334155;
}
.tab.active {
    background: #64D44E;
    color: black;
    font-weight: bold;
}
input, textarea {
    width: 100%;
    margin-top: 8px;
    padding: 8px;
    border-radius: 6px;
    border: none;
}
button {
    width: 100%;
    margin-top: 10px;
    padding: 10px;
    background: #64D44E;
    border: none;
    border-radius: 8px;
    font-weight: bold;
    cursor: pointer;
}
.status {
    text-align: center;
    margin-top: 10px;
}
.hidden {
    display: none;
}
</style>

</head>
<body>

<div class="container">
<h1>OAK UNITED</h1>

<!-- Tabs -->
<div class="tabs">
    <div class="tab active" onclick="switchTab('message')">Message</div>
    <div class="tab" onclick="switchTab('ban')">Global Ban</div>
    <div class="tab" onclick="switchTab('notice')">Yellow Notice</div>
</div>

<!-- Colour Picker -->
<input type="color" id="colorPicker" value="#64d44e">

<!-- MESSAGE TAB -->
<div id="messageTab">
    <textarea id="msg" placeholder="Enter message..."></textarea>
</div>

<!-- BAN TAB -->
<div id="banTab" class="hidden">
    <input id="banUser" placeholder="Discord User">
    <input id="banID" placeholder="Discord ID">
    <input id="banReason" placeholder="Reason">
    <input id="banServers" placeholder="Servers banned in">
</div>

<!-- NOTICE TAB -->
<div id="noticeTab" class="hidden">
    <textarea id="noticeDesc" placeholder="Enter warning description..."></textarea>
</div>

<button onclick="send()">Send</button>
<div class="status" id="status"></div>

</div>

<script>
const webhooks = [
"https://discord.com/api/webhooks/1486002707071504494/ZpX2lIm9_lpahlUTRgVf6o80eV8fnNBOBTKJ8cjw1s_ms26jj-LGb0v0h__8Lp060LGR",
"https://discord.com/api/webhooks/1486463800252305501/ykDEYjeYHJu-7dy7YPzZqrR3Fam1bFS1WwJFteCeFcHcz45dUWO_XW0NNY6GCJ8vu5Ih",
"https://discord.com/api/webhooks/1486002999510700034/OguNOSeKGffapjAlUeILrXcO5CerEah61Gnel0eXATjoYNKkz5WPc6tYejFFuyHccB7Q",
"https://discord.com/api/webhooks/1486467077442371594/0OIVc-dcVhVMVXfau_5u3Jixv9ID6WDu6V7FhwBs9crsd3A9H3r6BXyU1GDP7PbxJWmV"
];

const avatar = "https://media.discordapp.net/attachments/1484992634299875470/1485794067274272878/3a6f9c0a-22c1-448b-b5ee-617b97a0135e-removebg-preview.png";

let currentTab = "message";

function switchTab(tab) {
    currentTab = tab;

    document.querySelectorAll(".tab").forEach(t => t.classList.remove("active"));
    event.target.classList.add("active");

    document.getElementById("messageTab").classList.add("hidden");
    document.getElementById("banTab").classList.add("hidden");
    document.getElementById("noticeTab").classList.add("hidden");

    document.getElementById(tab + "Tab").classList.remove("hidden");
}

function getColor() {
    return parseInt(document.getElementById("colorPicker").value.replace("#",""), 16);
}

function send() {
    const status = document.getElementById("status");
    let embed = {};

    if (currentTab === "message") {
        embed = {
            description: document.getElementById("msg").value
        };
    }

    if (currentTab === "ban") {
        embed = {
            title: "🚫 GLOBAL BAN",
            fields: [
                { name: "User", value: document.getElementById("banUser").value },
                { name: "Discord ID", value: document.getElementById("banID").value },
                { name: "Reason", value: document.getElementById("banReason").value },
                { name: "Servers", value: document.getElementById("banServers").value }
            ]
        };
    }

    if (currentTab === "notice") {
        embed = {
            title: "⚠️ YELLOW NOTICE",
            description: document.getElementById("noticeDesc").value
        };
    }

    embed.color = getColor();
    embed.thumbnail = { url: avatar };
    embed.footer = { text: "OAK UNITED SYSTEM" };

    let sent = 0;

    webhooks.forEach(url => {
        fetch(url, {
            method: "POST",
            headers: {"Content-Type": "application/json"},
            body: JSON.stringify({
                username: "OAK UNITED",
                avatar_url: avatar,
                embeds: [embed]
            })
        })
        .then(() => {
            sent++;
            status.innerText = `✅ Sent ${sent}/${webhooks.length}`;
        })
        .catch(() => {
            status.innerText = "❌ Error sending";
        });
    });
}
</script>

</body>
</html>
