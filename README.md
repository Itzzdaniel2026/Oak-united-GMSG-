


<html lang="en">
<head>
<meta charset="UTF-8">
<title>OAK UNITED - Global Sender</title>
<style>
    body {
        background: #0f172a;
        color: white;
        font-family: Arial, sans-serif;
        display: flex;
        justify-content: center;
        align-items: center;
        height: 100vh;
    }

    .box {
        background: #1e293b;
        padding: 25px;
        border-radius: 12px;
        width: 400px;
        box-shadow: 0 0 20px rgba(0,0,0,0.4);
    }

    h1 {
        text-align: center;
        color: #64D44E;
    }

    textarea {
        width: 100%;
        height: 120px;
        border-radius: 8px;
        border: none;
        padding: 10px;
        resize: none;
        font-size: 14px;
    }

    button {
        width: 100%;
        margin-top: 10px;
        padding: 10px;
        border: none;
        border-radius: 8px;
        background: #64D44E;
        color: black;
        font-weight: bold;
        cursor: pointer;
    }

    button:hover {
        opacity: 0.85;
    }

    .status {
        margin-top: 10px;
        text-align: center;
        font-size: 14px;
    }
</style>
</head>
<body>

<div class="box">
    <h1>OAK UNITED</h1>
    <textarea id="message" placeholder="Type your global message..."></textarea>
    <button onclick="sendMessage()">Send Global Message</button>
    <div class="status" id="status"></div>
</div>

<script>
const webhooks = [
"https://discord.com/api/webhooks/1486002707071504494/ZpX2lIm9_lpahlUTRgVf6o80eV8fnNBOBTKJ8cjw1s_ms26jj-LGb0v0h__8Lp060LGR",
"https://discord.com/api/webhooks/1486463800252305501/ykDEYjeYHJu-7dy7YPzZqrR3Fam1bFS1WwJFteCeFcHcz45dUWO_XW0NNY6GCJ8vu5Ih",
"https://discord.com/api/webhooks/1486002999510700034/OguNOSeKGffapjAlUeILrXcO5CerEah61Gnel0eXATjoYNKkz5WPc6tYejFFuyHccB7Q",
"https://discord.com/api/webhooks/1486467077442371594/0OIVc-dcVhVMVXfau_5u3Jixv9ID6WDu6V7FhwBs9crsd3A9H3r6BXyU1GDP7PbxJWmV"
];

const avatarURL = "https://media.discordapp.net/attachments/1484992634299875470/1485794067274272878/3a6f9c0a-22c1-448b-b5ee-617b97a0135e-removebg-preview.png";
const thumbnailURL = avatarURL;

function sendMessage() {
    const msg = document.getElementById("message").value;
    const status = document.getElementById("status");

    if (!msg) {
        status.innerText = "⚠️ Enter a message first!";
        return;
    }

    let sent = 0;

    webhooks.forEach(url => {
        fetch(url, {
            method: "POST",
            headers: { "Content-Type": "application/json" },
            body: JSON.stringify({
                username: "OAK UNITED",
                avatar_url: avatarURL,
                embeds: [{
                    description: msg,
                    color: 0x64D44E,
                    thumbnail: {
                        url: thumbnailURL
                    },
                    footer: {
                        text: "OAK UNITED Global System"
                    }
                }]
            })
        })
        .then(() => {
            sent++;
            status.innerText = `✅ Sent to ${sent}/${webhooks.length}`;
        })
        .catch(() => {
            status.innerText = "❌ Error sending message";
        });
    });
}
</script>

</body>
</html>
