

# การใช้งาน joi-password-complexity ในการ validate ความถูกต้องของ password แบบซับซ้อน

## 1. ติดตั้ง package

```bash
npm i joi-password-complexity
```


## 2. กำหนดใช้งาน Joi เพื่อ validate format ของ Email

- เสร็จแล้วให้ลองสร้าง user ใหม่ โดยกำหนดรหัสผ่านให้ตรงกับเงื่อนไข

```js
// schemas/user.js

// import joi-password-complexity
import passwordComplexity from 'joi-password-complexity'

import Joi from 'joi';
import mongoose from 'mongoose'

const emailValidator = Joi.string().email()

const userSchema = new mongoose.Schema({
    email: {
        type: String,
        required: true,
        trim: true,
        validate: {
            validator: function (value) {
                return emailValidator.validate(value).error == undefined
            },
            message: props => `${props.value} is not a correct email, please check again`
        }
    },
    password: {
        type: String,
        required: true,
        trim: true,
        minlength: 6,
        validate: {
            // นำ passwordComplexity object มาใช้ โดยเรียกผ่าน .validate() ซึ่งถ้า validate แล้วไม่ผ่าน ผลลัพธ์ที่ได้จากการ validate จะมีค่า .error ทำให้เราเช็คได้ ผ่านการเช็คค่า undefined
            validator: function (value) {

                const result = passwordComplexity({
                    min: 6,
                    max: 30,
                    lowerCase: 1,
                    upperCase: 1,
                    numeric: 1,
                    requirementCount: 4
                }).validate(value)

                return result.error == undefined
            },
            // แน่นอนว่าสามารถปรับแต่งข้อความ error ได้
            message: props => `${props.value} should contain at least 1 lowercase, 1 uppercase, and 1 number`
        }
    },
});

const UserModel = mongoose.model('User', userSchema);

export default UserModel;
```