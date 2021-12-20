
# การเพิ่มผู้ใช้ใหม่

```ts
// index.ts

// ปรับ function ทำงานแบบ Async
app.post('/users', async (request: Request, response: Response) => {
    console.log(request.body)

    // สร้าง user model object ใหม่ โดยส่ง JavaScript object ที่ได้รับมาจาก Request
    const newUser = new UserModel(request.body)

    // บันทึกข้อมูลลง database ผ่าน .save() ตรงนี้จะได้ข้อมูลเป็น document ที่สร้างแล้วกลับมา
    const createdUser = await newUser.save()

    // ส่ง document กลับไปหา client
    response.status(200).json(createdUser)
})
```