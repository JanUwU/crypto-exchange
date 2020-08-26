<template>
  <button
    @click="buttonClick"
    class="bg-transparent hover:bg-indigo-500 text-gray-400 font-semibold hover:text-white py-2 px-2 border border-gray-300 hover:border-white rounded"
  >
    <beat-loader :loading="isLoading" :color="'#7f9cf5'" :size="8" />
    <p v-show="!isLoading">
      <slot></slot>
    </p>
  </button>
</template>

<script>
export default {
  name: 'PxButton',

  props: {
    isLoading: {
      type: Boolean,
      default: false
    }
  },

  methods: {
    buttonClick() {
      this.$emit('custom-click')
    }
  }
}
</script>
