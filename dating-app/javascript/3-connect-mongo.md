
# เชื่อมต่อฐานข้อมูลด้วย Mongoose

## 1. ตรวจสอบ database ใน MongoDB

ทดสอบใช้โปรแกรม client ในการดู database ที่มีอยู่ปัจจุบัน

## 2. เชื่อมต่อฐานข้อมูล 

เราจะทำการเชื่อมต่อฐานข้อมูลด้วย function `.connect()` 

```ts
//...

// import connect() 
import mongoose from 'mongoose'


main().catch(error => console.log(error))

async function main() {

    // เชื่อมต่อไปที่ database server และกำหนดชื่อฐานข้อมูล
    await connect('mongodb://localhost:27017/dating_app')

    const app = express();
```