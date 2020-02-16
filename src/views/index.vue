<!-- 首页 -->
<template>
   <div class="box">
       <div ref='mapbox' class="mapbox">

       </div>
   </div>
</template>

<script>
import echarts from 'echarts'
import 'echarts/map/js/china.js'
import jsonp from 'jsonp'
const option = {
    title:{
        text:"木槿疫情地图",
        link:'https://xiaomujin.club',
        subtext:'good good study,day day up'
    },
    series:[{
        name:"确诊人数",
        type:'map',
        map:'china',
        label:{
            show:true,
            color:'#000',
            fontSize:10
        },
        roam:true,
        zoom:1.2,
        emphasis:{
            label:{
                color:'#fff',
                fontSize:12
            },
            itemStyle:{
                areaColor:'#83b5e7'
            }
        }
    }],
    data:[],
    visualMap:[{
        type:'piecewise',
        show:true,
        pieces:[
            {min:10000},
            {min:1000,max:9999},
            {min:100,max:999},
            {min:10,max:99},
            {min:1,max:9},
            {max:0}
        ],
        inRange:{
            symbol:'rect',
            color:['#eee','#9c0505']
        },
        itemWidth:20,
        itemHeight:10
    }],
    tooltip:{
        trigger:'item'
    },
    toolbox:{
        show:true,
        orient:'vertical',
        left:'right',
        top:'center',
        feature:{
            dataView:{readOnly:false},
            restore:{},
            saveAsImage:{}
        }
    }
};
export default {
    data () {
       return {
       };
    },

    components: {},
    mounted(){
        this.mychart = echarts.init(this.$refs.mapbox);
        this.mychart.setOption(option);
        this.getData();
        window.addEventListener('resize',()=>{
            this.mychart.resize()
        })
    },
    methods: {
        getData(){
            jsonp('https://interface.sina.cn/news/wap/fymap2020_data.d.json?_=1580892522427',{},(err,data)=>{
                if(!err){
                    // console.log(data)
                    let list = data.data.list.map(item=>({name:item.name,value:item.value}))
                    option.series[0].data = list;
                    this.mychart.setOption(option)
                }
            })
        },
    }
}

</script>
<style>
.mapbox{
    height:600px;
    width:1000px;
    margin:50px auto;
}
.box{
    display: flex
}
</style>