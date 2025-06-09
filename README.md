<HR>
My name: 吳宗翰 <BR>
My SID: C113181132

const express = require('express');
const path = require('path');

const app = express();
const port = process.env.PORT || 3000;

let targetNumber = Math.floor(Math.random() * 100) + 1;  // 隨機生成 1 到 100 之間的數字
let attempts = 0;

// 解析 JSON 請求
app.use(express.json());
app.use(express.static(path.join(__dirname, 'public')));

// Route: 提交猜測數字
app.post('/guess', (req, res) => {
  const { guess } = req.body;
  attempts++;

  if (guess < targetNumber) {
    res.json({ result: 'Too low', attempts });
  } else if (guess > targetNumber) {
    res.json({ result: 'Too high', attempts });
  } else {
    res.json({ result: 'Correct', attempts });
    targetNumber = Math.floor(Math.random() * 100) + 1;  // 猜對後重新生成一個隨機數字
    attempts = 0;  // 重置嘗試次數
  }
});

// Serve the index.html file
app.get('/', (req, res) => {
  res.sendFile(path.join(__dirname, 'public', 'index.html'));
});

// 啟動伺服器
app.listen(port, () => {
  console.log(`Server running on http://localhost:${port}`);
});
