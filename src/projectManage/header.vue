<template>
    <div class="caseHeader">
        <el-container>
            <el-container>
                <el-header style="height: 40px;">
                    <div style="float: left;">
                        <el-breadcrumb separator-class="el-icon-arrow-right" >
                            <el-breadcrumb-item
                                    v-for="(item, index) in title"
                                    :key="index"
                            >
                                {{title[index]}}
                            </el-breadcrumb-item>

                        </el-breadcrumb>
                    </div>
                    <el-dropdown style="float:right;line-height:20px;top:12px;color: rgb(255, 255, 255);" @command="logOut" >
                        <span class="el-dropdown-link">{{userName}}<i
                                class="el-icon-arrow-down el-icon--right"></i></span>
                        <el-dropdown-menu slot="dropdown" style="line-height:10px">
                            <el-dropdown-item command="a">退出系统</el-dropdown-item>
                            <el-dropdown-item command="b">个人设置</el-dropdown-item>
                        </el-dropdown-menu>
                    </el-dropdown>
                </el-header>

                <!--<el-footer style="height: 30px;">-->
                <!--<span class="demonstration">author</span>-->
                <!--</el-footer>-->
            </el-container>
        </el-container>
    </div>
</template>

<script>
    import * as types from '../store/types'

    export default {

        name: 'caseHeader',
        data() {
            return {
                title: ['项目管理', '项目'],
                userName: '',
            }
        },
        methods: {
            getUserName() {
                // this.$store.state.userName = this.userName;
                this.userName = this.$store.state.userName;
            },
            logOut(command) {
                if (command === "a") {

                    this.$store.commit(types.LOGOUT);
                    this.$router.push({path: '/login'});
                    this.$axios.get('/api/api/logout').then((response) => {
                            if (response.data['status'] === 0) {
                                this.$message({
                                    showClose: true,
                                    message: response.data['msg'],
                                    type: 'warning',
                                });
                            }
                            else {
                                this.tableData = response.data['msg'];
                            }
                        }
                    );
                }
            },

            showTitle(path) {
                if (path === '/manage/projectManage') {
                    this.title = ['项目管理', '项目']
                }
                else if (path === '/manage/modelManage') {
                    this.title = ['接口管理', '接口模块']
                }
                else if (path === '/manage/caseManage') {
                    this.title = ['接口管理', '接口信息']
                }
                else if (path === '/manage/reportManage') {
                    this.title = ['报告管理', '接口报告']
                }
                else if (path === '/manage/sceneManage') {
                    this.title = ['接口管理', '接口用例']
                }
                else if (path === '/manage/buildInFunc') {
                    this.title = ['接口管理', '内置函数']
                }
                else if (path === '/manage/testTool') {
                    this.title = ['其他程序', '测试小工具']
                }
                else if (path === '/manage/taskManage') {
                    this.title = ['报告管理', '定时任务']
                }
                else if (path === '/manage/batch') {
                    this.title = ['其他程序', '批量操作']
                }
                else if (path === '/manage/sqlCheck') {
                    this.title = ['其他程序', '数据库查询']
                }
                else if (path === '/manage/config') {
                    this.title = ['接口管理', '业务配置']
                }
            },
            getTitle: function () {
                this.showTitle(this.$route.path);
            },
        },
        watch: {
            "$route": function (to, from) {

                this.showTitle(to.path);
                //from 对象中包含当前地址
                //to 对象中包含目标地址
                //其实还有一个next参数的，这个参数是控制路由是否跳转的，如果没写，可以不用写next()来代表允许路由跳转，如果写了就必须写next(),否则路由是不会生效的。
            }
        },
        mounted() {
            this.getTitle();
            this.getUserName()
        },
    }
</script>

<style>
    .el-header {
        background-color: #717275;
        color: #ffffff;
        text-align: center;
        line-height: 60px;
    }
    .el-breadcrumb__inner {
        color: rgb(255, 255, 255);
    }
    .el-breadcrumb__item:last-child .el-breadcrumb__inner, .el-breadcrumb__item:last-child .el-breadcrumb__inner a, .el-breadcrumb__item:last-child .el-breadcrumb__inner a:hover, .el-breadcrumb__item:last-child .el-breadcrumb__inner:hover{
        color: rgb(255, 255, 255);
    }
    /*.el-dropdown {*/
    /*vertical-align: top;*/
    /*}*/

    /*.el-dropdown + .el-dropdown {*/
    /*margin-left: 15px;*/
    /*}*/

    /*.el-icon-arrow-down {*/
    /*font-size: 12px;*/
    /*}*/
</style>
