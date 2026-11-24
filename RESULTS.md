# ผลการทำแล็บ UI

ส่วนของเครื่องมือที่เลือกทำ:
- Part 3: Robot Framework
- Part 4: Cypress

## Part 0
- Step 0.1, เวอร์ชัน Chrome: 157.0.8103.39
- Step 0.3, เวอร์ชัน ChromeDriver: 157.0.8103.39
- Step 0.4, locator ของ Pizza, Please!: `button[name='pizza-please']` (หรือ `name=pizza-please`)
- Step 0.4, locator ของ Excluded tools: `css=select` (หรือ `select`)
- Step 0.4, locator ของ Must be vegetarian: `id=default-checkbox` (หรือ `#default-checkbox`)
- Step 0.4, locator ของ Custom Pizza Name: `input#pizza-name`
- Step 0.5, ชื่อพิซซ่า เครื่องมือ และแคลอรี่ จากการรันด้วยมือ: Lab Pizza 20261105-154729, Pizza cutter, 400 แคลอรี่ต่อชิ้น

| Step | การกระทำ | ผลที่เกิดขึ้นจริง | ผ่านหรือไม่ผ่าน |
|------|----------|-------------------|-----------------|
| 1 | เปิดหน้าแรกของ QuickPizza | หัวข้อแสดงข้อความ "Looking to break out of your pizza routine?" | ผ่าน |
| 2 | เปิดสวิตช์ Advanced | สวิตช์ Advanced เปิดอยู่ และมีช่อง Max Calories per Slice, Excluded tools, Must be vegetarian และ Custom Pizza Name ปรากฏขึ้น | ผ่าน |
| 3 | กรอกข้อจำกัดและชื่อ (600, Knife, ติ๊ก vegetarian, ชื่อพิซซ่า) | ช่องแสดงค่า 600, เลือก Knife ใน Excluded tools, ติ๊กช่อง vegetarian แล้ว และชื่อพิซซ่าคือ "Lab Pizza 20261105-154729" | ผ่าน |
| 4 | คลิก Pizza, Please! | ผลแนะนำแสดงชื่อพิซซ่า "Lab Pizza 20261105-154729", เครื่องมือ "Pizza cutter" (ไม่ใช่ Knife) และ 400 แคลอรี่ต่อชิ้น (<= 600) | ผ่าน |
| 5 | คลิก Love it! โดยไม่ล็อกอิน | คลิกปุ่ม "Love it!" โดยไม่ล็อกอิน แล้วข้อความ "Please log in first." แสดงใน #rate-result | ผ่าน |
| 6 | เปิด /admin และล็อกอินด้วย admin / admin | ไปที่ /admin และล็อกอินด้วย admin/admin หน้าแสดง "Latest pizza recommendations" | ผ่าน |
| 7 | หาชื่อพิซซ่าในรายการ | พบพิซซ่า "Lab Pizza 20261105-154729" ในรายการ พร้อมเครื่องมือ "Pizza cutter" ตรงกับ step 4 | ผ่าน |

## Part 3: Robot Framework
- robot --version: Robot Framework 7.5.1 (Python 3.14.7 on win32)
- Step 3.2, ข้อความ error เมื่อตั้งชื่อ title ผิด: Title should have been 'Web Form' but was 'Web form'.
- Step 3.8, รันครั้งที่ 1, ผล, เวลาที่ใช้ และชื่อพิซซ่า: PASS, 8.70 วินาที, Lab Pizza 20261105-154729
- Step 3.8, รันครั้งที่ 2, ผล, เวลาที่ใช้ และชื่อพิซซ่า: PASS, 8.44 วินาที, Lab Pizza 20261105-154805

