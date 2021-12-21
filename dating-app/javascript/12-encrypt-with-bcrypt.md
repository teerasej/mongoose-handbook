
# การใช้ bcrypt เข้ารหัส password หรือ field ที่ต้องการก่อนบันทึกลง database

## 1. ติดตั้ง package

```bash
npm i bcrypt
```

## 2. กำหนดใช้ bcrypt ในขั้นตอน 'save' ของ Schema

เราจะทำการ implement การเข้ารหัสผ่านก่อนทำการ save ข้อมูลลง database 

- หลังทำเสร็จให้ทดสอบสร้าง user document ใหม่ และสังเกตค่ารหัสผ่านในฐานข้อมูลหลังเสร็จสิ้นกระบวนการแล้ว

```js
// schemas/user.js

// ...

// import bcrypt
import bcrypt from 'bcrypt'
// กำหนดค่า salt
const SALT_ROUNDS = 12;

//...

const userSchema = new Schema({
    //...
});


// function .pre ของ Schema สามารถทำให้เรากำหนดการทำงานก่อนเริ่มกระบวนการ CRUD ต่างๆ ได้ เช่นในที่นี้เราจะกำหนดให้ function ทำการเข้ารหัส ก่อนคำสั่ง save ทำงาน
userSchema.pre('save', async function preSave(next) {

    // อ้างอิงถึง document หรือ model ปัจจุบัน ทำให้เราสามารถเข้าถึง field ต่างๆ ของ document ปัจจุบันที่กำลังจะทำการ save ข้อมูลได้
    const user = this

    // ถ้ารหัสผ่านไม่ได้ถูกแก้ไข เราจะใช้ function next เพื่อเริ่มกระบวนการ save
    if(!user.isModified('password')) return next()

    // แต่ถ้าเป็นรหัสผ่านใหม่ (ในทีนี้รวมถึง user ที่กำลังจะถูกสร้างใหม่ด้วย) เราจะใช้ bcrypt ในการเข้ารหัส
    try {
        const hash = await bcrypt.hash(user.password, SALT_ROUNDS)

        // กำหนด password ที่เข้ารหัสแล้วลงไปใน model 
        user.password = hash

        // เรียกใช้ function next() เพ่ิอให้เริ่มขึ้นตอนการ save
        return next()
    } catch (error) {
        return next(error)
    }
})

const UserModel = mongoose.model('User', userSchema)

export default UserModel
```