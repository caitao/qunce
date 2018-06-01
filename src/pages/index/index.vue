<template>
  <!-- <div class="container" @click="clickHandle('test click', $event)">
    <div class="userinfo" @click="bindViewTap">
      <img class="userinfo-avatar" v-if="userInfo.avatarUrl" :src="userInfo.avatarUrl" background-size="cover" />
      <div class="userinfo-nickname">
        <p>{{userInfo.nickName}}</p>
      </div>
      <mptoast></mptoast> @click="clickHandle('test click', $event)"
    </div> -->
  <!-- <div class="container"> -->
    <div class="mpvue-demo">
      <p class="title" @click='bindViewTap'>{{title}}</p>
      <input type="text" v-model='mytodo'>
      <button @click='addTodo'>添加计划</button>
      <button @click='clearTodo'>清理</button>
      <ul class="todos">
        <li
          v-for='(todo,i) in todos'
          :key='i'
          @click='toggle(i)'
          :class="{'done': todo.done}">{{todo.todoname}}</li>
        <li>{{mytodo}}</li>
        <li>{{todoNum}}|{{todos.length}}</li>
      </ul>
      <mptoast></mptoast>
    </div>

  <!-- </div> -->
</template>

<script>
import mptoast from 'mptoast'
export default {
  components: {
    mptoast
  },

  data () {
    return {
      title: 'Todo List',
      userInfo: {},
      mytodo: '',
      todos: []
    }
  },

  computed: {
    todoNum () {
      return this.todos.filter(v => !v.done).length
    }
  },

  methods: {
    bindViewTap () {
      this.$mptoast('提示一下')
    },
    getUserInfo () {
      // 调用登录接口
      wx.login({
        success: () => {
          wx.getUserInfo({
            success: (res) => {
              this.userInfo = res.userInfo
            }
          })
        }
      })
    },
    clickHandle (msg, ev) {
      console.log('clickHandle:', msg, ev)
    },
    updateStorage () {
      wx.setStorageSync('todos', this.todos)
    },
    addTodo () {
      if (!this.mytodo) {
        return
      }
      this.todos.push({todoname: this.mytodo, done: false})
      this.mytodo = ''
      this.updateStorage()
    },
    toggle (i) {
      this.todos[i].done = !this.todos[i].done
      this.updateStorage()
    },
    clearTodo () {
      this.todos = this.todos.filter(v => !v.done)
      this.updateStorage()
    }
  },

  created () {
    this.todos = wx.getStorageSync('todos') || []
    // 调用应用实例的方法获取全局数据
    this.getUserInfo()
  }
}
</script>

<style scoped>
/* .userinfo {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.userinfo-avatar {
  width: 128rpx;
  height: 128rpx;
  margin: 20rpx;
  border-radius: 50%;
}

.userinfo-nickname {
  color: #aaa;
} */

.title {
  color: #38b0de;
  text-align: center;
  /* margin-top: 150px; */
}
ul.todos{
  margin: 20px;
}
input{
  border:1px solid #38b0de;
}
.done{
  text-decoration: line-through;
}
</style>