## Part 4: Cypress
- node --version, npm --version และ npx cypress version: node v24.19.0, npm 11.17.0, npx cypress 16.2.1
- Step 4.3, ข้อความ error เมื่อตั้งชื่อ title ผิด: Timed out retrying after 10000ms: expected 'Web form' to equal 'Web Form'
- Step 4.6, เครื่องมือและแคลอรี่ในคำตอบของ POST /api/pizza: Tool: Scissors, Calories per slice: 400
- Step 4.7, บรรทัดในรายการ admin ที่แสดงพิซซ่าของเรา: Lab Pizza 20261105-155149 (tool=Scissors, ingredients_number=5) (newest)
- Step 4.8, รันครั้งที่ 1, ผล, ระยะเวลา และชื่อพิซซ่า: All specs passed! (2 passed), 00:10, Lab Pizza 20261105-155149
- Step 4.8, รันครั้งที่ 2, ผล, ระยะเวลา และชื่อพิซซ่า: All specs passed! (2 passed), 00:10, Lab Pizza 20261105-155301
- Step 4.9 (ไม่บังคับ), เบราว์เซอร์และผล: Microsoft Edge 156 (headless), All specs passed! (2 passed, ระยะเวลา 00:10)

## Part B
1. เทสต์หา "Max Calories per Slice" ผ่าน label ที่อยู่ก่อนหน้า แทนการใช้ id เพราะช่องกรอกตัวเลขทั้งสามช่องในหน้า QuickPizza ใช้ `id="floating_filled"` เหมือนกันหมด ตามมาตรฐาน HTML ค่า id ต้องไม่ซ้ำกันในเอกสาร การค้นด้วย `#floating_filled` จึงเจอ 3 element (`$$('#floating_filled').length` ได้ 3) และแยกช่องแคลอรี่ออกมาไม่ได้อย่างน่าเชื่อถือ เพื่อให้เทสต์ใช้ id ตรงๆ ได้ ผู้พัฒนาควรกำหนด id ที่ไม่ซ้ำให้แต่ละช่อง เช่น `id="max-calories-per-slice"` หรือเพิ่ม attribute สำหรับการทดสอบโดยเฉพาะ เช่น `data-testid="max-calories"` การเปลี่ยนนี้ช่วยผู้ทดสอบเพราะได้ locator ที่ชัดเจน เร็ว และทนทาน ไม่ขึ้นกับโครงสร้าง DOM หรือถ้อยคำของ label ที่อาจเปลี่ยนเมื่อแปลภาษาหรือออกแบบหน้าใหม่

2. Step 7 เทียบเครื่องมือที่แสดงในรายการของ admin กับเครื่องมือที่บันทึกไว้ตอนขอคำแนะนำของลูกค้า การตรวจนี้เป็น test oracle แบบ Consistency Check (หรือเรียกว่า cross-referencing / invariant oracle) ซึ่งตรวจว่าข้อมูลชิ้นเดียวกันที่แสดงในมุมมองต่างๆ ของระบบยังตรงกัน ข้อบกพร่องหนึ่งใน QuickPizza ที่การตรวจนี้จับได้ คือบั๊กการบันทึกหรือการแมปข้อมูลฝั่ง backend เช่น ระบบแนะนำเครื่องมือ `Scissors` แต่ขั้นตอนบันทึกลงฐานข้อมูลทำค่านี้หายไป เปลี่ยนเป็นค่าตั้งต้น เช่น `Knife` หรือไปบันทึกให้ session อื่น ในการรันของเรา ทั้งแบนเนอร์ผลแนะนำและรายการใน admin แสดง `tool=Scissors` ตรงกัน แสดงว่าข้อมูลสอดคล้องกันระหว่างระบบย่อย

3. QP-01 ตรวจแค่ว่าช่อง vegetarian ถูกติ๊ก และคำขอ HTTP ส่ง `vegetarian: true` แต่ไม่ได้ตรวจเลยว่าส่วนผสมที่แนะนำเป็นมังสวิรัติจริง หากจะตรวจบนหน้าเว็บ เทสต์ควรอ่านรายการ `<ul>` ใต้ `Ingredients:` ในบล็อกผลแนะนำ แล้วยืนยันว่าทุกรายการอยู่ในรายการวัตถุดิบมังสวิรัติที่กำหนด หรือยืนยันว่าไม่มีวัตถุดิบต้องห้าม (เช่น `Ham`, `Pepperoni`, `Bacon`, `Chicken`, `Beef`) อีกวิธีคือใช้ `cy.intercept` ของ Cypress ตรวจ JSON response โดยตรง เช่น `expect(response.body.pizza.vegetarian).to.be.true` ร่วมกับการตรวจ `response.body.pizza.ingredients` การทำเช่นนี้ต้องมีแหล่งอ้างอิง (oracle) ที่ระบุว่าวัตถุดิบใดเป็นมังสวิรัติจากสูตรของ QuickPizza หรือเข้าถึง response object ของ backend เพื่อตรวจ flag การจัดประเภท

