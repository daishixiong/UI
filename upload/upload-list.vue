<template>
    <transition-group tag="ul" name="el-list">
        <li v-for="file in files" :key="file.uuid">
            <slot :file="file"></slot>
        </li>
    </transition-group>
</template>

<script>
/**
 * @Description: 上传组件
 * @author DaiSX
 * @date 2019-08-15
 */
  export default {
    props: {
      files: {
        type: Array,
        default () {
          return [];
        }
      }
    }
  };
</script>

<style lang="less" scoped>
 .ul {
     display: flex;
     justify-content: flex-start;
 }
 .el-list-enter-active,
 .el-list-leave-active {
     transition: all .5s;
 }
 .el-list-enter, .el-list-leave-active {
     opacity: 0;
     transform: translateY(-30px);
 }
</style>
