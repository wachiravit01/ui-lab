# แล็บทดสอบ UI: Robot Framework และ Cypress

repository นี้เก็บชุดทดสอบ UI อัตโนมัติสำหรับเว็บ **Selenium Web Form** และ **QuickPizza** เป็นส่วนหนึ่งของวิชา ITDS362 Software Quality Assurance and Testing

เครื่องมือที่เลือกทำในแล็บนี้ คือ
1. **Robot Framework** (Part 3)
2. **Cypress** (Part 4)

---

## โครงสร้างโฟลเดอร์และวิธีรันเทสต์

### 1. Robot Framework (`robot/`)
* **เนื้อหา:** เก็บเทสต์แบบ keyword-driven ได้แก่ `first.robot`, `web_form.robot` และ `pizza_scenario.robot` ใช้ `SeleniumLibrary` และไฟล์ resource แบบ Page Object (`resources/quickpizza.resource`) หลักฐานการรันเต็มรูปแบบอยู่ใน `robot/results/`
* **สิ่งที่ต้องมี:** Python 3.10 ขึ้นไป และ Google Chrome ที่ตรงกับเวอร์ชันของ ChromeDriver ที่ตั้งไว้ในตัวแปรสภาพแวดล้อม `CHROMEDRIVER` เปิดใช้งาน virtual environment (`.\.venv\Scripts\Activate.ps1` หรือ `source .venv/bin/activate`)
* **คำสั่งรัน:**
  ```powershell
  cd robot
  robot -d results/run1 -v BROWSER:headlesschrome pizza_scenario.robot
  ```

---

### 2. Cypress (`cypress/`)
* **เนื้อหา:** เก็บไฟล์ spec แบบ end-to-end (`cypress/e2e/web-form.cy.js` และ `cypress/e2e/pizza-scenario.cy.js`) พร้อม custom command (`cypress/support/commands.js`) และการดักจับ API ของ QuickPizza ด้วย `cy.intercept` วิดีโอและภาพหน้าจอของการรันเต็มรูปแบบอยู่ใน `cypress/cypress/videos/` และ `cypress/cypress/screenshots/`
* **สิ่งที่ต้องมี:** Node.js 22 ขึ้นไป และ npm รัน `npm install` ในโฟลเดอร์ `cypress/` ก่อน
* **คำสั่งรัน:**
  ```powershell
  cd cypress
  npx cypress run --browser chrome
  ```
