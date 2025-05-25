# 🤖 สคริปต์ Mute/Deafen ปลอมๆ ใน Discord (สำหรับใช้ใน Console ของเบราว์เซอร์)

<div align="center">

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?style=flat-square)](https://opensource.org/licenses/MIT)
[![GitHub Repo stars](https://img.shields.io/github/stars/Nattapat2871/fakemutediscord?style=flat-square)](https://github.com/Nattapat2871/fakemutediscord/stargazers)
![Visitor Badge](https://api.visitorbadge.io/api/VisitorHit?user=Nattapat2871&repo=fakemutediscord&countColor=%237B1E7A&style=flat-square)

</div>

<p align= "center">
        　<a href="/README.md">English</a>       <b>ภาษาไทย</b>

## 📜 สคริปต์นี้คืออะไร?
สคริปต์นี้จะช่วยให้เราแกล้งทำเป็น **ปิดไมค์ (Mute)** หรือ **ปิดหูฟัง (Deafen)** ให้เพื่อนๆ ในห้องเสียงของ Discord เห็น เวลาที่เราใช้ Discord ผ่านหน้าเว็บในคอมพิวเตอร์ มันจะมีปุ่มเล็กๆ ลอยขึ้นมา (หน้าตาประมาณนี้: "❌🎧fake mute") ให้เรากดเพื่อเปิด/ปิดสถานะปลอมๆ ที่ว่านี้ โดยสคริปต์จะแอบไปแก้ไขข้อความที่ Discord ส่งไปบอกเพื่อนๆ นั่นเอง

**สร้างโดย:** Nattapat2871 ([https://github.com/nattapat2871](https://github.com/nattapat2871))


## ✨ ทำอะไรได้บ้าง?
* **แกล้งปิดไมค์ (Fake Mute):** ทำให้เพื่อนๆ เห็นว่าเราปิดไมค์อยู่
* **แกล้งปิดหูฟัง (Fake Deafen):** ทำให้เพื่อนๆ เห็นว่าเราปิดหูฟังอยู่
* **ปุ่มลอยๆ สำหรับกดเปิด:** จะมีปุ่มเล็กๆ ลอยอยู่ที่มุมขวาล่างของหน้าจอ Discord (มีข้อความว่า "❌🎧fake mute")
* **หน้าต่างควบคุมเล็กๆ:** พอกดปุ่มลอยๆ ก็จะมีหน้าต่างเล็กๆ เด้งขึ้นมาให้เราเลือกเปิด/ปิด "แกล้งปิดไมค์" หรือ "แกล้งปิดหูฟัง" และมีลิงก์ไป GitHub ของผู้สร้างด้วย
* **กดข้างนอกแล้วหน้าต่างปิดเอง:** ถ้าเรากดที่อื่นที่ไม่ใช่หน้าต่างควบคุมเล็กๆ นั้น มันก็จะปิดตัวเองไป จะได้ไม่เกะกะ
* **แก้ไขข้อมูลที่ส่งไปให้เพื่อน (WebSocket Interception):** สคริปต์นี้จะแอบไปเปลี่ยนข้อมูลสถานะปิดไมค์/ปิดหูฟังของเรา ก่อนที่ Discord จะส่งไปให้เพื่อนๆ เห็น

## ⚠️ คำเตือนสำคัญมากๆ โปรดอ่าน!
* **โปรดใช้สคริปต์นี้อย่างระมัดระวัง และยอมรับความเสี่ยงที่อาจเกิดขึ้นเองด้วยนะครับ**
* การแก้ไขการทำงานของโปรแกรม Discord หรือข้อมูลที่มันส่งไป อาจทำให้เกิดปัญหาแปลกๆ ที่เราไม่ได้ตั้งใจได้
* การใช้สคริปต์แบบนี้ **อาจจะผิดกฎของ Discord ได้** ถึงแม้ว่าความเสี่ยงจะมากน้อยต่างกันไป แต่ก็ **อาจจะทำให้โดน Discord เตือน หรือโดนแบน ID ได้นะ!**
* สคริปต์นี้ทำขึ้นมาเพื่อ **ให้ศึกษาและทดลองเล่นเท่านั้น** คนที่สร้างสคริปต์นี้จะไม่รับผิดชอบใดๆ ทั้งสิ้น หากเกิดปัญหาขึ้นจากการใช้งาน

## 🛠️ ของที่ต้องมีก่อนใช้
* คอมพิวเตอร์ที่มีโปรแกรมเปิดเว็บ (เช่น Chrome, Firefox, Edge) ที่สามารถเปิด Discord (discord.com) ได้
* ต้องรู้วิธีเปิดหน้าต่าง "เครื่องมือสำหรับนักพัฒนา" (Developer Tools console) ในโปรแกรมเปิดเว็บ

## 🚀 วิธีใช้ (การติดตั้ง)
1.  **คัดลอกโค้ดสคริปต์:** คัดลอกโค้ด JavaScript ทั้งหมดของสคริปต์นี้
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
3.  **เปิด Discord ในโปรแกรมเปิดเว็บ:** ไปที่เว็บ `https://discord.com/app`
4.  **เปิดหน้าต่างเครื่องมือสำหรับนักพัฒนา:**
    * ส่วนใหญ่จะกดปุ่ม `F12` บนคีย์บอร์ด
    * หรือ คลิกขวาตรงไหนก็ได้ในหน้าเว็บ แล้วเลือก "Inspect" หรือ "ตรวจสอบองค์ประกอบ"
    * จากนั้นให้หาแท็บที่ชื่อว่า **"Console"**
5.  **วางโค้ดแล้วกด Enter:** เอาโค้ดที่ก๊อปปี้มา วางลงในช่อง Console แล้วกดปุ่ม `Enter`
6.  **วิธีใช้ปุ่มต่างๆ:**
    * จะมีปุ่มเล็กๆ ลอยอยู่ เขียนว่า "❌🎧fake mute" ขึ้นมาที่มุมขวาล่างของหน้าจอ Discord
    * กดที่ปุ่มนี้เพื่อเปิดหน้าต่างควบคุมเล็กๆ ขึ้นมา
    * ใช้ปุ่ม "Mute" และ "Deafen" ในหน้าต่างนั้นเพื่อเปิด/ปิดสถานะปลอมๆ ที่เราต้องการ
    * ถ้าอยากปิดหน้าต่างควบคุม ก็แค่กดที่อื่นนอกหน้าต่างนั้น 

## 🔧 หลักการทำงาน
* **ส่วนปุ่มกดต่างๆ (User Interface - UI):** สคริปต์จะสร้างปุ่มและหน้าต่างเล็กๆ ขึ้นมาบนหน้าเว็บ Discord ให้เรากดได้
* **การส่งสถานะปลอม (WebSocket Interception):**
    * สคริปต์จะไป "ดัก" ข้อมูลที่ Discord กำลังจะส่งไปบอกเพื่อนๆ ผ่านช่องทางที่เรียกว่า WebSocket
    * พอ Discord จะส่งข้อมูลว่าเราปิดไมค์หรือปิดหูฟัง (ข้อมูลนี้เรียกว่า Opcode 4 packet) สคริปต์จะเข้าไปดูข้อมูลนั้น
    * แล้วมันก็จะเปลี่ยนค่า "ปิดไมค์" (`self_mute`) กับ "ปิดหูฟัง" (`self_deaf`) ให้เป็นไปตามที่เราเลือก "ปลอม" ไว้ ก่อนที่ข้อมูลนั้นจะถูกส่งไปจริงๆ

## 🛑 วิธีหยุดหรือลบสคริปต์
ถ้าอยากจะหยุดการทำงานของสคริปต์ ลบปุ่มต่างๆ ออก และทำให้ทุกอย่างกลับเป็นเหมือนเดิม (สำหรับตอนที่เปิดหน้าเว็บนั้นอยู่):
1.  เปิดหน้าต่างเครื่องมือสำหรับนักพัฒนา (F12 แล้วไปแท็บ Console)
2.  พิมพ์คำสั่งนี้ลงไปใน Console แล้วกด `Enter`:
    ```javascript
    destroyFakeStatusFloatingScript();
    ```
3.  ปุ่มต่างๆ ที่สคริปต์สร้างไว้ควรจะหายไป และการดักจับข้อมูลก็จะหยุดลง
4.  **เพื่อให้แน่ใจว่าทุกอย่างกลับเป็นปกติจริงๆ ให้ลองโหลดหน้าเว็บ Discord ใหม่ดูนะ** (เช่น กด `Ctrl+R` หรือ `F5`) เพราะสคริปต์ที่เราใส่ไปใน Console จะทำงานแค่ในตอนที่เราเปิดหน้านั้นอยู่ ถ้าโหลดใหม่มันก็จะหายไป (ยกเว้นเราจะใส่เข้าไปใหม่)

## 📄 สิทธิ์การใช้งาน
สคริปต์นี้ใช้สิทธิ์แบบ **MIT License**

## 🐛 ปัญหาที่อาจจะเจอ / ที่รู้แล้วว่ามี
* **สคริปต์ไม่ทำงาน / สถานะไม่ปลอม:**
    * **Discord อัปเดตบ่อย:** บางที Discord อาจจะเปลี่ยนวิธีการส่งข้อมูล หรือเปลี่ยนโค้ดข้างใน ทำให้สคริปต์ของเราหาข้อมูลที่จะไปแก้ไขไม่เจอ หรือแก้ไขแล้วไม่ได้ผล (โดยเฉพาะข้อมูล Opcode 4 และชื่อช่อง `self_mute`/`self_deaf` ถึงจะค่อนข้างคงที่ แต่ก็ไม่แน่เสมอไป)
    * **ปัญหาตอนวางโค้ดใน Console:** ดูให้ดีนะว่าก๊อปปี้โค้ดมาถูกทั้งหมด ไม่มีอะไรขาดหรือเกิน และไม่มีตัวอักษรแปลกๆ ติดมา
    * **ส่วนเสริมอื่นในเบราว์เซอร์อาจจะตีกัน:** ถ้าเราลงส่วนเสริม (Extension) อื่นๆ ในโปรแกรมเปิดเว็บ บางทีมันอาจจะไปรบกวนการทำงานของสคริปต์นี้ได้ ลองปิดส่วนเสริมอื่นดูก่อน
* **ปุ่มที่สร้างขึ้นอาจจะไปทับกับของ Discord:** ในบางกรณี ปุ่มที่เราสร้างขึ้นมาอาจจะไปอยู่ทับกับปุ่มอื่นๆ ของ Discord หรือของส่วนเสริมอื่นได้
* **ดู Error ใน Console:** ถ้ามีอะไรผิดพลาด ลองดูในหน้าต่าง Console นะ มันอาจจะมีข้อความ Error สีแดงๆ บอกใบ้ว่าเกิดอะไรขึ้น

---
*สคริปต์นี้เข้าไปยุ่งกับข้อมูลที่ส่งหากันและแก้ไขการทำงานของโปรแกรมที่กำลังใช้อยู่ โปรดใช้อย่างมีความรับผิดชอบและเข้าใจความเสี่ยงที่อาจเกิดขึ้นด้วยนะจ๊ะ
