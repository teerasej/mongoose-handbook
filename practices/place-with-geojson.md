
# Place with GeoJSON

## 1. โจทย์ 

1. สร้าง Web API ที่สามารถเพิ่มข้อมูลสถานที่ (เรียกว่า **Place**) ที่มีชื่อสถานที่ และตำแหน่งพิกัดได้ 
2. การเก็บข้อมูลต้องเก็บแบบ GeoJSON 
3. สามารถค้นหาสถานที่ใกล้เคียงจากพิกัดที่กำหนดได้ 

### รายละเอียดอ้างอิง

- โปรเจคเป็น Node เขียนแบบ TypeScript
- มีการใช้งาน module Express และ Mongoose ในโปรเจค 
- ฐานข้อมูลที่ใช้คือ MongoDB

## 2. การ Setup โปรเจค 

[สามารถใช้ template ของโปรเจค จากที่นี่ได้](../dating-app/1-setup.md) และทำการแก้ไข URL Route ตามต้องการ

## 3. ตัวอย่างข้อมูลแบบ GeoJSON 

โครงสร้างของข้อมูลแบบ GeoJSON มีการกำหนดไว้แล้วซึ่งจะเป็นรูปแบบด้านล่างนี้

```json
{
  "type" : "Point",
  "coordinates" : [
    -122.5, // longitude
    37.7 // latitude
  ]
}
```

> ค่า longitude จะขึ้นก่อน latitude

ซึ่งวิธีการประกาศ โครงสร้างแบบนี้ใน Mongoose Schema จะเป็นตามตัวอย่างด้านล่าง 

```ts
const placeSchema = new Schema({
  //... field อื่นๆ 

  // field สำหรับ geoJSON
  location: {
    type: {
      type: String, 
      enum: ['Point'], 
      required: true
    },
    coordinates: {
      type: [Number],
      required: true
    }
  }

  //... field อื่นๆ 
});
```

เสร็จแล้วกำหนดให้ทำ index ได้ 

```ts
placeSchema.index({ "location": "2dsphere" });
```


## 4. การค้นหาโดยใช้ geoJSON 

คำสั่ง find() สามารถกำหนด parameter $near และใช้งานได้

```ts
const latitude = request.params.latitude
const longitude = request.params.longitude

placeModel.find({
    location: {
        $near: {
            $geometry: {
                type : "Point",
                coordinates : [parseFloat(latitude), parseFloat(longitude)]
            },
            // รัศมีจากจุดพิกัด หน่วยเป็นเมตร
            $maxDistance: 5000
        }
    }
})
```

อ้างอิงจาก [MongoDB: near](https://docs.mongodb.com/manual/reference/operator/query/near/)

## 5. ตัวอย่างคำสั่ง CURL ที่ใช้ทดสอบ

### POST

POST Request จะเป็นส่วนของการสร้างข้อมูล สังเกตว่าในข้อมูลที่ส่งไปที่ Server จะมี field **name**, **latitude** และ **longitude**

```bash
curl -X POST http://localhost:3000/places -H 'Content-Type: application/json' -d '{"name":"Icon Siam","latitude": 13.726498, "longitude": 100.5100807 }'
```

### GET

GET Request จะเป็นส่วนของการค้นหาข้อมูล ในที่นี้จะใช้ **longitude** และ **latitude**  เป็นตัวค้นหา สังเกตว่าเราจะต่อท้าย url `http://localhost:3000/places` ด้วย **longitude** และ **latitude** ตามลำดับ

```bash
curl -X GET http://localhost:3000/places/100.510585/13.7248617
```
- ตำแหน่งอ้างอิงโรงแรม Millenium Hilton bangkok
