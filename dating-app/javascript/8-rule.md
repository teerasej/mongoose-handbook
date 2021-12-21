# การใช้ rule ของ Mongoose Schema

## 1. ทดสอบ Web API

ทดสอบกรณีต่อไปนี้ 

- สร้าง User โดยไม่ระบุ Email หรือ Password


## 2. ปรับแต่ง Schema 

```ts
// schemas/user.ts

const userSchema = new Schema({

    // ปรับ field email ให้มีคุณสมบัติ required เพิ่มเติม
    email: {
        type: String,
        required: true,
    },
    password: String,
});
```

ทดสอบสร้าง user ใหม่ และสังเกตการทำงานของ Web API

## 3. ใช้ try/catch รองรับกรณีที่ Mongoose ทำการ validate ข้อมูลไม่ผ่าน

```ts
// index.ts

app.post('/users', async (request, response) => {
    console.log(request.body)

    // ปรับแต่่งมาใช้ try/catch เพื่อรองรับกรณีที่ mongoose คืนค่า error ออกมาจากการใช้งาน schema
    let createdUser
    try {

        const newUser = new UserModel(request.body)
        createdUser = await newUser.save()

    } catch (error) {
        // ถ้า error ให้ใส่รหัส 500 และข้อความ error กลับไปที่ client
        response.status(500).json(error.message)
    }
    

    response.status(200).json(createdUser)
})
```

- ทดสอบการสร้าง user ใหม่อีกครั้ง
- คราวนี้ลองใช้วิธีการเดียวกับ password field

## 4. ลองใช้งาน function ของ Schema

```ts
// schemas/user.ts

const userSchema = new Schema({

    // ปรับ field email ให้มีคุณสมบัติ trim เพิ่มเติม
    email: {
        type: String,
        required: true,
        trim: true
    },
    password: String,
});
```

- ทดสอบใส่ข้อมูล email ที่มี whitespace หรือช่องว่างด้านหน้า หรือด้านหลังข้อความ
- ลองทำแบบเดียวกับ password