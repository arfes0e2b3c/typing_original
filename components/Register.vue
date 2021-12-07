<template>
  <div class="register-container">
    <h1>新規問題登録</h1>
    <slot name="button-slot">
      buttons
    </slot>
    <div class="errors">
      <p v-for="error in errors" :key="error.key">
        {{ error.text }}
      </p>
    </div>
    <div class="register-container">
      <form action="">
        <div class="form-group">
          <label for="show-str">表示文字列</label><br>
          <input
            ref="input"
            type="text"
            class="form-control"
            aria-label="表示文字列"
            v-model="form.showStr"
            id="show-str">
        </div>
        <div class="form-group">
          <label for="input-str">入力文字列</label><br>
          <input
            ref="input2"
            type="text"
            class="form-control"
            aria-label="入力文字列"
            v-model="form.inputStr"
            spellcheck="false"
            id="input-str">
        </div>
        <div class="button-group">
          <button type="submit" class="button1" @click.prevent="toRegister">保存して新規登録</button>
          <button type="submit" class="button2" @click.prevent="toRegister">保存して新規登録</button>
          <button type="submit" class="button3" @click.prevent="toRegister"></button>
          <button type="submit" class="button4" @click.prevent="toRegister"></button>
        </div>
      </form>
    </div>
  </div>
</template>

<script>
export default {
  data(){
    return{
      itemNum: 0,
      dataList: [],
      form:{
        showStr:'',
        inputStr:'',
        key: 0
      },
      errors:[],
      key: 0
    }
  },
  methods:{
    validateForm() {
      if(!this.form.showStr) this.errors.push({text:'表示文字列を入力してください。', key: 1})
      if(!this.form.inputStr) this.errors.push({text:'入力文字列を入力してください。', key: 2})
    },
    toRegister(){
      this.errors = []
      this.validateForm()
      if(this.errors.length === 0) {
        this.dataList =  JSON.parse(localStorage.getItem("dataList"))
        this.form.key = this.dataList.length - 1
        this.dataList.push(this.form)
        this.setKey()
        this.itemNum++
        this.$emit('setDataNum', this.dataList.length)
        localStorage.setItem('dataList',JSON.stringify(this.dataList))
        this.form.showStr = ''
        this.form.inputStr = ''
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
    inputStart() {
      this.$nextTick(() => {
      this.$refs["input"].focus()
      })
    },
    setKey() {
      this.key = 0
      let that = this
      this.dataList.forEach(element => {
        element.key = that.key
        that.key++
      })
    }
  }
}
</script>

<style scoped>
.register-container{
  width:100%;
  height: 300px;
  position: absolute;
  text-align: center;
}
.errors{
  color:red;
  height: 56px;
  margin: 10px auto;
}
.errors p{
  margin: 0;
}
.errors p:nth-child(1){
  padding-bottom: 8px;
}
input{
  text-align: center;
  border: none;
  border-bottom: 1px solid rgba(0,0,0,0.2);
  width: 40%;
  height:30px;
  font-size: 2em;
  outline: none;
  transition: .5s;
}
input:focus{
  border-bottom: 1px solid rgba(0,0,0,0.8);
  width: 50%;
}
.form-group{
  margin-bottom: 30px;
}
.button-group{
  position: relative;
  width: 150px;
  height: 40px;
  margin: 0 auto;
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
  border: 1px solid #333;
  cursor: pointer;
  transition: .5s;
  overflow: hidden;
  color: #333;
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
