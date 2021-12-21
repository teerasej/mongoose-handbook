# การค้นหาผู้ใช้จาก Email

## 1. ค้นหาโดยระบุ Document 

ในที่นี้เราจะใช้ข้อมูลที่ส่งมากับ URL ในรูปแบบ Query string เพื่อใช้ในคำสั่งการค้นหา document ที่มีค่า email ตรงกับที่ได้จาก URL

```ts
// index.ts

// ปรับ function ทำงานแบบ Async
app.get('/users/:email', async (request: Request, response: Response) => {
    console.log(request.params.email)

    // ในที่นี้เรียกใช้ findOne และใช้ JavaScript object ในการระบุเงื่อนไข
    const doc = await UserModel.findOne({ email: request.params.email })

    response.status(200).json(doc)
})
```

- ทดสอบหา email ที่ไม่อยู่ในระบบ ว่า client ได้รับข้อมูลอะไร

> Tips: หากต้องการค้นหาส่วนใดส่วนหนึ่งใน field ให้ลอง { email: new RegExp(`^${request.params.email}$`, "i")}

## 2. รับมือกรณีที่หา document ไม่เจอ

จะเห็นว่า มีกรณีที่ findOne หา document ตามเงื่อนไขไม่เจอด้วย เราสามารถเช็คค่า null ของ doc ได้

```ts
app.get('/users/:email', async (request: Request, response: Response) => {
    console.log(request.params.email)

    const doc = await UserModel.findOne({ email: request.params.email })

    // เช็คค่า null เพื่อส่ง http code ที่เหมาะสมให้ client
    if(doc) {
        response.status(200).json(doc)
    } else {
        response.status(404).send('user not found')
    }
    
})

```
