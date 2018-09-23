<template>
  <button class="btn btn-default" @click="handleClick(id)" v-bind:class="{ 'flash-green': status === 'win', 'flash-red': status === 'lose' }">
    <img alt="An image of a person getting hit in the face with something." v-bind:src="`assets/img/${filename}`" class="img-fluid" />
  </button>
</template>

<script>
  export default {
    name: 'tile',
    props: ['filename', 'id', 'status', 'handleClick']
  }
</script>

<style lang="scss">
  @keyframes flashRed {
    0% { box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19); }
    50% { box-shadow: 0 0 20px red; }
    100% { box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19); }
  }

  @keyframes flashGreen {
    0% { box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19); }
    50% { box-shadow: 0 0 20px #00d636; }
    100% { box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19); }
  }

  .flash-red {
    animation: flashRed 0.82s cubic-bezier(.36,.07,.19,.97) both;
  }

  .flash-green {
    animation: flashGreen 0.82s cubic-bezier(.36,.07,.19,.97) both;
  }

  .container {
    & button {
      position: relative;
      display: inline-block;
      background-color: #fff;
      margin: 0 auto 20px auto;
      padding: 5px;
      border-radius: 10px;
      border: 1px solid #6c757d;
      box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
      transition: all .1s ease-in-out;
      & > img {
        border-radius: 10px;
      }
      &:hover {
        transform: scale(1.07);
        &::after {
          opacity: 1;
        }
      }
      &:after {
        content: "";
        border-radius: 10px;
        position: absolute;
        z-index: -1;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        padding: 5px;
        box-shadow: 0 8px 15px rgba(0, 0, 0, 0.4);
        opacity: 0;
        transition: all .1s ease-in-out;
      }
    }
  }
</style>