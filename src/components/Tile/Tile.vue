<template>
  <button class="btn btn-default" @click="handleClick(id)" v-bind:class="{ 'flash-green': status === 'win', 'flash-red': status === 'lose' }">
    <img alt="An image of a person getting hit in the face with something." v-bind:src="`assets/img/${filename}`" class="img-fluid" />
  </button>
</template>

<script>
  import './Tile.css';

  export default {
    name: 'tile',
    props: ['filename', 'id', 'status', 'handleClick']
  }
</script>