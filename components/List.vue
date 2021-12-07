<template>
  <div class="list-container">
    <h1>問題確認・編集</h1>
    <slot name="button-slot">
      buttons
    </slot>
    <div class="table-container">
      <table>
        <tr v-for="data in slicedList" :key="data.key">
          <td>{{ data.key + 1 }}</td>
          <td>{{ data.omittedShowStr }}</td>
          <td>{{ data.omittedInputStr }}</td>
          <td @click="openEdit(data.key)" class="td-button">編集</td>
          <td @click="deleteQuiz(data.key)" class="td-button">削除</td>
        </tr>
      </table>
      <div class="move">
        <i class="fas fa-chevron-left left" @click="moveQuiz(-1)"></i>
        <i class="fas fa-chevron-right right" @click="moveQuiz(1)"></i>
      </div>
    </div>
    <transition name="fade">
      <edit
        v-if="showEdit"
        :editKey="editKey"
        :dataList="dataList"
        @closeEdit="closeEdit"
        @emitDataList="emitDataList"
        ref="Edit"/>
    </transition>
  </div>
</template>

<script>
import Edit from '~/components/Edit.vue'
export default {
  components:{
    Edit
  },
  data() {
    return{
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
        ],
      slicedList: [],
      quizNum: 0,
      quizKey: 0,
      showEdit: false,
      editKey: 0
    }
  },
  methods:{
    getQuiz() {
      this.dataList = JSON.parse(localStorage.getItem("dataList"))
    },
    closeList() {
      this.$emit('closeList')
      this.resetPage()
    },
    moveQuiz(sign) {
      this.getQuiz()
      if(!(this.quizNum === 0 && sign === -1) && !(this.quizNum === this.dataList.length - this.dataList.length % 10 && sign === 1) && !(this.quizNum === this.dataList.length - 10)){
        this.quizNum += 10 * sign
      }
      this.sliceQuiz()
    },
    sliceQuiz() {
      this.slicedList = this.dataList.slice(this.quizNum, this.quizNum + 10)
      this.slicedList.map(e => {
        if(e.showStr.length > 10){
          e.omittedShowStr = e.showStr.slice(0, 10) + "..."
        }else{
          e.omittedShowStr = e.showStr
        }
        if(e.inputStr.length > 15){
          e.omittedInputStr = e.inputStr.slice(0, 15) + "..."
        }else{
          e.omittedInputStr = e.inputStr
        }
      })
    },
    deleteQuiz(key) {
      if(confirm('本当に削除しますか？')){
        this.dataList.splice(key, 1)
        this.sliceQuiz()
        this.$emit('setDataNum', this.dataList.length)
        localStorage.setItem("dataList", JSON.stringify(this.dataList))
        if(this.dataList.length === this.quizNum) {
          this.moveQuiz(-1)
        }
      }
    },
    setKey() {
      this.quizKey = 0
      let that = this
      this.dataList.map(element => {
        element.key = that.quizKey
        that.quizKey++
      })
    },
    resetPage() {
      this.quizNum = 0
      let that = this
      setTimeout(function(){
        that.sliceQuiz()
      },500)
    },
    openEdit(key) {
      this.editKey = key
      this.showEdit = true
      this.$nextTick(() => {
        this.$refs.Edit.inputStart()
      })
      // なんで？setTimeout0だと行けるのに無しだとinputStartがレンダリングされてない扱い。
      // ほんとはinputStartを読み込むことができてただけでエラーには変わりなかった
    },
    closeEdit() {
      this.showEdit = false
    },
    emitDataList(list) {
      this.dataList = list
      this.sliceQuiz()
    }
  },
  mounted(){
    if(!JSON.parse(localStorage.getItem("dataList"))){
      localStorage.setItem('dataList',JSON.stringify(this.dataList))
    }
    this.getQuiz()
    this.sliceQuiz()
  },
  watch:{
    dataList: function() {
      this.sliceQuiz()
      this.setKey()
    }
  }
}
</script>

<style scoped>
.list-container{
  width:100%;
  height: 100%;
  position: absolute;
  text-align: center;
}
.table-container{
  position: relative;
  width:100%;
  height: 490px;
}
table{
  margin: 40px auto 0;
  width: 80%;
}
tr{
  height: 40px;
}
td{
  padding: 0;
}
.move{
  position: absolute;
  bottom: 0;
  left: 50%;
  transform:translateX(-50%);
}
.move i{
  margin-top: 20px;
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
.td-button{
  background: none;
  border: 1px solid rgba(0,0,0,0.1);
  transition: .5s;
  margin: 10px 2px;
  cursor: pointer;
  border-radius: 5px;
}
.td-button:hover{
  border: 1px solid rgba(0,0,0,0.8);
}
</style>
