
# การใช้งาน Joi ในการ validate ความถูกต้องของ email

## 1. ติดตั้ง package

```bash
npm i joi
```


## 2. กำหนดใช้งาน Joi เพื่อ validate format ของ Email

- เสร็จแล้วให้ลองสร้าง user ใหม่ โดยกำหนด email ให้ผิดจากรูปแบบ email address ปกติ

```js
// schemas/user.js

// import joi
import Joi from 'joi';
import mongoose from 'mongoose'


// ใช้ Joi สร้าง validator object โดยในที่นี้กำหนดให้เป็น string ที่ต้องมี format ของ email ที่ถูกต้อง
const emailValidator = Joi.string().email()

const userSchema = new mongoose.Schema({
    email: {
        type: String,
        required: true,
        trim: true,

        // นำ joi validator object มาใช้ โดยเรียกผ่าน .validate() ซึ่งถ้า validate แล้วไม่ผ่าน ผลลัพธ์ที่ได้จากการ validate จะมีค่า .error ทำให้เราเช็คได้ ผ่านการเช็คค่า undefined
        validate: {
            validator: function (value) {
                const result = emailValidator.validate(value)
                return result.error == undefined
            },
            // ปรับแต่งข้อความ error ให้เป็นผู้เป็นคนมากขึ้น
            message: props => `${props.value} is not a correct email, please check again`
        }
    },
    password: {
        type: String,
        required: true,
        trim: true,
        minlength: 6
    },
});

const UserModel = mongoose.model('User', userSchema);

export default UserModel;
```