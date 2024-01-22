### การใช้ express-validator แบบ middleware เพื่อตรวจสอบข้อมูลใน API CRUD ของ Express.js มีขั้นตอนดังนี้:

### ติดตั้ง express-validator:

ใช้ npm เพื่อติดตั้ง express-validator:
```js client
npm install express-validator
```
### นำเข้า express-validator และ express:

ในไฟล์ Express.js (app.js หรือ server.js), นำเข้า express-validator และ express:
```js client
const express = require('express');
const { validationResult, body } = require('express-validator');

const app = express();
app.use(express.json());
```
### สร้าง Middleware ตรวจสอบข้อมูล:

สร้าง middleware ตรวจสอบข้อมูลที่จะใช้ใน route ของ API CRUD:

```js client
const validateCreateUser = [
  body('username').notEmpty().isString(),
  body('email').notEmpty().isEmail(),
  // เพิ่มตรวจสอบข้อมูลต่าง ๆ ตามความต้องการ
];

const validateUpdateUser = [
  body('username').optional().isString(),
  body('email').optional().isEmail(),
  // เพิ่มตรวจสอบข้อมูลต่าง ๆ ตามความต้องการ
];
```
### สร้าง middleware สำหรับตรวจสอบข้อผิดพลาดและส่งคำตอบหากมีข้อผิดพลาด:

```js client
const handleValidationErrors = (req, res, next) => {
  const errors = validationResult(req);
  if (!errors.isEmpty()) {
    return res.status(400).json({ errors: errors.array() });
  }
  next();
};
```
### ใช้ Middleware ใน Route:

ใน route ของ API CRUD, ใช้ middleware ตรวจสอบข้อมูลที่คุณได้สร้างไว้:

```js client
const createUser = (req, res) => {
  // ทำงานเมื่อข้อมูลถูกตรวจสอบผ่าน
  res.json({ message: 'User created successfully' });
};

app.post('/users', validateCreateUser, handleValidationErrors, createUser);
```
```js client
const updateUser = (req, res) => {
  // ทำงานเมื่อข้อมูลถูกตรวจสอบผ่าน
  res.json({ message: 'User updated successfully' });
};

app.put('/users/:id', validateUpdateUser, handleValidationErrors, updateUser);
```
### รันแอปพลิเคชัน:

รัน Express.js และทดสอบ API CRUD ที่ใช้ express-validator เพื่อตรวจสอบข้อมูลในรีเควส
ขั้นตอนนี้ทำให้คุณสามารถใช้ express-validator แบบ middleware ใน API CRUD ของ Express.js ได้. Middleware นี้จะทำการตรวจสอบข้อมูลในรีเควสและส่งคำตอบข้อผิดพลาดหากข้อมูลไม่ถูกต้อง. คุณสามารถปรับแต่ง middleware ตรวจสอบข้อมูลตามความต้องการของโปรเจ็กต์ของคุณ.





