
# การใช้ Schema.methods ใช้ประโยชน์จาก model ของ collection


## 1. กำหนด method ให้ Schema เพื่อใช้ในการเทียบค่ารหัสผ่าน

```js
// schemas/user.js

// ...

const userSchema = new Schema({
    //...
});

userSchema.pre('save', async function preSave(next) {
    //..
})

// กำหนดสร้าง method ชื่อ comparePassword โดยใช้ bcrypt ช่วยเช็ครหัสผ่านที่ส่งเข้ามาใน function กับตัวรหัสผ่านของ document ที่เข้ารหัสไว้อยู่แล้ว
userSchema.methods.comparePassword = async function comparePassword(candidate) {
    return bcrypt.compare(candidate, this.password)
}

const UserModel = mongoose.model('User', userSchema)

export default UserModel
```

## 2. สร้าง Route สำหรับการทดสอบการ login 

```js
// index.js

// กำหนด url เป็น /users/login
app.post('/users/login', async (request, response) => {
    console.log(request.body)

    // ค้นหา user document ด้วย email ที่ได้มาก่อน
    const doc = await UserModel.findOne({ email: request.body.email });

    // ถ้าพบว่าไม่มี document ที่ตรงกับ email ก็ตอบกลับไปที่ client เลย
    if (!doc) {
        response.status(404).send('email not found')
        return
    }

    // แต่ถ้าเจอ user document ก็ใช้ schema.methods ที่สร้างไว้ในการเช็ครหัสผ่าน
    const passValidation = await doc.comparePassword(request.body.password)

    // ตอบกลับ client ตามความเหมาะสม
    if(passValidation) {
        response.status(200).send('ok PASS')
    } else {
        response.status(401).send('password is not correct')
    }
    
})
```

## 3. ทดสอบโดยใช้คำสั่ง CURL

ในที่นี้ ให้ทดสอบโดยการสร้าง user ที่มี email ตรงกับที่จะใช้ login ก่อน 

### 3.1 CREATE USER

POST Request จะเป็นส่วนของการสร้างข้อมูล สังเกตว่าในข้อมูลที่ส่งไปที่ Server จะมี field **email** และ **password**

```bash
curl -X POST http://localhost:3000/users -H 'Content-Type: application/json' -d '{"email":"training@nextflow.in.th","password":"111Fdbs$"}'
```

### 3.2 LOGIN USER

```bash
curl -X POST http://localhost:3000/users/login -H 'Content-Type: application/json' -d '{"email":"training@nextflow.in.th","password":"111Fdbs$"}'
```