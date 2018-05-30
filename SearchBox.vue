<template>
    <div>
        <div class="searchWarp">
            <el-row>
                <div class="grid-content">
                <component
                v-for="(item, index) in items"
                v-if="!fold||index<items[0].num"
                :key="index"
                :is="item.type"
                :item="item"
                :form="form"
                :ref="item.name"
                @dateBlur="dateBlur(item,index,items)"
                @keyup.native.13="focusJump(index,items)"
                @change="handleChange"></component>
                </div>
            </el-row>
            <div v-if="!items[0].fade">
                <el-row  v-if="fold" class="FR MT45">
                    <el-button plain @click="reset()">重置</el-button>
                    <el-button type="primary" @click="print()">查询</el-button>
                    <span v-if="items.length>items[0].num" class="switch-up"
                        @click="foldBox">展开<i class="el-icon-arrow-down"></i></span>
                </el-row>
                <el-row  v-else class="FR">
                    <el-button plain @click="reset()">重置</el-button>
                    <el-button type="primary" @click="print()">查询</el-button>
                    <span v-if="items.length>items[0].num" class="switch-up"
                        @click="foldBox">收起<i class="el-icon-arrow-up"></i></span>
                </el-row>
            </div>
        </div>
    </div>
</template>

<script>
import { renderSelect } from '@/utils/datatransform';

import SearchInput from './Models/SearchInput';
import SearchSelect from './Models/SearchSelect';
import SearchSelect2 from './Models/SearchSelect2';
import SearchSelect3 from './Models/SearchSelect3';
import SearchSwitch from './Models/SearchSwitch';
import SearchDataPicker from './Models/SearchDataPicker';
import SearchDataTimePicker from './Models/SearchDataTimePicker';
import SearchDataTimeRangePicker from './Models/SearchDataTimeRangePicker';
import SearchTextarea from './Models/SearchTextarea';
import SearchInputNumber from './Models/SearchInputNumber';
import SearchInputNumber2 from './Models/SearchInputNumber2';
import SearchCheckBox from './Models/SearchCheckBox';
import SearchCityCascader from './Models/SearchCityCascader';


export default {
  name: 'searchBox',
  props: {
    items: {
      type: Array,
      default() {
        return [];
      },
    },
    contact: {
      type: Array,
      default() {
        return [];
      },
    },
    form: {
      type: Array,
      default() {
        return [];
      },
    },
    inpSel: {
      type: Boolean,
      default() {
        return false;
      },
    },
  },
  data() {
    return {
      fold: true,
    };
  },
  components: {
    SearchInput,
    SearchSelect,
    SearchSelect2,
    SearchSelect3,
    SearchSwitch,
    SearchDataPicker,
    SearchDataTimePicker,
    SearchDataTimeRangePicker,
    SearchTextarea,
    SearchInputNumber2,
    SearchInputNumber,
    SearchCheckBox,
    SearchCityCascader,
  },
  methods: {
    print() {
      const result = this.multiValidate();
      if (result) this.$emit('search', result);
      console.log(result);
      return result;
    },
    multiValidate() {
      let valids = true;
      const result = {};
      const validates = (dom) => {
        dom.$refs.item.validate((valid) => {
          if (!valid) {
            valids = false;
            return false;
          }
          result[dom.item.name] = dom.item.value;
          return true;
        });
      };
      const obj = this.$refs;
      Object.keys(obj).forEach((key) => {
      // console.log(key, obj[key]);
        validates(obj[key][0]);
      });
      // for (let i = 0, l = this.$refs.form.length; i < l; i += 1) {
      //   validates(this.$refs.form[i]);
      // }
      if (!valids) return false;
      return result;
    },
    foldBox() {
      this.fold = !this.fold;
    },
    reset() {
      // const list = this.$refs.form;
      // const list = this.items;

      // for (let i = 0, l = list.length; i < l; i += 1) {
      //   this.$refs[list[i].name].resetFields();
      // }
      const obj = this.$refs;
      Object.keys(obj).forEach((key) => {
        obj[key][0].reset();
      // console.log(key, obj[key]);
      });
      // for (let i = 0; i < this.items.length; i += 1) {
      //   this.items[i].value = '';
      // }
    },
    handleChange(val, item) {
      this.$emit('searchChange', item);
      for (let i = 0, l = this.contact.length; i < l; i += 1) {
        const one = this.contact[i];
        for (let j = 0, ll = one.length; j < ll; j += 1) {
          if (item.name === one[j].id) {
            if (j === one.length - 1) return;
            one[j].func({
              [item.name]: val,
            }).then((res) => {
              if (res.data) {
                renderSelect(this.items, res.data, one[j + 1].id, one[j + 1].id, one[j + 1].name);
              }
            });
          }
        }
      }
    },
    focusJump(index, items, isloop) {
      if (!isloop) {
        this.$refs[items[index].name][0].blur();
      }
      for (let i = index + 1; i < items.length; i += 1) {
        if (!items[i].disabled) {
          this.$refs[items[i].name][0].focus();
          return;
        }
      }
    },
    dateBlur(item, index, items) {
      if (item.value.length) {
        this.focusJump(index, items, true);
      }
    },
  },
};
</script>

<style scoped>
.searchWarp{
    border-radius: 3px;
    background: #fff;
    min-height: 140px;
    overflow: auto;
}
.FR{
    float: right;
    margin:25px;
}
.FR>span{
    margin:0 10px;
}
.grid-content{
    margin:10px;
}
.el-switch{
    display: block;
    line-height: 40px;
    height:40px;
}
.MT45{
    margin-top: -50px;
}
/* span{
    display: block;
} */
</style>
