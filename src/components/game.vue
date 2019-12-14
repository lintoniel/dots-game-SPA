 <template>
  <div class="game__field grid" :class="gameFieldClass">
    <div v-for="i in fieldSqured" 
      :key="i"
      :id="`game__field_item--${i}`">
    </div>
  </div>
</template>

<script>
export default {
  props: {
    customSettings: {
      type: Object,
      default() {
        return {
            "field":5,
            "delay":2000
          }
      }
    }
  },
  date() {
    return {
      clickedItems: null,
      userScore: 0,
      computerScore: 0
    }
  },
  computed: {
    gameFieldClass() {
      let  gameClass = 'game__field_easy';
        switch(this.customSettings.field) {
          case 10:
            gameClass = 'game__field_normal';
            break;
          case 15:
            gameClass = 'game__field_hard';
            break;
       }
       return gameClass;
    }, 
    fieldSqured() {
      return this.customSettings.field * this.customSettings.field;
    }
  },
  methods: {
    startGame() {
      this.clickedItems = new Set();
      this.gameStep();
    },
    gerWinner() {

    },
    gameStep() {
      let oldLength = this.clickedItems.size;
      let randomNumder = Math.floor(Math.random() * (this.fieldSqured - 1)) + 1;
      this.clickedItems.add(randomNumder);
      let newLength = this.clickedItems.size;

      if (newLength > oldLength) {
        this.activateField(randomNumder);
      } else {
        this.gameStep();
      }
    },
    activateField(number) {
      const elem = document.getElementById(`game__field_item--${number}`);
      elem.classList.add('item_red');
      let clicked = false;

      elem.addEventListener('click', ()=>{
        clicked = true;
        elem.classList.add('item_green');
        this.userScore++;
      });

      setTimeout(()=>{
          if (!clicked) {
            elem.classList.add('item_blue');
            this.computerScore++;
          }

          if (this.computerScore >= this.fieldSqured/2 || this.userScore >= this.fieldSqured/2) {
              this.$emit('game-over', this.gerWinner())
          } else {
            this.gameStep();
          }
      }, this.customSettings.delay);
    },
  },
  mounted() {
    this.startGame();
  },
  
}
</script>

<style scoped>
.game__field {
  margin: 0 auto;
}

.game__field_easy {
  width: 250px;
  grid-template: 1fr 1fr 1fr 1fr 1fr / 1fr 1fr 1fr 1fr 1fr;
}

.game__field_normal {
  width: 500px;
  grid-template: 1fr 1fr 1fr 1fr 1fr 1fr 1fr 1fr 1fr 1fr / 1fr 1fr 1fr 1fr 1fr 1fr 1fr 1fr 1fr 1fr;
}

.game__field_hard {
  width: 750px;
  grid-template: 1fr 1fr 1fr 1fr 1fr 1fr 1fr 1fr 1fr 1fr 1fr 1fr 1fr 1fr 1fr / 1fr 1fr 1fr 1fr 1fr 1fr 1fr 1fr 1fr 1fr 1fr 1fr 1fr 1fr 1fr; 
}

.game__field div {
  height: 50px;
  border: 1px solid #eeeef0;
}

.item_red {
  background-color: #e85a5f;
}

.item_green {
  background-color: green;
  pointer-events: none
}

.item_blue {
  background-color: blue;
  pointer-events: none
}
</style>