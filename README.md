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
