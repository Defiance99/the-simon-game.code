<template>
  <div class="game-container">
    <div class="simon-game-parts-container" ref='gameParts'>
      <div class="blue-game-part" data-tile="0"></div>
      <div class="red-game-part" data-tile="1"></div>
      <div class="yellow-game-part" data-tile="2"></div>
      <div class="green-game-part" data-tile="3"></div>
    </div>
    <div class="game-menu">
      <p>Round: {{round}}</p>
      <input type="button" value="Start" @click="startGame">
      <p v-if="showLose">Sorry, you lost after {{round}} rounds!</p>
      <h3>Game Options:</h3>
      <form class="game-options">
          <input type="radio" value="easy" id="easy" v-model="gameMode">
          <label for="easy">Easy</label>
          <br>
          <input type="radio" value="normal" id="normal" v-model="gameMode">
          <label for="normal">Normal</label>
          <br>
          <input type="radio" value="hard" id="hard" v-model="gameMode">
          <label for="hard">Hard</label>
      </form>
    </div>
    <!-- <div ref="audio"></div> -->
  </div>
</template>

<script>

export default {
  data() {
    return {
      sequence: [],
      copy: [],
      gameMode: 'normal',
      initMode: '',
      active: true,
      round: 0,
      showLose: false,
      audio0: File,
      audio1: File,
      audio2: File,
      audio3: File 
    }
  },
  mounted() {
    this.audio0 = new Audio(require('@/assets/sounds/0.mp3'));
    this.audio1 = new Audio(require('@/assets/sounds/1.mp3'));
    this.audio2 = new Audio(require('@/assets/sounds/2.mp3'));
    this.audio3 = new Audio(require('@/assets/sounds/3.mp3'));
  },
  methods: {
    startGame: function() {
      this.sequence = [];
      this.copy = [];
      this.round = 0;
      this.active = true;
      this.showLose = false;
      this.initMode = this.gameMode;
      this.newRound();
    },

    newRound: function() {
      this.round = this.round + 1;
      this.sequence.push(this.randomNumber());
      this.copy = this.sequence.slice();
      this.animate();
    },

    registerClick: function(event) {
      let desiredResponse = this.copy.shift();
      let actualResponse = event.target.dataset.tile;
      this.active = (desiredResponse == actualResponse);
      this.checkLose();
    },

    checkLose: function() {
      if (this.copy.length === 0 && this.active) {
        this.deactivateSimonBoard();
        this.newRound();

      }else if (!this.active) {
        this.deactivateSimonBoard();
        this.endGame();
      }
    },

    endGame: function() {
      this.showLose = true
    },

    changeMode: function() {
      return 5000
    },


    animate: function() {
        let i = 0;
        let that = this;
        let interval;

        if (this.initMode == 'easy') {
          interval = setInterval(animateHelper, 1500);
        }
        if (this.initMode == 'normal') {
          interval = setInterval(animateHelper, 1000)
        }
        if (this.initMode == 'hard') {
          interval = setInterval(animateHelper, 400);
        }

        function animateHelper() {
          that.playSound(that._data.sequence[i]);
          that.lightUp(that._data.sequence[i]);

          i++;
          if (i >= that.sequence.length) {
            clearInterval(interval);
            that.activatedSimonBoard();
          }
        } 
      },

    activatedSimonBoard: function() {
      this.$refs.gameParts.addEventListener('click', this.clickHelper);
      this.$refs.gameParts.addEventListener('mousedown', this.mousedownHelper);
      this.$refs.gameParts.addEventListener('mouseup', this.mouseupHelper);
    },

    deactivateSimonBoard: function() {
      this.$refs.gameParts.removeEventListener('click', (this.clickHelper));
      this.$refs.gameParts.removeEventListener('mousedown', (this.mousedownHelper));
      this.$refs.gameParts.removeEventListener('mouseup', (this.mouseupHelper));
    },
    
    lightUp: function(tile) {
      this.$refs.gameParts.children[tile].classList.add('lightUp')
      window.setTimeout(() => {
        this.$refs.gameParts.children[tile].classList.remove('lightUp')
      }, 300)
    },

    playSound: function(i) {
      /* let audio = document.createElement('audio');
      let source = document.createElement('source');
      audio.setAttribute('autoplay', '');
      source.setAttribute('src', `@/assets/sounds/${i}.mp3`);
      source.setAttribute('type', 'audio/mp3');

      audio.appendChild(source);
      this.$refs.audio.appendChild(audio) */
      if (i == 0) this.audio0.play();
      if (i == 1) this.audio1.play();
      if (i == 2) this.audio2.play();
      if (i == 3) this.audio3.play();
    },

    randomNumber: function() {
      return Math.floor((Math.random()*4));
    },


    clickHelper: function() {
      this.registerClick(event);
    },
    mousedownHelper: function() {
      event.target.classList.add('lightUp');
      this.playSound(event.target.dataset.tile);
    },
    mouseupHelper: function() {
      event.target.classList.remove('lightUp');
    }
  }
}
</script>

<style lang="scss">
.game-container {
  display: flex;
  justify-content: center;
}
.simon-game-parts-container {
    display: flex;
    flex-wrap: wrap;
    align-content: center;
    width: 310px;
    height: 310px;

      div {
        height: 150px;
        width: 150px;
        opacity: 0.5;
      }
      .blue-game-part {
        background-color: blue;
        border-left: 2px transparent solid;
        border-top: 2px transparent solid;
        border-top-left-radius: 100%;
      }
      .blue-game-part:hover {
        border-left: 2px black solid;
        border-top: 2px black solid;
      }
      .red-game-part {
        background-color: red;
        border-right: 2px transparent solid;
        border-top: 2px transparent solid;
        border-top-right-radius: 100%;
      }
      .red-game-part:hover {
        border-right: 2px black solid;
        border-top: 2px black solid;
      }
      .yellow-game-part {
        background-color:yellow;
        border-left: 2px transparent solid;
        border-bottom: 2px transparent solid;
        border-bottom-left-radius: 100%;
      }
      .yellow-game-part:hover {
        border-left: 2px black solid;
        border-bottom: 2px black solid;
      }
      .green-game-part {
        background-color:green;
        border-right: 2px transparent solid;
        border-bottom: 2px transparent solid;
        border-bottom-right-radius: 100%;
      }
      .green-game-part:hover {
        border-right: 2px black solid;
        border-bottom: 2px black solid;
      }
  }
.lightUp {
  opacity: 1 !important;
}
</style>
