connect whatsApp PHONE_NUMBER_ID = 123456789012345
WHATSAPP_TOKEN = EAAXXXXXXXXXXXXX
VERIFY_TOKEN = mon_token_verify<h1><img alt="Baileys logo" src="https://raw.githubusercontent.com/WhiskeySockets/Baileys/refs/heads/master/Media/logo.png" height="75"/></h1>// --- Anti-spam system ---
const spamCount = {};
const SPAM_LIMIT = 5; // max 5 messages
const SPAM_INTERVAL = 10000; // 10 seconds

sock.ev.on("messages.upsert", async (m) => {
  const msg = m.messages[0];
  if (!msg.message || msg.key.fromMe) return;

  const from = msg.key.remoteJid;
  const sender = msg.key.participant || msg.key.remoteJid;

  // Si se yon gwoup
  if (from.endsWith("@g.us")) {
    if (!spamCount[sender]) {
      spamCount[sender] = { count: 1, time: Date.now() };
    } else {
      const diff = Date.now() - spamCount[sender].time;
      if (diff < SPAM_INTERVAL) {
        spamCount[sender].count++;
      } else {
        spamCount[sender] = { count: 1, time: Date.now() };
      }

      // Si moun nan depase limit
      if (spamCount[sender].count >= SPAM_LIMIT) {
        await sock.sendMessage(from, {
          text: `⚠️ *@${sender.split("@")[0]}*, w ap spam gwoup la! Ou pral retire.`,
          mentions: [sender],
        });

        // Kick user la
        await sock.groupParticipantsUpdate(from, [sender], "remove");
        delete spamCount[sender];
      }
    }
  }
});


> [!CAUTION]
> NOTICE OF BREAKING CHANGE.
> 
> As of 7.0.0, multiple breaking changes were introduced into the library.
> 
> Please check out https://whiskey.so/migrate-latest for more information.

Baileys is a WebSockets-based TypeScript library for interacting with the WhatsApp Web API.

Join the WhiskeySockets community via the link: https://whiskey.github.com


> [!IMPORTANT]
> I made a survey for users of the project to ask questions, and provide Baileys valuable insights regarding its users. I will be publishing the results of this form (after filtering) as well so we can study and understand where we need to work.
> 
> The survey is anonymous and requires no personal info at all. You are required to sign-in with Google to keep responses to one person. You are able to edit your response after you submit. The deadline for this form is September 30, 2025.
> 
> I encourage you to put the effort, all it takes is 5-10 minutes and you get to ask me any questions you have.
> 
> \- Rajeh (purpshell)
> 
> Fill in the survey via the link: https://whiskey.so/survey




# Usage & Guide

> [!IMPORTANT]
> The new guide is a work in progress. Expect missing pages/content. [Report missing or incorrect content.](https://github.com/WhiskeySockets/baileys.wiki-site/issues/new)
> 
> **You can still access the old guide here:** [README.md](https://github.com/WhiskeySockets/Baileys/tree/master/README.md), or the [NPM homepage](https://npmjs.com/package/baileys).

The new guide is posted at https://baileys.wiki .

# Sponsor
> [!TIP]
> If you'd like to financially support this project, you can do so by supporting the current maintainer [here](https://purpshell.dev/sponsor).

# Disclaimer
> [!CAUTION]
> This project is not affiliated, associated, authorized, endorsed by, or in any way officially connected with WhatsApp or any of its subsidiaries or its affiliates.
> The official WhatsApp website can be found at whatsapp.com. "WhatsApp" as well as related names, marks, emblems and images are registered trademarks of their respective owners.
>
> The maintainers of Baileys do not in any way condone the use of this application in practices that violate the Terms of Service of WhatsApp. The maintainers of this application call upon the personal responsibility of its users to use this application in a fair way, as it is intended to be used.
> Use at your own discretion. Do not spam people with this. We discourage any stalkerware, bulk or automated messaging usage.

# License
Copyright (c) 2025 Rajeh Taher/WhiskeySockets

Licensed under the MIT License:
Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.

Thus, the maintainers of the project can't be held liable for any potential misuse of this project.import makeWASocket, { useMultiFileAuthState } from "@whiskeysockets/baileys";

const startBot = async () => {
  const { state, saveCreds } = await useMultiFileAuthState("auth");
  const sock = makeWASocket({ auth: state });

  sock.ev.on("creds.update", saveCreds);
  sock.ev.on("messages.upsert", async (m) => {
    const msg = m.messages[0];
    if (!msg.message) return;

    const from = msg.key.remoteJid;
    const text = msg.message.conversation || msg.message.extendedTextMessage?.text;

    if (text === "!menu") {
      await sock.sendMessage(from, { text: "📋 Meni Bot la:\n!tagall\n!ban\n!promote\n!demote\n!help" });
    }

    if (text === "!tagall") {
      const metadata = await sock.groupMetadata(from);
      const mentions = metadata.participants.map(p => p.id);
      const names = mentions.map(m => "@" + m.split("@")[0]).join(" ");
      await sock.sendMessage(from, { text: `👥 Tag tout moun:\n${names}`, mentions });
    }
  });
};

startBot();anti-spam
anti-ban<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8" />
    /title>
    <style>
      body { font-family: sans-serif; text-align: center; padding: 20px; }
      #chat { border: 1px solid #ccc; padding: 10px; height: 200px; overflow-y: scroll; margin-bottom: 10px; }
    </style>
  </head>
  <body>
    <h2>Byenveni nan Chatbot Kreyòl mwen 🤖</h2>
    <div id="chat"></div>
    <input id="msg" placeholder="Ekri mesaj ou..." />
    <button onclick="send()">Voye</button>

    <script>
      function send() {
        let msg = document.getElementById("msg").value;
        let chat = document.getElementById("chat");
        chat.innerHTML += "<p><b>Ou:</b> " + msg + "</p>";
        // Repons senp bot la
        let reply = "Bot: Mwen resevwa '" + msg + "'";
        chat.innerHTML += "<p>" + reply + "</p>";
        document.getElementById("msg").value = "";
      }
    </script>
  </body>
</html>
