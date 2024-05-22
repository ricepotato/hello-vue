<script lang="ts">
import ChildComp from './ChildComp.vue'
interface Todo {
  id: number
  text: string
  done: boolean
}
export default {
  data() {
    return {
      count: 0,
      text: '',
      message: 'hello vue!',
      titleClass: 'title',
      dynamicId: 'dynamic-id',
      dynamicId2: 'dynamic-id2',
      locale: 'ko',
      newTodo: '',
      todoId: 4,
      todos: [
        { id: 1, text: 'HTML 배우기', done: false },
        { id: 2, text: 'JavaScript 배우기', done: true },
        { id: 3, text: 'Vue 배우기', done: false }
      ],
      hideCompleted: false,
      msgFromChild: ''
    }
  },
  mounted() {
    ;(this.$refs.pElementRef as HTMLElement).textContent = '안녕하세요!'
  },
  watch: {
    count(newCount) {
      if (newCount % 2 === 0) {
        console.log('짝수!')
      } else {
        console.log('홀수!')
      }
    }
  },
  computed: {
    filteredTodos() {
      return this.todos.filter((todo) => (this.hideCompleted ? !todo.done : true))
    }
  },
  methods: {
    increment() {
      this.count++
    },
    onInput(e: any) {
      this.text = e.target.value
    },
    addTodo() {
      const newId = this.todoId + 1
      this.todos.push({ id: newId, text: this.newTodo, done: false })
      this.newTodo = ''
      this.todoId += 1
    },
    removeTodo(todo: Todo) {
      const index = this.todos.indexOf(todo)
      this.todos = this.todos.filter((_, i) => i !== index)
    }
  },
  components: {
    ChildComp
  }
}
</script>

<template>
  <h1>Make me dynamic!</h1>
  <h2>{{ message }}</h2>
  <section>
    <button type="button" v-on:click="increment">{{ `숫자 세기 ${count}` }}</button>
    <!-- 자주 사용되기 때문에 v-on에는 다음과 같은 단축 문법도 있습니다: -->
    <button type="button" @click="increment">{{ `숫자 세기 ${count}` }}</button>
  </section>
  <section>
    <div v-bind:id="dynamicId2" :class="titleClass">{{ dynamicId }}</div>
    <!-- v-bind는 너무 자주 사용되기 때문에 전용 단축 문법이 있습니다: :id -->
    <div :id="dynamicId" :class="titleClass">{{ dynamicId2 }}</div>
  </section>
  <section>
    <input :value="text" @input="onInput" placeholder="여기에 입력하기" />
    <div>입력된 내용! {{ text }}</div>
  </section>
  <!-- Vue는 양방향 바인딩을 간소화하기 위해, v-model이라는 디렉티브를 제공합니다. 이것은 위 내용에 대한 단축 표기법 입니다: -->
  <section>
    <input v-model="text" placeholder="여기에 입력하기" />
    <div>입력된 내용! {{ text }}</div>
  </section>
  <section>
    <button type="button" @click="locale = 'ko'">한글</button>
    <button type="button" @click="locale = 'en'">영문</button>
    <!-- 엘리먼트를 조건부로 렌더링하기 위해 v-if 디렉티브를 사용할 수 있습니다: -->
    <div v-if="locale === 'en'">hello</div>
    <div v-else>안녕</div>
    <div v-if="locale === 'ko'">안녕</div>
    <div v-else>hello</div>
  </section>
  <section>
    <form @submit.prevent="addTodo">
      <div>
        <input v-model="newTodo" placeholder="새로운 할 일을 입력하세요" />
      </div>
      <button>할 일 추가</button>
    </form>
    <ul class="todo-list">
      <li v-for="todo in filteredTodos" :key="todo.id">
        <input type="checkbox" v-model="todo.done" />{{ ' ' }}
        <span :class="{ done: todo.done }">{{ todo.id }} {{ todo.text }}</span>
        <span style="cursor: pointer" @click="removeTodo(todo)">❌</span>
      </li>
    </ul>
    <button @click="hideCompleted = !hideCompleted">
      {{ hideCompleted ? '완료된 작업 보이기' : '완료된 작업 숨기기' }}
    </button>
  </section>
  <section>
    <!-- 연적으로 DOM을 수동으로 작업해야 하는 경우가 있습니다. 우리는 특별한 속성인 ref를 사용하여 템플릿 참조를 요청할 수 있습니다: -->
    <p ref="pElementRef">안녕!</p>
  </section>
  <!-- 자식 component 에 props 전달 -->
  <section>
    <ChildComp :msg="'hello'" @response="(msg) => (msgFromChild = msg)">from parent</ChildComp>
    <div>{{ msgFromChild }}</div>
  </section>
</template>

<style scoped>
* {
  padding: 0;
  margin: 0;
  list-style: none;
}
.todo-list {
  margin-top: 10px;
}

.done {
  text-decoration: line-through;
}

.title {
  color: red;
}
section {
  border: 1px solid #ccc;
  background-color: #f9f9f9;
  padding: 10px;
  margin: 10px 0px;
  border-radius: 4px;
}
</style>
