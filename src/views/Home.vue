<template>
  <div class="home">
    <LifePlanner />
  </div>
</template>

<script>
  // @ is an alias to /src
  import LifePlanner from "@/components/LifePlanner.vue";

  export default {
    name: "Home",
    components: {
      LifePlanner,
    },
  };
</script>
