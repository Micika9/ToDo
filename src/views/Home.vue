<template>
  <div class="home">
    <navigation />
    <section>
      <div class="all-boards">
        <ul class="board-list">
          <router-link
            :to="{ name: 'Board' }">
            <li class="list">
              <div class="todo-list">
                <h1>Title</h1>
                <p
                  class="empty-text">
                  Create new board
                </p>
              </div>
            </li>
          </router-link>
        </ul>
      </div>
    </section>
  </div>
</template>

<script>
import axios from 'axios'
import Navigation from '@/components/Navigation'

export default {
  name: 'Home',
  components: {
    Navigation
  },
  data () {
    return {
      todos: []
    }
  },
  async created () {
    try {
      const res = await axios.get(`http://localhost:3000/todos`)
      this.todos = res.data
    } catch (e) {
      console.log(e)
    }
  }
}
</script>

<style lang="scss" scoped>
.home {
  width: 100%;
  height: 100%;
  section {
    width: 100%;
    height: 100%;
    background-color: rgb(241, 236, 236);
    display: flex;
    flex-direction: row;
    align-items: flex-start;
    justify-content: center;
    .all-boards {
      max-width: 1160px;
      width: 100%;
      height: 100%;
      margin: 0 auto;
      text-align: center;
      background-color: rgb(219, 219, 215);
      .board-list {
        display: flex;
        flex-wrap: wrap;
        margin: 50px 50px 0;
        .list {
          position: absolute;
          box-sizing: border-box;
          width: 250px;
          height: 150px;
          margin-right: 20px;
          border-radius: 5px;
          background-color: rgba(9,30,66,.04);
          .todo-list {
            background-color: rgba(9,30,66,.04);
            box-shadow: none;
            border: none;
            border-radius: 5px;
            display: table-cell;
            height: 150px;
            font-weight: 400;
            text-align: center;
            vertical-align: middle;
            width: inherit;
            transition-property: background-color,border-color,box-shadow;
            transition-duration: 85ms;
            transition-timing-function: ease;
            &:hover {
              background-color: #999;
              cursor: pointer;
            }
            h1 {
              position: absolute;
              box-sizing: border-box;
              top: 0;
              left: 20px;
              width: 100%;
              text-align: left;
              color:#172b4d;
            }
            .empty-text {
              margin-bottom: 0;
              color: #172b4d;
            }
          }
        }
      }
    }
  }
}
</style>
