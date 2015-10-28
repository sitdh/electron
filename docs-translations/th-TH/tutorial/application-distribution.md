# การกระจายแอปพลิเคชัน

การกระจายแอปของคุณด้วย Electron นั้น ก่อนอื่นไดเรคทอรี่ที่บรรจุแอปนั้นต้องตั้งชื่อ `app` เสียก่อน
และวางไว้ภายใต้ไดเรคทอรี่ทรัพยากร (resources directory) ของ Electron (สำหรับ OS X
จะอยู่ที่ `Electron.app/Contents/Resources/` ใน Linux และ Windows จะอยู่ที่ `resources/`)
ดังนี้:

ใน OS X:

```text
electron/Electron.app/Contents/Resources/app/
├── package.json
├── main.js
└── index.html
```

ใน Windows และ Linux:

```text
electron/resources/app
├── package.json
├── main.js
└── index.html
```

จากนั้นเรียกใช้ `Electron.app` (หรือ `electron` ใน Linux และ `electron.exe` ใน Windows),
Electron จะเริ่มต้นการทำงานแอปของคุณ ซึ่งไดเรคทอรี่ `electron` นี้จะนำไปใช้เพื่อแจกแจ่ายให้
กับผู้ใช้งานต่อไป

## บรรจุแอปของคุณใส่ในไฟล์

จากส่วนที่ใช้ส่งมอบแอปของคุณด้วยการคัดลอกไฟล์ทรัพยากรทั้งหมด
นอกจากการส่งมอบแอปด้วยการคัดลอกไฟล์ทรัพยากรทั้งหมดออกไปแล้ว คุณยังสามารถบรรจุแอปของคุณ
ใส่ไปใน [asar](https://github.com/atom/asar) เพื่อซ่อนซอร์สโค้ดจากผู้ใช้งาน

การใช้ `asar` บีบอัดข้อมูลเพื่อแทนที่โฟลเดอร์ `app` จำเป็นต้องเปลี่ยนชื่อชุดบันทึกข้อมูลเป็น `app.asr`
และวางใส่ไว้ภายใต้ไดเรคทอรี่ทรัพย์ยากรของ Electron ดังเช่นตัวอย่างด้านล่าง และ Electron จะ
พยายามอ่านข้อมูลชุดบันทึกข้อมูลและเริ่มต้นการทำงานจะชุดข้อมูลที่ว่า

ใน OS X:

```text
electron/Electron.app/Contents/Resources/
└── app.asar
```

ใน Windows และ Linux:

```text
electron/resources/
└── app.asar
```

รายละเอียดเพื่อเติมศึกษาได้จาก [การบรรจุแอปพลิเคชัน](application-packaging.md)

## การปรับอัตลักษณ์และดาวน์โหลดไบนาร่ี

หลังจากพัฒนาแอปของคุณใน Electron เรียบร้อยแล้ว คุณอาจจะอยากเปลี่ยนอัตลักษณ์จาก Electron
ให้เป็นไปตามที่ต้องการก่อนที่จะส่งมอบให้กับผู้ใช้งาน

### Windows

ไฟล์ `electron.exe` นั้นสามารถเปลี่ยนชื่อได้ตามที่ต้องการ และแก้ไขไอคอนรวมถึงข้อมูลอื่นๆ
ที่ต้องการ ด้วยเครื่องมืออย่างเช่น [rcedit](https://github.com/atom/rcedit) หรือ [ResEdit](http://www.resedit.net).

### OS X

`Electron.app` นั้นสามารถเปลี่ยนเป็นชื่อตามที่คุณต้องการได้ทันที่ นอกจากนั้นยังก็ให้
เปลี่ยนค่าของ `CFBundleDisplayName`, `CFBundleIdentifier` และ `CFBundleName`
ในไฟล์ดังต่อไปนี้:

* `Electron.app/Contents/Info.plist`
* `Electron.app/Contents/Frameworks/Electron Helper.app/Contents/Info.plist`

นอกจากนั้นแล้วก็ควรเปลี่ยนชื่อตัวช่วยแอป (helper app) เพื่อหลีกเลี่ยงการแสดงผล `
Electron Helper` ในตัวตรวจสอบกิจกรร (Activity Monitor) แต่ก็ควรแน่ใจว่าได้เปลี่ยนชื่อ
ตัวเรียกใช้งานตัวช่วยแอปด้วย

โครงสร้างของการเปลี่ยนชื่อเป็นดังนี้:

```
MyApp.app/Contents
├── Info.plist
├── MacOS/
│   └── MyApp
└── Frameworks/
    ├── MyApp Helper EH.app
    |   ├── Info.plist
    |   └── MacOS/
    |       └── MyApp Helper EH
    ├── MyApp Helper NP.app
    |   ├── Info.plist
    |   └── MacOS/
    |       └── MyApp Helper NP
    └── MyApp Helper.app
        ├── Info.plist
        └── MacOS/
            └── MyApp Helper
```

### Linux

`electron` นั้นสามารถเปลี่ยนชื่อได้ตามที่ชื่นชอบ

## การปรับอัตลักษณ์ ด้วยการสร้าง Electron ใหม่จากแหล่งซอร์สโค้ด

อีกแนวทางหนึ่งในการปรับอัตลักษณ์ของ Electron คือ การเปลี่ยนชื่อผลิตภัณฑ์ และสร้างใหม่
จากซอร์สโค้ด ซึ่งวิธีนี้จำเป็นจะต้องเปลี่ยนแปลงค่าในไฟล์ `atom.gyp` และต้องเริ่ม
สร้างใหม่ตั้งแต่ต้นอีกครั้ง

### grunt-build-atom-shell

ในกรณีที่ดึงซอร์สโค้ดของ Electron เพื่อสร้างใหม่ด้วยตัวเองนั้นสลับซับซ้อน จึงได้มีการสร้าง
งานของ Grunt เอาไว้ เพื่อช่วยจัดการกระบวนการเหล่านี้โดยอัตโนมัติ:
[grunt-build-atom-shell](https://github.com/paulcbetts/grunt-build-atom-shell).

ในการสร้างใหม่จากซอร์สโค้ดนั้น งานของจะคอยดูแลการแก้ไขไฟล์ `.gyp` โดยอัตโนมัติ  จากนั้น
ก็จะสร้างส่วนต่อขยายหลักของแอปเพื่อให้ตรงกับชื่อเรียกใช้ใหม่
