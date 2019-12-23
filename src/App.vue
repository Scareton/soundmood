<template>
    <v-app>
        <v-content>
            <scene></scene>
        </v-content>
    </v-app>
</template>

<script>
import scene from "./components/scene";
export default {
    name: "App",

    components: { scene },

    data: () => ({
        //
    })
};
</script>

<style>
body,
html {
    height: 100%;
    width:100%;
    overflow: hidden !important;
}
body {
    overflow: hidden;
}
.v-application {
  max-height: 100%;
}
</style>