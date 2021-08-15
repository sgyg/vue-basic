<template>
  <teleport to="#modal_content">
    <div id="center" v-if="isOpen">
      <h1><slot>这里是modal内容</slot></h1>
      <button @click="buttonClick">close</button>
    </div>
  </teleport>
</template>

<script>
import { defineComponent } from "vue";
export default defineComponent({
  props: {
    isOpen: Boolean,
  },
  emits: {
    "close-modal": null,
  },
  setup(porps, context) {
    const buttonClick = () => {
      context.emit("close-modal");
    };
    return { buttonClick };
  },
});
</script>

<style scoped>
#center {
  width: 200px;
  height: 200px;
  border: 2px solid black;
  background: white;
  position: fixed;
  left: 50%;
  top: 50%;
  margin-left: -100px;
  margin-top: -100px;
}
</style>