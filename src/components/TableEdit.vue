<template>
    <el-main style="padding: 0">
        <el-form :inline="true" class="toolBar">
            <el-form-item label="月份">
                <el-date-picker
                        v-model="month"
                        @change="monthChange"
                        type="month"
                        placeholder="选择月">
                </el-date-picker>
            </el-form-item>
            <el-form-item label="被考勤人">
                <el-select v-model="scheduler" placeholder="被考勤人">
                    <el-option label="张三" value="shanghai"></el-option>
                    <el-option label="李四" value="beijing"></el-option>
                </el-select>
            </el-form-item>
        </el-form>
        <el-table
                :data="monthDatas"
                :cell-style="cellStyle"
                @cell-click="cellClick"
                :header-cell-style="headerStyle"
                style="width: calc(100% - 20px); margin: 10px;">
            <el-table-column :label="title"
                             align="center">
                <el-table-column
                        prop="mon"
                        align="center"
                        label="周一">
                    <template slot-scope="scope">
                        <span>{{ scope.row.mon.date }}</span>
                        <el-tag type="success" :v-show="scope.row.mon.classes == ''? false : true" style="margin-left: 5px; border-radius: 16px">中</el-tag>
                    </template>
                </el-table-column>
                <el-table-column
                        prop="tue"
                        align="center"
                        label="周二">
                    <template slot-scope="scope">
                        <span>{{ scope.row.tue.date }}</span>
                        <!--<el-tag type="success" v-show="false" style="margin-left: 5px; border-radius: 16px">中</el-tag>-->
                    </template>
                </el-table-column>
                <el-table-column
                        prop="wed"
                        align="center"
                        label="周三">
                    <template slot-scope="scope">
                        <span>{{ scope.row.wed.date }}</span>
                        <!--<el-tag type="success" v-show="false" style="margin-left: 5px; border-radius: 16px">中</el-tag>-->
                    </template>
                </el-table-column>
                <el-table-column
                        prop="thu"
                        align="center"
                        label="周四">
                    <template slot-scope="scope">
                        <span>{{ scope.row.thu.date }}</span>
                        <!--<el-tag type="success" v-show="false" style="margin-left: 5px; border-radius: 16px">中</el-tag>-->
                    </template>
                </el-table-column>
                <el-table-column
                        prop="fri"
                        align="center"
                        label="周五">
                    <template slot-scope="scope">
                        <span>{{ scope.row.fri.date }}</span>
                        <!--<el-tag type="success" v-show="false" style="margin-left: 5px; border-radius: 16px">中</el-tag>-->
                    </template>
                </el-table-column>
                <el-table-column
                        prop="sat"
                        align="center"
                        label="周六">
                    <template slot-scope="scope">
                        <span>{{ scope.row.sat.date }}</span>
                        <!--<el-tag type="success" v-show="false" style="margin-left: 5px; border-radius: 16px">中</el-tag>-->
                    </template>
                </el-table-column>
                <el-table-column
                        prop="sun"
                        align="center"
                        label="周日">
                    <template slot-scope="scope">
                        <span>{{ scope.row.sun.date }}</span>
                        <!--<el-tag type="success" v-show="false" style="margin-left: 5px; border-radius: 16px">中</el-tag>-->
                    </template>
                </el-table-column>
            </el-table-column>
        </el-table>
        <el-dialog title="选择班次" :visible.sync="dialogFormVisible">
            <el-form>
                <el-form-item>
                    <el-radio-group v-model="classes" size="medium">
                        <el-radio border label="白"></el-radio>
                        <el-radio border label="1/2白"></el-radio>
                        <el-radio border label="中"></el-radio>
                        <el-radio border label="夜"></el-radio>
                    </el-radio-group>
                </el-form-item>
            </el-form>
            <div slot="footer" class="dialog-footer">
                <el-button @click="dialogFormVisible = false">取 消</el-button>
                <el-button type="primary" @click="confirmClasses">确 定</el-button>
            </div>
        </el-dialog>
    </el-main>
</template>

