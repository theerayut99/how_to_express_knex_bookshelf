การใช้ Express.js ร่วมกับ Knex.js และ Bookshelf.js เพื่อสร้าง API CRUD มีขั้นตอนทั้งหมดดังนี้:

ติดตั้ง Knex, Bookshelf และ mssql Driver:

ใช้ npm เพื่อติดตั้ง Knex, Bookshelf และ mssql driver (หรือ driver ที่เหมาะสมกับฐานข้อมูลที่คุณใช้):
bash
Copy code
npm install knex bookshelf mssql
สร้างไฟล์ Knexfile.js:

สร้างไฟล์ Knexfile.js ใน root directory ของโปรเจ็กต์ของคุณ. ไฟล์นี้จะใช้กำหนดค่าการเชื่อมต่อฐานข้อมูล:
javascript
Copy code
// Knexfile.js
module.exports = {
  client: 'mssql',
  connection: {
    server: 'your-mssql-server',
    user: 'your-mssql-username',
    password: 'your-mssql-password',
    database: 'your-database-name',
  },
  pool: {
    min: 2,
    max: 10,
  },
  options: {
    enableArithAbort: true,
  },
};
สร้าง Connection ใน Express.js:

ในไฟล์ Express.js (app.js หรือ server.js), import Knex และใช้ Knex เพื่อสร้าง connection กับ Microsoft SQL Server. จากนั้น, import Bookshelf และทำการติดตั้ง Knex เข้ากับ Bookshelf:
javascript
Copy code
const express = require('express');
const knexConfig = require('./Knexfile');
const knex = require('knex')(knexConfig);
const bookshelf = require('bookshelf')(knex);

const app = express();
สร้าง Model ด้วย Bookshelf:

สร้าง Model สำหรับแทนตารางในฐานข้อมูล. ตัวอย่างนี้ให้สร้าง Model สำหรับตาราง "users":
javascript
Copy code
// models/User.js
const bookshelf = require('../path/to/bookshelf-instance');

const User = bookshelf.model('User', {
  tableName: 'users',
});

module.exports = User;
สร้าง Controller สำหรับ API CRUD:

สร้าง Controller ที่จะใช้ในการจัดการรีเควสที่เข้ามา และใช้ Model เพื่อทำ CRUD:
javascript
Copy code
// controllers/UserController.js
const User = require('../path/to/User-model');

// ดึงข้อมูลทั้งหมด
const getAllUsers = async (req, res) => {
  try {
    const users = await User.fetchAll();
    res.json(users);
  } catch (error) {
    console.error(error);
    res.status(500).json({ error: 'Internal Server Error' });
  }
};

// ดึงข้อมูลตาม ID
const getUserById = async (req, res) => {
  const userId = req.params.id;
  try {
    const user = await User.where({ id: userId }).fetch();
    res.json(user);
  } catch (error) {
    console.error(error);
    res.status(500).json({ error: 'Internal Server Error' });
  }
};

// สร้างผู้ใช้ใหม่
const createUser = async (req, res) => {
  const userData = req.body;
  try {
    const newUser = await User.forge(userData).save();
    res.json(newUser);
  } catch (error) {
    console.error(error);
    res.status(500).json({ error: 'Internal Server Error' });
  }
};

// อัปเดตข้อมูลผู้ใช้
const updateUser = async (req, res) => {
  const userId = req.params.id;
  const updatedUserData = req.body;
  try {
    const user = await User.where({ id: userId }).fetch();
    if (!user) {
      return res.status(404).json({ error: 'User not found' });
    }
    await user.save(updatedUserData);
    res.json(user);
  } catch (error) {
    console.error(error);
    res.status(500).json({ error: 'Internal Server Error' });
  }
};

// ลบผู้ใช้
const deleteUser = async (req, res) => {
  const userId = req.params.id;
  try {
    const user = await User.where({ id: userId }).fetch();
    if (!user) {
      return res.status(404).json({ error: 'User not found' });
    }
    await user.destroy();
    res.json({ message: 'User deleted successfully' });
  } catch (error) {
    console.error(error);
    res.status(500).json({ error: 'Internal Server Error' });
  }
};

module.exports = {
  getAllUsers,
  getUserById,
  createUser,
  updateUser,
  deleteUser,
};
กำหนด Route ใน Express.js:

กำหนด route ใน Express.js เพื่อให้ API CRUD ทำงาน:
javascript
Copy code
// routes/api.js
const express = require('express');
const router = express.Router();
const userController = require('../path/to/UserController');

router.get('/users', userController.getAllUsers);
router.get('/users/:id', userController.getUserById);
router.post('/users', userController.createUser);
router.put('/users/:id', userController.updateUser);
router.delete('/users/:id', userController.deleteUser);

module.exports = router;
เรียกใช้ Route ใน Express.js:

เรียกใช้ route ที่เราได้กำหนดใน Express.js:
javascript
Copy code
// app.js or server.js
const express = require('express');
const apiRoutes = require('./path/to/routes/api');

const app = express();

app.use(express.json());
app.use('/api', apiRoutes);

const port = 3000;
app.listen(port, () => {
  console.log(`Server is running on port ${port}`);
});
รันแอปพลิเคชัน:

รัน Express.js และทดสอบการใช้ API CRUD ในเบราว์เซอร์หรือโปรแกรม API testing
ขั้นตอนนี้ทำให้คุณสามารถใช้ Express.js, Knex.js และ Bookshelf.js เพื่อสร้าง API CRUD ที่เชื่อมต่อกับฐานข้อมูล Microsoft SQL Server ได้. คุณสามารถปรับแต่ง Model, Controller, และ route ตามความต้องการของโปรเจ็กต์ของคุณ.
