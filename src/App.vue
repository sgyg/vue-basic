<template>
  <img alt="Vue logo" src="./assets/logo.png" />
  <h1>{{ count }}</h1>
  <h1>{{ double }}</h1>
  <h1>x:{{ x }},y:{{ y }}</h1>
  <h1 v-if="loading">Loading...</h1>
  <img v-if="loaded" :src="result[0].url" />
  <button @click="openModal">Open Modal</button>
  <Modal :isOpen="modalIsOpen" @close-modal="closeModal" />
  <ul>
    <li v-for="number in numbers" :key="number">{{ number }}</li>
  </ul>
  <h1>{{ proson.name }}</h1>
  <button @click="increase">+1</button>
  <button @click="updateGreeting">Update Title</button>
</template>

<script lang="ts">
import { defineComponent, computed, reactive, ref, toRefs, watch } from "vue";
import useMousePosition from "./hooks/useMousePosition";
import useURLLoader from "./hooks/useURLLoader";
import Modal from "./components/Modal.vue";
interface DataPrpops {
  count: number;
  double: number;
  increase: () => void;
  numbers: number[];
  proson: { name?: string };
}
interface DogResult {
  message: string;
  stutes: string;
}
interface CatResult {
  id: string;
  url: string;
  width: number;
  height: number;
}
export default defineComponent({
  name: "App",
  components: {
    Modal,
  },
  setup() {
    const data: DataPrpops = reactive({
      count: 0,
      increase: () => {
        data.count++;
      },
      double: computed(() => {
        return data.count * 2;
      }),
      numbers: [1, 2, 3],
      proson: {},
    });
    const { x, y } = useMousePosition();
    const { result, loading, loaded } = useURLLoader<CatResult[]>(
      "https://api.thecatapi.com/v1/images/search?limit=1"
    );
    const greetings = ref("");
    const updateGreeting = () => {
      greetings.value += " Hello";
    };
    watch([greetings, () => data.count], (newValue, oldValue) => {
      console.log(newValue);
      console.log(oldValue);
      document.title = "updated" + greetings.value + data.count;
    });
    const modalIsOpen = ref(false);
    const openModal = () => {
      modalIsOpen.value = true;
    };
    const closeModal = () => {
      modalIsOpen.value = false;
    };
    // data.numbers[0] = 5;
    // data.proson.name = "susu"
    const refData = toRefs(data);
    return {
      ...refData,
      greetings,
      updateGreeting,
      x,
      y,
      result,
      loading,
      loaded,
      modalIsOpen,
      openModal,
      closeModal,
    };
  },
});
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
