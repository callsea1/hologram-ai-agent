<script lang="ts">
  import { onMount } from "svelte";
  import * as THREE from "three";
  import { OrbitControls } from "three/examples/jsm/controls/OrbitControls";

  export let emotion:
    | "happy"
    | "sad"
    | "surprised"
    | "thoughtful"
    | "confident" = "happy";

  let container: HTMLDivElement;
  let scene: THREE.Scene;
  let camera: THREE.PerspectiveCamera;
  let renderer: THREE.WebGLRenderer;
  let controls: OrbitControls;
  let face: THREE.Mesh;

  onMount(() => {
    // Scene setup
    scene = new THREE.Scene();
    camera = new THREE.PerspectiveCamera(
      75,
      container.clientWidth / container.clientHeight,
      0.1,
      1000
    );
    renderer = new THREE.WebGLRenderer({ antialias: true, alpha: true });
    renderer.setSize(container.clientWidth, container.clientHeight);
    container.appendChild(renderer.domElement);

    // Add holographic effect
    const hologramShader = {
      uniforms: {
        time: { value: 0 },
        color: { value: new THREE.Color(0x00ffff) },
      },
      vertexShader: `
        varying vec2 vUv;
        void main() {
          vUv = uv;
          gl_Position = projectionMatrix * modelViewMatrix * vec4(position, 1.0);
        }
      `,
      fragmentShader: `
        uniform float time;
        uniform vec3 color;
        varying vec2 vUv;
        void main() {
          float noise = fract(sin(dot(vUv, vec2(12.9898, 78.233))) * 43758.5453);
          float scanline = sin(vUv.y * 800.0 + time * 2.0) * 0.1 + 0.9;
          vec3 finalColor = color * scanline * (0.8 + noise * 0.2);
          gl_FragColor = vec4(finalColor, 0.8);
        }
      `,
    };

    // Create a basic face geometry (placeholder)
    const geometry = new THREE.SphereGeometry(1, 32, 32);
    const material = new THREE.ShaderMaterial({
      uniforms: hologramShader.uniforms,
      vertexShader: hologramShader.vertexShader,
      fragmentShader: hologramShader.fragmentShader,
      transparent: true,
    });
    face = new THREE.Mesh(geometry, material);
    scene.add(face);

    // Add lights
    const ambientLight = new THREE.AmbientLight(0xffffff, 0.5);
    scene.add(ambientLight);
    const directionalLight = new THREE.DirectionalLight(0xffffff, 0.8);
    directionalLight.position.set(5, 5, 5);
    scene.add(directionalLight);

    // Camera position
    camera.position.z = 3;

    // Controls
    controls = new OrbitControls(camera, renderer.domElement);
    controls.enableDamping = true;
    controls.dampingFactor = 0.05;

    // Animation loop
    function animate() {
      requestAnimationFrame(animate);
      hologramShader.uniforms.time.value += 0.01;
      controls.update();
      renderer.render(scene, camera);
    }
    animate();

    // Handle window resize
    window.addEventListener("resize", () => {
      camera.aspect = container.clientWidth / container.clientHeight;
      camera.updateProjectionMatrix();
      renderer.setSize(container.clientWidth, container.clientHeight);
    });

    return () => {
      window.removeEventListener("resize", () => {});
      container.removeChild(renderer.domElement);
      renderer.dispose();
    };
  });

  // Update emotion
  $: if (face) {
    // This is a placeholder for emotion changes
    // In a real implementation, you would morph the face geometry
    // based on the emotion value
    console.log("Emotion changed to:", emotion);
  }
</script>

<div bind:this={container} class="hologram-container">
  <!-- Three.js canvas will be inserted here -->
</div>

<style>
  .hologram-container {
    width: 100%;
    height: 400px;
    position: relative;
    background: rgba(0, 0, 0, 0.1);
    border-radius: 8px;
    overflow: hidden;
  }
</style>
