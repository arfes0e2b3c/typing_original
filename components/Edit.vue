<template>
  <div class="background">
    <div class="container">
      Edit
    </div>
  </div>
</template>

<script>
export default {

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
</style>
