<template>
  <div>
    <img :src="searchImage" alt="" />
    <input
      type="text"
      name=""
      id=""
      :value="modelValue"
      @input="$emit('update:modelValue', $event.target.value)"
      @change="change"
      maxlength="30"
    />
  </div>
</template>

<script>
import searchImage from '@/assets/Vector.png';

export default {
  data() {
    return {
      searchImage
    };
  },
  props: ['modelValue'],
  emits: ['update:modelValue']
};
</script>

<style scoped>
div {
  padding: 8px 12px;
  border: 1px solid white;
  background-color: white;
  border-radius: 15px;
  display: flex;
  align-items: center;
  width: 300px;
}

img {
  width: 18px;
  margin-right: 8px;
}

input {
  width: 100%;
  background-color: white;
  border: none;
  outline: none;
  font-family: 'Nanum Gothic', sans-serif;
}
</style>
