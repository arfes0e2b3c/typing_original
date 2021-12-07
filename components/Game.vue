<template>
  <div class="background">
    <div class="container">
      <i id="icon" class="fas fa-times-circle" @click="closeGame"></i>
      <div class="quiz-container" v-show="showQuiz">
        <p>{{ this.quiz[0].showStr }}</p>
        <p>{{ this.quiz[0].inputStr }}</p>
        <input type="text" v-model="answer" id="input" ref="input" spellcheck="false" autocomplete="off">
        <p class="answer-per-quiz"> {{ answerNum }} / {{ quizNum }}</p>
      </div>
      <div class="clear-container" v-show="showClear">
        <h2>Clear!</h2>
        <p>ミスタイプ数：{{ missType }}</p>
        <p>クリアタイム：{{ showTime }}</p>
        <div class="button-group">
          <button class="button1" @click="closeGame">ゲームを終了する</button>
          <button class="button2" @click="closeGame">ゲームを終了する</button>
          <button class="button3" @click="closeGame"></button>
          <button class="button4" @click="closeGame"></button>
        </div>
        <div class="button-group">
          <button class="button1" @click="gameStart">もう一度</button>
          <button class="button2" @click="gameStart">もう一度</button>
          <button class="button3" @click="gameStart"></button>
          <button class="button4" @click="gameStart"></button>
        </div>
      </div>
      <div class="progressBox">
        <div class="progressBar" :style="bar"></div>
      </div>
    </div>
  </div>
</template>
<script>
import { shuffle } from 'lodash';
export default {
  data(){
    return{
      quiz:[{showStr:' ',inputStr:' '}],
      answer:'',
      answerNum: 0,
      bar:{"width":"0%","transition":".5s"},
      barWidth:'',
      showQuiz: true,
      showClear: false,
      missType: 0,
      startTime: 0,
      endTime: 0,
      time: 0,
      showTime: '',
    }
  },
  methods:{
    gameStart() {
      this.getStartTime()
      this.getQuiz()
      this.closeClear()
      this.openQuiz()
      this.focusInput()
    },
    getQuiz(){
      this.quiz = [{showStr:' ',inputStr:' '}]
      this.quiz = _.shuffle(JSON.parse(localStorage.getItem("dataList"))).concat(this.quiz)
      this.answerNum = 0
      this.missType = 0
    },
    openQuiz() {
      this.showQuiz = true
    },
    closeClear() {
      this.showClear = false
    },
    gameClear() {
      this.getEndTime()
      this.calcTime()
      this.showQuiz = false
      this.showClear = true
    },
    closeGame() {
      this.$emit('closeGame')
    },
    focusInput() {
      this.$nextTick(() => {
        this.$refs["input"].focus()
      })
    },
    getStartTime() {
      this.startTime = new Date()
      console.log(this.startTime)
    },
    getEndTime() {
      this.endTime = new Date()
      console.log(this.endTime)
    },
    calcTime() {
      this.time = this.endTime - this.startTime
      let m = Math.floor(this.time / 60000)
      let s = Math.floor(this.time % 60000 / 1000)
      let ms = this.time % 1000
      m = ('0' + m).slice(-2)
      s = ('0' + s).slice(-2)
      ms = ('00' + ms).slice(-3).slice(0, 2)
      this.showTime = m + ':' + s + ':' + ms
    }
  },
  props:{
    quizNum:{
      type: Number
    }
  },
  watch:{
    answer:function() {
      if(this.answer === this.quiz[0].inputStr){
        this.answerNum++
        if(this.answerNum === this.quizNum) {
          this.gameClear()
        }
        this.quiz.splice(0, 1)
        this.answer = ''
      }
      else if(this.answer !== this.quiz[0].inputStr.slice(0, this.answer.length)){
        this.answer = this.answer.slice(0, this.answer.length - 1)
        this.missType++
      }
    },
    answerNum:function() {
      this.barWidth = this.answerNum / this.quizNum * 100 + "%"
      this.bar.width = this.barWidth
    }
  }
}
</script>

<style scoped>
*{
  color:white !important;
}
.background{
  width:100%;
  height:100%;
  position:absolute;
  left: 0;
  top: 0;
  background:rgba(0,0,0,0.6);
}
.container{
  width:70%;
  height:70%;
  position:absolute;
  left:50%;
  top:50%;
  transform:translate(-50%,-50%);
  text-align: center;
  background:#0e2b3c;
  justify-content: center;
  align-items: center;
  box-shadow: 0 0 16px #444;
}
.quiz-container,.clear-container{
  position:absolute;
  width: 100%;
  top:50px;
  left:50%;
  transform:translateX(-50%);
}
.quiz-container{
  top: 150px;
}
.quiz-container p{
  font-size:1.3em;
}
.answer-per-quiz{
  margin: 8px auto;
}
.progressBox{
  position: absolute;
  top:350px;
  left: 50%;
  width: 50%;
  height: 30px;
  transform:translateX(-50%);
  border: 1px solid #aaa;
  margin: 10px auto;
}
.progressBar{
  height:100%;
  background-color: #56789a;
}
#input{
  text-align: center;
  background: #0e2b3c;
  border: none;
  border-bottom: 1px solid rgba(255,255,255,0.2);
  width: 50%;
  height:30px;
  color: white;
  font-size: 2em;
  outline: none;
}
#icon{
  font-size: 4em;
  position: absolute;
  top: 30px;
  right: 30px;
  cursor: pointer;
  z-index: 1;
}
#icon:hover{
  transition: .5s;
  transform: rotate(90deg);
}
h2{
  font-size: 2em;
}
.button-group{
  position: relative;
  width: 150px;
  height: 40px;
  margin: 20px auto;
  transform-style: preserve-3d;
  perspective: 60000px;
  transform-origin: 50% 50% 20px;
}
button{
  position: absolute;
  top: 0;
  left: 0;
  width:150px;
  height:40px;
  background: none;
  border: 1px solid white;
  cursor: pointer;
  transition: .5s;
  overflow: hidden;
  color: white;
}
.button2{
  transform: translateY(-50%)translateZ(20px)rotateX(-90deg);
}
.button3{
  transform: translateY(50%)translateZ(20px)rotateX(90deg);
}
.button4{
  transform: translateY(0)translateZ(40px)rotateX(0deg);
}
.button-group:hover{
  transform: rotateX(90deg);
  transition: .5s;
}
</style>
