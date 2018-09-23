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
import Tile from './components/Tile.vue';
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

<style lang="scss">
  @keyframes fadeIn {
    0% { opacity: 0 }
    100% { opacity: 1 }
  }

  body {
    margin: 0;
    padding: 0;
    font-family: 'Lato', sans-serif;
  }

  .green {
    color: #34dd34;
  }

  .red {
    color: #f00;
  }

  .yellow {
    color: #ecec02;
  }

  .score,
  .top-score,
  .result {
    display: inline-block;
    position: relative;
    border-radius: 6px;
    margin: 0 0 5px 5px;
    padding: 0 10px;
    text-align: center;
    &:after {
      position: absolute;
      left: 0;
      top: 50%;
      height: 1px;
      content: "";
      width: 100%;
      display: block;
    }
    
  }

  .score,
  .top-score {
    color: #000;
    background-color: #fff;
  }

  .fade-in {
    animation: fadeIn 0.5s linear;
  }

  .site {
    display: flex;
    min-height: 100vh;
    flex-direction: column;
  }

  .container {
    max-width: 960px;
  }

  .site-content {
    flex: 1;
  }

  .wrapper .col-md-3 {
    text-align: center;
  }

  main {
    margin-top: 80px;
  }

  .navbar {
    padding: 0;
  }

  .navbar-brand {
    font-family: 'Dancing Script', cursive;
    font-weight: bold;
    font-size: 36px;
    line-height: 0.75;
    margin-top: 5px;
    & > img {
      width: 40px;
      margin-bottom: 5px;
    }
  }

  .navbar-text {
    font-size: 20px;
  }

  .score,
  .result {
    margin-right: 15px;
  }

  .highlight {
    color: #ecec02;
  }

  .instructions {
    font-size: 17px;
    margin: 0 0 20px;
    box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);
    border-radius: 10px;
    border: 1px solid #666;
  }

  p:last-of-type {
    margin-bottom: 0;
  }

  footer {
    text-align: center;
    color: #fff !important;
    margin-top: 5px;
    padding: 10px;
    & a,
    & a:active,
    & a:focus {
      color: #fff;
      text-decoration: underline;
    }
    & a:hover {
      color: #ecec02;
    }
  }

  @media screen and (max-width: 660px) {
    .navbar-brand {
      display: none;
    }
    .navbar-text {
      display: block;
      text-align: center;
      width: 100%;
    }
  }

  @media screen and (max-width: 518px) {
    .result {
      display: block;
      font-size: 20px;
      margin-bottom: 5px;
      text-align: center;
    }
    .navbar-text {
      font-size: 16px;
      padding: 0;
      margin: 7px auto;
    }
  }
</style>