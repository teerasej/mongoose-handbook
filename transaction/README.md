
# การใช้งาน Transaction 

## การสร้าง session 

เราสามารถใช้ connection object ที่ได้จากการสั่งเชื่อมต่อฐานข้อมูลมาสร้าง session เพื่อใช้งาน transaction ได้

```js
const connection = await mongoose.connect('mongodb://localhost:27017/dating_app');

const session = await connection.startSession()
```

## การใช้งานใน CRUD operation ทั่วไป

```js
app.post('/users', async function (request, response) {
        console.log(request.body)

        let createdUser
        try {

            // สร้าง session
            const session = await connection.startSession()
            // เริ่มการทำงานของ transaction
            session.startTransaction()

            const newUser = new UserModel(request.body)

            // ใส่ session ในการบันทึกข้อมูล
            createdUser = await newUser.save({session})

            // commit transaction
            await session.commitTransaction()

        } catch (error) {
            response.status(500).json(error.message)
        }


        response.status(200).json(createdUser)
    });

```


## การใช้งานกับพวก Many()

```js
app.get('/products/preload', async function(request,response) {

    // สร้าง session สำหรับการทำงานกับ insertMany
    const session = await connection.startSession()
    session.startTransaction()

    // ใส่ session ไปในขั้นตอน insertMany
    let products = await ProductModel.insertMany([
        { productName: 'iPhone 13 Pro', price: 42900 },
        { productName: 'iPhone 13', price: 38900 },
        { productName: 'iPhone 12', price: 28900 }
        ], { session });

    // commit transaction
    await session.commitTransaction()


})
```