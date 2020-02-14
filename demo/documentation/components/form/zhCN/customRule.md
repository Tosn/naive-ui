# 自定义规则
你可以通过设定自定的 trigger 来控制验证。
```html
<n-form :model="model" ref="form" :rules="rules">
  <n-form-item-row path="age" label="年龄">
    <n-input v-model="model.age"/>
  </n-form-item-row>
  <n-form-item-row path="password" label="密码">
    <n-input v-model="model.password" @input="handlePasswordInput" type="password"/>
  </n-form-item-row>
  <n-form-item-row
    first
    path="reenteredPassword"
    label="重复密码"
    ref="reenteredPassword"
  >
    <n-input :disabled="!model.password" v-model="model.reenteredPassword" type="password"/>
  </n-form-item-row>
  <n-row :gutter="[0, 24]">
    <n-col :span="24">
      <div style="display: flex; justify-content: flex-end;">
        <n-button @click="handleValidateButtonClick" round type="primary">验证</n-button>
      </div>
    </n-col>
  </n-row>
</n-form>

<pre>
{{  JSON.stringify(model, 0, 2) }}
</pre>
```
```js
export default {
  data () {
    return {
      model: {
        age: null,
        password: null,
        reenteredPassword: null
      },
      rules: {
        age: [
          {
            required: true,
            validator (rule, value) {
              if (!value) {
                return new Error('需要年龄')
              } else if (!/^\d*$/.test(value)) {
                return new Error('年龄应该是个整数')
              } else if (Number(value) < 18) {
                return new Error('年龄应该超过十八岁')
              }
              return true
            },
            trigger: ['input', 'blur']
          }
        ],
        reenteredPassword: [
          {
            required: true,
            message: '需要重复输入密码',
            trigger: ['input', 'blur']
          },
          {
            validator: this.validatePasswordStartWith,
            message: '两次密码输入不一致',
            trigger: 'input'
          },
          {
            validator: this.validatePasswordSame,
            message: '两次密码输入不一致',
            trigger: ['blur', 'password-input']
          }
        ]
      }
    }
  },
  methods: {
    handlePasswordInput () {
      if (this.model.reenteredPassword) {
        this.$refs.reenteredPassword.validate('password-input')
      }
    },
    handleValidateButtonClick (e) {
      e.preventDefault()
      this.$refs.form.validate(errors => {
        if (!errors) {
          this.$NMessage.success('验证成功')
        } else {
          console.log(errors)
          this.$NMessage.error('验证失败')
        }
      })
    },
    validatePasswordStartWith (rule, value) {
      return this.model.password && this.model.password.startsWith(value) && this.model.password.length >= value.length
    },
    validatePasswordSame (rule, value) {
      return value === this.model.password
    }
  }
}
```