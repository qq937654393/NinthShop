<template lang='pug'>
cube-form.form-register(
  :model='model'
  :schema='schema'
  :immediate-validate='false'
  :options='options'
  @submit='submitHandler'
  @reset='resetHandler'
  @validate='validateHandler')
</template>

<script>
/**
* @name 注册表单
 */
export default {
  name: 'FormLogin',
  data () {
    return {
      // form表单
      validity: {},
      valid: undefined,
      model: {}, // 数据源
      schema: { // 模式用于定义表单中的各个字段，可以选择是否分组。
        fields: [
          {
            modelKey: 'username',
            type: 'input',
            label: '用户名',
            props: {
              placeholder: '请输入'
            },
            rules: {
              required: true
            }
          },
          {
            modelKey: 'password',
            type: 'input',
            label: '密码',
            props: {
              type: 'password',
              placeholder: '请输入',
              eye: { open: false, reverse: false }
            },
            rules: {
              required: true
            }
          },
          {
            modelKey: 'password2',
            type: 'input',
            label: '重复密码',
            props: {
              type: 'password',
              placeholder: '请输入',
              eye: { open: false, reverse: false }
            },
            rules: {
              required: true
            }
          },
          {
            modelKey: 'tell',
            type: 'input',
            label: '手机号',
            props: {
              placeholder: '请输入'
            },
            rules: {
              required: true
            }
          },
          {
            type: 'submit',
            label: '注册'
          },
          {
            type: 'reset',
            label: '重置'
          }
        ]
      },
      options: { // 配置项
        scrollToInvalidField: true,
        layout: 'standard' // classic fresh
      }
    }
  },
  methods: {
    validateHandler (result) {
      this.validity = result.validity
      this.valid = result.valid
      console.log('validity', result)
    },
    submitHandler (e) {
      e.preventDefault()
      console.log('提交')
      this.$post({
        url: '/userinfo/register',
        data: {
          username: this.model.username,
          password: this.model.password,
          tell: this.model.tell,
          type: 'user'
        }
      }).then(data => {
        if (!data) return
        this.$store.dispatch('recordUser', data)
        this.$router.replace('/profile')
      })
    },
    resetHandler (e) {
      console.log('重置', e)
    }
  }
}
</script>

<style lang='sass' scoped>

</style>
