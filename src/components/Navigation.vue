<template>
  <header>
    <nav>
      <div class="logo">
        <img :src="img">
      </div>
      <h1>To Do list</h1>
    </nav>
  </header>
</template>

<script>
import img from '@/assets/images/logo.png'

export default {
  name: 'Navigation',

  data () {
    return {
      img
    }
  }
}
</script>

<style scoped lang="scss">
  header {
    position: relative;
    z-index: 2;
    width: 100%;
    height: 60px;
    background-color: #15bafb;
    display: flex;
    align-items: center;
    justify-content: center;
    -webkit-box-shadow: 0px 2px 5px 1px rgba(0,0,0,0.29);
    -moz-box-shadow: 0px 2px 5px 1px rgba(0,0,0,0.29);
    box-shadow: 0px 2px 5px 1px rgba(0,0,0,0.29);
    nav {
      width: 100%;
      text-align: center;
      .logo {
          display: inline-block;
          vertical-align: middle;
          margin-right: 30px;
          width: 110px;
        img {
          height: 60px;
        }
      }
      h1 {
        display: inline-block;
        vertical-align: middle;
        font-size: 25px;
        color: #fff;
      }
    }
  }
</style>
