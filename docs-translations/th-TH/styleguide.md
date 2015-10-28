# รูปแบบแนวทางเอกสารของ Electron

หาส่วนที่เหมาะสมกับงานของคุณ: [อ่านเอกสาร Electron](#reading-electron-documentation)
หรือ [เขียนเอกสาร Electron](#writing-electron-documentation)

## การเขียนเอกสาร Electron

มีหลายแนวทางที่เราใช้สร้างเอกสาร Electron

- มีหัวเรื่องที่ใช้ `h1` ได้เพียงตำแหน่งเดียวในหน้าเพจ
- ใช้ในกลุ่มของโค้ด `bash` แทน `cmd` (เพราะการเน้นสีไวยากรณ์)
- หัวเรื่องที่ใช้ `h1` ของเอกสารต้องตรงกับชื่อออบเจค (ตัวอย่างเช่น `browser-window` →
  `BrowserWindow`)
  - อย่างไรก็ดี ใช้ขีดกลางแยกชื่อไฟล์
- ไม่ควรใส่หัวเรื่อง ตามด้วยหัวเรื่องเลยทันที ใส่คำอธิบายอย่างน้อย 1-2 ประโยค
- หัวเรื่องของเมธอดจะหุ้มด้วย `code`
- หัวเรื่องของเหตุการณ์ (Event) จะหุ้มด้วยเครื่องหมาย 'quotation'
- ไม่ควรใส่รายการซ้อนกันมากกว่า 2 ชั้น (เพราะเป็นข้อจำกัดของตัวแปลง markdown)
- ใส่หัวเรื่องของส่วน: เหุตการณ์ เมธอดของคลาส (Class Methods)
  และ มธอดของอินสแตนซ์ (Instance Methods)
- ใช้ 'จะ' แทน 'น่าจะ' ในกรณีการอธิบายผลลัพธ์
- เหตุการณ์ และเมธอดจะใช้หัวเรื่องเป็น `h3`
- ค่าที่ต้องใส่ให้กับการดำเนินงาน (Optional arguments) จะเขียนอยู่ในรูปของ
  `function (required[, optional])`.
- ค่าที่ต้องใส่ให้กับการดำเนินงานจะแสดงเมื่ออยู่ในรายกร
- ความยาวบรรทัดจะตัดอยู่ที่ 80 ตัวอักษร
- เมธอดที่จำเพาะเจาะจงกับแพลตฟอร์มจะหมายเหตุไว้ด้วยตัวเองตามด้วยหัวเรื่องของเมธอด
 - ```### `method(foo, bar)` _OS X_```
- ควรจะใส่ 'ในกระบวนการ___' แทนที่จะเป็น 'บน'

### การแปลเอกสาร

การแปลเอกสาร Electron จะวางไว้ภายในไดเรคทอรี่ `docs-translations`

ใส่กลุ่มอื่นๆ (หรือส่วนของกลุ่มอื่น):

- สร้างไดเรคทอรี่ด้านใน และตั้งชื่อตามตัวย่อของภาษา
- ภายในำดเรคทอรี่ที่สร้างขึ้นมา ให้ทำสำเนาไดเรคทอรี่ `doc` และใช้ชื่อไฟล์และไดเรคทอรี่เดิม
- แปลไฟล์
- แก้เอกสาร `README.md` ที่อยู่ภายในไดเรคทอรี่ภาษาของคุณเพื่อเชื่อมโยงไปยังไฟล์ที่คุณแปลแล้ว
- เพิ่มลิงก์ไปยังไดเรคทอรี่การแปลของคุณไปยังเอกสาร [README](https://github.com/atom/electron#documentation-translations) หลักของ Electron

## อ่านเอกสาร Electron

นี่คือข้อแนะนำบางส่วนที่จะช่วยให้เข้าใจการใช้งานเอกสาร Electron

### เมธอด (Methods)

ตัวอย่างของเอกสาร [method](https://developer.mozilla.org/en-US/docs/Glossary/Method):

---

`methodName(required[, optional]))`

* `require` String, **required**
* `optional` Integer

---

ชื่อเมธอดจะตามด้วยค่าที่ค่าที่จะป้อนให้ ส่วนค่าที่ไม่จำเป็นจะระบุด้วยวงเล็บล้อมรอบค่านั้น ตลอดจนเครื่องหมายจุลภาคที่จำเป็นต้องคั่นระหว่างค่าที่ป้อนให้กับเมธอดเหล่านั้น

เมธอดด้านล่างแสดงข้อมูลของค่าภายในเมธอดที่ทำเครื่องหมายในแต่ละประเภท:

[`String`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String),
[`Number`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number),
[`Object`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object),
[`Array`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)
หรือประเภทที่กำหนดเองอย่างเช่น [`webContent`](api/web-content.md) ของ Electron

### เหตุการณ์ (Events)

ตัวอย่างของเอกสารเหตุการณ์ [event](https://developer.mozilla.org/en-US/docs/Web/API/Event):

---

Event: 'wake-up'

Returns:

* `time` String

---

เหตุการณ์คือตัวอักษรที่อยู่ด้านหลังเมธอดจับเหตุการณ์ (listener method) `.on`
หากเหตุการณ์นั้นคือค่า ประเภทของค่าเหล่านั้นจะถูกทำเครื่องหมายเอาไว้ หากคุณดักฟังและตอบสนอง
กับเหตุการณ์นั้น จะมีลักษณะดังเช่นโค้ดด้านล่าง:

```javascript
Alarm.on('wake-up', function(time) {
  console.log(time)
})
```
