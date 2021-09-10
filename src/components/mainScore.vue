<template>
  <div id="mainScore">
    <div class="main__container">
      <h1 class="main__title">Set {{ set }} </h1>
      <div class="header__container">
        <div class="header__item"></div>
        <div
          class="header__item"
          v-for="(item, index) in listPlayer"
          v-bind:key="index"
        >
          <p class="score__final">{{ item.score }}</p>
          <p class="player__name">{{ item.name.toLowerCase().replace(/\b(\w)/g, s => s.toUpperCase()) }}</p>
        </div>
      </div>
      <div class="body__container">
        <ul
          class="score__list"
          v-for="(round, index) in listRound"
          v-bind:key="index"
        >
          <li class="score__item">
            {{ round.round }}
          </li>
          <li
            class="score__item"
            v-for="(score, index) in round.scores"
            v-bind:key="index"
          >
            <span class="score__total"  v-if="score.flat > 0">
              {{ score.totalScore }}
            </span>
              <span class="score__detail">
                <span
                  class="score__main"
                  v-if="Math.abs(score.plus) > 0 || Math.abs(score.minus) > 0"
                  >{{ score.score }}</span
                >
                <span class="score__plus" v-if="Math.abs(score.plus) > 0"
                  >+{{ Math.abs(score.plus) }}
                </span>
                <span class="score__minus" v-if="Math.abs(score.minus) > 0"
                  >-{{ Math.abs(score.minus) }}
                </span>
              </span>
            <button class="btn-finish" v-on:click="setScore(round.round,index)" v-if="score.flat <= 0">TỚI</button>
          </li>
        </ul>
        
      </div>
    </div>

        <button v-on:click="setPlusMinus()">TRÙM HOẶC NHỐT HEO</button>
    <button v-on:click="newSet()">NEW GAME</button>
  </div>
</template>

<script>
export default {
  name: "mainScore",
  props: {
    listPlayer: Array,
    listRound: Array,
    listitem: Array,
    set: Number,
  
  },
  methods: {
    newSet(){
      this.$emit('newSet');
    },
    setScore(round,index){
      this.$emit('setScore', round,index);
      console.log("emit con");
    },
    setPlusMinus(){
       this.$emit('setPlusMinus');
    }
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
/* .mainScore{
   font-family: "Dancing Script";
} */
.main__container{
  padding: 1rem;
}

.header__container {
  display: flex;
  justify-content: space-around;
  margin-bottom: 0.5rem;
}
.header__item {
  width: 100%;
  padding: 0.5rem;
  /* font-size: 1.8rem; */
  text-align: center;
}
.header__item:first-child {
  width: 4rem;
}
.score__final {
  /* padding: 0 0.5rem; */
  font-size: 3rem;
  font-weight: bold;
  border-bottom: 0.5rem solid rgba(128, 128, 128, 0.6);
}
.player__name {
  width: 100%;
  padding: 1rem 0;
  text-overflow: ellipsis;
}
.body__container{
  margin-bottom: 3rem;
}
.score__list {
  display: flex;
  justify-content: space-around;
  
}
.score__item {
  width: 100%;
  padding-top: .5rem;
  padding-bottom: 0.5rem;
   position: relative;
  font-size: 1rem;
  text-align: center;
  list-style: none;
  border-bottom: 0.1px solid rgba(128, 128, 128, 0.2);
}
.score__item:first-child { 
  width: 4rem;
  /* margin: auto 0;
  font-size: 1rem;
  border: none; */
}

.score__total {
  font-size: 1.2rem;
  font-weight: 500;  

}
.score__detail {
  /* background-color: rgba(128, 128, 128, 0.1); */
  border-radius: 0.2rem;
  /* padding: 0.2rem 0.4rem; */
  margin-left: 0.5rem;
  position: absolute;
  /* top: -0.5rem; */
  font-size: 0.5rem;
}
.score__main {
  padding-right: 0.2rem;
}
.score__plus {
  color: green;
}
.score__minus {
  color: red;
}
.btn-finish{
  position: absolute;
  bottom: -2rem;
  left: 50%;
  transform: translateX(-50%);

  border: none;
  width: 80%;
  padding: 0.3rem;
  border-radius: 0.25rem;
  font-family: " cursive";
}
</style>
