<template>
  <div>{{ msg }}</div>
</template>

<script>
  export default {
    name: "LifeBox",
    props: ["msg"],
  };
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
  $boxSize: 0.5rem;

  div {
    background-color: cyan;
    border: 1px black solid;
    display: inline-block;
    height: $boxSize;
    margin: 0 0.25rem 0.125rem 0;
    width: $boxSize;

    &.--eating {
      background-color: orange;
      border: 1px gray solid;
    }
    &.--expired {
      background-color: white;
      border: 1px gray solid;
    }

    &.--sleeping {
      background-color: purple;
      border: 1px purple solid;
    }

    &.--working {
      background-color: yellow;
      border: 1px gray solid;
    }
  }
</style>
