# Lumina Assist - Complete Source Code

This document contains the core source code files for the Visual Assist Desktop Application.

## src\main.jsx
``javascript
import { createRoot } from 'react-dom/client'
import './index.css'
import App from './App.jsx'

createRoot(document.getElementById('root')).render(
  <App />
)

``

## src\App.jsx
``javascript
import React from 'react';
import Navbar from './components/Navbar';
import Dashboard from './components/Dashboard';

function App() {
  return (
    <div className="app-container">
      <Navbar />
      <main>
        <Dashboard />
      </main>
    </div>
  );
}

export default App;

``

## src\index.css
``css
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@300;400;600;700&family=Outfit:wght@400;600;800&display=swap');

:root {
  --bg-primary: #080A10;
  --bg-secondary: #131722;
  --bg-panel: rgba(19, 23, 34, 0.6);
  --text-primary: #FFFFFF;
  --text-secondary: #A0ABC0;
  --accent-cyan: #00E5FF;
  --accent-magenta: #FF007F;
  --accent-yellow: #FFD600;
  --danger: #FF3B30;
  --success: #34C759;
  
  --glass-border: rgba(255, 255, 255, 0.08);
  --glass-shadow: 0 8px 32px 0 rgba(0, 0, 0, 0.37);
}

* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

body {
  font-family: 'Inter', sans-serif;
  background-color: var(--bg-primary);
  color: var(--text-primary);
  line-height: 1.6;
  overflow-x: hidden;
  min-height: 100vh;
  background-image: 
    radial-gradient(circle at 15% 50%, rgba(0, 229, 255, 0.05), transparent 25%),
    radial-gradient(circle at 85% 30%, rgba(255, 0, 127, 0.05), transparent 25%);
}

h1, h2, h3, h4, h5, h6 {
  font-family: 'Outfit', sans-serif;
}

/* Glassmorphism Panel Utilities */
.glass-panel {
  background: var(--bg-panel);
  backdrop-filter: blur(16px);
  -webkit-backdrop-filter: blur(16px);
  border: 1px solid var(--glass-border);
  border-radius: 20px;
  box-shadow: var(--glass-shadow);
  padding: 24px;
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}

.glass-panel:hover {
  transform: translateY(-2px);
  box-shadow: 0 12px 40px 0 rgba(0, 229, 255, 0.1);
}

/* Animations */
@keyframes pulse-ring {
  0% { transform: scale(0.8); opacity: 0.5; }
  100% { transform: scale(2); opacity: 0; }
}

@keyframes float {
  0% { transform: translateY(0px); }
  50% { transform: translateY(-10px); }
  100% { transform: translateY(0px); }
}

@keyframes scanline {
  0% { top: -10%; }
  100% { top: 110%; }
}

