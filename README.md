



<html lang="en">
<head>
<meta charset="UTF-8">
<title>OAK UNITED ULTRA</title>

<style>
* {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
    font-family: "Segoe UI", sans-serif;
}

body {
    background: radial-gradient(circle at top, #0f172a, #020617);
    color: white;
    display: flex;
    justify-content: center;
    align-items: center;
    min-height: 100vh;
}

/* CONTAINER */
.container {
    width: 460px;
    padding: 20px;
    border-radius: 16px;
    background: rgba(30, 41, 59, 0.55);
    backdrop-filter: blur(18px);
    border: 1px solid rgba(255,255,255,0.08);
    box-shadow: 0 0 40px rgba(100,212,78,0.15);
}

/* TITLE */
h1 {
    text-align: center;
    font-weight: 600;
    margin-bottom: 15px;
    letter-spacing: 1px;
    color: #64D44E;
    text-shadow: 0 0 12px rgba(100,212,78,0.6);
}

/* TABS */
.tabs {
    display: flex;
    gap: 6px;
    margin-bottom: 12px;
}

.tab {
    flex: 1;
    padding: 10px;
    text-align: center;
    border-radius: 10px;
    background: rgba(255,255,255,0.05);
    cursor: pointer;
    transition: 0.2s;
}

.tab:hover {
    background: rgba(255,255,255,0.1);
}

.tab.active {
    background: #64D44E;
    color: black;
    font-weight: bold;
    box-shadow: 0 0 15px #64D44E;
}

/* INPUTS */
input, textarea {
    width: 100%;
    margin-top: 10px;
    padding: 10px;
    border-radius: 10px;
    border: none;
    background: rgba(255,255,255,0.05);
    color: white;
    outline: none;
    transition: 0.2s;
}

input:focus, textarea:focus {
    background: rgba(255,255,255,0.1);
    box-shadow: 0 0 10px rgba(100,212,78,0.3);
}

/* COLOR PICKER */
input[type="color"] {
    height: 45px;
    padding: 4px;
}

/* BUTTON */
button {
    width: 100%;
    margin-top: 14px;
    padding: 12px;
    border-radius: 12px;
    border: none;
    background: linear-gradient(135deg, #64D44E, #4ade80);
    color: black;
    font-weight: bold;
    cursor: pointer;
    transition: 0.2s;
}

button:hover {
    transform: translateY(-1px);
    box-shadow: 0 0 20px rgba(100,212,78,0.5);
}

/* WEBHOOK BOX */
.webhook-box {
    margin-top: 15px;
    padding: 12px;
    border-radius: 12px;
    background: rgba(0,0,0,0.25);
}

.webhook-box h3 {
    margin-bottom: 10px;
    font-size: 14px;
    color: #64D44E;
}

/* WEBHOOK ITEMS */
.webhook-item {
    display: flex;
    align-items: center;
    gap: 10px;
    padding: 8px;
    border-radius: 10px;
    background: rgba(255,255,255,0.05);
    margin-bottom: 6px;
    transition: 0.2s;
}

.webhook-item:hover {
    background: rgba(255,255,255,0.1);
}

.webhook-item input {
    width: auto;
}

/* STATUS */
.status {
    text-align: center;
    margin-top: 10px;
    font-size: 14px;
}

/* HIDDEN */
.hidden {
    display: none;
}
</style>
</head>

<body>

<div class="container">
<h1>OAK UNITED</h1>

<div class="tabs">
    <div class="tab active" onclick="switchTab(event,'message')">Message</div>
    <div class="tab" onclick="switchTab(event,'ban')">Ban</div>
    <div class="tab" onclick="switchTab(event,'notice')">Notice</div>
</div>

<input type="color" id="colorPicker" value="#64d44e">

<div id="messageTab">
    <textarea id="msg" placeholder="Enter message..."></textarea>
</div>

<div id="banTab" class="hidden">
    <input id="banUser" placeholder="User">
    <input id="banID" placeholder="ID">
    <input id="banReason" placeholder="Reason">
    <input id="banServers" placeholder="Servers">
</div>

<div id="noticeTab" class="hidden">
    <textarea id="noticeDesc" placeholder="Warning..."></textarea>
</div>

<div class="webhook-box">
    <h3>Webhooks</h3>
    <div id="webhookList"></div>
</div>

<button onclick="send()">Send</button>
<div class="status" id="status"></div>

</div>

<script>
const webhooks = {
    GPS: "https://discord.com/api/webhooks/1486002707071504494/ZpX2lIm9_lpahlUTRgVf6o80eV8fnNBOBTKJ8cjw1s_ms26jj-LGb0v0h__8Lp060LGR",
    WBA: "https://discord.com/api/webhooks/1486463800252305501/ykDEYjeYHJu-7dy7YPzZqrR3Fam1bFS1WwJFteCeFcHcz45dUWO_XW0NNY6GCJ8vu5Ih",
    Oak: "https://discord.com/api/webhooks/1486002999510700034/OguNOSeKGffapjAlUeILrXcO5CerEah61Gnel0eXATjoYNKkz5WPc6tYejFFuyHccB7Q",
    MS: "https://discord.com/api/webhooks/1486467077442371594/0OIVc-dcVhVMVXfau_5u3Jixv9ID6WDu6V7FhwBs9crsd3A9H3r6BXyU1GDP7PbxJWmV"
};

let currentTab = "message";

function switchTab(e, tab){
    currentTab = tab;

    document.querySelectorAll(".tab").forEach(t=>t.classList.remove("active"));
    e.target.classList.add("active");

    ["message","ban","notice"].forEach(t=>{
        document.getElementById(t+"Tab").classList.add("hidden");
    });

    document.getElementById(tab+"Tab").classList.remove("hidden");
}

function render(){
    const list = document.getElementById("webhookList");
    Object.keys(webhooks).forEach(name=>{
        list.innerHTML += `
            <label class="webhook-item">
                <input type="checkbox" value="${name}">
                ${name}
            </label>`;
    });
}

function getColor(){
    return parseInt(document.getElementById("colorPicker").value.replace("#",""),16);
}

function getSelected(){
    return [...document.querySelectorAll(".webhook-item input:checked")]
        .map(i=>webhooks[i.value]);
}

async function send(){
    const status = document.getElementById("status");
    const hooks = getSelected();

    if(!hooks.length){
        status.innerText="❌ Select webhook";
        return;
    }

    let embed={};

    if(currentTab==="message"){
        embed.description=document.getElementById("msg").value;
    }

    if(currentTab==="ban"){
        embed.title="🚫 GLOBAL BAN";
        embed.fields=[
            {name:"User",value:banUser.value},
            {name:"ID",value:banID.value},
            {name:"Reason",value:banReason.value},
            {name:"Servers",value:banServers.value}
        ];
    }

    if(currentTab==="notice"){
        embed.title="⚠️ NOTICE";
        embed.description=noticeDesc.value;
    }

    embed.color=getColor();

    for(const url of hooks){
        await fetch(url,{
            method:"POST",
            headers:{"Content-Type":"application/json"},
            body:JSON.stringify({embeds:[embed]})
        });
    }

    status.innerText="✅ Sent";
}

render();
</script>

</body>
</html>
