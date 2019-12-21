<template>
    <div id="scene"></div>
</template>

<script>
import * as THREE from "three";
import { WEBGL } from "three/examples/jsm/WebGL.js";

export default {
    name: "Scene",
    data: () => ({}),
    mounted() {
        var SceneElement = document.getElementById("scene");
        var scene = new THREE.Scene();
        var camera = new THREE.PerspectiveCamera(
            75,
            document.body.scrollWidth / document.body.scrollHeight,
            0.1,
            1000
        );
        var renderer = new THREE.WebGLRenderer();
        renderer.setSize(document.body.scrollWidth, document.body.scrollHeight);

        SceneElement.appendChild(renderer.domElement);

        var geometry = new THREE.BoxGeometry(1, 1, 1);
        var material = new THREE.MeshBasicMaterial({ color: 0x00ff00 });
        var cube = new THREE.Mesh(geometry, material);
        scene.add(cube);

        camera.position.z = 5;

        var animate = function() {
            requestAnimationFrame(animate);

            cube.rotation.x += 0.01;
            cube.rotation.y += 0.01;

            renderer.render(scene, camera);
        };

        if (WEBGL.isWebGLAvailable()) {
            // Initiate function or other initializations here
            animate();
        } else {
            var warning = WEBGL.getWebGLErrorMessage();
            document.getElementById("container").appendChild(warning);
        }
    }
};
</script>

<style scoped>
</style>