<template>
  <div id="container"></div>
</template>

<script>
import * as THREE from "three";
import { OrbitControls } from "three/examples/jsm/controls/OrbitControls";

export default {
  name: "Scene",
  data() {
    return {
      camera: null,
      controls: null,
      scene: null,
      renderer: null,
      loader: null,
      mesh: null,
      global_light: null,
    };
  },
  methods: {
    init() {
      let container = document.getElementById("container");

      // Создание камеры, сцены и отображения
      this.camera = new THREE.PerspectiveCamera(
        75,
        window.innerWidth / window.innerHeight,
        0.01,
        1000
      );
      this.camera.position.set(-65, 50, 50);

      this.scene = new THREE.Scene();

      this.renderer = new THREE.WebGLRenderer({ antialias: true });
      this.renderer.setSize(window.innerWidth, window.innerHeight);
      // -----
      
      // Включение управления мышью
      this.controls = new OrbitControls(this.camera, this.renderer.domElement);

      // Добавление canvas в DOM
      container.appendChild(this.renderer.domElement);
    },
    /**
     * Функция отрисовки каждого кадра
     */
    animate() {
      requestAnimationFrame(this.animate);
      this.controls.update(); //controls.update() Должно быть вызвано после каждого ручного изменения положения камеры
      this.scene.rotation.y += 0.0005; // Поворот сцены по оси Y
      this.renderer.render(this.scene, this.camera); // Отрисовка кадра в отображении
    },
    /**
     * Загрузка и добавление объектов на сцену
     */
    LoadObjects() {
      // Создание глобального источника света
      this.global_light = new THREE.DirectionalLight(0xffffff, 1);
      this.global_light.position.set(50,50,50);
      this.scene.add(this.global_light);
      // -----

      // Объявление лоадера и получение сцены из локального json файла
      var loader = new THREE.ObjectLoader();
      loader.load(
        "http://localhost:8080/objects/scene.json",
        obj => {
          this.scene.add(obj);
        },

        // Прогресс загрузки в процентах
        function(xhr) {
          console.log((xhr.loaded / xhr.total) * 100 + "% loaded");
        },
        function(err) {
          console.error("An error happened", err);
        }
      );
      // -----

      // Создание частиц на сцене
      var particle = new THREE.Object3D();
      this.scene.add(particle); // "Слой", к которому будут добавляться частицы. 

      // Создание материала и геометрии для частиц
      var material = new THREE.MeshPhongMaterial({
        color: 0xffffff,
        shading: THREE.FlatShading
      });
      var geometry = new THREE.TetrahedronGeometry(2, 0);
      // -

      for (var i = 0; i < 500; i++) {
        var mesh = new THREE.Mesh(geometry, material);
        mesh.position.set(Math.random() - 0.5, Math.random() - 0.5, Math.random() - 0.5).normalize();
        mesh.position.multiplyScalar(90 + (Math.random() * 700));
        mesh.rotation.set(Math.random() * 2, Math.random() * 2, Math.random() * 2);
        particle.add(mesh);
      }
      // -----
    }
  },
  mounted() {
    this.init();
    this.animate();

    this.LoadObjects();

    // Изменение размеров canvas и перерисовка сцены при изменении размеров окна
    window.addEventListener(
      "resize",
      () => {
        this.camera.aspect = window.innerWidth / window.innerHeight;
        this.camera.updateProjectionMatrix();

        this.renderer.setSize(window.innerWidth, window.innerHeight);
      },
      true
    );
    // -----
  }
};
</script>

<style scoped>
#container {
  height: 100%;
  width: 100%;
}
</style>