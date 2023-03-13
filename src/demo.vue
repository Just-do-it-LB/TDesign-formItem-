<template>
  <!--  scrollToFirstError="smooth" -->
  <t-form :data="formData" ref="form">
    <t-form-item label="用户名" name="account" :rules="rules.account">
      <t-input v-model="formData.account" placeholder="请输入用户名"></t-input>
    </t-form-item>
    <t-form-item label="密码" name="password" :rules="rules.password">
      <t-input
        type="password"
        v-model="formData.password"
        placeholder="请输入密码"
      ></t-input>
    </t-form-item>
    <t-form-item label="学院" name="college" :rules="rules.college">
      <t-select
        v-model="formData.college"
        class="demo-select-base"
        clearable
        filterable
        placeholder="请选择所在学院"
      >
        <t-option
          v-for="(item, index) in options"
          :value="item.value"
          :label="item.label"
          :key="index"
        >
          {{ item.label }}
        </t-option>
      </t-select>
    </t-form-item>
    <t-button @click="vv">12345</t-button>
  </t-form>
</template>
<script>
const INITIAL_DATA = {
  account: '',
  password: '',
  college: '',
};
export default {
  data() {
    return {
      formData: { ...INITIAL_DATA },
      options: [
        { label: '计算机学院', value: '1' },
        { label: '软件学院', value: '2' },
        { label: '物联网学院', value: '3' },
      ],
      // FormItem.rules 优先级大于 Form.rules
      rules: {
        account: [
          {
            required: true,
            message: '姓名必填',
            type: 'error',
            trigger: 'blur',
          },
          // trigger 默认为 'change'
          { required: true, message: '姓名必填', type: 'error' },
          { whitespace: true, message: '姓名不能为空' },
          {
            min: 2,
            message: '至少需要两个字符，一个中文等于两个字符',
            type: 'warning',
            trigger: 'blur',
          },
          {
            max: 10,
            message: '姓名字符长度超出',
            type: 'warning',
            trigger: 'blur',
          },
        ],
        password: [
          { required: true, message: '密码必填', type: 'error' },
          { len: 3, message: '请输入 3 位密码', type: 'warning' },
        ],
        college: [{ required: true, message: '学院--必填11', type: 'error' }],
      },
    };
  },

  methods: {
    onReset() {
      this.$message.success('重置成功');
      console.log('formData', this.formData);
    },
    onSubmit({ validateResult, firstError }) {
      if (validateResult === true) {
        this.$message.success('提交成功');
      } else {
        console.log('Errors: ', validateResult);
        this.$message.warning(firstError);
      }
      this.vv();
    },
    handleClear() {
      this.$refs.form.clearValidate();
    },
    vv() {
      this.$children[0].children[0].validate('blur');
      this.$children[0].children[1].validate('all');
      // this.$children[0].children[2].value = '12';
      this.$children[0].children[2].validate('all');
      // const t2 = this.$children[1].validate('all');
      console.log('ccc--', this.$children[0]);
    },
  },
};
</script>

<style scoped>
.demo-select-base {
  width: 300px;
}
</style>
