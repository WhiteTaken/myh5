<style lang="scss" scoped>
@import "~@/css/variables.scss";
.wrapper {
  width: 470px;
  background: #fff;
  border-radius: 6px;
  z-index: $panelZIndex;
  position: absolute;
  margin-left: 50%;
  left: -235px;
  top: 80px;
  box-shadow: 0 5px 15px rgba(0, 0, 0, 0.5);
  .header {
    padding: 15px 20px;
    border-bottom: 1px solid #ccd5db;
    min-height: 21px;
    background-color: #f7f7f7;
    border-top-left-radius: 6px;
    border-top-right-radius: 6px;
    display: flex;
    justify-content: space-between;
    align-items: center;
    h4 {
      font-size: 18px;
      font-weight: bold;
      span {
        color: #a3afb7;
        font-size: 12px;
        padding-left: 10px;
      }
    }
    .close {
      display: block;
      width: 14px;
      height: 14px;
      color: #a3afb7;
      line-height: 14px;
      font-size: 21px;
      font-weight: bold;
      text-align: center;
      cursor: pointer;
    }
  }
  .main {
    display: flex;
    height: 300px;
    padding: 20px;
    .left {
      width: 300px;
      .item {
        display: flex;
        align-items: center;
        height: 40px;
        line-height: 40px;
        label {
          min-width: 50px;
        }
      }
    }
    .right {
      display: flex;
      flex-direction: column;
      justify-content: space-between;
    }
  }
}
</style>

<template>
  <div class="wrapper">
    <div class="header">
      <h4>二维码</h4>
      <span @click="closePanel(types.QRCODE)" class="close">x</span>
    </div>
    <div class="main" style="height:280px;display:flex;">
      <div style="display:flex;flex-direction:column;" class="left">
        <div class="item">
          <label>链接</label>
          <el-input v-model="url" size="mini" placeholder="http://www.baidu.com"></el-input>
        </div>
        <div class="item">
          <label>边距</label>
          <el-input-number v-model="margin" size="mini" :max="4" :step="1" :min="0"></el-input-number>
        </div>
        <div class="item">
          <label></label>
          <el-button @click="create" size="mini" type="success">生成</el-button>
        </div>
      </div>
      <div class="right">
        <img :src="src" style="width:200px;border:1px solid #dce0e2" />
        <footer>
          <el-button @click="closePanel(types.QRCODE)" size="mini">取消</el-button>
          <el-button @click="confirm" size="mini" type="success">确定</el-button>
        </footer>
      </div>
    </div>
  </div>
</template>
<script>
import * as api from "@/api";
import * as types from "@/tpl/types";
import { mapActions } from "vuex";
export default {
  data() {
    return {
      types: types,
      url: "",
      margin: 2,
      src:
        "http://p7m90pgef.bkt.clouddn.com/e81c1f5749545c5f7d247b3a100ffe62.svg"
    };
  },
  methods: {
    ...mapActions(["addItem", "openPanel", "closePanel"]),
    create() {
      api
        .createQRCode({
          url: encodeURIComponent(this.url),
          margin: this.margin
        })
        .then(({ result }) => {
          this.src = result;
        });
    },
    confirm() {
      this.addItem({
        type: this.types.QRCODE,
        url: this.src
      });
      this.closePanel(types.QRCODE);
    }
  }
};
</script>