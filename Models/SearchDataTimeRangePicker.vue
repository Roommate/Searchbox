<template>
        <el-col :span="item.span">
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
              :picker-options="pickerOptions"
              :disabled="item.disabled"
              :range-separator="item.rangeSeparator"
              :start-placeholder="item.startPlaceholder"
              :end-placeholder="item.endPlaceholder"
              :placeholder="item.placeholder">
              </el-date-picker>
            </el-form-item>
           </el-form>
      </el-col>
</template>

<script>
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
      pickerOptions: {
        // disabledDate(time) {
        //   return time.getTime() > Date.now();
        // },
      },
    };
  },
  mounted() {
    if (this.item.focus) {
      this.$refs.elInput.focus();
    }
    if (this.item.deadline) {
      this.pickerOptions = {
        disabledDate(time) {
          return time.getTime() > Date.now();
        },
      };
    }
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
