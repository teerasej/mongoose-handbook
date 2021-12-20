# การลบผู้ใช้อ้างอิงจาก id

```ts
// index.ts

// ปรับ function ทำงานแบบ Async
app.delete('/users', async (request: Request, response: Response) => {
    console.log(request.body.id)

    // เรามีคำสั่่ง .findByIdAndRemove ซึ่งทำให้ง่ายในการระบุ id เพื่อลบ document ออกจาก collection
    await userModel.findByIdAndRemove(request.body.id)

    response.status(200).send('ok DELETE')
})
```

- ทดสอบระบุ document id ที่ไม่มีอยู่ในฐานข้อมูลและสังเกตผล