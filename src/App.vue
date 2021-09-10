<template>
  <div id="app">
    <mainScore
      v-bind:listPlayer="listPlayer"
      v-bind:listRound="listRound"
      v-bind:set="set"
      @setScore="setScore"
      @setPlusMinus="setPlusMinus"
      @newSet="newSet"
      v-bind:properCase="properCase"
    />
    <!-- <button v-on:click="newSet()">NEW GAME</button> -->
    <!-- <button v-on:click="scoreFinal()">tính điểm</button> -->
  </div>
</template>

<script>
import mainScore from "./components/mainScore.vue";
import toi from './assets/toi.mp3'


export default {
  name: "App",
  components: {
    mainScore,
  },
  setup() {  
  
  },
  data() {
   
    return {      
      maxScore: 21,
      set: 1,
      listPlayer: [],
      listRound: [
        {
          round: 1,
          scoreWin: 3,
          scores: [
            { totalScore: null, score: null, plus: null, minus: null , flat: 0},
            { totalScore: null, score: null, plus: null, minus: null , flat: 0},
            { totalScore: null, score: null, plus: null, minus: null , flat: 0},
            { totalScore: null, score: null, plus: null, minus: null, flat: 0 },
          ],
        },
    
      ],
                      
    };
  },
  methods: {
   
    properCase: function (str) {
      return str.toLowerCase().replace(/\b(\w)/g, (s) => s.toUpperCase());
    },
     newSet: function() {
      // let i = 1;
      let lenght = this.listRound.length;
      let set = this.listRound.push({
          round: lenght+1,
          scoreWin: 3,
          scores: [
            { totalScore: null, score: null, plus: null, minus: null , flat: 0},
            { totalScore: null, score: null, plus: null, minus: null , flat: 0},
            { totalScore: null, score: null, plus: null, minus: null , flat: 0},
            { totalScore: null, score: null, plus: null, minus: null, flat: 0 },
          ],
        },); 
      console.log(set);     
      return  0;
    },
     setScore: function(round,index) {
      console.log("cha nhan");    
      // for (const r of this.listRound) {
        // if (r.round === round) {
        let r =this.listRound[round-1];
          r.scores[index].score =  r.scoreWin;
          r.scores[index].flat =  1;
           console.log( r.scores[index].score);
          r.scoreWin--;
          this.scoreFinal();
        // }
      // } 
      
     this.playSound(toi);
     
    },
     playSound (sound) {
      if(sound) {
        var audio = new Audio(sound);
        audio.play();
      }
    },
    setPlusMinus(){
        // let currentRound = this.listRound.length;
        this.listRound[this.listRound.length -1].scores[1].minus--;
         this.listRound[this.listRound.length -1].scores[1].plus++;
         this.scoreFinal();
    },
    scoreFinal() {
      for (let i = 0; i < 4; i++) {
        let finalScore = 0;
        for (const round of this.listRound) {
          let flat = 0;
          for (const item of round.scores) {
            if (flat === i) {
              item.totalScore = 0;
              item.totalScore += item.score;
              item.totalScore += Math.abs(item.plus);
              item.totalScore -= Math.abs(item.minus);
              // item.totalScore -= item.minus;
              finalScore += item.totalScore;
            }
            flat++;
          }
        }
        this.listPlayer[i].score = finalScore;
      }
    },

    createPlayerlist: function () {
      let i = 1;
      for (i = 1; i <= 4; i++) {
        this.listPlayer.push({ name: "player " + i, score: 0 });
      }
      // console.log(this);
    },
    
  },
  computed: {
   
  },
  beforeMount() {
    this.createPlayerlist();
    // this.scoreFinal();
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Dancing+Script:wght@400;500;700&display=swap");
@import url(reset.css);
html {
  font-size: 62.5%;
}
body {
  font-size: 1.2rem;
  font-family: "Dancing Script";
}
*,
*:after,
*:before {
  box-sizing: border-box;
  -webkit-box-sizing: border-box;
}

#app {
  font-size: 1.2rem;
  font-family: "Dancing Script";
}
</style>
