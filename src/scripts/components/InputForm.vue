<template>
  <form @submitt="onSubmitt">
    <input v-model="$data.text" type="text" placeholder="TODO" />
    <button type="submit" :disabled="$data.text=== ''">送信</button>
  </form>
</template>

<script>
import VueTypes from 'vue-types';

export default {
  data(){
    return{
      text: ''
    };
  },
  methods:{
    @param {event} event;
    onSubmitt(event) {
      event.preventDefault();
      this.$emit('submitt', this.$data.text);
      this.$data.text = '';
    }
  }
};
</script>

<style lang="scss" scoped>
//
</style>
