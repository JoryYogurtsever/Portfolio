<template>
  <div class="project">
    <table>
      <th class="tableColumn" v-show="!alternator">
        <img :src="image" class="projectImage">
      </th>
      <th class="tableColumn">
        <h1><a class="unitTitle" :href="link"> {{ title }} </a></h1>
        <p class="projectEntry">
          {{ text }}
        </p>
        <img class="small-logo" src="../../static/html5-with-wordmark-color.svg">
        <img class="small-logos" src="../../static/javascript.svg">
        <img class="small-logo" src="../../static/vuejs-seeklogo.com.svg">
      </th>
      <th class="tableColumn" v-show="alternator">
        <img :src="image" class="projectImage">
      </th>
    </table>
  </div>
</template>

<script>
  export default {
    data: function () {
      return {
        alternator: this.left
    }
    },
    props: [
      'image',
      'title',
      'text',
      'left',
      'link'
    ],
    mounted: () => {
    if (this.left <= "1") {
      this.alternator = true
    }
  }
  }
</script>

<style>
  .unitTitle {
    color: darkgreen;
    text-decoration: none;
  }
  .unitTitle:hover {
    color: goldenrod;
  }
  .projectImage {
    width: 300px;
    height: 300px;
  }
  .tableColumn {
    vertical-align: middle;
    width: 50%
  }
  .small-logo {
    padding-bottom: 7px;
    height: 40px;
    width: 40px;
  }
  .small-logos {
    margin-top: 0;
    height: 55px;
    width: 55px;
  }
  .projectEntry {
    color: olive;
  }
</style>