<script>
    export default {
        name: "TableEdit",
        data() {
            return {
                monthDatas: [],
                month: '',
                scheduler: '',
                title: '考勤登记（月）',
                weeks: ['mon', 'tue', 'wed', 'thu', 'fri', 'sat', 'sun'],
                thisMonth: '',
                dialogFormVisible: false,
                classes: '',
                tar: ''
            }
        },
        methods: {
            confirmClasses() {
                console.log(this.tar)
            },
            monthChange(val) {
                let date = new Date(val)
                let year = date.getFullYear(), month = date.getMonth() + 1
                this.title = '考勤登记（月）' + '[' + year + '-' + this.addZero(month) + ']'
                this.initThisMonthData(year, month - 1)
                this.thisMonth = month
            },
            cellClick(row, column, cell, event) {
                console.log(event.target.innerText)
                this.tar = event.target
                console.log(event)
                this.dialogFormVisible = true
            },
            cellStyle({row, column, rowIndex, columnIndex}) {
                let cellVal = this.monthDatas[rowIndex][this.weeks[columnIndex]]    // 当前单元格值
                // let month = cellVal.split('-')[0]
                // if (month == this.thisMonth) { //指定坐标
                //     return 'cursor: pointer'
                // } else {
                //     return 'color:#C0C4CC; cursor: not-allowed'
                // }
            },
            headerStyle({row, column, rowIndex, columnIndex}) {
                if (rowIndex === 1 && columnIndex === 6 || columnIndex === 5) { //指定坐标
                    return 'color:#F56C6C'
                } else {
                    return ''
                }
            },
            // 初始化当前月份数据
            initThisMonthData(year, month) {
                let monthData = new Array(), weekData = {}
                let currentDate = new Date(year, month);
                // 获取当前月份
                let currentMonth = currentDate.getMonth(),
                    currentYear = currentDate.getFullYear()
                // 当月第一天
                let firstDay = new Date(currentYear, currentMonth, 1)
                currentDate.setDate(1)
                // 判断当月第一天是周几；0-周日
                let dayOfMonth = currentDate.getDay()
                console.log(dayOfMonth)
                if (dayOfMonth === 0) {
                    dayOfMonth = 7
                }
                // 组装数据
                for (let i = 0; i < 6; i++) {
                    weekData = {}
                    if (i === 0) {
                        // 第一周
                        let z = 0
                        for (var k = dayOfMonth - 1; k >= 0; k--, z++) {
                            currentDate = new Date(currentYear, currentMonth, 1)
                            console.log(currentDate)
                            currentDate.setDate(firstDay.getDate() - k)
                            let map = {}
                            map.month = currentDate.getMonth() + 1
                            map.date = currentDate.getDate()
                            map.classes = '中'
                            weekData[this.weeks[z]] = map
                        }
                        for (let j = z; j < 7; j++) {
                            currentDate = new Date(currentYear, currentMonth, 1)
                            currentDate.setDate(Math.abs(j - dayOfMonth + 2))
                            let map = {}
                            map.month = currentDate.getMonth() + 1
                            map.date = currentDate.getDate()
                            weekData[this.weeks[j]] = map
                        }
                    }
                    else {
                        for (let j = 0; j < 7; j++) {
                            currentDate = new Date(currentYear,monthData[0][this.weeks[j]].month, monthData[0][this.weeks[j]].date)
                            currentDate.setDate(currentDate.getDate() + 7 * i)
                            let map = {}
                            map.month = currentDate.getMonth() + 1
                            map.date = currentDate.getDate()
                            weekData[this.weeks[j]] = map
                        }
                    }
                    monthData.push(weekData)
                }

                this.monthDatas = monthData
            },
            // getWeekByDay(dayValue) { //dayValue=“2014-01-01”
            //     var day = new Date(Date.parse(dayValue.replace(/-/g, '/'))); //将日期值格式化
            //     var today = new Array("星期日", "星期一", "星期二", "星期三", "星期四", "星期五", "星期六"); //创建星期数组
            //     console.log(today[day.getDay()])
            //     return today[day.getDay()];  //返一个星期中的某一天，其中0为星期日
            // }
            addZero(val) {
                return val < 10 ? '0' + val : val
            }
        },
        mounted() {
            // 初始化当前月份数据
            let currentDate = new Date()
            this.thisMonth = currentDate.getMonth() + 1
            this.initThisMonthData(currentDate.getFullYear(), currentDate.getMonth())
        }
    }

</script>

<style lang="less" scoped>
    .toolBar {
        margin-top: .5em;
        margin-left: 10px;
        height: 40px;
        text-align: left;
    }
</style>