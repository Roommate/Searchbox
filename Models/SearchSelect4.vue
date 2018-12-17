<template>
      <el-col :span="item.span">
          <!-- <span>{{item.title}}</span> -->
          <el-form :model="items" ref="item" :rules="rules">
            <el-form-item prop="value" :label='items.title'>
              <el-select v-model="items.value" filterable
              :disabled="items.disabled" :clearable="item.clearable" remote reserve-keyword
              :remote-method="remoteMethod"
              :loading="loading"
              :autofocus="item.focus"
              ref="elInput"
              :placeholder="items.placeholder"
              :popper-append-to-body="item.popperAppendToBody"
              @change="handleChange">
                  <el-option
                  v-for="options in items.options"
                  :key="options.value"
                  :label="options.label"
                  :value="adapter(options.value)">
                    <span v-if="options.fvalue || item.half"
                     style="float: left">{{ options.label }}</span>
                    <span v-else style="float: left">{{ options.value+'-'+options.label }}</span>
                  </el-option>
              </el-select>
            </el-form-item>
          </el-form>

      </el-col>
</template>

<script>
import axios from '@/libs/utils/http';
import Locale from '@/libs/mixins/locale';
import { t } from '@/libs/Locale';

export default {
  mixins: [Locale],
  name: 'SearchSelect4',
  props: {
    item: {
      type: Object,
      default() {
        return {};
      },
    },
  },
  mounted() {
    if (this.item.focus) {
      this.$refs.elInput.focus();
    }
  },
  data() {
    return {
      rules: {
        value: this.item.rule,
      },
      items: this.item,
      loading: false,
    };
  },
  methods: {
    handleChange(val) {
      if (val.trim() === '') {
        this.items.options = [{ label: t('libsKey.key27'), fvalue: true }];
      }
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
    remoteMethod(query) {
      if (query.trim() !== '') {
        this.loading = true;
        this.items.options = [];
        axios.get(this.items.url, { params: { [this.items.query]: query.trim() } }).then((res) => {
          this.loading = false;
          for (let i = 0; i < res.data.length; i += 1) {
            this.items.options.push({ label: res.data[i][this.items.resLabel],
              value: res.data[i][this.items.resValue] });
          }
        });
      } else {
        this.items.options = [{ label: t('libsKey.key27'), fvalue: true }];
      }
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
/* span{
    display: block;
    margin: 15px 0 5px 15px;
    font-size: 14px;
} */
.el-select{
  width:calc(100% - 20px);
  /* padding: 10px 10px 15px; */
}
.el-form-item{
  margin-bottom: 10px;
}
</style>
