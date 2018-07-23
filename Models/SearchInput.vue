<template>
    <div>
      <el-col :span="item.span">
        <div class="grid-content bg-purple">
          <!-- <span>{{item.title}}</span> -->
          <el-form :model="item" ref="item" :rules="rules">
            <el-form-item prop="value" :label='item.title'>
              <el-input v-model="item.value" :disabled="item.disabled"
              :placeholder="item.placeholder"
              :prop="item.value"
              :autofocus="item.focus"
              :type="item.inputType||'text'"
              @change="handleChange"
              ref="elInput"
              >
              <template v-if="item.append" slot="append">{{item.appendItem}}</template>
              </el-input>
              <input type='text' style="display:none;" />
            </el-form-item>
          </el-form>
        </div>
      </el-col>
    </div>
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
  methods: {
    handleChange(val) {
      this.$emit('change', val, this.item);
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
.el-input{
  width:calc(100% - 20px);
  /* padding: 10px 10px 15px; */
}
.el-form-item{
  margin-bottom: 10px;
}
</style>
