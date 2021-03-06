# การอัพเดตผู้ใช้ อ้างอิง Email

เรามักจะใช้ Request แบบ PATCH (หรือ PUT) ในการอัพเดตข้อมูลบน server ของเรา

```ts
// index.ts

// ปรับ function ทำงานแบบ Async
app.patch('/users', async (request, response) => {
    console.log(request.body)

    // ใช้คำสั่ง updateOne เพื่ออัพเดตข้อมูล
    // สังเกตว่า parameter แรกคือ condition และ parameter ที่ 2 คือค่าที่จะส่งเข้าไปอัพเดต
    await UserModel.updateOne({ email: request.body.email }, request.body)

    response.status(200).send('ok PATCH')

    // ใช้ .findOneAndUpdate แทน ถ้าต้องการ doc กลับมาใช้งานจาก database ด้วย
    // const updatedDoc = await userModel.findOneAndUpdate({ email: request.body.email }, request.body)
    // response.status(200).send(updatedDoc)
})
```

- ทดสอบแทนที่ข้อมูลใน JSON ด้วยข้อมูลที่ไม่มีอยู่ก่อนหน้านี้ดู เช่น username, isAdmin
- เราสามารถกำหนดให้ mongoDB เพิ่มข้อมูล update เป็น insert document ใหม่ได้ ผ่าน option ที่ชื่อว่า **upsert**

```js
 Model.updateOne({},{},{ upsert: true })
```

## อ้างอิง

- [UpdateOne](https://docs.mongodb.com/manual/reference/method/db.collection.updateOne/)