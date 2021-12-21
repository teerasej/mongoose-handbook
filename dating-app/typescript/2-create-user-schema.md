
# สร้าง Schema สำหรับ User Document

สร้างไฟล์ใหม่ขึ้นมาในโปรเจคตามรายละเอียดด้านล่าง


```ts
// schemas/user.ts
// 
// Copyright (c) 2020 Teerasej Jiraphatchandej
// 
// This software is released under the MIT License.
// https://opensource.org/licenses/MIT

// import Schema Class และ function model มาเพื่อใช้งาน
import { Schema, model } from 'mongoose'

// กำหนด Interface เพื่อให้สามารถเรียกใช้ใน TypeScript ได้โดยง่าย 
interface IUser {
    email: String
    password: String
}

// กำหนด field ให้กับ Schema 
const userSchema = new Schema({
    email: String,
    password: String,
});

// เรากำหนด Interface ที่สร้างเทียบกับ Schema ตอนสร้าง model เพื่อใช้งานใน TypeScript ได้โดยง่าย
// ชื่อของ model ที่ใส่ลงไปใน function จะถูกแปลงเป็นแบบพหูพจน์ และใช้เป็นชื่อ collection 
const UserModel = model<IUser>('User', userSchema)

// นำ User Model ไปใช้ในแอพ
export default UserModel
```