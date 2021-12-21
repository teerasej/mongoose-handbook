
# Setup Project

อ้างอิงถึง [Mongo handbook](https://github.com/teerasej/mongodb-dba-workshop) 

# Setup Project



## 1. Download

สร้างโฟลเดอร์ใหม่ ชื่อ **my-mongoose**

รันคำสั่งด้านล่างใน Terminal ของโปรเจค เพื่อสร้างไฟล์ **package.json** 

```bash
npm init
```

โดยให้กด enter จนระบบยืนยันข้อมูล และได้ไฟล์ **package.json** มา

## 2. Install Package

เปิด Terminal ขึ้นมาใน Directory ของโปรเจค และรันคำสั่งด้านล่างทีละคำสั่ง

```bash
npm i mongoose
npm i express 
npm i -g nodemon
```

### Nodemon บน MacOS

สำหรับคำสั่งติดตั้ง nodemon บน macOS อาจจะต้องขึ้นต้นด้วยคำสั่ง sudo เช่นด้านล่าง 

```bash
sudo npm i -g nodemon
```

## 3. ทำการเปิดการทำงานของ MongoDB

- [ดูวิธีการติดตั้ง และรันใช้งานใน Windows](https://nextflow.in.th/2021/mongodb-5-windows-install-thai/)
- [ดูวิธีการติดตั้ง และรันใช้งานใน MacOS](https://nextflow.in.th/2017/install-and-start-mongodb-macos-osx-thai/)

## 4. สร้าง Web API

สร้างไฟล์ชื่อ **index.js** และเขียนกำหนด route ของ Web API ด้านล่าง

```js
// index.js
import express from 'express'

const app = express()
const port = 3000

// กำหนดใช้ตัว Web API แปลงข้อมูล JSON ที่ส่งมากับ Request ให้อยู่ในสภาพ JavaScript object ที่นำไปใช้งานได้
app.use(express.json())


// กำหนดการทำงานแบบ Asynchronous ให้ Web API 
main().catch(function(error){ console.log(error) })

async function main() {
   
    // Route URL สำหรับทดสอบ Web API 
    app.get('/', function (request, response) {
        response.send('hello')
    })

    // Route URL สำหรับการเพิ่ม User เข้าระบบ 
    app.post('/users', async function(request, response){
        console.log(request.body)
        response.send('ok CREATE')
    })

    // Route URL สำหรับการค้นหา User Document ด้วย email
    app.get('/users/:email', async function(request, response){
        console.log(request.params.email)
        response.send('ok SEARCH')
    })

    // Route URL สำหรับการอัพเดต password โดยอิงจาก Email
    app.patch('/users', async function(request, response){
        console.log(request.body)
        response.send('ok UPDATE')
    })

    // Route URL สำหรับการค้นหา User Document ด้วย email
    app.delete('/users', async function(request, response){
        console.log(request.body)
        response.send('ok DELETE')
    })

    // เริ่มการทำงานของ Web API
    app.listen(port, function () {
        console.log('Server is running at http://localhost:' + port)
    })
}
```
## 5. รัน Web API ใช้งาน

รันคำสั่งด้านล่าง ใน Directory ของโปรเจค

```bash
nodemon index
```

- ถ้าต้องการปิดการทำงานของ nodemon ให้กลับไปที่ Terminal และใช้ปุ่มลัด Ctrl + C 