/* Gradient Text */
.text-gradient {
  background: linear-gradient(90deg, var(--accent-cyan), var(--accent-magenta));
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

/* Custom Scrollbar */
::-webkit-scrollbar {
  width: 8px;
}
::-webkit-scrollbar-track {
  background: var(--bg-primary);
}
::-webkit-scrollbar-thumb {
  background: var(--bg-secondary);
  border-radius: 4px;
}
::-webkit-scrollbar-thumb:hover {
  background: var(--text-secondary);
}

/* Interactive Elements */
button {
  cursor: pointer;
  font-family: 'Inter', sans-serif;
  border: none;
  outline: none;
}

.btn-primary {
  background: linear-gradient(135deg, var(--accent-cyan), #0088FF);
  color: #000;
  font-weight: 700;
  padding: 12px 28px;
  border-radius: 12px;
  transition: all 0.3s ease;
  font-size: 1rem;
  box-shadow: 0 4px 15px rgba(0, 229, 255, 0.3);
}

.btn-primary:hover {
  transform: translateY(-2px) scale(1.02);
  box-shadow: 0 8px 25px rgba(0, 229, 255, 0.5);
}

.btn-icon {
  background: rgba(255, 255, 255, 0.05);
  border: 1px solid var(--glass-border);
  color: var(--text-primary);
  width: 48px;
  height: 48px;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.3s ease;
}

.btn-icon:hover {
  background: rgba(255, 255, 255, 0.1);
  transform: scale(1.05);
  border-color: var(--accent-cyan);
  color: var(--accent-cyan);
}

/* Layout Utilities */
.container {
  max-width: 1400px;
  margin: 0 auto;
  padding: 0 24px;
}

.flex-between {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.flex-center {
  display: flex;
  justify-content: center;
  align-items: center;
}

.grid-2 {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 24px;
}

.grid-3 {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
  gap: 24px;
}

/* Status dots */
.status-dot {
  width: 10px;
  height: 10px;
  border-radius: 50%;
  display: inline-block;
  position: relative;
}

.status-dot.active {
  background-color: var(--success);
  box-shadow: 0 0 10px var(--success);
}

.status-dot.active::after {
  content: '';
  position: absolute;
  top: -4px;
  left: -4px;
  right: -4px;
  bottom: -4px;
  border-radius: 50%;
  border: 2px solid var(--success);
  animation: pulse-ring 2s cubic-bezier(0.215, 0.61, 0.355, 1) infinite;
}

.status-dot.warning {
  background-color: var(--accent-yellow);
  box-shadow: 0 0 10px var(--accent-yellow);
}

.status-dot.danger {
  background-color: var(--danger);
  box-shadow: 0 0 10px var(--danger);
}

``

## src\components\Dashboard.jsx
``javascript
import React, { useState, useEffect, useRef } from 'react';
import VisionCamera from './VisionCamera';
import VoiceVisualizer from './VoiceVisualizer';
import { Navigation, AlertTriangle, Headphones } from 'lucide-react';

const Dashboard = () => {
  const [detectedObstacles, setDetectedObstacles] = useState([]);
  const [recentCues, setRecentCues] = useState([]);
  const lastSpokenTime = useRef(0);

  const speak = (text) => {
    const now = Date.now();
    // Don't spam speech, wait at least 5 seconds between cues
    if (now - lastSpokenTime.current < 5000) return;
    
    if ('speechSynthesis' in window) {
      const utterance = new SpeechSynthesisUtterance(text);
      utterance.rate = 1.1; // slightly faster
      window.speechSynthesis.speak(utterance);
      lastSpokenTime.current = now;

      // Add to log
      setRecentCues(prev => {
        const newCues = [{
          time: new Date().toLocaleTimeString([], { hour: '2-digit', minute: '2-digit' }),
          msg: text,
          type: text.includes('Caution') ? 'warning' : 'info'
        }, ...prev].slice(0, 5); // keep last 5
        return newCues;
      });
    }
  };

  const handleDetections = (data) => {
    // Check if data has the new format or old format (backward compatibility)
    const predictions = data.predictions || data;
    const videoWidth = data.videoWidth || 640;

    // Filter out low confidence
    const confidentPredictions = predictions.filter(p => p.score > 0.6);
    setDetectedObstacles(confidentPredictions);

    if (confidentPredictions.length > 0) {
      // Find the largest object by area
      const largest = confidentPredictions.reduce((prev, current) => {
        const prevArea = prev.bbox[2] * prev.bbox[3];
        const currArea = current.bbox[2] * current.bbox[3];
        return (currArea > prevArea) ? current : prev;
      });

      // Calculate position
      const centerX = largest.bbox[0] + (largest.bbox[2] / 2);
      let direction = "straight ahead";
      if (centerX < videoWidth * 0.33) {
        direction = "on your left";
      } else if (centerX > videoWidth * 0.66) {
        direction = "on your right";
      }

      speak(`Caution, ${largest.class} detected ${direction}.`);
    }
  };

  return (
    <div className="container" style={{ padding: '40px 24px' }}>
      <header style={{ marginBottom: '40px' }}>
        <h2 style={{ fontSize: '2.5rem', marginBottom: '8px' }}>Real-World <span className="text-gradient">Assistant</span></h2>
        <p style={{ color: 'var(--text-secondary)', fontSize: '1.1rem' }}>Live camera object detection and auditory feedback.</p>
      </header>

      <div style={{ display: 'grid', gridTemplateColumns: '1fr 350px', gap: '32px' }}>
        
        {/* Main Feed area */}
        <div style={{ display: 'flex', flexDirection: 'column', gap: '32px' }}>
          
          {detectedObstacles.length > 0 ? (
            <div className="glass-panel" style={{ 
              background: 'rgba(255, 59, 48, 0.1)', 
              border: '1px solid var(--danger)',
              display: 'flex',
              alignItems: 'center',
              justifyContent: 'space-between'
            }}>
              <div style={{ display: 'flex', alignItems: 'center', gap: '16px' }}>
                <div style={{ backgroundColor: 'var(--danger)', padding: '12px', borderRadius: '50%', color: '#fff', animation: 'pulse-ring 2s infinite' }}>
                  <AlertTriangle size={24} />
                </div>
                <div>
                  <h3 style={{ color: 'var(--danger)', fontSize: '1.2rem', fontWeight: 700 }}>Obstacle Detected!</h3>
                  <p style={{ color: 'var(--text-primary)' }}>
                    {detectedObstacles.map(o => o.class).join(', ')} in path.
                  </p>
                </div>
              </div>
            </div>
          ) : (
             <div className="glass-panel" style={{ 
              background: 'rgba(52, 199, 89, 0.1)', 
              border: '1px solid var(--success)',
              display: 'flex',
              alignItems: 'center',
              justifyContent: 'space-between'
            }}>
               <div style={{ display: 'flex', alignItems: 'center', gap: '16px' }}>
                <div style={{ backgroundColor: 'var(--success)', padding: '12px', borderRadius: '50%', color: '#fff' }}>
                  <Navigation size={24} />
                </div>
                <div>
                  <h3 style={{ color: 'var(--success)', fontSize: '1.2rem', fontWeight: 700 }}>Path Clear</h3>
                  <p style={{ color: 'var(--text-primary)' }}>No immediate obstacles detected.</p>
                </div>
              </div>
            </div>
          )}

          {/* Camera Feed */}
          <VisionCamera onDetect={handleDetections} />
          
        </div>

        {/* Sidebar */}
        <div style={{ display: 'flex', flexDirection: 'column', gap: '32px' }}>
          <VoiceVisualizer />

          <div className="glass-panel">
            <h3 style={{ fontSize: '1.2rem', marginBottom: '20px', display: 'flex', alignItems: 'center', gap: '10px' }}>
              <Headphones size={20} color="var(--accent-cyan)" />
              Recent Audio Cues
            </h3>
            {recentCues.length === 0 ? (
               <p style={{ color: 'var(--text-secondary)', fontSize: '0.9rem' }}>No auditory cues yet.</p>
            ) : (
              <ul style={{ listStyle: 'none', display: 'flex', flexDirection: 'column', gap: '16px' }}>
                {recentCues.map((cue, i) => (
                  <li key={i} style={{ 
                    padding: '16px', 
                    background: 'var(--bg-primary)', 
                    borderRadius: '12px',
                    borderLeft: `4px solid ${
                      cue.type === 'warning' ? 'var(--accent-yellow)' : 
                      cue.type === 'success' ? 'var(--success)' : 'var(--accent-cyan)'
                    }`
                  }}>
                    <p style={{ fontSize: '0.9rem', marginBottom: '4px' }}>"{cue.msg}"</p>
                    <span style={{ fontSize: '0.75rem', color: 'var(--text-secondary)' }}>{cue.time}</span>
                  </li>
                ))}
              </ul>
            )}
          </div>
        </div>

      </div>
    </div>
  );
};

export default Dashboard;

``

## src\components\VisionCamera.jsx
``javascript
import React, { useRef, useEffect, useState } from 'react';
import * as tf from '@tensorflow/tfjs';
import * as cocossd from '@tensorflow-models/coco-ssd';
import { Camera, AlertTriangle } from 'lucide-react';

const VisionCamera = ({ onDetect }) => {
  const videoRef = useRef(null);
  const canvasRef = useRef(null);
  const [model, setModel] = useState(null);
  const [loading, setLoading] = useState(true);

  const [errorMsg, setErrorMsg] = useState(null);

  // Load the model
  useEffect(() => {
    const loadModel = async () => {
      try {
        await tf.ready();
        const loadedModel = await cocossd.load();
        setModel(loadedModel);
        setLoading(false);
      } catch (err) {
        console.error("Failed to load model", err);
        setErrorMsg(err.toString());
        setLoading(false);
      }
    };
    loadModel();
  }, []);

  // Setup Camera
  useEffect(() => {
    if (navigator.mediaDevices && navigator.mediaDevices.getUserMedia) {
      const webCamPromise = navigator.mediaDevices
        .getUserMedia({
          video: { facingMode: "environment" }
        })
        .then(stream => {
          window.stream = stream;
          if (videoRef.current) {
            videoRef.current.srcObject = stream;
            return new Promise((resolve) => {
              videoRef.current.onloadedmetadata = () => {
                resolve();
              };
            });
          }
        });
    }
  }, []);

  // Detect loop
  useEffect(() => {
    let animationId;
    const detectFrame = async () => {
      if (videoRef.current && canvasRef.current && model && videoRef.current.readyState === 4) {
        const video = videoRef.current;
        const predictions = await model.detect(video);
        
        drawPredictions(predictions);
        
        if (onDetect && predictions.length > 0) {
          onDetect({ predictions, videoWidth: video.videoWidth });
        }
      }
      animationId = requestAnimationFrame(detectFrame);
    };

    if (!loading && model) {
      detectFrame();
    }

    return () => {
      if (animationId) cancelAnimationFrame(animationId);
    };
  }, [loading, model, onDetect]);

  const drawPredictions = (predictions) => {
    if (!canvasRef.current || !videoRef.current) return;
    const ctx = canvasRef.current.getContext('2d');
    
    // Match canvas size to video size
    canvasRef.current.width = videoRef.current.videoWidth;
    canvasRef.current.height = videoRef.current.videoHeight;
    
    ctx.clearRect(0, 0, ctx.canvas.width, ctx.canvas.height);
    
    // Draw predictions
    predictions.forEach(prediction => {
      const [x, y, width, height] = prediction.bbox;
      const text = `${prediction.class} - ${Math.round(prediction.score * 100)}%`;

      // Set styling
      ctx.strokeStyle = '#00F0FF';
      ctx.lineWidth = 4;
      ctx.fillStyle = '#00F0FF';
      ctx.font = '18px Arial';

      // Draw bounding box
      ctx.strokeRect(x, y, width, height);

      // Draw text background
      const textWidth = ctx.measureText(text).width;
      const textHeight = parseInt(ctx.font, 10); // base 10
      ctx.fillRect(x, y, textWidth + 4, textHeight + 4);

      // Draw text
      ctx.fillStyle = '#000000';
      ctx.fillText(text, x, y + textHeight);
    });
  };

  return (
    <div className="glass-panel" style={{ padding: '0', overflow: 'hidden', position: 'relative', display: 'flex', flexDirection: 'column' }}>
      <div style={{ padding: '16px 24px', borderBottom: '1px solid var(--glass-border)', display: 'flex', justifyContent: 'space-between', alignItems: 'center' }}>
        <h3 style={{ display: 'flex', alignItems: 'center', gap: '8px', fontSize: '1.2rem', margin: 0 }}>
          <Camera size={20} color="var(--accent-cyan)" /> Real-Time Environment Scanner
        </h3>
        {loading && <span className="text-gradient">Loading AI Model...</span>}
        {errorMsg && <span style={{ color: 'var(--danger)' }}>Error: {errorMsg}</span>}
      </div>
      
      <div style={{ position: 'relative', width: '100%', backgroundColor: '#000', aspectRatio: '16/9', display: 'flex', justifyContent: 'center', alignItems: 'center' }}>
        <video
          ref={videoRef}
          autoPlay
          playsInline
          muted
          style={{ width: '100%', height: '100%', objectFit: 'cover', position: 'absolute' }}
        />
        <canvas
          ref={canvasRef}
          style={{ width: '100%', height: '100%', objectFit: 'cover', position: 'absolute', pointerEvents: 'none' }}
        />
      </div>
    </div>
  );
};

export default VisionCamera;

``

## src\components\VoiceVisualizer.jsx
``javascript
import React, { useEffect, useState } from 'react';

const VoiceVisualizer = () => {
  const [isListening, setIsListening] = useState(false);
  const [transcript, setTranscript] = useState('');
  const [bars, setBars] = useState(Array(12).fill(10));
  
  // Animation for bars
  useEffect(() => {
    if (!isListening) {
      setBars(Array(12).fill(10));
      return;
    }
    const interval = setInterval(() => {
      setBars(bars => bars.map(() => Math.floor(Math.random() * 40) + 10));
    }, 100);
    return () => clearInterval(interval);
  }, [isListening]);

  const toggleListen = () => {
    if (isListening) {
      setIsListening(false);
      return;
    }

    if (!('webkitSpeechRecognition' in window)) {
      alert("Speech recognition not supported in this browser. Try Chrome.");
      return;
    }

    const recognition = new window.webkitSpeechRecognition();
    recognition.continuous = false;
    recognition.interimResults = true;

    recognition.onstart = () => {
      setIsListening(true);
      setTranscript('');
    };

    recognition.onresult = (event) => {
      let currentTranscript = '';
      for (let i = event.resultIndex; i < event.results.length; i++) {
        currentTranscript += event.results[i][0].transcript;
      }
      setTranscript(currentTranscript);
    };

    recognition.onerror = (event) => {
      console.error(event.error);
      setIsListening(false);
    };

    recognition.onend = () => {
      setIsListening(false);
      // Here you would process the command, e.g. "navigate to kitchen"
      if (transcript.toLowerCase().includes('what is in front of me')) {
         // trigger vision read
      }
    };

    recognition.start();
  };

  return (
    <div 
      className="glass-panel flex-center" 
      style={{ padding: '32px', flexDirection: 'column', gap: '24px', cursor: 'pointer' }}
      onClick={toggleListen}
    >
      <div style={{ display: 'flex', alignItems: 'center', gap: '6px', height: '60px' }}>
        {bars.map((height, i) => (
          <div 
            key={i} 
            style={{ 
              width: '8px', 
              height: `${height}px`, 
              backgroundColor: isListening ? 'var(--accent-cyan)' : 'var(--text-secondary)',
              borderRadius: '4px',
              transition: 'height 0.1s ease, background-color 0.3s ease',
              boxShadow: isListening ? '0 0 10px var(--accent-cyan)' : 'none'
            }} 
          />
        ))}
      </div>
      <div style={{ textAlign: 'center' }}>
        <h3 style={{ fontSize: '1.2rem', marginBottom: '8px' }}>
          {isListening ? 'Listening...' : 'Voice Assistant Ready'}
        </h3>
        <p style={{ color: 'var(--text-secondary)', fontSize: '0.9rem', minHeight: '40px' }}>
          {isListening ? (transcript || 'Listening...') : 'Click to say "What is in front of me?"'}
        </p>
      </div>
    </div>
  );
};

export default VoiceVisualizer;

``

## electron\main.cjs
``javascript
const { app, BrowserWindow, systemPreferences } = require('electron');
const path = require('path');

const isDev = process.env.NODE_ENV === 'development';

async function createWindow() {
  // Request camera and microphone permissions on macOS (no-op on Windows/Linux but good practice)
  if (process.platform === 'darwin') {
    await systemPreferences.askForMediaAccess('camera');
    await systemPreferences.askForMediaAccess('microphone');
  }

  const mainWindow = new BrowserWindow({
    width: 1280,
    height: 800,
    webPreferences: {
      nodeIntegration: true,
      contextIsolation: false
    },
    autoHideMenuBar: true,
    title: "Lumina Assist - Desktop Mode"
  });

  if (isDev) {
    // In development, load the Vite dev server
    mainWindow.loadURL('http://localhost:5173');
    mainWindow.webContents.openDevTools();
  } else {
    // In production, load the built index.html
    mainWindow.loadFile(path.join(__dirname, '../dist/index.html'));
  }
}

app.whenReady().then(() => {
  createWindow();

  app.on('activate', () => {
    if (BrowserWindow.getAllWindows().length === 0) {
      createWindow();
    }
  });
});

app.on('window-all-closed', () => {
  if (process.platform !== 'darwin') {
    app.quit();
  }
});

``

## package.json
``json
{
  "name": "visual-assist-frontend",
  "private": true,
  "version": "0.0.0",
  "type": "module",
  "main": "electron/main.cjs",
  "scripts": {
    "dev": "vite",
    "build": "vite build",
    "lint": "eslint .",
    "preview": "vite preview",
    "electron:dev": "cross-env NODE_ENV=development concurrently \"npm run dev\" \"wait-on tcp:5173 && electron .\"",
    "electron:build": "npm run build && electron-builder"
  },
  "dependencies": {
    "@tensorflow-models/coco-ssd": "^2.2.3",
    "@tensorflow/tfjs": "^4.22.0",
    "lucide-react": "^1.14.0",
    "react": "^19.2.5",
    "react-dom": "^19.2.5"
  },
  "devDependencies": {
    "@eslint/js": "^10.0.1",
    "@types/react": "^19.2.14",
    "@types/react-dom": "^19.2.3",
    "@vitejs/plugin-react": "^6.0.1",
    "concurrently": "^9.2.1",
    "cross-env": "^10.1.0",
    "electron": "^41.5.0",
    "electron-builder": "^26.8.1",
    "eslint": "^10.2.1",
    "eslint-plugin-react-hooks": "^7.1.1",
    "eslint-plugin-react-refresh": "^0.5.2",
    "globals": "^17.5.0",
    "vite": "^8.0.10",
    "wait-on": "^9.0.5"
  }
}

``

