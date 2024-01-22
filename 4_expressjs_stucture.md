การจัดโครงสร้าง Express.js ในรูปแบบที่มี routes, controllers, services, models, และ helpers จะช่วยให้โค้ดของคุณเป็นระเบียบและง่ายต่อการบริหารจัดการ. ต่อไปนี้คือขั้นตอนที่คุณสามารถทำเพื่อสร้างโครงสร้างดังกล่าว:

### สร้างโครงสร้างโปรเจ็กต์:

สร้างโฟลเดอร์ที่มีโครงสร้างพื้นฐานของ Express.js:
```
/your-project
├── routes
├── controllers
├── services
├── models
├── helpers
├── app.js
└── package.json
```
### ติดตั้ง Express.js และ Dependencies:

ใช้ npm เพื่อติดตั้ง Express.js และ dependencies อื่น ๆ:
```js client
npm init -y
npm install express body-parser
```
### สร้างไฟล์ app.js:

สร้างไฟล์ app.js ใน root directory ของโปรเจ็กต์:
```js client
const express = require('express');
const bodyParser = require('body-parser');

const app = express();
const port = 3000;

app.use(bodyParser.json());

// Import routes
const mainRoute = require('./routes/mainRoute');
app.use('/main', mainRoute);

app.listen(port, () => {
  console.log(`Server is running on port ${port}`);
});
```
### สร้างไฟล์ routes/mainRoute.js:

สร้างไฟล์ routes/mainRoute.js และกำหนด route หลัก:
```js client
const express = require('express');
const router = express.Router();

// Import controllers
const mainController = require('../controllers/mainController');

// Define routes
router.get('/', mainController.home);

module.exports = router;
```
### สร้างไฟล์ controllers/mainController.js:

สร้างไฟล์ controllers/mainController.js และกำหนด controller หลัก:
```js client
// controllers/mainController.js
const mainService = require('../services/mainService');

const mainController = {
  home: (req, res) => {
    const message = mainService.getMessage();
    res.json({ message });
  },
};

module.exports = mainController;
```
### สร้างไฟล์ services/mainService.js:

สร้างไฟล์ services/mainService.js และกำหนด service หลัก:
```js client
// services/mainService.js
const mainModel = require('../models/mainModel');
const mainHelper = require('../helpers/mainHelper');

const mainService = {
  getMessage: () => {
    const data = mainModel.getData();
    return mainHelper.formatMessage(data);
  },
};

module.exports = mainService;
```
### สร้างไฟล์ models/mainModel.js:

สร้างไฟล์ models/mainModel.js และกำหนด model หลัก (ถ้ามีการเชื่อมต่อกับฐานข้อมูล):
```js client
// models/mainModel.js
const mainModel = {
  getData: () => {
    // ระบุการทำงานกับฐานข้อมูล (ถ้ามี)
    return { message: 'Welcome to the home page' };
  },
};

module.exports = mainModel;
```
### สร้างไฟล์ helpers/mainHelper.js:

สร้างไฟล์ helpers/mainHelper.js และกำหนด helper functions (ในกรณีที่ต้องการฟังก์ชั่นช่วยเหลือ):
```js client
// helpers/mainHelper.js
const mainHelper = {
  formatMessage: (data) => {
    return `Formatted Message: ${data.message}`;
  },
};

module.exports = mainHelper;
```
ปรับแต่ง Middleware ต่าง ๆ ใน Express.js:

ใน app.js, ปรับแต่ง middleware ต่าง ๆ ตามความต้องการ (ยกตัวอย่างเช่น middleware สำหรับการจัดการ errors):
```js client
// Error handling middleware
app.use((err, req, res, next) => {
  console.error(err.stack);
  res.status(500).json({ error: 'Internal Server Error' });
});
```
รันแอปพลิเคชัน:

### รัน Express.js และทดสอบ route ที่ได้สร้างใน routes/mainRoute.js
เพิ่ม Unit Test (ตัวเลือก):

ตามความเหมาะสม, คุณสามารถเพิ่ม unit test สำหรับ functions ใน services ได้ โดยใช้ Mocha, Chai, และ Supertest เช่นเดียวกับตอนก่อนหน้า.
โครงสร้างนี้มีจุดไฟฟ้าที่แยกแยะแต่ละส่วนของแอปพลิเคชันออกมา, ทำให้ง่ายต่อการบริหารและเพิ่มประสิทธิภาพในการพัฒนา. คุณสามารถปรับแต่งโครงสร้างนี้ตามความต้องการของโปรเจ็กต์ของคุณได้.
