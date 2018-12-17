<template>
        <el-col :span="item.span">
          <!-- <span>{{item.title}}</span> -->
          <el-form :model="item" ref="item" :rules="rules">
            <el-form-item prop="value" :label='item.title'>
              <el-date-picker
              v-model="item.value"
              type="datetime" value-format="timestamp"
              @blur="blur(item)"
              :autofocus="item.focus"
              ref="elInput"
              :disabled="item.disabled"
              :placeholder="item.placeholder">
              </el-date-picker>
            </el-form-item>
           </el-form>

      </el-col>
</template>

<script>
export default {
  name: 'SearchDataPicker',
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
    blur(type) {
      this.$refs.elInput.blur();
      this.$emit('dateBlur', type);
    },
  },
};
</script>

<style scoped>
span{
  display: block;
  margin: 15px 0 5px 15px;
  font-size: 14px;
}
.el-date-editor--datetime{
  width:calc(100% - 20px);
  /* padding: 10px 10px 15px; */
}
.el-form-item{
  margin-bottom: 10px;
}
</style>
