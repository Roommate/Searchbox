<template>
      <el-col :span="item.span">
          <!-- <span>{{item.title}}</span> -->
          <el-form :model="item" ref="item" :rules="rules">
            <el-form-item prop="value" :label='item.title'>
              <el-select v-model="item.value"
              :disabled="item.disabled" :clearable="item.clearable"
              :autofocus="item.focus"
              :popper-append-to-body="item.popperAppendToBody"
              ref="elInput"
              :placeholder="item.placeholder"
              @change="handleChange">
                  <el-option
                  v-for="options in item.options"
                  :key="options.value"
                  :label="options.label"
                  :value="adapter(options.value)">
                  </el-option>
              </el-select>
            </el-form-item>
          </el-form>
      </el-col>
</template>

<script>
export default {
  name: 'SearchSelect',
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
      // item: this.item,
    };
  },
  mounted() {
    if (this.item.focus) {
      this.$refs.elInput.focus();
    }
  },
  methods: {
    handleChange(val) {
      this.$emit('change', val, this.item);
    },
    adapter(val) {
      let ret = val;
      if (typeof this.item.value === 'number') {
        ret = Number(ret);
      }
      if (typeof this.item.value === 'string') {
        ret = String(ret);
      }
      return ret;
    },
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
.el-select{
  width:calc(100% - 20px);
  /* padding: 10px 10px 15px; */
}
.el-form-item{
  margin-bottom: 10px;
}
</style>
