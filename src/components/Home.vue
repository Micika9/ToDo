<template>
  <div>
    <h1>To do list</h1>

    <el-row :gutter="20">
      <el-col :span="12" :offset="6">
        <div class="flex">
          <el-input
            ref="input"
            placeholder="Enter task name..."
            v-model="input"
            clearable
            @keyup.enter="addTask()"/>
          <el-button
            type="primary"
            class="btn"
            @click="addTask()">
            Add
          </el-button>
        </div>

        <ul>
          <li
            v-for="(task, index) in list"
            :key="task.id">
            <span>{{ task.name }}</span>
            <div class="pull-right">
              <el-switch
                v-model="task.done"
                @change="updateTask($event, index)" />

              <el-button
                class="delete"
                type="danger"
                size="mini"
                icon="el-icon-delete"
                @click="remove(task.id, index)" />
            </div>
          </li>
        </ul>
      </el-col>
    </el-row>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  data () {
    return {
      input: '',
      list: [],
      url: 'http://localhost:3000/todos'
    }
  },

  created () {
    this.getTasks()
  },

  methods: {
    getTasks () {
      axios.get(this.url).then(response => {
        this.list = response.data
      })
    },

    addTask () {
      if (this.input) {
        const payload = {
          name: this.input,
          done: false
        }
        const loading = this.$loading()
        axios.post(this.url, payload).then(response => {
          this.list.push(response.data)
          this.input = ''
          this.$refs.input.focus()
          loading.close()
          this.$message.success('Task added')
        })
      }
    },

    remove (id, index) {
      this.$confirm('Are you sure you want to delete this task?', 'Warning', {
        type: 'warning',
        confirmButtonText: 'OK',
        cancelButtonText: 'Cancel'
      }).then(() => {
        const loading = this.$loading()
        axios.delete(`${this.url}/${id}`).then(() => {
          this.list.splice(index, 1)
          this.$message.success('Delete completed')
          loading.close()
        })
      }).catch(() => {})
    },

    updateTask (value, index) {
      const payload = this.list[index]
      axios.put(`${this.url}/${payload.id}`, payload).then(() => {
        this.$message.success(`Task updated to ${value ? 'done' : 'not done'}`)
      })
    }
  }
}
</script>

<style lang="scss" scoped>
h1 {
  text-align: center;
  color: #fff;
  font-size: 30px;
  margin-bottom: 40px;
}
.flex {
  display: flex;
  .btn {
    margin-left: 10px;
  }
}
ul {
  margin-top: 30px;
  li {
    margin: 20px 0;
    span {
      font-size: 15px;
      color: #fff;
    }
  }
  .pull-right {
    float: right;
    .delete {
      margin-left: 10px;
    }
  }
}
</style>



