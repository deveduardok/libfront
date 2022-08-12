<template>
  <div class="text-center">
    <h1>BIBLIOTECA FATEC</h1>
    <p>Bem vindo!</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      componentKey: 0,
    };
  },
  methods: {
    forceRerender() {
      this.componentKey += 1;
    },
  },
};
</script>
