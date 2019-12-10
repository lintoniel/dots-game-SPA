<template>
  <div id="app">
    <div class="game_container flex">
      <div class="game">
        <div class="game__panel">
          <div class="game__panel_mode inline_block relative"
            :class="{ 'game__panel_mode_dropdown_alert': isModeFailed}"
            @click="toggleDropdownVisibility">{{mode}}
              <img src="../src/assets/images/dropdown.png" alt="arrow">
              <div class="game__panel_mode_dropdown absolute"
                v-show="dropdownVisibility"
              >
                <div class="game__panel_mode_dropdown_item"
                  v-for="(val, k) in gameSettings"
                  :key="k"
                  @click.stop="toggleDropdownVisibility"
                  @click="setCustomSettings(k)"  
                > {{k}}
                </div>
              </div>
          </div>
          <input type="text" class="game__panel_name" placeholder="Enter your name"
            :class="{'game__panel_name_alert': isInputFailed}"
            v-model="userName"
          >
          <button class="game__panel_play"
              :disabled="isValidateStarted || isGameStarted"
              @click="startGame()"
            >
            PLAY
          </button>
        </div>
        <div class="game__message"></div>
        <app-game></app-game>
      </div>
      <div class="score">
        <div class="score__table">
          <div class="score__table_title">Leader Board</div>
          <div class="score__table_container">
            <div class="score__table_item flex"
              v-for="(item, index) in winnersTable"
              :key="index"
            >
              <div>{{item.winner}}</div>
              <div>{{item.date}} </div> 
          </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import 'normalize.css'
import appGame from './components/game'

export default {
  name: 'dots-game',
  components: {
    "app-game": appGame
  },
  data() {
    return {
      gameSettings: {},
      winnersTable: [],
      mode: "Pick game mode",
      userName: '',
      dropdownVisibility: false,
      isValidateStarted: false,
      isGameStarted: false,
      isInputFailed: false,
      isModeFailed: false
    }
  },
  computed: {
    customeMode() {
      return this.gameSettings[this.mode];
    }
  },
  methods: {
    toggleDropdownVisibility() {
      this.dropdownVisibility = !this.dropdownVisibility;
    }, 
    setCustomSettings(key) {
      this.mode = key;
    
    },
    async getGameSettings() {
      const response = await axios.get('https://starnavi-frontend-test-task.herokuapp.com/game-settings');
      this.gameSettings = response.data;
    },
    async getWinnersList() {
      const response = await axios.get('https://starnavi-frontend-test-task.herokuapp.com/winners');
      this.winnersTable = response.data;
    },
    startGame() {
      this.isValidateStarted = true;
      this.isInputFailed = false;
      this.isModeFailed = false;
      if (!this.userName || this.userName.length > 16) {
        this.isValidateStarted = false;
        this.isInputFailed = true;
        return;
      } else if (!this.customeMode) {
        this.isValidateStarted = false;
        this.isModeFailed = true;
        return
      }
    }
  },
  created() {
    this.getGameSettings();
    this.getWinnersList();
  }
}
</script>

<style>
* {
  outline: none; 
  box-sizing: border-box;
  font-family: sans-serif;
}

.flex {
  display: flex;
}

.inline_block {
  display: inline-block;
}

.relative {
  position: relative;
}

.absolute {
  position: absolute;
}

.game, .score {
  flex: 50%;
  height: 1000px;
  border: 1px solid #e0e0e0;
}

.game__panel {
  margin-top: 70px;
  text-align: center
}

.game__panel_mode {
  border-radius: 10px;
}

.game__panel_mode, .game__panel_mode_dropdown_item {
  width: 200px;
  height: 50px;
  line-height: 50px;
  cursor: pointer;
  text-align: center;
  background-color:#cfd8dc;
  color: #8a9aa0;
}

.game__panel_mode_dropdown_alert {
  border: 1px solid rgb(226, 80, 80);
}

.game__panel_mode_dropdown_item {
  border: 1px solid #bcc5c9;
}

.game__panel_mode_dropdown_item:hover {
  background-color:#bcc5c9;
}

.game__panel_mode img{
  margin-left: 10px;
  height: 15px;
  width: 15px;
  vertical-align: middle;
}

.game__panel_name {
  margin-left: 20px;
  padding-left: 20px;
  padding-right: 20px;
  width: 200px;
  height: 50px;
  border: none;
  border-radius: 10px;
  background-color: #f3f3f3;
  color: #a9b4b8;
}

.game__panel_name_alert {
  border: 1px solid rgb(226, 80, 80);
}

.game__panel_name::placeholder { /* Chrome, Firefox, Opera, Safari 10.1+ */
  color: #a9b4b8;
  opacity: 1; /* Firefox */
}

.game__panel_name:-ms-input-placeholder { /* Internet Explorer 10-11 */
  color: #a9b4b8;
}

.game__panel_name::-ms-input-placeholder { /* Microsoft Edge */
  color: #a9b4b8;;
}

.game__panel_play {
  margin-left: 20px;
  width: 100px;
  height: 50px;
  background-color: #7b8d93;
  color: #f8f9f9;
  border: none;
  border-radius: 10px;
  cursor: pointer;
}

.game__message {
  margin: 50px 0;
  height: 30px;
  line-height: 30px;
  color: #909090;
  text-align: center;
}

.game__field {
  text-align: center;
}

.game__field div {
  height: 50px; 
}

.game__field_item {
  border: 1px solid #f2f2f3;
  width: 50px;
}

.score__table {
  margin-top: 75px;
  margin-left: 50px;
}

.score__table_title {
  font-size: 38px;
  color: #888888;
}

.score__table_container {
  margin-top: 20px;
}

.score__table_item {
  width: 400px;
  height: 60px;
  line-height: 60px;
  padding: 0 10px;
  background-color: #cfd8dc;
  color: #9fa8ab;
  margin-bottom: 5px;
  font-size: 20px;
  justify-content: space-between;
}
</style>
