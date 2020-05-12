<template>
  <div class="Game">
    <div id="display">
      <div id="player">
        <p> {{ playerHP }} </p>
        <div id="border">
          <div id="healthBar" class="player" :style="playerBar"></div>
        </div>
      </div>
      <div id="monster">
        <p> {{ monsterHP }} </p>
        <div id="border">
          <div id="healthBar" class="monster" :style="monsterBar"></div>
        </div>
      </div>
    </div>

    <div id="movements" v-if="endGame==false">
      <button id="attack" @click="attack()">Ataque</button>
      <button id="specialAttack" @click="specialAttack()">Ataque Especial</button>
      <button id="heal" @click="heal()">Curar</button>
      <button id="giveUp" @click="giveUp()" >Desistir</button>
    </div>

    <div id="endGameStatus" v-else>
      <p v-if="win==false" class="loseStatus">Voce Perdeu D:</p>
      <p v-else class="winStatus">Voce Ganhou!!!</p>
      <button id="restart" @click="restart()"> Reiniciar o Jogo</button>
    </div>

    <div id="logList">
      <ul>
        <li v-for="(logOne, i) in logList" :key="i"> 
          <p v-if="logOne.who=='monster'" class='monsterLog'> MONSTRO ATINGIU JOGADOR COM {{ logOne.damage }}. </p>
          <p v-else-if="logOne.type=='attack'" class="playerLog"> JOGADOR ATINGIU MONSTRO COM {{ logOne.damage }}. </p>
          <p v-else class="playerLog"> JOGADOR SE CUROU EM {{ logOne.damage }}. </p>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Game',
  props: {
    msg: String
  },
  data(){
    return{
      logList: [],
      playerHP: 100,
      monsterHP: 100,
      endGame: false,
      win: false,
    }
  },
  computed:{
    playerBar(){
      const widthBar = this.playerHP * 2
      if(this.playerHP<15){
        return{
          backgroundColor: 'red',
          width: `${widthBar}px`
        }
      }
      else{
        return{
          backgroundColor: 'greenyellow',
          width: `${widthBar}px`
        }
      }
    },
    monsterBar(){
      const widthBar = this.monsterHP * 2
      if(this.monsterHP<15){
        return{
          backgroundColor: 'red',
          width: `${widthBar}px`
        }
      }
      else{
        return{
          backgroundColor: 'greenyellow',
          width: `${widthBar}px`
        }
      }
    }
  },
  methods: {
    attack(){
      if(this.endGame==false){
        const monsterDMG = parseInt(Math.random()* (10 - 5) + 5)
        const playerDMG = parseInt(Math.random()* (15 - 10) + 10)
        this.monsterHP -= monsterDMG
        this.playerHP -= playerDMG
        this.logList.push({who:'player', damage: monsterDMG, type:'attack'})
        this.logList.push({who:'monster', damage: playerDMG, type:'attack'})
      }
    },
    specialAttack(){
      if(this.endGame==false){
        const monsterDMG = parseInt(Math.random()* (20 - 15) + 15)
        const playerDMG = parseInt(Math.random()* (15 - 10) + 10)
        this.monsterHP -= monsterDMG
        this.playerHP -= playerDMG
        this.logList.push({who:'player', damage: monsterDMG, type:'attack'})
        this.logList.push({who:'monster', damage: playerDMG, type:'attack'})
      }
    },
    heal(){
      if(this.endGame==false){
        const playerHeal = parseInt(Math.random()* (20 - 15) + 15)
        const playerDMG = parseInt(Math.random()* (15 - 10) + 10)
        this.playerHP += playerHeal
        this.playerHP -= playerDMG
        this.logList.push({who:'player', damage: playerHeal, type:'heal'})
        this.logList.push({who:'monster', damage: playerDMG, type:'attack'})
      }
    },
    giveUp(){
      this.endGame = true;
    },
    restart(){
      this.playerHP = 100
      this.monsterHP = 100
      this.endGame = false
      this.logList =[]
    }
  },
  watch:{
    playerHP: function(newV){
      if(newV<=0){
        this.playerHP = 0;
        this.endGame = true;
        this.win = false;
      }
      if(newV>100){
        this.playerHP = 100;
      }
    },
    monsterHP: function(newV){
      if(newV<=0){
        this.monsterHP = 0;
        this.endGame = true;
        this.win = true;
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
  padding: 5px 100px;
}
a {
  color: #42b983;
}
button{
  width: 150px;
  height: 60px;
  border-radius: 5px;
  margin-right: 35px;
}
.monsterLog{
  background: rgb(252, 76, 76);
  border-radius: 10px;
  color: white;
}
.playerLog{
  background: rgb(85, 85, 248);
  border-radius: 10px;
  color: white;
}
.winStatus{
  color: greenyellow;
}
.loseStatus{
  color: red;
}

#movements{
  margin-top: 50px;
}
#border{
  border-style: solid;
  width: 200px;
  height: 25px;
}
#healthBar{
  background: greenyellow;
  width: 200px;
  height: 25px;
}
#display{
  display: flex;
}
#monster{
  margin: 0 auto;
}
#player{
  margin: 0 auto;
}
</style>