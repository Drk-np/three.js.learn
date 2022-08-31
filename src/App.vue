<script>
let scene = null,
  camera = null,
  renderer = null,
  mesh = null;
import * as Three from "three";
import { defineComponent, onMounted, reactive, ref, toRefs } from "vue";
import { OrbitControls } from "three/examples/jsm/controls/OrbitControls";
import * as dat from 'dat.gui'
import gsap from 'gsap'
export default defineComponent({
  setup() {

    const container = ref(null)
    const init = () => {
      camera = new Three.PerspectiveCamera(
        70,
        window.innerWidth / window.innerHeight,
        0.01,
        1000
      );
      camera.position.set(0, 0, 10)
      scene = new Three.Scene();
      let geometry = new Three.BoxGeometry(1, 1, 1);
      let material = new Three.MeshNormalMaterial({ color: 0xffff00 });

      const cube = new Three.Mesh(geometry, material)
      const gui = new dat.GUI()
      scene.add(cube)
      cube.scale.x = 2  //缩放

      gui.add(cube.position, 'x').min(0).max(5).step(1)
      //旋转
      // cube.rotation.set(Math.PI/4,0,0)

      // 坐标系
      const axesHelper = new Three.AxesHelper(5)
      scene.add(axesHelper)

      const renderer = new Three.WebGLRenderer()
      renderer.setSize(window.innerWidth, window.innerHeight)
      container.value.appendChild(renderer.domElement)
      // renderer.render(scene,camera)
      const controls = new OrbitControls(camera, renderer.domElement)
      controls.enableDamping = true
      // gsap.to(cube.position, { x: 5, duration: 5, ease: 'power1.inOut' })
      // gsap.to(cube.rotation, { x: 2 * Math.PI, duration: 5, ease: 'power1.inOut' })

      function render() {
        // cube.rotation.x += 0.1 // 旋转
        controls.update()
        renderer.render(scene, camera)

        requestAnimationFrame(render)
      }
      render()
    };

    onMounted(() => {
      init();
    });
    return {
      container
    }

  },
});
</script>

<template>
  <div ref="container"></div>
</template>

<style scoped>
</style>
