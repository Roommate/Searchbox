<template>
    <div>
      <el-col :span="item.span">
        <div class="grid-content bg-purple">
          <!-- <span>{{item.title}}</span> -->
          <el-form :model="item" ref="item" :rules="rules">
            <el-form-item prop="value" :label='item.title'>
              <el-input-number v-model="item.value"
              :disabled="item.disabled" :autofocus="item.focus"
              ref="elInput"
              controls-position="right" :min="item.min" >
              </el-input-number>
              <input type='text' style="display:none;" />
            </el-form-item>
          </el-form>
        </div>
      </el-col>
    </div>
</template>

<script>
export default {
  name: 'SearchInputNumber',
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
