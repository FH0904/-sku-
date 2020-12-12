<template>
	<view class="content">
		<view>{{ selectSku.length>0 ? selectSku.join(',').replace(/,/g," ") : '请选择'  }}</view>
		<view class="specifications" v-for="(item, index) in specificationList" :key='index'>
			<view class="s-title">{{item.name}}</view>
			<view class="s-list">
				<view class="l-item" v-for="(citem,cindex) in item.valueList"
				 :key="cindex" 
				 :class="item.checked === cindex ? 'l-item-active' : '' "
				 @click="selectClassify(index,cindex)"
				 >{{ citem }}</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				specificationList:[
					{name: "颜色", valueList: ["黄色", "红色"]},
					{name: "大小", valueList: ["小","大"]},
					{name: "口味", valueList: ["咸","淡"]},
				],
				productList:[
					{
						id:1,
						price:99,
						picUrl:'',
						specifications: ["黄色","小","咸"]
					},{
						id:2,
						price:99,
						picUrl:'',
						specifications: ["黄色","小","淡"]
					},{
						id:3,
						price:299,
						picUrl:'',
						specifications: ["黄色","大","咸"]
					},{
						id:4,
						price:299,
						picUrl:'',
						specifications: ["黄色","大","淡"]
					},{
						id:5,
						price:399,
						picUrl:'',
						specifications: ["红色","小","咸"]
					},{
						id:6,
						price:399,
						picUrl:'',
						specifications: ["红色","小","淡"]
					},{
						id:7,
						price:499,
						picUrl:'',
						specifications: ["红色","大","咸"]
					},{
						id:8,
						price:499,
						picUrl:'',
						specifications: ["红色","大","淡"]
					},
					
				],
				selectSku:[], //选择到的sku
				
			}
		},
		onLoad() {

		},
		methods: {
			selectClassify(index,cindex) {
				console.log(index,cindex);
				
				this.$set(this.specificationList,index,{
					...this.specificationList[index],
					checked: cindex
				})
				
				this.specificationList.forEach((item,index)=>{  
					if('checked' in item) {
						this.selectSku[index] = item.valueList[item.checked]
					}
				})
				
				let t = [] 
				this.selectSku.forEach( item1 => {
					if( t.length >0 ) { //第二次或第三次...
						for(let i = 0; i<t.length; i++) {
							let status = false;
							t[i].specifications.forEach( item3 => {
								if(item1 == item3) {
									status = true
									return false;
								}
							})
							if (!status) {
								t.splice(i--, 1)
								console.log(t,i);
							}
							
						}
					} else {  //点击规格时候的第一次筛选
						this.productList.forEach((item2 , index2) => {
							item2.specifications.forEach( item3 => {
								if(item3 == item1) {
									t.push(item2)
									return false;
								}
							})
						})
					}
				})
			}
		}
	}
</script>

<style lang="less">
	.specifications {
		margin-top: 30rpx;
	
		.s-title {
			font-size: 28rpx;
			color: #333333;
			font-weight: bold;
		}
	
		.s-list {
			display: flex;
			flex-wrap: wrap;
	
			.l-item {
				min-width: 100rpx;
				height: 60rpx;
				line-height: 60rpx;
				text-align: center;
				background: #F2F2F2;
				border-radius: 30px;
				color: #333;
				margin-right: 20rpx;
				margin-top: 22rpx;
				padding: 0 20rpx;
			}
	
			.l-item-active {
				color: #12B770;
				background: rgba(18, 183, 122, .12);
			}
		}
	
	}

</style>
