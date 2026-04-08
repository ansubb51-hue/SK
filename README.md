// src/App.js
import React, { useState, useEffect } from 'react';
import './App.css';

function App() {
  const [recording, setRecording] = useState(false);
  const [audio, setAudio] = useState(null);

  useEffect(() => {
    if (recording) {
      navigator.mediaDevices.getUserMedia({ audio: true })
        .then(stream => {
          const audioContext = new AudioContext();
          const source = audioContext.createMediaStreamSource(stream);
          const gain = audioContext.createGain();
          gain.gain.value = 1;
          source.connect(gain);
          gain.connect(audioContext.destination);
          setAudio(audioContext);
        })
        .catch(error => console.error(error));
    }
  }, [recording]);

  const handleRecord = () => {
    setRecording(true);
  };

  const handleStop = () => {
    setRecording(false);
    audio.disconnect();
  };

  return (
    <div className="App">
      <header className="App-header">
        <h1>SK Rakesh</h1>
        <p>Usmein main apna hacksail karun</p>
        <button onClick={handleRecord}>Record</button>
        <button onClick={handleStop}>Stop</button>
      </header>
    </div>
  );
}

export default App;
