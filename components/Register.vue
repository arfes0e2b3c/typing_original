<template>
  <div class="background">
    <div class="container">
      <i id="icon" class="fas fa-times-circle" @click="closeRegister"></i>
      <h1>新規問題登録</h1>
      <div class="error">
        {{ this.errors[0]}}<br>
        {{ this.errors[1]}}
      </div>
      <div class="register-container">
        <form action="">
          <div class="form-group">
            <label>表示文字列</label><br>
            <input ref="input" type="text" class="form-control" aria-label="表示文字列" v-model="form.showStr">
          </div>
          <div class="form-group">
            <label>入力文字列</label><br>
            <input type="text" class="form-control" aria-label="入力文字列" v-model="form.inputStr" spellcheck="false">
          </div>
          <div class="button-group">
            <button type="submit" class="button1" @click.prevent="toRegister">保存して新規登録</button>
            <button type="submit" class="button2" @click.prevent="toRegister">保存して新規登録</button>
            <button type="submit" class="button3" @click.prevent="toRegister"></button>
            <button type="submit" class="button4" @click.prevent="toRegister"></button>
          </div>
          <br>
          <div class="button-group">
            <button type="submit" class="button1" @click.prevent="toRegisterClose">保存</button>
            <button type="submit" class="button2" @click.prevent="toRegisterClose">保存</button>
            <button type="submit" class="button3" @click.prevent="toRegisterClose"></button>
            <button type="submit" class="button4" @click.prevent="toRegisterClose"></button>
          </div>
          <br>
          <div class="button-group">
            <button class="button1" @click.prevent="closeRegister">閉じる</button>
            <button class="button2" @click.prevent="closeRegister">閉じる</button>
            <button class="button3" @click.prevent="closeRegister"></button>
            <button class="button4" @click.prevent="closeRegister"></button>
          </div>
        </form>
      </div>
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
        inputStr:''
      },
      errors:[]
    }
  },
  methods:{
    closeRegister(){
      let that = this
      setTimeout(function(){
        that.errors = []
        that.form.showStr = ''
        that.form.inputStr = ''
      },500)
      this.$emit('closeRegister')
    },
    validateForm() {
      if(!this.form.showStr) this.errors.push('表示文字列を入力してください。')
      if(!this.form.inputStr) this.errors.push('入力文字列を入力してください。')
    },
    toRegister(){
      this.errors = []
      this.validateForm()
      if(this.errors.length === 0) {
        this.dataList =  JSON.parse(localStorage.getItem("dataList"))
        this.dataList.push(this.form)
        this.itemNum++
        this.$emit('setDataNum', this.dataList.length)
        localStorage.setItem('dataList',JSON.stringify(this.dataList))
        this.form.showStr = ''
        this.form.inputStr = ''
      }
      this.$nextTick(() => {
      this.$refs["input"].focus()
      })
    },
    toRegisterClose(){
      if(this.form.showStr && this.form.inputStr) {
        this.closeRegister()
      }
      this.toRegister()
    },
    inputStart() {
      this.$nextTick(() => {
      this.$refs["input"].focus()
      })
    }
  },
  mounted(){
    if(!JSON.parse(localStorage.getItem("dataList"))){
      localStorage.setItem('dataList',JSON.stringify(this.dataList))
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
h1{
  margin-top:50px;
}
.error{
  color:red;
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
}
input:focus{
  border-bottom: 1px solid rgba(255,255,255,1);
  width: 45%;
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
