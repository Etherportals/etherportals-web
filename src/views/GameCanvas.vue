<template>

  <div id="gamecanvas"></div>

</template>


<script>
import * as THREE from 'three'

export default {
  name: 'GameCanvas',
  data() {
    return {
      cube: null,
      renderer: null,
      scene: null,
      camera: null
    }
  },
  methods: {
    init: function() {
      this.scene = new THREE.Scene()
      this.camera = new THREE.PerspectiveCamera(
        75,
        window.innerWidth / window.innerHeight,
        0.1,
        1000
      )

      this.renderer = new THREE.WebGLRenderer()
      this.renderer.setSize(window.innerWidth, window.innerHeight)
      document.getElementById('gamecanvas').appendChild(this.renderer.domElement)

      const geometry = new THREE.BoxGeometry(1, 1, 1)
      const material = new THREE.MeshBasicMaterial({ color: 0x00ff00 })
      this.cube = new THREE.Mesh(geometry, material)
      this.scene.add(this.cube)

      this.camera.position.z = 5

      const animate = function() {}
    },
    animate: function() {
      requestAnimationFrame(this.animate)

      this.cube.rotation.x += 0.01
      this.cube.rotation.y += 0.01

      this.renderer.render(this.scene, this.camera)
    },
    teleportToRoom: function(roomId) {
      console.log(' -- teleport to room -- ',roomId);
      //clear all assets in the room

      //set the proper room size

      //teleport the player to the middle of the room


    }
  },
  mounted() {
    this.init()
    this.animate()
  }
}
</script>
