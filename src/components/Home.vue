<template>
    <el-container class="home-container">
        <!-- 头部区域 -->
        <el-header>
            <div>
                <img src="../assets/a1.jpg" alt="">
                <span>电商后台管理系统</span>
            </div>
            <el-button type="info" @click="logout">
                退出
            </el-button>
        </el-header>
        <!-- 页面主体区域 -->
        <el-container>
            <!-- 侧边栏 -->
            <el-aside :width="isCollapse? '64px' : '200px'">
                <div class="toggle-button" @click="toggltCollapse">|||</div>
                
                <!-- 侧边栏菜单区域 -->
                <!-- 注意:router 启用该模式会在激活导航时，以index作为path进行路由跳转 -->
                <el-menu background-color="gray" text-color="#fff"
                active-text-color="#ffd04b" :unique-opened="true"
                :collapse="isCollapse" :collapse-transition="false" router
                :default-active="activePath">

                    <!-- 一级菜单 -->
                    <!-- 注意:index后面只能接受字符串，不接收数值，使用 +'' 可以将字符串转为数值 -->
                    <el-submenu :index="item.id + ''" v-for="item in menuList" :key=item.id>
                        <template slot="title">
                            <i :class="iconsObj[item.id]"></i>
                            <span>{{item.authName}}</span>
                        </template>

                        <!-- 二级菜单 -->
                        <el-menu-item :index="'/' + subItem.path" v-for="subItem in item.children"
                        :key="subItem.id" @click="saveNavState('/' + subItem.path)">
                            <template slot="title">
                            <i class="el-icon-menu"></i>
                            <span>{{subItem.authName}}</span>
                        </template>
                        </el-menu-item>

                    </el-submenu>
                </el-menu>
            </el-aside>
            <!-- 右侧内容主体 -->
            <el-main>
                <!-- welcome的路由占位符 -->
                <router-view></router-view>
            </el-main>
        </el-container>
    </el-container>
</template>

<script>
export default {
    data () {
        return {
            menuList: [],
            // 取出data中的数组里的children中的id
            iconsObj: {
                '125': 'iconfont icon-user',
                '103': 'iconfont icon-tijikongjian',
                '101': 'iconfont icon-shangpin',
                '102': 'iconfont icon-danju',
                '145': 'iconfont icon-baobiao',
            },
            // 是否折叠
            isCollapse: false,
            // 被激活的链接地址
            activePath: ''
        }
    },
    created () {
        this.getMenuList(),
        this.activePath = window.sessionStorage.getItem('activePath')
    },
    methods: {
        // 退出登录
        logout () {
            window.sessionStorage.clear();
            this.$router.push('/login')
        },
        // 获取左侧菜单数据
        async getMenuList () {
            const {data: res} = await this.$http.get('menus')
            if (res.meta.status !== 200) return this.$message.error(res.mate.msg)
            this.menuList = res.data
            console.log(res)
        },
        // 点击切换左侧菜单的折叠和展开
        toggltCollapse () {
            this.isCollapse = ! this.isCollapse
        },
        // 保存链接的激活状态
        saveNavState (activePath) {
            window.sessionStorage.setItem('activePath', activePath)
            this.activePath = activePath
        }
    }
    
}
</script>

<style scoped>
.home-container {
    height: 600px;
    background-image: url('../assets/a2.jpg');
}
.home-container img {
    width: 60px;
    height: 60px;
    border-radius: 50%;
}
.el-header {
    background-color: rgba(125,125,125,0.3);
    display: flex;
    justify-content: space-between;
    padding-left: 0;
    align-items: center;
    color: #fff;
    font-size: 20px;
}
.el-header div {
    display: flex;
    align-items: center;
}
.el-header div span {
    margin-left:  15px;
}
.el-aside {
    background-color: rgba(0,0,0,0.7);
}
.el-aside .el-menu {
    border-right: none;
}
.el-main {
    background-color: rgba(0,0,0,0.5);
}
.iconfont {
    margin-right: 10px;
}
.toggle-button {
    background-color: rgba(0,0,0,0.1);
    font-size: 10px;
    line-height: 24px;
    color: white;
    text-align: center;
    letter-spacing: 0.2em;
    cursor: pointer;
}

</style>