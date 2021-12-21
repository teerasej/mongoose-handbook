
# การใช้งานคำสั่ง CURL ในการส่ง Request ไปที่ Web API

### POST

POST Request จะเป็นส่วนของการสร้างข้อมูล สังเกตว่าในข้อมูลที่ส่งไปที่ Server จะมี field **email** และ **password**

```bash
curl -X POST http://localhost:3000/users -H 'Content-Type: application/json' -d '{"email":"training@nextflow.in.th","password":"111222"}'
```

### DELETE

DELETE Request จะเป็นส่วนของการสร้างลบข้อมูล โดยในที่นี้เราจะใช้ Document ID เป็นตัวอ้างอิง สังเกตว่าในข้อมูลที่ส่งไปที่ Server จะมี field **id**

```bash
curl -X DELETE http://localhost:3000/users -H 'Content-Type: application/json' -d '{ "id": "61c089efc246a7de85c98ce1" }'
```

### GET

GET Request จะเป็นส่วนของการค้นหาข้อมูล ในที่นี้จะใช้ email เป็นตัวค้นหา สังเกตว่าเราจะต่อท้าย url `http://localhost:3000/users/` ด้วย email ที่ต้องการค้นหา

```bash
curl -X GET http://localhost:3000/users/training@nextflow.in.th
```

### PATCH

PATCH Request จะเป็นส่วนของการอัพเดตรหัสผ่าน ในที่นี้จะใช้ email เป็นตัวค้นหา สังเกตว่าในข้อมูลที่ส่งไปที่ Server จะมี field **email** และ **password**

```bash
curl -X PATCH http://localhost:3000/users -H 'Content-Type: application/json' -d '{"email":"training@nextflow.in.th","password":"444555"}'
```