<template>
  <div class="validate-input-container pb-3">
    <input
      v-if="tag != 'textarea'"
      type="email"
      class="form-control"
      :class="{ 'is-invalid': inputRef.error }"
      id="exampleInputEmail1"
      aria-describedby="emailHelp"
      :value="inputRef.val"
      @blur="ValidateInput"
      @input="updateValue"
      v-bind="$attrs"
    />
    <textarea
      v-else
      type="email"
      class="form-control"
      :class="{ 'is-invalid': inputRef.error }"
      id="exampleInputEmail1"
      aria-describedby="emailHelp"
      :value="inputRef.val"
      @blur="ValidateInput"
      @input="updateValue"
      v-bind="$attrs"
    ></textarea>
    <span v-if="inputRef.error" class="invalid-feedback">{{ inputRef.message }}</span>
  </div>
</template>

<script lang="ts">
import { defineComponent, reactive, PropType, onMounted } from 'vue'
import { emitter } from './ValidateForm.vue'

interface RuleProp {
  type: 'required' | 'email' | 'password'
  message: string
}
export type RulesProp = RuleProp[]
export type TagType = 'input' | 'textarea'
const emailReg = /^[a-zA-Z0-9_.-]+@[a-zA-Z0-9-]+(\.[a-zA-Z0-9-]+)*\.[a-zA-Z0-9]{2,6}$/

export default defineComponent({
  name: 'App',
  props: {
    rules: Array as PropType<RulesProp>,
    modelValue: String,
    tag: {
      type: String as PropType<TagType>,
      default: 'input'
    }
  },
  inheritAttrs: false,
  setup(props, context) {
    const inputRef = reactive({
      val: props.modelValue || '',
      error: false,
      message: ''
    })
    const updateValue = (e: KeyboardEvent) => {
      const targetValue = (e.target as HTMLInputElement).value
      inputRef.val = targetValue
      context.emit('update:modelValue', targetValue)
    }
    const ValidateInput = () => {
      if (props.rules) {
        const allPassed = props.rules.every((rule) => {
          let passed = true
          inputRef.message = rule.message
          switch (rule.type) {
            case 'required':
              passed = inputRef.val.trim() !== ''
              break
            case 'email':
              passed = emailReg.test(inputRef.val)
              break
            default:
              break
          }
          return passed
        })
        inputRef.error = !allPassed
        return allPassed
      }
      return true
    }
    onMounted(() => {
      emitter.emit('form-item-created', ValidateInput)
    })
    return {
      inputRef,
      ValidateInput,
      updateValue
    }
  }
})
</script>

<style scoped>
</style>
