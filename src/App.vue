<template>
  <div id="app" class="container">
    <MyComponent />
  </div>
</template>

<script>
import MyComponent from "./components/MyComponent.vue";
export default {
  name: 'App',
  components: {
    MyComponent
  }
}
</script>