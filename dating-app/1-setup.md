
# Setup Project

อ้างอิงถึง [Mongo handbook](https://github.com/teerasej/mongodb-dba-workshop) 

# Setup Project

ในที่นี้เราจะมีการดาวน์โหลดโครงสร้างของโปรเจค Web API ที่เตรียมไว้มาสร้างเพิ่มเติมในส่วนของการทำงานกับฐานข้อมูล

ก่อนเริ่ม ให้ทำการติดตั้ง TypeScript package ในเครื่องก่อนด้วย 

```bash
npm i -g typescript
```

## 1. Download

ดาวน์โหลด zip file หรือ clone repository มาจาก link ด้านล่างนี้

[teerasej/dating-web-api at starter-for-mongoose (github.com)](https://github.com/teerasej/dating-web-api/tree/starter-for-mongoose)

## 2. Setup Project

เปิด Terminal ขึ้นมาใน Directory ของโปรเจค และรันคำสั่ง

```bash
npm i 
```

## 3. ทำการเปิดการทำงานของ MongoDB

- [ดูวิธีการติดตั้ง และรันใช้งานใน Windows](https://nextflow.in.th/2021/mongodb-5-windows-install-thai/)
- [ดูวิธีการติดตั้ง และรันใช้งานใน MacOS](https://nextflow.in.th/2017/install-and-start-mongodb-macos-osx-thai/)

## 4. รัน Web API ใช้งาน

```bash
npm start
```

## 5. ทดสอบส่ง Request โดยใช้ CURL command

### POST

POST Request จะเป็นส่วนของการสร้างข้อมูล

```bash
curl -X POST http://localhost:3000/users -H 'Content-Type: application/json' -d '{"email":"training@nextflow.in.th","password":"111222"}'
```

### DELETE

DELETE Request จะเป็นส่วนของการสร้างลบข้อมูล โดยในที่นี้เราจะใช้ Document ID เป็นตัวอ้างอิง

```bash
curl -X DELETE http://localhost:3000/users -H 'Content-Type: application/json' -d '{ "id": "61c089efc246a7de85c98ce1" }'
```

### GET

GET Request จะเป็นส่วนของการค้นหาข้อมูล ในที่นี้จะใช้ email เป็นตัวค้นหา

```bash
curl -X GET http://localhost:3000/users/training@nextflow.in.th
```

### PATCH

PATCH Request จะเป็นส่วนของการอัพเดตรหัสผ่าน ในที่นี้จะใช้ email เป็นตัวค้นหา 

```bash
curl -X PATCH http://localhost:3000/users -H 'Content-Type: application/json' -d '{"email":"training@nextflow.in.th","password":"444555"}'
```