# 🔷 Iron Hologram Lab

> **Gesture-controlled 3D cube interface inspired by Iron Man's holographic displays**

[![Live Demo](https://img.shields.io/badge/Demo-Live-00f3ff?style=for-the-badge)](https://harshitha027-gif.github.io/jarvis-gesture-dashboard/)
[![License](https://img.shields.io/badge/License-MIT-31ffa5?style=for-the-badge)](LICENSE)

![Iron Holo Demo](https://via.placeholder.com/800x400/000/00f3ff?text=Iron+Holo+Demo+GIF)
*Replace with actual demo GIF or screenshot*

---

## ✨ Overview

**Iron Holo** brings futuristic gesture control to your browser! Using real-time hand tracking powered by MediaPipe and immersive 3D graphics via Three.js, you can manipulate a neon-glowing holographic cube with intuitive hand gestures—no mouse, no keyboard, just your hands.

Perfect for exploring computer vision, gesture recognition, and interactive 3D interfaces.

---

## 🎯 Key Features

- **🖐️ Real-time Hand Tracking**: Powered by [MediaPipe Hands](https://google.github.io/mediapipe/solutions/hands.html)
- **🎨 Stunning 3D Visuals**: Built with [Three.js](https://threejs.org/) featuring neon wireframes and holographic effects
- **🎮 Intuitive Gestures**:
  - **Pinch** to grab and move the cube
  - **Twist** your pinched hand to rotate around Z-axis
  - **Peace sign (✌️)** to toggle rotation mode
  - **Two-hand pinch** for dynamic scaling
- **💎 Iron Man Aesthetics**: Cyan/neon color scheme with futuristic HUD
- **📱 Responsive Design**: Works on desktop and mobile browsers with camera access
- **⚡ Smooth Interactions**: Lerp-based smoothing for fluid motion

---

## 🚀 Quick Start

### Prerequisites
- Modern web browser (Chrome, Edge, Firefox, Safari)
- Webcam access
- Good lighting and clear background for optimal hand detection

### Run Locally

1. **Clone the repository**
   ```bash
   git clone https://github.com/harshitha027-gif/jarvis-gesture-dashboard.git
   cd jarvis-gesture-dashboard
   ```

2. **Open the app**
   - Simply open `index.html` in your browser
   - Or use a local server:
     ```bash
     # Python 3
     python -m http.server 8000
     
     # Node.js (with http-server)
     npx http-server
     ```

3. **Grant camera permissions** when prompted

4. **Start gesturing!** 🎉

---

## 🕹️ Gesture Controls

| Gesture | Action | Description |
|---------|--------|-------------|
| 👉 **Single Pinch** | Move | Pinch thumb + index finger to grab and drag the cube in 3D space |
| 🌀 **Twist (Pinched)** | Z-Rotation | Rotate your pinched hand to spin the cube around its Z-axis |
| ✌️ **Peace Sign** | Toggle Mode | Switch between **Move** and **Rotate** modes (cooldown: 800ms) |
| 🎯 **Pinch + Drag (Rotate Mode)** | Orbit | Drag to rotate cube around X and Y axes |
| 👐 **Two-Hand Pinch** | Scale | Pinch with both hands and move them apart/together to scale the cube |
| 🤚 **Open Palm** | Idle | Release pinch to enable idle rotation animation |

---

## 🛠️ Technical Details

### Tech Stack

| Technology | Purpose |
|------------|----------|
| **[MediaPipe Hands](https://google.github.io/mediapipe/solutions/hands.html)** | Real-time hand landmark detection (21 keypoints per hand) |
| **[Three.js](https://threejs.org/)** | 3D rendering, lighting, and scene management |
| **Vanilla JavaScript** | Core application logic and gesture recognition |
| **HTML5 Canvas** | Hand skeleton overlay rendering |
| **CSS3** | Holographic UI styling and effects |

### Architecture

```
index.html (645 lines)
├── MediaPipe Integration
│   ├── Camera stream capture
│   ├── Hand detection (max 2 hands)
│   └── 21-point landmark tracking
├── Gesture Recognition Engine
│   ├── Pinch detection (hysteresis: 0.40/0.52)
│   ├── Peace sign detection
│   └── Hand angle calculation
├── Three.js Scene
│   ├── PerspectiveCamera (FOV: 45°)
│   ├── Neon cube (mesh + wireframe + glow)
│   ├── GridHelper (holographic floor)
│   └── Dynamic lighting
└── Interaction System
    ├── Canvas → 3D world raycasting
    ├── Smooth lerp interpolation (α=0.35)
    └── Multi-hand coordination
```

### Core Logic Highlights

**Pinch Detection**
```javascript
// Ratio-based pinch detection with hysteresis
const ratio = distance(thumb, index) / distance(wrist, indexMCP);
if (ratio < 0.40) → Pinch ON
if (ratio > 0.52) → Pinch OFF
```

**3D Projection**
```javascript
// Convert 2D canvas coords to 3D world space
canvasToWorldOnZ(xCanvas, yCanvas, zWorld) {
  const ray = new THREE.Raycaster();
  const plane = new THREE.Plane(new THREE.Vector3(0, 0, 1), -zWorld);
  ray.ray.intersectPlane(plane, worldPoint);
}
```

**Smooth Motion**
```javascript
// Linear interpolation for fluid animations
cubeGroup.position.lerp(targetPosition, 0.35);
cubeGroup.rotation.z = lerpAngle(current, target, 0.35);
```

---

## 📂 File Structure

```
jarvis-gesture-dashboard/
├── index.html          # Complete application (HTML + CSS + JS)
├── README.md           # This file
└── LICENSE             # MIT License (optional)
```

*Single-file architecture for easy deployment and sharing!*

---

## 🎨 Customization

### Change Colors
```javascript
// Line ~300: Cube colors
const meshMat = new THREE.MeshStandardMaterial({
  color: 0x033a4a,        // Dark teal
  emissive: 0x0094ff,     // Cyan glow
});
```

### Adjust Gesture Sensitivity
```javascript
// Line ~50: Pinch thresholds
this.PINCH_ON = 0.40;   // Lower = easier to activate
this.PINCH_OFF = 0.52;  // Higher = less jittery
```

### Modify Smoothing
```javascript
// Line ~51: Interpolation alpha
this.smoothAlpha = 0.35; // 0=instant, 1=very smooth
```

---

## 🤝 Contributing

Contributions are welcome! Here's how you can help:

1. **Fork** the repository
2. **Create** a feature branch
   ```bash
   git checkout -b feature/AmazingFeature
   ```
3. **Commit** your changes
   ```bash
   git commit -m 'Add some AmazingFeature'
   ```
4. **Push** to the branch
   ```bash
   git push origin feature/AmazingFeature
   ```
5. **Open** a Pull Request

### Ideas for Contributions
- Add more gestures (fist, thumbs up, etc.)
- Support for multiple objects
- Color picker gesture interface
- VR/AR integration
- Mobile touch fallback
- Performance optimizations

---

## 🐛 Troubleshooting

| Issue | Solution |
|-------|----------|
| **Camera not working** | Check browser permissions and use HTTPS (required for camera access) |
| **Hand not detected** | Ensure good lighting and clear background; try adjusting `minDetectionConfidence` |
| **Laggy performance** | Lower `maxNumHands` or reduce `modelComplexity` in line ~80 |
| **Pinch too sensitive** | Increase `PINCH_ON` threshold (line ~50) |

---

## 📜 License

This project is open source and available under the [MIT License](LICENSE).

---

## 🙏 Acknowledgments

- **[MediaPipe](https://google.github.io/mediapipe/)** by Google for incredible hand tracking
- **[Three.js](https://threejs.org/)** community for powerful 3D web graphics
- **Iron Man** franchise for the holographic interface inspiration
- All contributors and supporters of this project

---

## 📬 Contact

**Created by** [harshitha027-gif](https://github.com/harshitha027-gif)

Feel free to reach out for questions, suggestions, or collaborations!

---

<div align="center">

### ⭐ Star this repo if you found it useful!

**[🔗 Live Demo](https://harshitha027-gif.github.io/jarvis-gesture-dashboard/)** | **[🐛 Report Bug](https://github.com/harshitha027-gif/jarvis-gesture-dashboard/issues)** | **[💡 Request Feature](https://github.com/harshitha027-gif/jarvis-gesture-dashboard/issues)**

*Built with 💙 and futuristic vibes*

</div>
