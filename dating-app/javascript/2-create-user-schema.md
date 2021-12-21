
# สร้าง Schema สำหรับ User Document

สร้างไฟล์ใหม่ขึ้นมาในโปรเจคตามรายละเอียดด้านล่าง


```ts
// schemas/user.js
// 
// Copyright (c) 2020 Teerasej Jiraphatchandej
// 
// This software is released under the MIT License.
// https://opensource.org/licenses/MIT

// import Schema Class และ function model มาเพื่อใช้งาน
import mongoose from 'mongoose'


// กำหนด field ให้กับ Schema 
const userSchema = new mongoose.Schema({
    email: String,
    password: String,
});

// เรากำหนด Interface ที่สร้างเทียบกับ Schema ตอนสร้าง model เพื่อใช้งานใน TypeScript ได้โดยง่าย
// ชื่อของ model ที่ใส่ลงไปใน function จะถูกแปลงเป็นแบบพหูพจน์ และใช้เป็นชื่อ collection 
const UserModel = mongoose.model('User', userSchema)

// นำ User Model ไปใช้ในแอพ
export default UserModel
```