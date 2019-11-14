<template>
  <div class="home">
    <navigation />

    <section>
      <div class="container">
        <div class="add-item">
          <div class="add-icon" @click="addItem()">
            <i
              class="el-icon-plus">
            </i>
          </div>
          <form @submit.prevent="addItem()">
            <input
              type="text"
              placeholder="Add to-do list"
              v-model="entry">
          </form>
          <div class="star-icon">
            <i
              v-if="!entryFavorite"
              class="el-icon-star-off"
              @click="entryFavorite = !entryFavorite">
            </i>
            <i
              v-else
              class="el-icon-star-on"
              @click="entryFavorite = !entryFavorite">
            </i>
          </div>
        </div>
        <div class="todo-list">
          <div
            class="item"
            :class="{'show': item.show}"
            v-for="(item, index) in todoList"
            :key="index">
            <div class="checkbox">
              <i
                v-if="!item.complete"
                class="el-icon-error"
                @click="completeItem(item)">
              </i>
              <i
                v-else
                class="el-icon-success">
              </i>
            </div>
            <div
              class="title">
              {{ item.title }}
            </div>
            <div class="feature-icon">
              <i
                v-if="!item.favorite"
                class="el-icon-star-off"
                @click="setFavoriteItem(item)">
              </i>
              <i
                v-else
                class="el-icon-star-on"
                @click="item.favorite = !item.favorite">
              </i>
            </div>
            <div class="delete">
              <i
                class="el-icon-delete"
                @click="deleteToDo()">
              </i>
            </div>
          </div>
          <div
            class="completed"
            v-if="completedTodoList.length > 0 ">
            <div
              class="done"
              @click="showCompletedList = !showCompletedList">Done</div>
          </div>
          <div
            class="todo-list completed"
            v-if="showCompletedList">
            <div
              class="item"
              :class="{'show': item.show}"
              v-for="(item, index) in completedTodoList"
              :key="index">
              <div class="checkbox">
                <i
                  v-if="!item.complete"
                  class="el-icon-error">
                </i>
                <i
                  v-else class="el-icon-success"
                  @click="uncompleteItem(item)">
                </i>
              </div>
              <div class="title">
                {{ item.title }}
              </div>
              <div class="feature-icon">
                <i
                  v-if="!item.favorite"
                  class="el-icon-star-off">
                </i>
                <i
                  v-else class="el-icon-star-on">
                </i>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>
  </div>
</template>

<script>
import Navigation from '@/components/Navigation'
import { setTimeout } from 'timers'

export default {
  name: 'Home',
  components: {
    Navigation
  },

  data () {
    return {
      entry: '',
      entryFavorite: false,
      todoList: [],
      completedTodoList: [],
      showCompletedList: false
    }
  },

  methods: {
    addItem () {
      if (this.entry !== '') {
        let date = new Date()
        let newEntry = {
          id: date.getTime(),
          title: this.entry,
          favorite: this.entryFavorite,
          complete: false,
          show: false
        }
        if (newEntry.favorite) {
          this.todoList.splice(0, 0, newEntry)
        } else {
          this.todoList.push(newEntry)
        }

        setTimeout(() => {
          this.todoList.find(element => element.id === newEntry.id).show = true
        }, 10)
      }
      this.entry = ''
      this.entryFavorite = false
    },
    setFavoriteItem (item) {
      item.favorite = !item.favorite
      item.show = false

      setTimeout(() => {
        let index = this.todoList.findIndex(element => element.id === item.id)
        this.todoList.splice(index, 1)
        this.todoList.splice(0, 0, item)
        item.show = true
      }, 500)
    },
    completeItem (item) {
      item.complete = !item.complete
      item.show = false

      setTimeout(() => {
        this.completedTodoList.push(item)
        let index = this.todoList.findIndex(element => element.id === item.id)
        this.todoList.splice(index, 1)
        item.show = true
      }, 500)
    },
    uncompleteItem (item) {
      item.complete = !item.complete
      item.show = false

      setTimeout(() => {
        if (item.favorite) {
          this.todoList.splice(0, 0, item)
        } else {
          this.todoList.push(item)
        }
        let index = this.completedTodoList.findIndex(element => element.id === item.id)
        this.completedTodoList.splice(index, 1)
        item.show = true
      }, 500)
    },
    deleteToDo (index) {
      this.todoList.splice(index, 1)
    }
  }
}
</script>

<style lang="scss" scoped>
section {
  width: 100%;
  .container {
    padding: 30px;
    width: 100%;
    min-height: 100%;
    .add-item {
      display: grid;
      grid-template-columns: 70px auto 70px;
      grid-template-rows: 60px;
      width: 100%;
      height: 60px;
      background-color: rgba(0,0, 0, .3);
      border-radius: 5px;
      overflow: hidden;
      .add-icon {
        grid-column-start: 1;
        grid-column-end: 1;
        display: flex;
        justify-content: center;
        align-items: center;
        color: #fff;
        font-size: 25px;
        cursor: pointer;
      }
    }
    input {
      grid-column-start: 2;
      grid-column-end: 2;
      color: #fff;
      width: 100%;
      height: 60px;
      background: none;
      border: none;
      font-size: 15px;
      &::placeholder {
        color: #fff;
      }
      &:focus {
        outline: none;
      }
    }
    .star-icon {
      grid-column-start: 3;
      grid-column-end: 3;
      display: flex;
      justify-content: center;
      align-items: center;
      color: #fff;
      font-size: 25px;
      cursor: pointer;
    }
  }
  .todo-list {
    padding-top: 20px;
    .item {
      display: inline-block;
      width: 100%;
      height: 60px;
      margin-bottom: 5px;
      background-color: #fff;
      border-radius: 5px;
      overflow: hidden;
      opacity: 0;
      transition: all .5 linear;
      .checkbox {
        width: 60px;
        padding: 20px 20px;
        display: inline-block;
        align-items: left;
        font-size: 20px;
        cursor: pointer;
      }
      .title {
        width: 60%;
        display: inline-block;
        justify-content: center;
        align-items: center;
        font-size: 15px;
      }
      .feature-icon {
        width: 60px;
        padding: 20px 10px;
        float: right;
        color: #000;
        font-size: 20px;
        cursor: pointer;
      }
      .delete {
        width: 60px;
        padding: 20px 10px;
        float: right;
        color: #000;
        font-size: 20px;
        cursor: pointer;
      }
    }
    .show {
      opacity: 1;
    }
  }
}
</style>
