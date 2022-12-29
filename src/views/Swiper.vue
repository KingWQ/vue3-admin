<template>
    <el-card class="swiper-container">
        <el-table :data="state.tableData" 
            :loading="state.loading"
            tooltip-effect="dark"
            style="width: 100%"
        >
            <el-table-column type="selection" width="55"></el-table-column>
            <el-table-column label="轮播图" width="200" >
                <template #default="scope">
                    <img style="width:80px;height:80px" :src="scope.row.carouselUrl" alt="轮播图">
                </template>
            </el-table-column>
            <el-table-column label="跳转链接">
                <template #default="scope">
                    <a target="_blank" :href="scope.row.redirectUrl">{{ scope.row.redirectUrl }}</a>
                </template>
            </el-table-column>

            <el-table-column prop="carouselRank" label="排序值" width="120" />
            <el-table-column prop="createTime" label="添加时间" width="200" />
        </el-table>
    </el-card>
</template>

<script setup>
import { onMounted, reactive, ref } from 'vue'
import axios from '@/utils/axios'

const state = reactive({
    loading:false, //控制加载动画
    tableData:[],
    currentPage:1,
    pageSize:10,
})

onMounted(()=>{
    getCarousels()
})

// 获取轮播图列表
const getCarousels = () => {
    state.loading = true
    axios.get('/carousels', {
        params: {
            pageNumber: state.currentPage,
            pageSize: state.pageSize
        }
    }).then(res => {
        state.tableData = res.list
        state.loading = false
    })
}
</script>