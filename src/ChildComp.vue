<script lang="ts">
export default {
  emits: ['response'],
  created() {
    this.$emit('response', 'Hello from ChildComp!')
  },
  props: {
    msg: String
  }
}
</script>

<template>
  <slot>자식 컴포넌트입니다! msg: {{ msg || 'no msg' }}</slot>
</template>
