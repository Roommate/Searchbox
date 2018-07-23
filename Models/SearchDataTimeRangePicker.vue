<template>
    <div>
        <el-col :span="item.span">
        <div class="grid-content bg-purple">
          <!-- <span>{{item.title}}</span> -->
          <el-form :model="item" ref="item" :rules="rules">
            <el-form-item prop="value" :label='item.title'>
              <el-date-picker
              value-format="timestamp"
              v-model="item.value"
              type="datetimerange"
              :autofocus="item.focus"
              @blur="blur(item)"
              ref="elInput"
              :disabled="item.disabled"
              :range-separator="item.rangeSeparator"
              :start-placeholder="item.startPlaceholder"
              :end-placeholder="item.endPlaceholder"
              :placeholder="item.placeholder">
              </el-date-picker>
            </el-form-item>
           </el-form>
        </div>
      </el-col>
    </div>
</template>

<script>
// 下面这种方式是时间范围默认显示当天0点到23：59：59
// value: [
//   new Date(new Date().toLocaleDateString()).getTime(),
//   new Date(new Date().toLocaleDateString()).getTime() + (24 * 60 * 60 * 1000) - 1],
export default {
  name: 'SearchDataRangePicker',
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
  methods: {
    reset() {
      this.item.value = [];
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
.el-range-editor{
  width:calc(100% - 20px);
  /* padding: 10px 10px 15px; */
}
.el-form-item{
  margin-bottom: 10px;
}
</style>
