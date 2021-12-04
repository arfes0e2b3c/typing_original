<template>
  <div class="background">
    <div class="container">
      <i id="icon" class="fas fa-times-circle" @click="closeEdit"></i>
      <h2>問題確認・編集</h2>
      <table>
        <tr v-for="data in splicedList" :key="data.key">
          <td>{{ data.key + 1 }}</td>
          <td>{{ data.showStr }}</td>
          <td>{{ data.inputStr }}</td>
          <td><button>編集</button></td>
          <td><button>削除</button></td>
        </tr>
      </table>
      <div class="move">
        <i class="fas fa-chevron-left left" @click="moveQuiz(-1)"></i>
        <i class="fas fa-chevron-right right" @click="moveQuiz(1)"></i>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return{
      dataList: [],
      splicedList: [],
      quizNum: 0
    }
  },
  methods:{
    closeEdit() {
      this.$emit('closeEdit')
    },
    moveQuiz(sign) {
      if(!(this.quizNum === 0 && sign === -1) && !(this.quizNum === this.dataList.length - this.dataList.length % 10 && sign === 1)){
        this.quizNum += 10 * sign
        this.modifyQuiz()
      }
    },
    modifyQuiz() {
      this.splicedList = this.dataList.slice(this.quizNum, this.quizNum + 10)
      this.splicedList.forEach(e => {
        if(e.showStr.length > 10){
          e.showStr = e.showStr.slice(0, 10) + "..."
        }
        if(e.inputStr.length > 15){
          e.inputStr = e.inputStr.slice(0, 10) + "..."
        }
      })
    }
  },
  mounted(){
    this.dataList = JSON.parse(localStorage.getItem("dataList"))
    this.modifyQuiz()
  }
}
</script>

<style scoped>
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
  color:white;
  justify-content: center;
  align-items: center;
  box-shadow: 0 0 16px #444;
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
table{
  margin: 40px auto 0;
  width: 80%;
}
tr{
  height: 35px;
}
.move{
  position: absolute;
  left: 50%;
  transform: translateX(-50%);
  bottom: 20px;
}
.move i{
  margin-top: 20px;
  /* margin-left: 20px; */
  font-size:3em;
  cursor: pointer;
}
.right:hover{
  animation: moveRight .5s;
}
@keyframes moveRight{
  0%{
    transform: translateX(0);
  }
  50%{
    transform: translateX(5px);
  }
  100%{
    transform: translateX(0);
  }
}
.left:hover{
  animation: moveLeft .5s;
}
@keyframes moveLeft{
  0%{
    transform: translateX(0);
  }
  50%{
    transform: translateX(-5px);
  }
  100%{
    transform: translateX(0);
  }
}
button{
  width:100%;
  height: 2em;
  background-color: #0e2b3c;
  border: none;
  color: white;
  transition: .5s;
  padding: 5px auto;
}
button:hover{
  background-color: rgba(115,143,160);
}
</style>
