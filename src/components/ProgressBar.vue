<template>
<div class="wrapper">
    <div class="bar" :style="barStyle" @click.stop="barClick">
        <div class="leftBar" :style="leftBarStyle">
            <div @touchstart.stop="touchstart" @touchmove.stop="touchmove" @touchend.stop="touchend" draggable="true" class="ball" :style="ballStyle"></div>
        </div>
    </div>
</div>
</template>

<script>
/* eslint-disable */
export default {
    name: 'ProgressBar',
    props: {
        barBgColor: String,
        leftBgColor: String,            
        ballBgColor: String,
        percent: String
    },
    data()  {
        return {
            startX: 0,
            barWidth: 0,
            barLeft: 0,
            barRight: 0,
            ballWidth: 0,
            barStyle: {
                background: this.barBgColor,
            },
            leftBarStyle: {
                width: this.percent,
                background: this.leftBgColor,
            },
            ballStyle: {
                background: this.ballBgColor,
            }
        }
    },
    computed: {
        barElement(){
            return this.$el.getElementsByClassName('bar')[0];
        },
        ballElement(){
            return this.$el.getElementsByClassName('ball')[0];
        }
    },
    methods: {
    	touchstart(e){
	        //console.log(this.ballStyle.left);
        },
        touchmove(e){		        
	        this.startX = event.targetTouches[0].pageX;	//触摸点离窗口左侧的偏移距离
	        this.barWidth = this.barElement.offsetWidth;//进度条宽度
	        this.barLeft = this.barElement.offsetLeft;//进度条左侧离窗口左边的距离
	        this.barRight =  this.barElement.offsetWidth + this.barLeft;//进度条右侧离窗口左边的距离
            this.ballWidth = this.ballElement.offsetWidth;//圆球宽度
	        
	        if(this.startX<this.barLeft){
		        this.ballLeft =  -(this.ballWidth/2);
	        }
	        if(this.startX>(this.barRight-this.ballWidth/2)){
		        this.ballLeft = this.barElement.offsetWidth - this.ballWidth/2;
	        }
	        if(this.startX>this.barLeft && this.startX<this.barRight){
		        this.ballLeft = this.startX - this.barLeft - this.ballWidth/2;
	        }
	        //左侧进度条宽度
	        this.leftBarStyle.width = (this.ballLeft + this.ballWidth/2) + 'px';
        },
        touchend(e){
            let nowPercent = (parseInt(this.leftBarStyle.width)/this.barWidth*100).toFixed(2) + '%'
	        this.$emit('bar-drag',nowPercent)
        },
        barClick(e){//点击进度条
            this.barWidth = this.barElement.offsetWidth;//进度条宽度
            this.barLeft = this.barElement.offsetLeft;//进度条左侧离窗口左边的距离
            this.leftBarStyle.width = (event.pageX - this.barLeft + this.ballWidth/2) + 'px';
            let nowPercent = (parseInt(event.pageX - this.barLeft)/this.barWidth*100).toFixed(2) + '%'
            this.$emit('bar-drag',nowPercent)
        }
    }
}
</script>

<style scoped>
    .wrapper{
        width:100%;
    }
    .bar{
        width:100%;
        height:4px;
        margin-top: 18px;
        border-radius: 2px;
        background-color: #ccc;
        cursor: pointer;
    }
    .leftBar{
        height:4px;
        border-radius: 2px;
        width: 30%;
        background-color: #000000;
        position: relative;
    }
    .ball{
        height:20px;
        width:20px;
        -webkit-border-radius:10px;
        -moz-border-radius:10px;
        border-radius:10px;
        background: #3d9b98;
        position: absolute;
        right: -10px;
        top: -7px;
    }
</style>
