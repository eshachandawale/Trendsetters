*HTML
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>3D Avatar Fashion Viewer</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <div id="app">
        <h1>3D Avatar Fashion Viewer</h1>
        <select id="modelSelector">
            <option value="model1.glb">Model 1</option>
            <option value="model2.glb">Model 2</option>
        </select>
        <div id="avatar-container"></div>
    </div>
    <!-- Firebase -->
    <script src="https://www.gstatic.com/firebasejs/8.6.8/firebase-app.js"></script>
    <script src="https://www.gstatic.com/firebasejs/8.6.8/firebase-auth.js"></script>
    <script src="https://www.gstatic.com/firebasejs/8.6.8/firebase-firestore.js"></script>
    <!-- Three.js -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
    <!-- GLTF Loader -->
    <script src="https://cdn.jsdelivr.net/npm/three/examples/js/loaders/GLTFLoader.js"></script>
    <!-- OrbitControls -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/examples/js/controls/OrbitControls.js"></script>
    <!-- Custom script -->
    <script src="script.js"></script>
</body>
</html>





*Script
// Your Firebase configuration
const firebaseConfig = {
    apiKey: "YOUR_API_KEY",
    authDomain: "YOUR_AUTH_DOMAIN",
    projectId: "YOUR_PROJECT_ID",
    storageBucket: "YOUR_STORAGE_BUCKET",
    messagingSenderId: "YOUR_MESSAGING_SENDER_ID",
    appId: "YOUR_APP_ID"
};

// Initialize Firebase
firebase.initializeApp(firebaseConfig);
const auth = firebase.auth();
const db = firebase.firestore();

// Set up the scene, camera, and renderer
const scene = new THREE.Scene();
const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
const renderer = new THREE.WebGLRenderer({ antialias: true });
renderer.setSize(window.innerWidth, window.innerHeight);
document.getElementById('avatar-container').appendChild(renderer.domElement);

// Set camera position
camera.position.z = 2;

// Add a light source
const light = new THREE.AmbientLight(0x404040, 2); // Soft white light
scene.add(light);

// Add OrbitControls
const controls = new THREE.OrbitControls(camera, renderer.domElement);
controls.update();

// Load the initial model
const loader = new THREE.GLTFLoader();
loader.load('models/model1.glb', function (gltf) {
    scene.add(gltf.scene);
    animate();
}, undefined, function (error) {
    console.error(error);
});

// Handle model selection
document.getElementById('modelSelector').addEventListener('change', (event) => {
    const model = event.target.value;
    loader.load(`models/${model}`, function (gltf) {
        scene.clear(); // Clear previous model
        scene.add(gltf.scene);
        animate();
    }, undefined, function (error) {
        console.error(error);
    });
});

// Render loop
function animate() {
    requestAnimationFrame(animate);
    controls.update(); // Only required if controls.enableDamping = true, or if controls.autoRotate = true
    renderer.render(scene, camera);
}





*CSS
body {
    font-family: Arial, sans-serif;
    margin: 0;
    padding: 0;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    height: 100vh;
    background-color: #f0f0f0;
}

#app {
    text-align: center;
}

#avatar-container {
    width: 100%;
    height: 80vh;
}

select {
    margin-bottom: 10px;
    padding: 5px;
    font-size: 16px;
}





*firebase.json

{
  "hosting": {
    "public": "public",
    "ignore": [
      "firebase.json",
      "**/.*",
      "**/node_modules/**"
    ],
    "rewrites": [
      {
        "source": "**",
        "destination": "/index.html"
      }
    ]
  }
}








*Deploy
# Install Firebase CLI
npm install -g firebase-tools

# Log in to Firebase
firebase login

# Initialize Firebase in your project directory
firebase init

# Serve the project locally
firebase serve

# Deploy the project to Firebase Hosting
firebase deploy
