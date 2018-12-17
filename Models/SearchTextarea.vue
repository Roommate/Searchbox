<template>
      <el-col :span="item.span">
          <!-- <span>{{item.title}}</span> -->
          <el-form :model="item" ref="item" :rules="rules">
            <el-form-item prop="value" :label='item.title'>
              <el-input type="textarea"
              :rows="2"
              :autofocus="item.focus"
              ref="elInput"
              :placeholder="item.placeholder"
              :disabled="item.disabled"
              v-model="item.value">
              </el-input>
              <input type='text' style="display:none;" />
            </el-form-item>
          </el-form>
      </el-col>
</template>

<script>
export default {
  name: 'SearchInput',
  props: {
    item: {
      type: Object,
      default() {
        return {};
      },
    },
  },
  data() {
    return {
      rules: {
        value: this.item.rule,
      },
    };
  },
  mounted() {
    if (this.item.focus) {
      this.$refs.elInput.focus();
    }
  },
  methods: {
    reset() {
      this.item.value = '';
    },
    focus() {
      this.$refs.elInput.focus();
    },
    blur() {
      this.$refs.elInput.blur();
    },
  },
};
</script>

<style scoped>
span{
    display: block;
    margin: 5px 0 0 15px;
    font-size: 14px;
}
.el-input{
  width:calc(100% - 20px);
  /* padding: 10px 10px 15px; */
}
.el-form-item{
  margin-bottom: 10px;
}
</style>
