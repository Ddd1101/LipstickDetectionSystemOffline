<template>
    <div class="fillcontain">
        <head-top></head-top>
        <div class="table_container">
            <el-table
                :data="tableData"
                highlight-current-row
                style="width: 100%">
                <el-table-column
                    type="index"
                    width="100">
                </el-table-column>
                <el-table-column
                    property="registe_time"
                    label="生产状态"
                    width="220">
                </el-table-column>
                <el-table-column
                    property="username"
                    label="产线编号"
                    width="220">
                </el-table-column>
                <el-table-column
                    property="city"
                    label="产线位置">
                </el-table-column>
                <el-table-column
                    property="staff"
                    label="巡检人员">
                </el-table-column>
            </el-table>
            <div class="Pagination" style="text-align: left;margin-top: 10px;">
                <el-pagination
                    @size-change="handleSizeChange"
                    @current-change="handleCurrentChange"
                    :current-page="currentPage"
                    :page-size="20"
                    layout="total, prev, pager, next"
                    :total="count">
                </el-pagination>
            </div>
        </div>
    </div>
</template>

<script>
    import headTop from '../components/headTop'
    import {getUserList, getUserCount} from '@/api/getData'

    export default {
        data() {
            return {
                tableData: [{
                    registe_time: '2016-05-02',
                    username: '王小虎',
                    city: '上海市普陀区金沙江路 1518 弄',
                    staff:"001"
                }, {
                    registe_time: '2016-05-04',
                    username: '王小虎',
                    city: '上海市普陀区金沙江路 1517 弄',
                    staff:"001"
                }, {
                    registe_time: '2016-05-01',
                    username: '王小虎',
                    city: '上海市普陀区金沙江路 1519 弄',
                    staff:"001"
                }, {
                    registe_time: '2016-05-03',
                    username: '王小虎',
                    city: '上海市普陀区金沙江路 1516 弄',
                    staff:"001"
                }],
                currentRow: null,
                offset: 0,
                limit: 20,
                count: 0,
                currentPage: 1,
            }
        },
        components: {
            headTop,
        },
        created() {
            this.initData();
        },
        methods: {
            async initData() {
                try {
                    // const countData = await getUserCount();
                    const countData = {status:1,count:20}
                    console.log(countData);
                    if (countData.status == 1) {
                        this.count = countData.count;
                    } else {
                        throw new Error('获取数据失败');
                    }
                    this.getUsers();
                } catch (err) {
                    console.log('获取数据失败', err);
                }
            },
            handleSizeChange(val) {
                console.log(`每页 ${val} 条`);
            },
            handleCurrentChange(val) {
                this.currentPage = val;
                this.offset = (val - 1) * this.limit;
                this.getUsers()
            },
            async getUsers() {
                // const Users = await getUserList({offset: this.offset, limit: this.limit});

                var Users = new Array();
                // var param = ["city","registe_time","username"];
                // var item = {
                //     balance: 0,
                //     brand_member_new: 0,
                //     city: "宜昌",
                //     registe_time: "2020-11-21 18:55",
                //     username:"L01",
                // };
                for (var i = 0; i < 20; ++i) {
                    var tmp = {};
                    tmp["city"] = "宜昌";
                    if(i%3==0){
                        tmp["registe_time"] = "离线";
                    }else{
                        tmp["registe_time"] = "在线";
                    }

                    if(i<10){
                        tmp["username"] = "L00"+i;
                    }else{
                        tmp["username"] = "L0"+i;
                    }
                    tmp["staff"] = "001";
                    Users.push(tmp);
                }
                console.log(Users);

                this.tableData = [];
                Users.forEach(item => {
                    const tableData = {};
                    tableData.username = item.username;
                    tableData.registe_time = item.registe_time;
                    tableData.city = item.city;
                    tableData.staff = item.staff;
                    this.tableData.push(tableData);
                })
            }
        },
    }
</script>

<style lang="less">
    @import '../style/mixin';

    .table_container {
        padding: 20px;
    }
</style>
