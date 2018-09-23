<template>
  <div id="app" class="animated" v-bind:class="{ tada: status === 'win', shake: status === 'lose' }">
    <div class="row" v-for="(imageGroup, index) in imageGroups" v-bind:key="`row-${index}`">
      <div class="col-md-3" v-for="(image, index) in imageGroup" v-bind:key="`image-${index}`">
        <Tile v-bind:id="image.id" v-bind:filename="image.filename" v-bind:status="status" v-bind="{ handleClick }" />
      </div>
    </div>
  </div>
</template>

<script>
import Tile from './components/Tile/Tile.vue';
import './main.css';
import imagesArray from './images.json';

let resultTimeout;

function splitArray (input, spacing) {
  const output = [];
  for (let i = 0; i < input.length; i += spacing) {
    output[output.length] = input.slice(i, i + spacing);
  }
  return output;
}

function shuffleImages (images) {
  return images
    .map(a => [Math.random(), a])
    .sort((a, b) => a[0] - b[0])
    .map(a => a[1]);
}

export default {
  name: 'app',
  data: function() {
    return {
      score: 0,
      topScore: 0,
      clicked: [],
      imageGroups: splitArray(shuffleImages(imagesArray), 4),
      status: 'playing'
    }
  },
  methods: {
    handleClick(id) {
      clearTimeout(resultTimeout);
      const resultSpan = document.getElementById('result');
      resultSpan.className = 'result';
      resultSpan.classList.add('fade-in');
      resultTimeout = setTimeout(() => {
        resultSpan.classList.remove('fade-in');
      }, 500);

      const { score, topScore } = this;

      this.status = 'playing';

      if (this.clicked.includes(id)) {
        this.status = 'lose';
        this.score = 0;
        this.clicked = [];
        resultSpan.classList.add('alert', 'alert-danger');
        resultSpan.innerHTML = '<i class="fas fa-times-circle"></i> YOU LOSE!';
      } else {
        this.clicked.push(id);
        // check to see if user has finished the game
        if (this.clicked.length === imagesArray.length) {
          this.status = 'win';
          this.clicked = [];
          resultSpan.classList.add('alert', 'alert-success');
          resultSpan.innerHTML = '<i class="fas fa-trophy"></i> YOU WIN!';
          this.topScore = score + 1;
          this.score = 0;
        } else {
          resultSpan.classList.add('alert', 'alert-info');
          resultSpan.innerHTML = '<i class="fas fa-check-circle"></i> GOOD!';
          this.score = score + 1;
          this.topScore = this.score > topScore ? this.score : topScore;
          this.imageGroups = splitArray(shuffleImages(imagesArray), 4);
        }
        document.getElementById('topScore').innerText = this.topScore;
      }
      document.getElementById('score').innerText = this.score;
    }
  },
  components: {
    Tile
  }
}
</script>
