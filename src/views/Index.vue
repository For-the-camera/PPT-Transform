<script>
import NavBar from "../components/NavBar.vue";
import pptConfig from "../ppt.config";
import { usePPTStore } from "../stores/ppt";

export default {
  components: { NavBar },
  props: {},

  data() {
    const store = usePPTStore();
    return {
      pptConfig,
      pptRender: [],
      store,
    };
  },
  mounted() {
    const store = usePPTStore();
    this.pptRender = [store.nowPage.left, store.nowPage.right];
    this.$watch("store.nowPage.left", function (newVal) {
      this.$set(this.pptRender, 0, newVal);
    });
    this.$watch("store.nowPage.right", function (newVal) {
      this.$set(this.pptRender, 1, newVal);
    });
  },
};
</script>
<template>
  <div class="container">
    <NavBar ref="navBarRef" :config="pptConfig"></NavBar>
    <div class="ppt-content">
      <div class="left">
        <keep-alive>
          <component v-bind:is="pptRender[0]"></component>
        </keep-alive>
      </div>
      <div class="right">
        <keep-alive>
          <component v-bind:is="pptRender[1]"></component>
        </keep-alive>
      </div>
    </div>
    <div class="ctrl">
      <el-button
        type="primary"
        @click="
          () => {
            this.$refs.navBarRef.back();
          }
        "
        >上一页</el-button
      >
      <el-button
        type="primary"
        @click="
          () => {
            this.$refs.navBarRef.go();
          }
        "
        >下一页</el-button
      >
    </div>
  </div>
</template>
<style lang="scss" scoped>
$ppt-border: 5px;
$ppt-padding: 5px;
.container {
  height: 100%;
  display: flex;
  flex-direction: column;

  .ppt-content {
    flex-grow: 2;
    margin-top: 20px;

    display: grid;
    grid-template-rows: 1fr;
    grid-template-columns: 1fr 1fr;
    .left {
      border-top: $ppt-border solid black;
      border-bottom: $ppt-border solid black;
      border-right: $ppt-border solid black;
      padding: $ppt-padding;
    }
    .right {
      border-top: $ppt-border solid black;
      border-bottom: $ppt-border solid black;
      padding: $ppt-padding;
    }
  }
  .ctrl {
    display: flex;
    justify-content: space-between;
    margin-top: 10px;
  }
}
</style>
