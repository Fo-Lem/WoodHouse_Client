<script lang="ts">
import { defineComponent } from 'vue'
import { ErrorMessage, Field } from 'vee-validate'

export default defineComponent({
  name: 'AdminInput',
  components: {
    Field, ErrorMessage,
  },
  props: {
    value: {
      required: true,
      type: [String, Number],
    },
    placeholder: {
      type: String,
    },
    inputIn: {
      required: true,
      type: String,
    },
    inputName: {
      required: true,
      type: String,
    },
  },
  emits: {
    updateInput(value: string) {
      return value
    },
  },
  methods: {
    updateInput(event: Event) {
      if (event.target instanceof HTMLInputElement)
        this.$emit('updateInput', event.target.value)
    },
  },

})
</script>

<template>
  <div class="flex flex-col gap-2">
    <label
      :for="inputIn"
      class="block text-md leading-6 text-gray-900"
    >{{ inputName }}</label>
    <div class="flex rounded-md shadow-sm ring-1 ring-inset ring-gray-300 sm:max-w-md">
      <Field
        :id="inputIn"
        :placeholder="placeholder"
        type="text"
        required
        :value="value !== 0 ? value : ''"
        :name="inputIn"
        class="block flex-1 border-0 bg-transparent p-2.5 text-gray-900 placeholder:text-gray-400 sm:text-sm sm:leading-6"
        @input="updateInput"
      />
    </div>
    <ErrorMessage class="text-red-500 text-sm" :name="inputIn" />
  </div>
</template>
