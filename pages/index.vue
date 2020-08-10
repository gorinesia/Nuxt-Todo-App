<template>
  <section class="container">
    <h1>Todo App</h1>
    <p><input type="text" name="content" v-model="content" @focus="set_flg"></p>
    <div>
      <button @click="insert">save</button>
      <button @click="find">find</button>
    </div>
    <ul>
      <li v-for="(todo, index) in todos" :key="index">
        <span>{{ todo.content }}</span><span>{{ todo.created }}</span><span @click="remove(todo)">☓</span>
      </li>
    </ul>
  </section>
</template>

<script>
import {mapState} from 'vuex';

export default {
  data: function() {
    return {
      content: '',
      find_flg: false
    }
  },
  computed: {
    ...mapState(['todos']),
    display_todos: function() {
      if (this.find_flg) {
        var arr = [];
        var data = this.todos;
        data.forEach(element => {
          if (element.content.toLowerCase() == this.content.toLowerCase()) {
            arr.push(element);
          }
        });
        return arr;
      } else {
        return this.todos;
      }
    }
  },
  methods: {
    insert: function() {
      this.$store.commit('insert', {content: this.content});
      this.content = '';
    },
    find: function() {
      this.find_flg = true;
    },
    set_flg: function() {
      if (this.find_flg) {
        this.find_flg = false;
        this.content = '';
      }
    },
    remove: function(todo) {
      this.$store.commit('remove', todo)
    }
  }
}
</script>

<style>
.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.title {
  font-family:
    'Quicksand',
    'Source Sans Pro',
    -apple-system,
    BlinkMacSystemFont,
    'Segoe UI',
    Roboto,
    'Helvetica Neue',
    Arial,
    sans-serif;
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
}
</style>
