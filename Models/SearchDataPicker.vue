<template>
    <div>
        <el-col :span="item.span">
        <div class="grid-content bg-purple">
          <!-- <span>{{item.title}}</span> -->
          <el-form :model="item" ref="item" :rules="rules">
            <el-form-item prop="value" :label='item.title'>
              <el-date-picker v-model="item.value"
              type="date" value-format="timestamp"
              :autofocus="item.focus"
              @blur="blur(item)"
              ref="elInput"
              :disabled="item.disabled"
              :placeholder="item.placeholder">
              </el-date-picker>
            </el-form-item>
           </el-form>

        </div>
      </el-col>
    </div>
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
.el-date-picker{
  width:calc(100% - 20px);
  /* padding: 10px 10px 15px; */
}
.el-form-item{
  margin-bottom: 10px;
}
</style>
