<template>
    <section class="contianer">
        <el-row  type="flex" justify="space-between">
            
            <!-- 顶部过滤列表 -->
            <div class="flights-content">
                <!-- 过滤条件 -->
                 <FlightsFilters :data="flightsData"/>
                
                <!-- 航班头部布局 -->
                <FightsListHead/>
                
                <!-- 航班信息 -->
                <div>
                    <!-- 航班列表 -->
                    <FlightsItem 
                    v-for="(item, index) in dataList" 
                    :key="index"
                    :data="item"/>

                    <!-- 分页 -->
                    <!-- size-change：切换条数时候触发 -->
                    <!-- current-change：页数切换时候触发 -->
                    <!-- current-page: 当前页数 -->
                    <!-- total:总数 -->
                    <el-pagination
                        @size-change="handleSizeChange"
                        @current-change="handleCurrentChange"
                        :current-page="pageIndex"
                        :page-sizes="[5, 10, 15, 20]"
                        :page-size="pageSize"
                        layout="total, sizes, prev, pager, next, jumper"
                        :total="total">
                    </el-pagination>
                </div>
            </div>

            <!-- 侧边栏 -->
            <div class="aside">
                <!-- 侧边栏组件 -->
            </div>
        </el-row>
    </section>
</template>

<script>

import FightsListHead from "@/components/air/fightsListHead.vue";
import FlightsItem from "@/components/air/flightsItem.vue";
import FlightsFilters from "@/components/air/flightsFilters.vue"

export default {
    data(){
        return {
            // 默认机票列表总数据
            flightsData: {
                // 默认机票列表
                flights: [],
                info: {},
                options: {}
            },

            pageIndex: 1,   // 默认显示第一页
            pageSize: 5,    // 默认显示多少条数据
            total: 0,       // 总条数
            dataList: []    // 分页之后的数据列表
        }
    },

    components: {
        FightsListHead,
        FlightsItem,
        FlightsFilters
    },

    methods: {
        // 分页切换条数触发
        handleSizeChange(value){
            this.pageSize = value;
        },
        
        // 页数切换时候触发
        handleCurrentChange(value){
            this.pageIndex = value;

            // 计算截图列表的数据
            this.dataList = this.flightsData.flights.slice(
                (this.pageIndex - 1) * this.pageSize,
                this.pageIndex * this.pageSize
            )
        }
    },

    mounted(){
        // 请求机票列表的数据
        this.$axios({
            url: "/airs",
            params: this.$route.query
        }).then(res => {
            this.flightsData = res.data;
            // 总条数
            this.total = res.data.total;
            
            // 获取第一到第5条
            this.dataList = res.data.flights.slice(0, 5);
        })
    }
}
</script>

<style scoped lang="less">
    .contianer{
        width:1000px;
        margin:20px auto;
    }

    .flights-content{
        width:745px;
        font-size:14px;
    }

    .aside{
        width:240px;
    } 
</style>