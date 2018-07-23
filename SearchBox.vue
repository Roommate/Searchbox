<template>
  <div class="searchWarp">
      <el-row>
        <div class="grid-content">
          <component
          v-for="(item, index) in items"
          v-if="!newFold||index<items[0].num"
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
          <el-row  v-if="newFold" class="FR MT45">
              <div v-if="!hideBtn">
              <el-button plain @click="reset()">重置</el-button>
              <el-button type="primary" @click="print()">{{ btnVal }}</el-button>
              <span v-if="items.length>items[0].num" class="switch-up"
                  @click="foldBox">展开<i class="el-icon-arrow-down"></i></span>
              </div>
              <div v-else>
                <span v-if="items.length>items[0].num" class="switch-up"
                  @click="foldBox">展开<i class="el-icon-arrow-down"></i></span>
              </div>
          </el-row>
          <el-row  v-else class="FR">
              <el-button plain @click="reset()">重置</el-button>
              <span v-if="!hideBtn">
              <el-button type="primary" @click="print()">{{ btnVal }}</el-button>
              </span>
              <el-button type="primary" v-for="(item, index) in btnList" :key="index"
                @click="print(item.info)">{{ item.val }}</el-button>
              <span v-if="items.length>items[0].num" class="switch-up"
                  @click="foldBox">收起<i class="el-icon-arrow-up"></i></span>
          </el-row>
      </div>
  </div>
</template>

<script>
import { renderSelect } from '@/libs/utils/datatransform';

import SearchInput from './Models/SearchInput';
import SearchSelectAndInput from './Models/SearchSelectAndInput';
import SearchSelect from './Models/SearchSelect';
import SearchMultiSelect from './Models/SearchMultiSelect';
import SearchSelect2 from './Models/SearchSelect2';
import SearchSelect3 from './Models/SearchSelect3';
import SearchSelect4 from './Models/SearchSelect4';
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
    hideBtn: {
      type: Boolean,
      default() {
        return false;
      },
    },
    btnVal: {
      type: String,
      default() {
        return '查询';
      },
    },
    btnList: {
      type: Array,
      default() {
        return [];
      },
    },
    searchAllInfo: {
      type: Boolean,
      default() {
        return false;
      },
    },
    fold: {
      type: Boolean,
      default() {
        return true;
      },
    },
  },
  data() {
    return {
      newFold: this.fold,
    };
  },
  components: {
    SearchInput,
    SearchSelectAndInput,
    SearchSelect,
    SearchMultiSelect,
    SearchSelect2,
    SearchSelect3,
    SearchSelect4,
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
    print(val) {
      const result = this.multiValidate();
      let param = {};
      if (result && !this.searchAllInfo) {
        Object.keys(result).forEach((key) => {
          if (result[key] !== '') {
            param[key] = result[key];
          }
        });
      } else {
        param = result;
      }
      if (param) this.$emit('search', param, val);
      console.log(param, val);
      return param;
    },
    multiValidate() {
      let valids = true;
      const result = {};
      const validates = (dom) => {
        if (dom) {
          dom.$refs.item.validate((valid) => {
            if (!valid) {
              valids = false;
              return false;
            }
            if (dom.item.name) {
              result[dom.item.name] = dom.item.value;
            }
            return true;
          });
        }
      };
      const obj = this.$refs;
      Object.keys(obj).forEach((key) => {
        validates(obj[key][0]);
      });
      if (!valids) return false;
      return result;
    },
    foldBox() {
      this.newFold = !this.newFold;
    },
    reset() {
      const obj = this.$refs;
      Object.keys(obj).forEach((key) => {
        if (obj[key][0]) {
          obj[key][0].reset();
        }
      });
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
      if (item.value && item.value.length) {
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
    line-height: 40px;
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
