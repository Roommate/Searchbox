<template>
    <div>
      <el-col :span="item.span1">
          <!-- <span>{{item.title}}</span> -->
          <el-form :model="item" ref="item" :rules="rules">
            <el-form-item prop="value" :label='item.title'>
              <el-input-number v-model="item.value1"
              :disabled="item.disabled" :autofocus="item.focus"
              ref="elInput"
              controls-position="right" :min="item.min1" >
              </el-input-number>
            </el-form-item>
          </el-form>
      </el-col>
      <el-col :span="item.span2">
          <!-- <span>&nbsp;</span> -->
          <el-form :model="item" ref="item" :rules="rules">
            <el-form-item prop="value" :label='item.title2'>
              <el-input-number v-model="item.value2"
              :disabled="item.disabled"
              controls-position="right" :min="item.min2" >
              </el-input-number>
            </el-form-item>
          </el-form>
      </el-col>
    </div>
</template>

<script>
export default {
  name: 'SearchInputNumber2',
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
  margin: 15px 0 5px 15px;
  font-size: 14px;
}
.el-input-number{
  width:calc(100% - 20px);
  /* padding: 10px 10px 15px; */
}
.el-form-item{
  margin-bottom: 10px;
}
</style>
