# Fake Mute/Deafen Status Script for Discord (Browser Console)

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
This script is released under the **MIT License** (as stated in the script's header comment).

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
