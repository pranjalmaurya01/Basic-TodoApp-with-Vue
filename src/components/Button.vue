<template>
  <button class="btn" :style="{background: color}" @click="onClick()">{{ buttonText }}</button>
</template>

<script>
export default {
  name: 'Button',
  props: {
    buttonText: String,
    color: String,
  },
  methods: {
    onClick() {
      this.$emit('btn-click');
    },
  },
};
</script>
