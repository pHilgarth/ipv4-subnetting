<template>
  <div class="form-group">
    <label :for="name" class="form-label">{{ text }}</label>
    <input
      type="text"
      class="form-control"
      v-model="userInput"
      :id="name"
      @blur="emitValue"
    />
    <p class="feedback hidden"></p>
  </div>
</template>

<script>
export default {
    data() {
        return {
            userInput: ''
        }
    },

    methods: {
        emitValue() {
            this.$emit('emit-value', this.name, this.userInput)
        }
    },

    props: {
        name: String,
        text: String
    }
};
</script>