4. Robot Framework รอด้วย keyword ของ SeleniumLibrary ที่เขียนเอง เช่น `Wait Until Element Is Visible` พร้อม timeout (`timeout=10s`) ส่วน Cypress retry ทุก `cy.get` และ assertion บน DOM โดยอัตโนมัติสูงสุด `defaultCommandTimeout: 10000` โดยไม่ต้องเขียนคำสั่งรอเอง การเขียน QP-01 ด้วย Cypress เร็วและลื่นกว่ามาก เพราะเป็น JavaScript ที่ต่อ method เป็น chain ได้ และดักคำขอด้วย `cy.intercept()` ได้ง่าย ขณะที่ Robot Framework ต้องระวังการย่อหน้าแบบตารางและแยก keyword ไปไว้ในไฟล์ resource ด้านความอ่านง่ายของผลลัพธ์ Cypress ให้วิดีโอ `.mp4` ที่เห็น Command Log คู่กับหน้าเว็บแบบเรียลไทม์ ส่วน Robot Framework ให้ `log.html` และ `report.html` ที่ขยายดูต้นไม้ของ keyword และภาพหน้าจอได้ละเอียด เวลาที่ใช้ในการรันจริง Robot Framework ใช้ประมาณ 8.5 วินาทีต่อรอบ และ Cypress รันทั้งชุด spec เสร็จใน 10 วินาที

5. สำหรับเทสต์ UI อัตโนมัติของ Project 2 ผมจะเลือก Cypress เป็นเครื่องมือหลัก เหตุผลแรก กลไกรอและ retry อัตโนมัติของ Cypress ลดความไม่เสถียรของเทสต์บนเว็บสมัยใหม่ที่ทำงานแบบ asynchronous ได้มาก โดยโค้ดไม่ต้องรกด้วยคำสั่งรอซ้ำๆ เหตุผลที่สอง Cypress มีการดักเครือข่ายในตัว (`cy.intercept`) ทำให้ตรวจ response จาก backend และจำลองกรณีขอบได้สะดวก อีกทั้งสร้างวิดีโอและภาพหน้าจอเป็นหลักฐานสำหรับส่งงานให้เองโดยอัตโนมัติ

6. ใน Robot Framework (Part 3) ถ้า QuickPizza เปลี่ยน locator ของช่องแคลอรี่ เราต้องแก้ที่เดียว คือตัวแปร `${MAX CALORIES}` ในไฟล์ `robot/resources/quickpizza.resource` (line 10) ไฟล์ resource ทำหน้าที่เป็นชั้น Page Object ที่ซ่อน locator ของ DOM และขั้นตอนทางเทคนิคไว้หลัง keyword ที่อ่านเข้าใจง่าย เช่น `Fill In The Pizza Request` เนื่องจากไฟล์เทสต์ `pizza_scenario.robot` เรียกแค่ keyword นี้และไม่เคยอ้างถึง XPath ดิบ การเปลี่ยนแปลงของหน้าเว็บจึงไม่กระทบไฟล์ scenario และค่าบำรุงรักษาต่ำ

7. ใน Cypress (Part 4) QP-01 ต้องเป็นเทสต์เดียว เพราะ QuickPizza ทำงานบน backend หลายเครื่อง และใช้ session cookie ผูกเบราว์เซอร์กับเซิร์ฟเวอร์เครื่องเดียวที่เก็บพิซซ่าของ session นั้น โดยการออกแบบ Cypress ล้าง cookie, browser storage และสถานะระหว่าง `it()` แต่ละบล็อกเพื่อให้เทสต์เป็นอิสระต่อกัน ถ้าแยก step 6 และ 7 ไปเป็นเทสต์ที่สอง cookie ของ session จะถูกล้าง การเข้าหน้า admin อาจไปที่ backend คนละเครื่องหรือ session ที่ไม่เกี่ยวข้อง ซึ่งไม่มีพิซซ่าที่สร้างใน Step 4 อยู่ในรายการ "Latest pizza recommendations" ทำให้ assertion หา element หมดเวลาและเทสต์ล้มเหลว
