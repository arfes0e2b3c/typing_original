<template>
  <div class="game-start-container">
    <h1>タイピングゲーム</h1>
    <slot name="button-slot">
      buttons
    </slot>
    <h2 v-cloak>出題数(最大{{ dataNum }}問)</h2>
    <input type="text" v-model="quizNum" ref="input"><br>
    <div class="button-group game-start-button">
      <button class="button1" @click="gameStart">ゲームスタート</button>
      <button class="button2" @click="gameStart">ゲームスタート</button>
      <button class="button3" @click="gameStart"></button>
      <button class="button4" @click="gameStart"></button>
    </div>
    <div class="errors">
      <p v-for="error in errors" :key="error.key">{{ error.mes }}</p>
    </div>
  </div>
</template>
<script>
export default{
  data(){
    return{
      quizNum: 1,
      dataNum: 0,
      errors: []
    }
  },
  methods:{
    gameStart(){
      this.checkQuizNum()
      if(this.errors.length === 0) {
        //!this.errorsだと判定できなかった
        this.$emit('gameStart')
        this.$emit('quizNum', this.quizNum)
      }
    },
    checkQuizNum() {
      this.errors = []
      if(isNaN(this.quizNum)){
        this.errors.push({mes: '数値以外が入力されています', key: 0})
      }
      if(this.quizNum < 1) {
        this.errors.push({mes: '出題数は１以上を入力してください', key: 1})
        this.quizNum = 1
      }
      if(this.quizNum > this.dataNum) {
        this.errors.push({mes: '出題数が最大値を超過しています', key: 2})
        this.quizNum = this.dataNum
      }
      if(this.dataNum == 0) {
        this.errors.push({mes: '問題が登録されていません', key: 3})
      }
    }
  },
  mounted(){
    this.$nextTick(() => {
      this.dataNum =  JSON.parse(localStorage.getItem("dataList")).length - 0
      this.$refs["input"].focus()
      this.$refs["input"].setSelectionRange(1, 1)
    })
  },
  props:{
    sentDataNum:{
      type: Number
    }
  },
  watch:{
    sentDataNum:function() {
      this.dataNum = this.sentDataNum
    }
  }
}
</script>
<style scoped>
.game-start-container{
  position:absolute;
  width:100%;
  text-align: center;
}
.button-group{
  position: relative;
  width: 150px;
  height: 40px;
  transform-style: preserve-3d;
  perspective: 60000px;
  transform-origin: 50% 50% 20px;
}
.game-start-button{
  margin: 30px auto 0;
}
button{
  position: absolute;
  top: 0;
  left: 0;
  width:150px;
  height:40px;
  background: none;
  border: 1px solid gray;
  cursor: pointer;
  transition: .5s;
  overflow: hidden;
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
input{
  text-align: center;
  border: none;
  border-bottom: 1px solid rgba(0,0,0,0.2);
  width: 30%;
  height:30px;
  font-size: 2em;
  outline: none;
  transition: .5s;
}
input:focus{
  border-bottom: 1px solid rgba(0,0,0,0.8);
  width: 10%;
}
.errors{
  margin-top: 20px;
}
.errors p{
  color: red !important;
}
</style>
