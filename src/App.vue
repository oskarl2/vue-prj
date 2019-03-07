<template>
  <div id="app">
    <v-main></v-main>
  </div>
</template>

<script>
import Main from './components/Main.vue';
import './styles/reset.css';

export default {
  name: 'app',

  components: {
    'v-main': Main,
  }
}
</script>