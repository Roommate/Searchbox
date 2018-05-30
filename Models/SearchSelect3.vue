<template>
     <div>
      <el-col :span="item.span">
        <div class="grid-content bg-purple">
          <!-- <span>{{item.title}}</span> -->
          <el-form :model="items" ref="item" :rules="rules">
            <el-form-item prop="value" :label='items.title'>
              <el-select v-model="items.value" filterable allow-create
              :disabled="items.disabled" clearable
              :placeholder="items.placeholder"
              :autofocus="item.focus"
              ref="elInput"
              @change="handleChange">
                  <el-option
                  v-for="options in items.options"
                  :key="options.value"
                  :label="options.label"
                  :value="adapter(options.value)">
                  </el-option>
              </el-select>
            </el-form-item>
          </el-form>

        </div>
      </el-col>
    </div>
</template>

<script>
export default {
  name: 'SearchSelect3',
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
      items: this.item,
    };
  },
  methods: {
    handleChange(val) {
      this.$emit('change', val, this.items);
    },
    adapter(val) {
      let ret = val;
      if (typeof this.items.value === 'number') {
        ret = Number(ret);
      }
      if (typeof this.items.value === 'string') {
        ret = String(ret);
      }
      return ret;
    },
    reset() {
      this.items.value = '';
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
.el-select{
  width:calc(100% - 20px);
  /* padding: 10px 10px 15px; */
}
.el-form-item{
  margin-bottom: 10px;
}
</style>
