
# การกำหนด custom validator function ใน Schema

ในที่นี้เราจะลองใช้ custom validator แบบง่ายๆ ก่อน (อย่าหาทำจริงใน production นะ)

```js
// schemas/user.js

import mongoose from 'mongoose'

const userSchema = new mongoose.Schema({
    email: {
        type: String,
        required: true,
        trim: true,

        // กำหนด function validate ซึ่งสามารถรับค่าที่ส่งเข้ามาใน Schema ก่อนที่จะ save หรือนำไปใช้ได้ คืนค่าเป็น boolean ว่าให้ผ่านมั้ย?
        // สามารถ custom ข้อความ error ได้ด้วย
        validate: {
            validator: function (value) {
                return value == 'admin@gmail.com'
            },
            message: props => `${props.value} is not admin@gmail.com`
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