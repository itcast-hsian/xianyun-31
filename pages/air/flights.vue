<template>
    <section class="contianer">
        <el-row  type="flex" justify="space-between">
            
            <!-- 顶部过滤列表 -->
            <div class="flights-content">
                <!-- 过滤条件 -->
                <div>
                    
                </div>
                
                <!-- 航班头部布局 -->
                <FightsListHead/>
                
                
                <!-- 航班信息 -->
                <div>
                    <!-- 航班列表 -->
                    <FlightsItem 
                    v-for="(item, index) in flightsData.flights" 
                    :key="index"
                    :data="item"/>
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

export default {
    data(){
        return {
            // 默认机票列表总数据
            flightsData: {
                // 默认机票列表
                flights: []
            }
        }
    },

    components: {
        FightsListHead,
        FlightsItem
    },

    mounted(){
        // 请求机票列表的数据
        this.$axios({
            url: "/airs",
            params: this.$route.query
        }).then(res => {

            this.flightsData = res.data;
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