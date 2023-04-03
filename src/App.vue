<template>
  <div>
    <div id="computer" :style="computedStyleObject"></div>
    <div>
      <button @click="onClickButton('Rock')">Rock</button>
      <button @click="onClickButton('Scossor')">Scossor</button>
      <button @click="onClickButton('Paper')">Paper</button>
    </div>
    <div>{{result}}</div>
    <div>Current Score: {{score}}</div>
  </div>
</template>

<script>
  const rspCoords = {
    Rock: '0',
    Scossor: '-142px',
    Paper: '-284px',
  };
  const scores = {
    Rock: 1,
    Scossor: 0,
    Paper: -1,
  };
  const computerChoice = (imgCoord) => {
    return Object.entries(rspCoords).find(function (v) {
      return v[1] === imgCoord;
    })[0];
  };
  let interval = null;
  export default {
    data() {
      return {
        imgCoord: rspCoords.Rock,
        result: '',
        score: 0,
      };
    },
    computed: {
      computedStyleObject() {
        return {
          background: `url(https://en.pimg.jp/023/182/267/1/23182267.jpg) ${this.imgCoord} 0`,
        };
      }
    },
    methods: {
      changeHand() {
        interval = setInterval(() => {
          if (this.imgCoord === rspCoords.Rock) {
            this.imgCoord = rspCoords.Scossor;
          } else if (this.imgCoord === rspCoords.Scossor) {
            this.imgCoord = rspCoords.Paper;
          } else if (this.imgCoord === rspCoords.Paper) {
            this.imgCoord = rspCoords.Rock;
          }
        }, 100);
      },
      onClickButton(choice) {
        clearInterval(interval);
        const myScore = scores[choice];
        const cpuScore = scores[computerChoice(this.imgCoord)];
        const diff = myScore - cpuScore;
        if (diff === 0) {
          this.result = 'Not Winner.';
        } else if ([-1, 2].includes(diff)) {
          this.result = 'You are Winner!.';
          this.score += 1;
        } else {
          this.result = 'Sorry, Try it again';
          this.score -= 1;
        }
        setTimeout(() => {
          this.changeHand();
        }, 1000);
      },
    },
    mounted() {
      console.log('mounted');
      this.changeHand();
    },
    beforeUnmount() {
      clearInterval(interval);
    }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}
</style>

<style scoped>
  #computer {
    width: 142px;
    height: 200px;
    background-position: 0 0;
  }
</style>