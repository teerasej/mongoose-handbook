
# การทำ Aggregation

การทำ Agregation มักจะอยู่ในรูปแบบที่ใกล้เคียงกับการ join ในฐานข้อมูลแบบ SQL 

## 1. โหลดข้อมูลเข้าไปใน products collection ผ่าน Mongo Compass

ใช้ไฟล์ [product_mock.json](../data/product_mock.json)

## 2. สร้าง product Schema

```js
// schemas/product.js

import mongoose from 'mongoose'

const productSchema = new mongoose.Schema({
    productName: String,
    price: Number,
    expireDate: Date,
    color: String
})

const ProductModel = mongoose.model('Product', productSchema);

export default ProductModel;
```

## 3. สร้าง route สำหรับการใช้งาน aggregation
```js
// index.js 

app.get('/products/expansive-list', async (request, response) => {


        const docs = await ProductModel.aggregate([
            // $project มักเป็นการเลือกให้ document มี field อะไรบ้างหลังผ่าน aggretion นี้
            {
                $project: {
                    productName: 1,
                    price: 1,
                    color: 1
                }
            },
            // $match มักเป็นการเทียบค่าเพื่อเอาเฉพาะ document ที่ตรงกับเงื่อนไข
            {
                $match: {
                    color: "Mauv"
                }
            },

            // เพิ่ม field ที่ต้องการ ในที่นี้มีการกำหนดเงื่อนไขการเปรียบเทียบโดยใช้ aggregation operator
            {
                $addFields: {
                    expensive: {
                        $gte: [
                            '$price',
                            1000
                        ]
                    }
                }
            }
        ])

        response.json(docs)

    })
```

## 3. ทดสอบโดยใช้คำสั่ง CURL 

GET Request จะเป็นส่วนของการร้องขอข้อมูล

```bash
curl -X GET http://localhost:3000/products/expansive-list
```