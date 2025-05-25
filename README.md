# Fake Mute/Deafen Status Script for Discord (Browser Console)

<div align="center">
   
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?style=flat-square)](https://opensource.org/licenses/MIT)
[![GitHub Repo stars](https://img.shields.io/github/stars/Nattapat2871/fakemutediscord?style=flat-square)](https://github.com/Nattapat2871/fakemutediscord/stargazers)
![Visitor Badge](https://api.visitorbadge.io/api/VisitorHit?user=Nattapat2871&repo=fakemutediscord&countColor=%237B1E7A&style=flat-square) 

</div>

<p align= "center">
        <b>English</b>　<a href="/README_TH.md">ภาษาไทย</a>

## 📜 Description
This script allows you to display a "fake" muted or deafened status to other users in Discord voice channels when using Discord in your web browser. It provides a user interface, triggered by a floating panel, to toggle these fake states by intercepting and modifying WebSocket messages.

**Created by:** Nattapat2871 ([https://github.com/nattapat2871](https://github.com/nattapat2871))

## ✨ Features
* **Fake Mute:** Appear muted to other users.
* **Fake Deafen:** Appear deafened to other users.
* **Floating UI Trigger:** A discreet floating panel (initially showing "❌🎧fake mute") in the bottom-right corner of the Discord web app.
* **Pop-up Control Panel:** Clicking the trigger panel opens a main control panel with:
    * Toggle buttons for "Fake Mute" and "Fake Deafen".
    * A link to your specified GitHub profile/repository.
* **Click-Away Dismiss:** The main control panel closes automatically when you click anywhere outside of it.
* **WebSocket Interception:** Directly modifies outgoing voice state packets to achieve the fake status.

## ⚠️ Important Disclaimer & Warning
* **USE THIS SCRIPT WITH CAUTION AND AT YOUR OWN RISK.**
* Modifying the Discord client's behavior or network traffic can have unintended consequences and might lead to unexpected issues.
* Using scripts that alter client behavior or network data **may be against Discord's Terms of Service.** While the risk can vary, using such scripts could potentially lead to warnings or account suspension.
* This script is provided for **educational and experimental purposes only.** The author(s) are not responsible for any issues, damages, or account actions that may arise from its use.
* **Crucial Limitation:** While this script makes *other users see you* as muted or deafened according to your "fake" setting, your Discord client will still *actually* mute your microphone or deafen your audio output locally.


## 🛠️ Prerequisites
* A modern web browser (e.g., Chrome, Firefox, Edge) capable of running Discord (discord.com).
* Access to the browser's Developer Tools console.

## 🚀 How to Use (Installation)
1.  **Copy the Script:** Copy the entire JavaScript code provided (the one that starts with `(() => {` and includes WebSocket interception).


```Javascript 
(() => {


  /*
   * This script was created by Nattapat2871 (https://github.com/nattapat2871).
   *
   * Released under the MIT License.
   *
   * PLEASE USE WITH CAUTION AND AT YOUR OWN RISK.
   * Modifying client behavior can have unintended consequences
   * and may potentially be against Discord's Terms of Service.
   * This script is intended for educational and experimental purposes.
   */ 



    // --- STATE ---
    let spoofMute = false;
    let spoofDeafen = false;
    let isMainUIVisible = false;

    // --- UI Elements ---
    let triggerPanel = null;        
    let mainUIPopup = null;         
    let fakeMuteButton = null;
    let fakeDeafenButton = null;
    let githubLinkElement = null;

    // --- WebSocket Interception ---
    const originalWebSocketSend = WebSocket.prototype.send;
    WebSocket.prototype.send = function (data) {
        let modifiedData = data;
        try {
            if (typeof data === "string") {
                const jsonPayload = JSON.parse(data);
                if (jsonPayload && jsonPayload.op === 4 && jsonPayload.d) { // Opcode 4: Voice State Update
                    let changed = false;
                    if (typeof jsonPayload.d.self_mute === "boolean" && jsonPayload.d.self_mute !== spoofMute) {
                        jsonPayload.d.self_mute = spoofMute;
                        changed = true;
                    }
                    if (typeof jsonPayload.d.self_deaf === "boolean" && jsonPayload.d.self_deaf !== spoofDeafen) {
                        jsonPayload.d.self_deaf = spoofDeafen;
                        changed = true;
                    }
                    if (changed) {
                        modifiedData = JSON.stringify(jsonPayload);
                        console.log("%c[FakeStatusFloating] Spoofing voice state:", "color: orange;", jsonPayload.d);
                    }
                }
            }
        } catch (err) {
            console.warn("[FakeStatusFloating] Error processing WebSocket data for spoofing:", err, "Original data:", data);
        }
        return originalWebSocketSend.call(this, modifiedData);
    };

    function updateMainUIButtonDisplay() {
        if (fakeMuteButton) {
            fakeMuteButton.textContent = `🎤 Mute: ${spoofMute ? "ON" : "OFF"}`;
            fakeMuteButton.style.backgroundColor = spoofMute ? "#f04747" : "#43b581";
        }
        if (fakeDeafenButton) {
            fakeDeafenButton.textContent = `🎧 Deafen: ${spoofDeafen ? "ON" : "OFF"}`;
            fakeDeafenButton.style.backgroundColor = spoofDeafen ? "#f04747" : "#43b581";
        }
    }

    function createElements() {
        // --- สร้าง Trigger Panel ---
        if (!document.getElementById("fake-status-trigger-panel")) {
            triggerPanel = document.createElement("div");
            triggerPanel.id = "fake-status-trigger-panel";
            triggerPanel.style.position = "fixed";
            triggerPanel.style.bottom = "20px";
            triggerPanel.style.right = "20px";
            triggerPanel.style.padding = "8px 12px";
            triggerPanel.style.backgroundColor = "rgba(47, 49, 54, 0.75)"; 
            triggerPanel.style.color = "#dcddde";
            triggerPanel.style.borderRadius = "8px";
            triggerPanel.style.cursor = "pointer";
            triggerPanel.style.zIndex = "10001";
            triggerPanel.style.fontFamily = "Whitney, 'Helvetica Neue', Helvetica, Arial, sans-serif";
            triggerPanel.style.fontSize = "13px";
            triggerPanel.style.boxShadow = "0 2px 5px rgba(0,0,0,0.2)";
            triggerPanel.textContent = "❌🎧fake mute"; // ข้อความบน Trigger
            triggerPanel.title = "Toggle Fake Status Panel";
            triggerPanel.onclick = (event) => {
                event.stopPropagation();
                toggleMainUIPopup();
            };
            document.body.appendChild(triggerPanel);
        }

        // --- สร้าง Main UI Popup  ---
        if (!document.getElementById("fake-status-main-popup-container")) {
            mainUIPopup = document.createElement("div");
            mainUIPopup.id = "fake-status-main-popup-container";
            mainUIPopup.style.position = "fixed";
            mainUIPopup.style.bottom = "60px"; 
            mainUIPopup.style.right = "20px";
            mainUIPopup.style.width = "230px";
            mainUIPopup.style.backgroundColor = "var(--background-floating, #18191c)";
            if (mainUIPopup.style.backgroundColor === "") mainUIPopup.style.backgroundColor = "#1e1f22"; 
            mainUIPopup.style.padding = "12px";
            mainUIPopup.style.borderRadius = "8px";
            mainUIPopup.style.boxShadow = "var(--elevation-stroke), var(--elevation-high)";
            if (mainUIPopup.style.boxShadow === "") mainUIPopup.style.boxShadow = "0 0 0 1px rgba(4,4,5,0.15), 0 8px 16px rgba(0,0,0,0.24)";
            mainUIPopup.style.zIndex = "10002"; 
            mainUIPopup.style.color = "var(--text-normal, #dcddde)";
            mainUIPopup.style.fontFamily = "Whitney, 'Helvetica Neue', Helvetica, Arial, sans-serif";
            mainUIPopup.style.fontSize = "14px";
            mainUIPopup.style.display = "none"; 
            mainUIPopup.style.flexDirection = "column";
            mainUIPopup.style.gap = "10px";

            const title = document.createElement("div");
            title.innerHTML = "❌ Fake 🎤 Mute / 🎧 Deafen Controls";
            title.style.fontWeight = "600";
            title.style.textAlign = "center";
            title.style.borderBottom = "1px solid var(--background-modifier-accent, #4f545c)";
            title.style.paddingBottom = "8px";
            title.style.marginBottom = "5px";
            mainUIPopup.appendChild(title);

            fakeMuteButton = document.createElement("button");
            fakeMuteButton.style.padding = "8px 12px";
            fakeMuteButton.style.border = "none";
            fakeMuteButton.style.borderRadius = "5px";
            fakeMuteButton.style.cursor = "pointer";
            fakeMuteButton.style.transition = "background-color 0.2s ease, color 0.2s ease";
            fakeMuteButton.style.color = "white";
            fakeMuteButton.onclick = (e) => {
                e.stopPropagation();
                spoofMute = !spoofMute;
                updateMainUIButtonDisplay();
            };
            mainUIPopup.appendChild(fakeMuteButton);

            fakeDeafenButton = document.createElement("button");
            fakeDeafenButton.style.padding = "8px 12px";
            fakeDeafenButton.style.border = "none";
            fakeDeafenButton.style.borderRadius = "5px";
            fakeDeafenButton.style.cursor = "pointer";
            fakeDeafenButton.style.transition = "background-color 0.2s ease, color 0.2s ease";
            fakeDeafenButton.style.color = "white";
            fakeDeafenButton.onclick = (e) => {
                e.stopPropagation();
                spoofDeafen = !spoofDeafen;
                updateMainUIButtonDisplay();
            };
            mainUIPopup.appendChild(fakeDeafenButton);

            githubLinkElement = document.createElement("a");
            githubLinkElement.href = `https://github.com/nattapat2871/fakemuteDiscord`;
            githubLinkElement.target = "_blank";
            githubLinkElement.rel = "noopener noreferrer";
            githubLinkElement.style.display = "flex";
            githubLinkElement.style.alignItems = "center";
            githubLinkElement.style.justifyContent = "center";
            githubLinkElement.style.textDecoration = "none";
            githubLinkElement.style.color = "var(--text-link, #0096cf)";
            githubLinkElement.style.marginTop = "5px";
            githubLinkElement.style.fontSize = "12px";
            githubLinkElement.innerHTML = `
                <svg width="16" height="16" viewBox="0 0 16 16" fill="currentColor" style="margin-right: 6px;">
                    <path fill-rule="evenodd" d="M8 0C3.58 0 0 3.58 0 8c0 3.54 2.29 6.53 5.47 7.59.4.07.55-.17.55-.38 0-.19-.01-.82-.01-1.49-2.01.37-2.53-.49-2.69-.94-.09-.23-.48-.94-.82-1.13-.28-.15-.68-.52-.01-.53.63-.01 1.08.58 1.23.82.72 1.21 1.87.87 2.33.66.07-.52.28-.87.51-1.07-1.78-.2-3.64-.89-3.64-3.95 0-.87.31-1.59.82-2.15-.08-.2-.36-1.02.08-2.12 0 0 .67-.21 2.2.82.64-.18 1.32-.27 2-.27.68 0 1.36.09 2 .27 1.53-1.04 2.2-.82 2.2-.82.44 1.1.16 1.92.08 2.12.51.56.82 1.27.82 2.15 0 3.07-1.87 3.75-3.65 3.95.29.25.54.73.54 1.48 0 1.07-.01 1.93-.01 2.2 0 .21.15.46.55.38A8.013 8.013 0 0016 8c0-4.42-3.58-8-8-8z"></path>
                </svg>
                My GitHub
            `;
            mainUIPopup.appendChild(githubLinkElement);
            document.body.appendChild(mainUIPopup);
            updateMainUIButtonDisplay(); 
        }
    }

    function toggleMainUIPopup() {
        isMainUIVisible = !isMainUIVisible;
        if (mainUIPopup && triggerPanel) {
            if (isMainUIVisible) {
                mainUIPopup.style.display = "flex";
                triggerPanel.style.display = "none"; 
                document.addEventListener("mousedown", handleClickOutside);
            } else {
                mainUIPopup.style.display = "none";
                triggerPanel.style.display = "block"; 
                document.removeEventListener("mousedown", handleClickOutside);
            }
        }
    }

    function handleClickOutside(event) {
        if (mainUIPopup && !mainUIPopup.contains(event.target) &&
            triggerPanel && !triggerPanel.contains(event.target) && 
            isMainUIVisible) {
            toggleMainUIPopup();
        }
    }

    function init() {
        if (window.fakeStatusFloatingScriptLoaded) {
            console.log("[FakeStatusFloating] Script already loaded. Destroying old instance first.");
            if (typeof window.destroyFakeStatusFloatingScript === 'function') {
                window.destroyFakeStatusFloatingScript();
            }
        }

        createElements(); 

        console.log("%c[FakeStatusFloating] ❌🎧fake mute/deafen script ready.", "color: lime; font-weight: bold;");
        console.log("[FakeStatusFloating] Click the '❌🎧fake mute' panel in the bottom-right to open controls.");
        console.log("[FakeStatusFloating] Remember: This spoofs your status to others, but you will be muted/deafened locally.");

        window.fakeStatusFloatingScriptLoaded = true;
        window.destroyFakeStatusFloatingScript = () => {
            WebSocket.prototype.send = originalWebSocketSend;
            if (mainUIPopup) mainUIPopup.remove();
            if (triggerPanel) triggerPanel.remove();
            document.removeEventListener("mousedown", handleClickOutside);
            delete window.fakeStatusFloatingScriptLoaded;
            delete window.destroyFakeStatusFloatingScript;
            console.log("[FakeStatusFloating] Script destroyed. WebSocket send restored, UI removed.");
        };
    }


    init();

})();
  
```

2.  **Open Discord in your Web Browser:** Navigate to `https://discord.com/app`.
3.  **Open Developer Tools:**
    * Typically, press `F12`.
    * Or, right-click anywhere on the page and select "Inspect" or "Inspect Element".
    * Then, navigate to the **"Console"** tab within the Developer Tools.
4.  **Paste and Run:** Paste the entire script into the browser's console and press `Enter`.
5.  **Using the UI:**
    * A small floating panel labeled "❌🎧fake mute" should appear in the bottom-right corner of the Discord web page.
    * Click this panel to open the main controls pop-up.
    * Use the "Mute" and "Deafen" buttons within the pop-up to toggle your desired fake status.
    * Click anywhere outside the main control pop-up to close it (the trigger panel will reappear).

## 🔧 How It Works (Briefly)
* **User Interface (UI):** The script dynamically creates and injects HTML elements directly into the Discord web page to form the floating trigger panel and the main control pop-up.
* **Status Spoofing (WebSocket Interception):**
    * The script intercepts the `send` method of the global `WebSocket.prototype`.
    * When Discord attempts to send a voice state update (Opcode 4 packet), the script parses the JSON data.
    * It then modifies the `self_mute` and `self_deaf` fields in this packet to match the "fake" states selected by the user before the packet is actually sent to Discord's servers.

## 🛑 How to Stop/Remove the Script
To completely stop the script, remove its UI elements, and restore the original WebSocket functionality for the current session:
1.  Open the browser's Developer Tools console (F12, then Console tab).
2.  Type the following command into the console and press `Enter`:
    ```javascript
    destroyFakeStatusFloatingScript();
    ```
3.  The UI elements should disappear, and the WebSocket interception should be removed for new WebSocket messages in that session.
4.  **To fully ensure all modifications are gone, simply reload the Discord web page (e.g., press `Ctrl+R` or `F5`).** Since this is a console script, its effects are generally limited to the current page session and do not persist after a full page reload unless re-injected.

## 📄 License
This script is released under the **MIT License**

*(You can create a separate file named `LICENSE` in your GitHub repository and paste the full text of the MIT License there.)*

## 🐛 Troubleshooting / Known Issues
* **Script Doesn't Work / Status Not Spoofed:**
    * **Discord Updates:** Discord can change its WebSocket message structure or client-side logic, which might break the script's ability to correctly identify or modify voice state packets. The Opcode 4 and `self_mute`/`self_deaf` fields have been relatively stable but are not guaranteed to remain unchanged.
    * **Browser Console Issues:** Ensure you are pasting the script correctly without any leading/trailing characters or syntax errors from the copy-paste process.
    * **Other Extensions/Scripts:** Other browser extensions or user scripts might interfere with WebSocket communication or DOM manipulation. Try disabling them if you encounter issues.
* **UI Conflicts:** The injected UI might, in rare cases, conflict with Discord's own UI elements or other browser extensions that modify the page.
* **Console Errors:** Pay attention to any errors logged in the browser's console, as they can provide clues about what might be going wrong.

---
*This script directly interacts with network data and modifies a live application's behavior. Use it responsibly and be aware of the potential risks.*
