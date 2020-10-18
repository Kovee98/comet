<template>
    <section class="section">
        <div class="container">
            <h1 class="title">
                Hello World
            </h1>
            <p class="subtitle">
                My first website with <strong>Bulma</strong>!
            </p>
            <img alt="Vue logo" src="./assets/logo.png" />
            <HelloWorld msg="Hello Vue 3.0 + Vite" />
        </div>
    </section>
</template>

<script lang="ts">
    import { defineComponent } from 'vue';
    import HelloWorld from './components/HelloWorld.vue';

    export default defineComponent({
        name: 'App',
        components: {
            HelloWorld
        }
    });
</script>
