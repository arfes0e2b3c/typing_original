<template>
  <div class="background">
    <div class="edit-container">
      <i id="icon" class="fas fa-times-circle" @click="closeEdit"></i>
      <div class="errors">
        <p v-for="error in errors" :key="error.key">
        {{ error.text }}
        </p>
      </div>
      <form action="">
        <div class="form-group">
          <label for="edit-show-str">表示文字列</label><br>
          <input ref="input" type="text" class="form-control" aria-label="表示文字列" v-model="dataList[editKeyData].showStr" value="s" id="edit-show-str">
        </div>
        <div class="form-group">
          <label for="edit-input-str">入力文字列</label><br>
          <input ref="input2" type="text" class="form-control" aria-label="入力文字列" v-model="dataList[editKeyData].inputStr" spellcheck="false" id="edit-input-str">
        </div>
        <div class="buttons">
          <div class="button-group">
            <button type="submit" class="button1" @click.prevent="toRegisterClose">保存</button>
            <button type="submit" class="button2" @click.prevent="toRegisterClose">保存</button>
            <button type="submit" class="button3" @click.prevent="toRegisterClose"></button>
            <button type="submit" class="button4" @click.prevent="toRegisterClose"></button>
          </div>
          <br>
          <div class="button-group">
            <button class="button1" @click.prevent="closeEdit">閉じる</button>
            <button class="button2" @click.prevent="closeEdit">閉じる</button>
            <button class="button3" @click.prevent="closeEdit"></button>
            <button class="button4" @click.prevent="closeEdit"></button>
          </div>
          <br>
          <div class="button-group">
            <button class="button1" @click.prevent="deleteQuiz">削除</button>
            <button class="button2" @click.prevent="deleteQuiz">削除</button>
            <button class="button3" @click.prevent="deleteQuiz"></button>
            <button class="button4" @click.prevent="deleteQuiz"></button>
          </div>
        </div>
      </form>
      <i class="fas fa-chevron-left left" @click="moveQuiz(-1)"></i>
      <i class="fas fa-chevron-right right" @click="moveQuiz(1)"></i>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return{
      quizKey: 0,
      errors: [],
      editKeyData: this.editKey
    }
  },
  props:{
    editKey:{
    },
    dataList:{
      type: Array
    }
  },
  methods:{
    inputStart() {
      this.$nextTick(() => {
      this.$refs["input"].focus()
      })
    },
    closeEdit() {
      this.$emit('closeEdit')
    },
    toRegisterClose() {
      this.validateForm()
      if(this.errors.length === 0) {
        this.emitDataList()
        localStorage.setItem('dataList', JSON.stringify(this.dataList))
        this.closeEdit()
      }else if(this.errors[0].key === 2) {
        this.$nextTick(() => {
          this.$refs["input2"].focus()
        })
      }else{
        this.$nextTick(() => {
          this.$refs["input"].focus()
        })
      }
    },
    deleteQuiz() {
      if(confirm('本当に削除しますか？')){
        this.closeEdit()
        this.dataList.splice(this.editKey, 1)
        this.$emit('setDataNum', this.dataList.length)
        this.emitDataList()
        localStorage.setItem('dataList', JSON.stringify(this.dataList))
      }else{
        this.inputStart()
      }
    },
    emitDataList() {
      this.$emit('emitDataList', this.dataList)
    },
    setKey() {
      this.quizKey = 0
      let that = this
      this.dataList.map(element => {
        element.key = that.quizKey
        that.quizKey++
      })
    },
    moveQuiz(num) {
      if(!(num === 1 && this.editKeyData === this.dataList.length - 1) && !(num === -1 && this.editKeyData === 0)) {
        this.errors = []
        this.editKeyData += num
        this.$nextTick(() => {
          this.$refs["input"].focus()
        })
      }
    },
    validateForm() {
      this.errors = []
      if(!this.dataList[this.editKeyData].showStr) this.errors.push({text:'表示文字列を入力してください。', key: 1})
      if(!this.dataList[this.editKeyData].inputStr) this.errors.push({text:'入力文字列を入力してください。', key: 2})
    }
  },
  watch: {
    dataList: function() {
      this.setKey()
    }
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
.edit-container{
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
  color: white;
}
#icon:hover{
  transition: .5s;
  transform: rotate(90deg);
}
form{
  margin-top:90px;
}
p{
  color: white;
}
input{
  text-align: center;
  background: #0e2b3c;
  border: none;
  border-bottom: 1px solid rgba(255,255,255,0.2);
  width: 60%;
  height:30px;
  color: white;
  font-size: 1.7em;
  outline: none;
  transition: .5s;
  margin-top: 30px;
}
input:focus{
  border-bottom: 1px solid rgba(255,255,255,1);
  width: 45%;
}
h1{
  margin-top:50px;
}
.errors{
  position: absolute;
  left: 50%;
  transform: translateX(-50%);
  top: 20px;
}
.errors p{
  color:red;
}
.form-group{
  margin-bottom: 30px;
}
.form-group label{
  color: white;
}
.buttons{
  width: 60%;
  margin: 0 auto;
  display: flex;
  flex-wrap: wrap;
}
.button-group{
  position: relative;
  width: 150px;
  height: 40px;
  margin: 8px auto;
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
.left{
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  left: 40px;
  font-size: 3em;
  color: white;
  cursor: pointer;
}
.right{
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  right: 40px;
  font-size: 3em;
  color: white;
  cursor: pointer;
}
.right:hover{
  animation: moveRight .5s;
}
@keyframes moveRight{
  0%{
    transform: translateX(0)translateY(-50%);
  }
  50%{
    transform: translateX(5px)translateY(-50%);
  }
  100%{
    transform: translateX(0)translateY(-50%);
  }
}
.left:hover{
  animation: moveLeft .5s;
}
@keyframes moveLeft{
  0%{
    transform: translateX(0)translateY(-50%);
  }
  50%{
    transform: translateX(-5px)translateY(-50%);
  }
  100%{
    transform: translateX(0)translateY(-50%);
  }
}
</style>
