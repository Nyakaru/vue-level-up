<script setup lang="ts">
import { reactive, computed, ref, watch } from 'vue';
import { useMousePosition } from './mouse';
const author = reactive({
  name: 'John Doe',
  age: 30,
  books: ['book1', 'book2', 'book3']
});

const publishedBooks = computed<string>(() => {
  return author.books.length > 0 ? author.books.join(', ') : 'No books published yet';
});

const items = ref<string[]>(['item1', 'item2', 'item3']);
const myObject = reactive<{ name: string, age: number }>({ name: 'John Doe', age: 30 });

function greet(event: { target: { tagName: any; }; }) {
  alert('Hello, ' + author.name);

  if (event) {
    alert(event.target.tagName);
  }
}
const message = ref('')
const quiz = ref('');
const answer = ref('Questions usually contain a question mark at the end. ;-');
const loading = ref(false);
const show = ref(true);

watch(message, (newVal, oldVal) => {
  if (newVal.includes('?')) {
    loading.value = true;
    answer.value = '🤔';
  }
  try {
    fetch(`https://yesno.wtf/api?${newVal}`)
      .then(res => res.json())
      .then(data => {
        answer.value = data.answer;
        loading.value = false;
      });
  } catch (error) {
    answer.value = 'Error fetching data';
    loading.value = false;
  } finally {
    console.log('Done fetching data');
    loading.value = false;
  }
});
const { x, y } = useMousePosition()

</script>

<template>
  <div>
    <p>
      Ask a yes/no question:
      <input v-model="message" placeholder="e.g. Is Vue.js awesome?" :disabled="loading" />
    </p>
    <p>{{ answer }}</p>
  </div>
  <button @click="greet">Greet</button>
    <p>Message is: {{ message }}</p>
    <input v-model="message" placeholder="edit me" />
    <h1>{{ author.name }}</h1>
    <p>Age: {{ author.age }}</p>
    <p>Published Books: {{ publishedBooks }}</p>
  <div>
    <h2>Items</h2>
    <ul>
      <li v-for="item in items" :key="item">{{ item }}</li>
    </ul>
  </div>
  <ul>
    <li v-for="(value, key, index) in myObject" :key="key"> {{ index }}, {{ key }}: {{ value }}</li>
  </ul>
  Mouse position: {{ x }}, {{ y }}
  <button @click="show = !show">Toggle</button>
  <Transition>
    <p v-if="show">hello1</p>
  </Transition>
  <Transition name="slide-fade">
  <p v-if="show">hello</p>
</Transition>

</template>

<style>
.v-enter-active,
.v-leave-active {
  transition: opacity 0.5s ease;
}

.v-enter-from,
.v-leave-to {
  opacity: 0;
}
/*
  Enter and leave animations can use different
  durations and timing functions.
*/
.slide-fade-enter-active {
  transition: all 0.3s ease-out;
}

.slide-fade-leave-active {
  transition: all 0.8s cubic-bezier(1, 0.5, 0.8, 1);
}

.slide-fade-enter-from,
.slide-fade-leave-to {
  transform: translateX(20px);
  opacity: 0;
}
</style>