<template>
    <div>
        <div class="crumbs">
            <el-breadcrumb separator="/">
                <el-breadcrumb-item>
                    <i class="el-icon-pie-chart"></i> LTC数据统计
                </el-breadcrumb-item>
            </el-breadcrumb>
        </div>
        <div class="container"  style="display: flex">
            <div class="schart-box" >
                <div class="content-title">最近一周消费充值数据柱状图</div>
                <schart v-if="loading" class="schart" canvasId="bar" :options="options1"></schart>
            </div>
            <div class="schart-box">
                <div class="content-title">最近一周消费充值数据折线图</div>
                <schart v-if="loading" class="schart" canvasId="line" :options="options2"></schart>
            </div>
        </div>
    </div>
</template>

<script>
import Schart from 'vue-schart';
import {getSchart1Data} from "../../api/index";
import {getSchart2Data} from "../../api/index";
import {getSchart3Data} from "../../api/index";

export default {
    name: 'basecharts',
    components: {
        Schart
    },
    data() {
        return {
            loading:false,
            options1: {
                type: 'bar',
                title: {
                    text: '最近一周消费充值数据'
                },
                bgColor: '#fbfbfb',
                labels: ['周一', '周二', '周三', '周四', '周五'],
                datasets: [
                    {
                        label: '人民币消费',
                        fillColor: 'rgba(241, 49, 74, 0.5)',
                        data: []
                    },
                    {
                        label: '充值',
                        data: []
                    },
                    {
                        label: '积分兑换',
                        data: []
                    }
                ]
            },
            options2: {
              type: 'line',
              title: {
                text: '最近一周消费充值数据'
              },
              bgColor: '#fbfbfb',
              labels: ['周一', '周二', '周三', '周四', '周五'],
              datasets: [
                {
                  label: '人民币消费',
                  fillColor: 'rgba(241, 49, 74, 0.5)',
                  data: []
                },
                {
                  label: '充值',
                  data: []
                },
                {
                  label: '积分兑换',
                  data: []
                }
              ]
            }

        };

    },
    created() {
        this.schart1Data();
    },
    methods:{
        schart1Data(){
            // this.options1.datasets[0].data=[1000,1200,0,2300,1700]
            // this.options1.datasets[1].data=[1500,1600,1700,1800,3500]
            // this.options1.datasets[2].data=[600,1800,3000,5000,2000]
            // this.options2.datasets[0].data=[1000,1200,0,2300,1700]
            // this.options2.datasets[1].data=[1500,1600,1700,1800,3500]
            // this.options2.datasets[2].data=[600,1800,3000,5000,2000]
            // this.loading=true;
            getSchart1Data().then(res=>{
                if(res.status==200){
                    this.options1.datasets[0].data=res.data.consume;
                    this.options1.datasets[1].data=res.data.recharge;
                    this.options1.datasets[2].data=res.data.integral;
                    this.options2.datasets[0].data=res.data.consume;
                    this.options2.datasets[1].data=res.data.recharge;
                    this.options2.datasets[2].data=res.data.integral;
                    this.loading=true;
                }
            })
        },
    }
};
</script>

<style scoped>
.schart-box {
    display: inline-block;
    margin: 20px;
}
.schart {
    width: 600px;
    height: 400px;
}
.content-title {
    clear: both;
    font-weight: 400;
    line-height: 50px;
    margin: 10px 0;
    font-size: 22px;
    color: #1f2f3d;
}
</style>
