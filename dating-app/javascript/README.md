
# Dating Web API - TypeScript version

- ในการทดสอบการทำงานของ Web API ในที่นี้ เราสามารถ[ใช้คำสั่ง CURL ที่เตรียมไว้](../curl-command.md)ได้ 

## Setup Schema 

1. [Setup Project](1-setup.md)
2. [สร้าง Schema สำหรับ User Document](2-create-user-schema.md)
3. [เชื่อมต่อฐานข้อมูลด้วย Mongoose](3-connect-mongo.md)

## CRUD 

4. [การเพิ่มผู้ใช้ใหม่](4-insert.md) 
5. [การค้นหาผู้ใช้จาก Email](5-select.md)
6. [การอัพเดตผู้ใช้ อ้างอิง Email](6-update.md)
7. [การลบผู้ใช้อ้างอิงจาก id](7-delete.md)
8. [การใช้ rule ของ Mongoose Schema](8-rule.md)

# Validator 

9. [การกำหนด custom validator function ใน Schema](9-custom-validator-function.md)
10. [การใช้งาน Joi ในการ validate ความถูกต้องของ email](10-validate-email-with-joi.md)
11. [การใช้งาน joi-password-complexity ในการ validate ความถูกต้องของ password แบบซับซ้อน](11-validate-complex-password.md) 

# Encryption

12. [การใช้ bcrypt เข้ารหัส password หรือ field ที่ต้องการก่อนบันทึกลง database](12-encrypt-with-bcrypt.md)
13. [การใช้ Schema.methods ใช้ประโยชน์จาก model ของ collection](13-schema-methods.md)

# Aggregation & Transaction

- [Aggregation](../../agregation/README.md)
- [Transaction](../../transaction/README.md)