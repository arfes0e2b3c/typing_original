<template class="body">
  <div>
    <div class="container">
      <h1>タイピングゲーム</h1>
      <game-start
        @openRegister="openRegister"
        @gameStart="gameStart"
        @quizNum="setQuizNum"
        @openEdit="openEdit"
        :sentDataNum="dataNum"/>
    </div>
    <transition name="fade" class="register-container">
      <register
        @closeRegister="closeRegister"
        @setDataNum="setDataNum"
        v-show="showRegister"
        ref="Register"
        class="register"/>
    </transition>
    <transition name="fade">
      <game
        v-show="showGame"
        ref="Game" :quizNum="quizNum"
        @closeGame="closeGame"/>
    </transition>
    <transition name="fade">
      <edit
        v-show="showEdit"
        @closeEdit="closeEdit"/>
    </transition>
  </div>
</template>

<script>
import GameStart from '~/components/GameStart.vue'
import Register from '~/components/Register.vue'
import Game from '~/components/Game.vue'
import Edit from '~/components/Edit.vue'
export default {
    components: {
      GameStart,
      Register,
      Game,
      Edit,
    },
    data(){
      return{
        showRegister: false,
        showGame: false,
        quizNum: 0,
        showEdit: false,
        dataNum: 0
      }
    },
    methods:{
      openRegister() {
        this.showRegister = true
        this.$refs.Register.inputStart()
      },
      closeRegister(){
        this.showRegister = false
      },
      gameStart() {
        this.$refs.Game.gameStart()
        this.showGame = true
      },
      setQuizNum(num) {
        this.quizNum = num - 0
      },
      closeGame() {
        this.showGame = false
      },
      openEdit() {
        this.showEdit = true
      },
      closeEdit() {
        this.showEdit = false
      },
      setDataNum(num) {
        this.dataNum = num
      }
    }
  }
</script>

<style lang="scss" scoped>
.body{
  margin: 0 !important;
}
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
.register-container{
  position: relative;
}
.register{
  position: absolute;
  top:50%;
  left:50%;
  transform: translate(-50%,-50%);
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
.rotate{
  &-enter{
    opacity:0;
    width:1px;
    height: 1px;
    &-to{
      opacity:1;
      width:100%;
      height: 100%;
    }
    &-active{
      transition: 5s;
    }
  }
  &-leave{
    opacity:1;
    transform:rotateY(0deg);
    &-to{
      opacity:0;
      transform:rotateY(180deg);
    }
    &-active{
      transition: .5s;
    }
  }
}
</style>
