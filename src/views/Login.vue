<template>
  <div class="container">
    <validate-form @form-submit="onFormSubmit">
      <div class="mb-3">
        <label for="exampleInputEmail1" class="form-label">Email address</label>
        <validate-input :rules="emailRules" v-model="emailVal" placeholder="请输入邮箱地址" type="text" />
      </div>
      <div class="mb-3">
        <label for="exampleInputPassword1" class="form-label">Password</label>
        <validate-input :rules="passwordRules" v-model="passwordlVal" placeholder="请输入密码" type="password" />
      </div>
      <template v-slot:submit>
        <span class="btn btn-danger">Submit</span>
      </template>
    </validate-form>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue'
import { useRouter } from 'vue-router'
import { useStore } from 'vuex'
import ValidateInput, { RulesProp } from '../components/ValidateInput.vue'
import ValidateForm from '../components/ValidateForm.vue'
export default defineComponent({
  name: 'LoGin',
  components: {
    ValidateInput,
    ValidateForm
  },
  setup() {
    const emailVal = ref('')
    const router = useRouter()
    const store = useStore()
    const emailRules: RulesProp = [
      { type: 'required', message: '电子邮箱地址不能为空' },
      { type: 'email', message: '请输入正确的电子邮箱格式' }
    ]
    const passwordlVal = ref('')
    const passwordRules: RulesProp = [{ type: 'required', message: '密码不能为空' }]

    const onFormSubmit = (result: boolean) => {
      console.log(result)
      if (result) {
        router.push('/')
        store.commit('login')
      }
    }
    return {
      emailRules,
      emailVal,
      passwordlVal,
      passwordRules,
      onFormSubmit
    }
  }
})
</script>
