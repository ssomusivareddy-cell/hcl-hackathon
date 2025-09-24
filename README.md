// App.js
import React, { useState } from 'react';
import { View, Text, Button } from 'react-native';

export default function App() {
  const [progress, setProgress] = useState(null);

  const fetchProgress = async () => {
    const response = await fetch('http://localhost:8000/api/progress/123');
    const data = await response.json();
    setProgress(data);
  };

  return (
    <View>
      <Text>My Progress</Text>
      <Button title="Fetch" onPress={fetchProgress} />
      {progress && <Text>{JSON.stringify(progress)}</Text>}
    </View>
  );
}
# hcl-hackathon
// server.js
const express = require('express');
const cors = require('cors');
const sqlite3 = require('sqlite3');
const app = express();
app.use(cors());
app.use(express.json());

// Local SQLite DB setup
const db = new sqlite3.Database('learnapp.db');

// REST API endpoint: student progress
app.get('/api/progress/:studentId', (req, res) => {
  db.get('SELECT * FROM progress WHERE studentId = ?', [req.params.studentId], (err, row) => {
    if (err) return res.status(500).json({ error: err.message });
    res.json(row);
  });
});

// REST API endpoint: update progress
app.post('/api/progress', (req, res) => {
  const { studentId, lessonId, score } = req.body;
  db.run('INSERT INTO progress(studentId, lessonId, score) VALUES (?, ?, ?)', [studentId, lessonId, score], function (err) {
    if (err) return res.status(500).json({ error: err.message });
    res.json({ id: this.lastID });
  });
});

app.listen(8000, () => console.log('Backend running on 8000'));

