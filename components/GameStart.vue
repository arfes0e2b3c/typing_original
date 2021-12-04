<template>
  <div class="container">
    <div class="start-container">
      <div class="button-group">
        <button class="button1" @click="openRegister">新規問題登録</button>
        <button class="button2" @click="openRegister">新規問題登録</button>
        <button class="button3" @click="openRegister"></button>
        <button class="button4" @click="openRegister"></button>
      </div>
      <div class="button-group">
        <button class="button1" @click="openEdit">問題確認・編集</button>
        <button class="button2" @click="openEdit">問題確認・編集</button>
        <button class="button3" @click="openEdit"></button>
        <button class="button4" @click="openEdit"></button>
      </div>
      <h2 v-cloak>出題数(最大{{ dataNum }}問)</h2>
      <input type="text" v-model="quizNum"><br>
      <div class="button-group">
        <button @click="gameStart" class="game-start-button button1">ゲームスタート</button>
        <button @click="gameStart" class="game-start-button button2">ゲームスタート</button>
        <button @click="gameStart" class="game-start-button button3"></button>
        <button @click="gameStart" class="game-start-button button4"></button>
      </div>
    </div>
  </div>
</template>
<script>
export default{
  data(){
    return{
      quizNum: 1,
      dataNum: ''
    }
  },
  methods:{
    openRegister(){
      this.$emit('openRegister')
    },
    openEdit(){
      this.$emit('openEdit')
    },
    gameStart(){
      this.$emit('gameStart')
      this.$emit('quizNum', this.quizNum)
    }
  },
  mounted(){
    this.dataNum =  JSON.parse(localStorage.getItem("dataList")).length - 0
  },
  props:{
    sentDataNum:{
      type: Number
    }
  },
  watch:{
    sentDataNum:function() {
      this.dataNum = this.sentDataNum
    },
    quizNum:function() {
      if(this.quizNum > this.dataNum){
        this.quizNum = this.dataNum
      }
    }
  }
}
</script>
<style scoped>
.container{
  width:100%;
  text-align: center;
}
.button-group{
  position: relative;
  width: 150px;
  height: 40px;
  margin: 30px auto 0;
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
</style>
