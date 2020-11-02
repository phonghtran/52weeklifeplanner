<template>
  <div>
    <label for="name">Activity {{ parseInt(offset) + 1 }}</label>
    <input
      @keyup="$emit('activityNewName', [activityName, offset])"
      id="name"
      type="text"
      placeholder="Activity Name"
      v-model="activityName"
    />
    <label for="count">Hours Per Week</label>
    <input id="count" type="text" placeholder="0" v-model="activityCount" />
    <button @click="$emit('activityDelete', offset)">
      Delete Activity
    </button>
  </div>
</template>

<script>
  export default {
    name: "Activity",
    props: ["count", "name", "offset"],
    data: function() {
      return {
        activityName: this.name,
        activityCount: this.count,
      };
    },
  };
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss"></style>
