
# การเพิ่มผู้ใช้ใหม่

- เราสามารถสร้าง model จาก UserModel ได้โดยการส่งค่าตัวแปรที่เรามีในรูปแบบ JavaScript object 
- ตัว model จะยังไม่ถูกบันทึกลงไปในรูปแบบ Document จนกว่าเราจะสั่ง `.save()`

```ts
// index.ts

import UserModel from './schemas/user'

// ปรับ function ทำงานแบบ Async
app.post('/users', async (request: Request, response: Response) => {
    console.log(request.body)

    // สร้าง user model object ใหม่ โดยส่ง JavaScript object ที่ได้รับมาจาก Request
    const newUser = new UserModel(request.body)

    // หรือจะกำหนด JavaScript เองก็ได้เหมือนกัน
    // const newUser = new UserModel({
    //     email: request.body.email,
    //     password: request.body.password
    // });

    // บันทึกข้อมูลลง database ผ่าน .save() ตรงนี้จะได้ข้อมูลเป็น document ที่สร้างแล้วกลับมา
    const createdUser = await newUser.save()

    // ส่ง document กลับไปหา client
    response.status(200).json(createdUser)
})
```