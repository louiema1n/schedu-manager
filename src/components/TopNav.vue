<template>
    <div>
        <el-menu
            :default-active="activeIndex"
            class="el-menu-demo"
            mode="horizontal"
            @select="handleSelect"
            background-color="#545c64"
            text-color="#fff"
            active-text-color="#ffd04b">
        <el-menu-item index="1">考勤登记</el-menu-item>
        <el-menu-item index="99" class="title"><a href="/" target="_self"><i class="el-icon-setting"></i>考勤管理系统</a></el-menu-item>
    </el-menu>
    </div>
</template>

<script>
    export default {
        name: "TopNav",
        data() {
            return {
                activeIndex: '1',
            };
        },
        methods: {
            handleSelect(key, keyPath) {
                console.log(key, keyPath);
            }
        }
    }
</script>

<style scoped lang="less">
    .title {
        float: right;
        a {
            text-decoration: none;
            font-size: larger;
        }
        i{
            font-size: larger;
            color: #FFFFFF;
        }
    }

</style>