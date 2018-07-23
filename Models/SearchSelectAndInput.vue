<template>
    <div>
      <el-col :span="item.span">
        <div class="grid-content bg-purple">
          <!-- <span>{{item.title}}</span> -->
          <el-form :model="item" ref="item" :rules="rules">
            <el-form-item prop="fakeValue" :label='item.title'>
              <el-select v-model="item.fakeValue"
              :disabled="item.disabled" clearable
              :autofocus="item.focus1"
              ref="elInput"
              :placeholder="item.placeholder1"
              @change="handleChange">
                  <el-option
                  v-for="options in item.options"
                  :key="options.fakeValue"
                  :label="options.label"
                  :value="options.fakeValue">
                  </el-option>
              </el-select>
            </el-form-item>
            <el-form-item prop="value" :label='item.title'>
              <el-input v-model="item.value" :disabled="item.disabled"
              :placeholder="item.placeholder2"
              :prop="item.value"
              :autofocus="item.focus2"
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
import axios from '@/libs/utils/http';

export default {
  name: 'SearchSelectAndInput',
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
  created() {
    console.log(22);
    axios.post(this.item.url, this.item.query).then((res) => {
      const Status = res.data.receipt_code_type.details.map(item => Object.assign({}, {
        fakeValue: item.fieldCode,
        label: item.fieldValue,
      }));
      console.log(res, Status);
      this.item.options = Status;
      // for (let i = 0; i < res.data.length; i += 1) {
      //   this.item.options.push({ label: res.data[i][this.item.resLabel],
      //     value: res.data[i][this.item.resValue] });
      // }
    });
  },
  methods: {
    handleChange(val) {
      this.$emit('change', val, this.item);
      this.item.name = val;
      console.log(val);
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
    selectChange(val) {
      console.log('111', val);
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
.el-select{
  width:calc(100% - 20px);
  /* padding: 10px 10px 15px; */
}
.el-form-item{
  padding-top: 40px;
  float: left;
  width:50%;
  margin-bottom: 10px;
}
</style>
