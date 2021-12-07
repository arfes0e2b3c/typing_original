<template class="body">
    <div class="container">
      <link href="https://use.fontawesome.com/releases/v5.6.1/css/all.css" rel="stylesheet">

      <transition name="fade">
        <game-start
          @gameStart="gameStart"
          @quizNum="setQuizNum"
          :sentDataNum="dataNum"
          v-show="showStartGame">
          <template #button-slot>
            <div class="buttons">
              <div class="button-group">
                <button class="button1" @click="openStartGame">Home</button>
                <button class="button2" @click="openStartGame">Home</button>
                <button class="button3" @click="openStartGame"></button>
                <button class="button4" @click="openStartGame"></button>
              </div>
              <div class="button-group">
                <button class="button1" @click="openRegister">新規問題登録</button>
                <button class="button2" @click="openRegister">新規問題登録</button>
                <button class="button3" @click="openRegister"></button>
                <button class="button4" @click="openRegister"></button>
              </div>
              <div class="button-group">
                <button class="button1" @click="openList">問題確認・編集</button>
                <button class="button2" @click="openList">問題確認・編集</button>
                <button class="button3" @click="openList"></button>
                <button class="button4" @click="openList"></button>
              </div>
            </div>
          </template>
        </game-start>
      </transition>

      <transition name="fade">
        <game
          v-show="showGame"
          ref="Game"
          :quizNum="quizNum"
          @closeGame="closeGame"/>
      </transition>

      <transition name="fade">
        <register
          v-show="showRegister"
          ref="Register"
          @setDataNum="setDataNum">
          <template #button-slot>
            <div class="buttons">
              <div class="button-group">
                <button class="button1" @click="openStartGame">Home</button>
                <button class="button2" @click="openStartGame">Home</button>
                <button class="button3" @click="openStartGame"></button>
                <button class="button4" @click="openStartGame"></button>
              </div>
              <div class="button-group">
                <button class="button1" @click="openRegister">新規問題登録</button>
                <button class="button2" @click="openRegister">新規問題登録</button>
                <button class="button3" @click="openRegister"></button>
                <button class="button4" @click="openRegister"></button>
              </div>
              <div class="button-group">
                <button class="button1" @click="openList">問題確認・編集</button>
                <button class="button2" @click="openList">問題確認・編集</button>
                <button class="button3" @click="openList"></button>
                <button class="button4" @click="openList"></button>
              </div>
            </div>
          </template>
        </register>
      </transition>


      <transition name="fade">
        <list
          v-show="showList"
          @setDataNum="setDataNum"
          ref="List">
          <template #button-slot>
            <div class="buttons">
              <div class="button-group">
                <button class="button1" @click="openStartGame">Home</button>
                <button class="button2" @click="openStartGame">Home</button>
                <button class="button3" @click="openStartGame"></button>
                <button class="button4" @click="openStartGame"></button>
              </div>
              <div class="button-group">
                <button class="button1" @click="openRegister">新規問題登録</button>
                <button class="button2" @click="openRegister">新規問題登録</button>
                <button class="button3" @click="openRegister"></button>
                <button class="button4" @click="openRegister"></button>
              </div>
              <div class="button-group">
                <button class="button1" @click="openList">問題確認・編集</button>
                <button class="button2" @click="openList">問題確認・編集</button>
                <button class="button3" @click="openList"></button>
                <button class="button4" @click="openList"></button>
              </div>
            </div>
          </template>
        </list>
      </transition>

    </div>
</template>

<script>
import GameStart from '~/components/GameStart.vue'
import Register from '~/components/Register.vue'
import Game from '~/components/Game.vue'
import List from '~/components/List.vue'
export default {
    components: {
      GameStart,
      Register,
      Game,
      List,
    },
    data(){
      return{
        showRegister: false,
        showGame: false,
        showList: false,
        showStartGame: true,
        quizNum: 0,
        dataNum: 0,
        data: [],
        key: 0,
        dataList: [
          {showStr: '頭隠して尻隠さず', inputStr: 'atamakakusitesirikakusazu'},
          {showStr: '後は野となれ山となれ', inputStr: 'atohanotonareyamatonare'},
          {showStr: '虻蜂取らず', inputStr: 'abuhatitorazu'},
          {showStr: '案ずるより産むが易し', inputStr: 'anzuruyoriumugayasusi'},
          {showStr: '石の上にも三年', inputStr: 'isinouenimosannnenn'},
          {showStr: '石橋を叩いて渡る', inputStr: 'isibasiwotataitewataru'},
          {showStr: '急がば回れ', inputStr: 'isogabamaware'},
          {showStr: '犬も歩けば棒に当たる', inputStr: 'inumoarukebabouniataru'},
          {showStr: '井の中の蛙大海を知らず', inputStr: 'inonakanokawazutaikaiwosirazu'},
          {showStr: '馬の耳に念仏', inputStr: 'umanomimininenbutu'},
          {showStr: '海老で鯛を釣る', inputStr: 'ebidetaiwoturu'},
          {showStr: '縁の下の力持ち', inputStr: 'ennnositanotikaramoti'},
          {showStr: '鬼に金棒', inputStr: 'oninikanabou'},
          {showStr: '帯に短し襷に長し', inputStr: 'obinimijikasitasukininagasi'}
        ]
      }
    },
    methods:{
      openStartGame() {
        this.closeList()
        this.closeRegister()
        this.showStartGame = true
      },
      closeStartGame() {
        this.showStartGame = false
      },
      gameStart() {
        this.$refs.Game.gameStart()
        this.showGame = true
      },
      openRegister() {
        this.closeList()
        this.closeStartGame()
        this.showRegister = true
        this.$refs.Register.inputStart()
      },
      closeRegister(){
        this.showRegister = false
      },
      setQuizNum(num) {
        this.quizNum = num - 0
      },
      closeGame() {
        this.showGame = false
      },
      openList() {
        this.closeRegister()
        this.closeStartGame()
        this.$refs.List.getQuiz()
        this.showList = true
        //データにkeyを追加する時に使った。
        // this.data = JSON.parse(localStorage.getItem("dataList"))
        // this.data.forEach(element => {
        //   element.key = this.key
        //   this.key++
        // })
        // localStorage.setItem('dataList', JSON.stringify(this.data))
      },
      closeList() {
        this.showList = false
      },
      setDataNum(num) {
        this.dataNum = num
      }
    }
  }
</script>

<style lang="scss">
*{
  margin: 0;
  color: #333;
}
h1{
  margin: 48px auto 16px;
}
h2{
  margin: 32px auto;
}
.fade{
  &-enter{
    opacity:0;
    &-to{
      opacity:1;
    }
    &-active{
      transition:.5s;
    }
  }
  &-leave{
    opacity:1;
    &-to{
      opacity:0;
    }
    &-active{
      transition:.5s;
    }
  }
}
</style>
<style lang="scss" scoped>
h1{
  padding-top: 20px;
  margin: 0;
}
.container{
  width:100vw;
  height: 100vh;
  text-align: center;
  // position: absolute;
  top: 0;
  left: 0;
}
.buttons{
  width: 50%;
  margin: 30px auto 0;
  display: flex;
  flex-wrap: wrap;
}
.button-group{
  position: relative;
  width: 150px;
  height: 40px;
  margin: 0 auto;
  transform-style: preserve-3d;
  perspective: 60000px;
  transform-origin: 50% 50% 20px;
  flex-direction: column;
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
</style>
