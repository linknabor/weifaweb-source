<style scoped>
	.bottomBtn{
		font-size: 0.35rem;         
		position: fixed;         
		bottom: 0;
		height: 49px; overflow: hidden;        
		background-color:#ff8a00;         
		color:white;letter-spacing: 2px;
		width: 100%;line-height:49px;text-align: center;     
	}
	ul{
		margin: 0.1rem 0 0;
		padding: 0;
	}
	ul li{
		line-height: 0.7rem;
		color: #000;
		padding: 0.1rem 0.15rem;
		list-style: none;
		overflow: hidden;
	}
	.fr{
		float: right;
		
	}
</style>

<template>
	<div class="bindhouse">
		<ul  v-show="canAddhouse ">
			<li>{{data.sect_name}}&nbsp;{{data.city_name}}</li>
			<li>
				<span class="fl">{{data.cell_addr}}</span>
				<span class="fr">{{data.cnst_area}}平米</span>
			</li>
			<li>
				{{data.mng_cell_id}}(户号)
			</li>
		</ul>
		<div class="bottomBtn"  @click="addHouse">绑定房子</div>
	</div>
</template>

	
<script>
	let vm;
	import { MessageBox } from 'mint-ui';
	import loading from '../../components/loading.vue';

	export default {
	  components:{loading},
	  created(){
	  	vm = this;
	  },
	  data(){
	  	return{
	  		loadingShow : true,
	  		data:{
	  			name:'',
	  		},
	  		axiosParams:{
        		number: this.$route.params.number
			},
			canAddhouse:false,

	  	}
	  },
	  mounted(){
	  	//查询number下的房屋
	  	 let url = '/hexiehouse/'+ this.axiosParams.number;
  		vm.receiveData.getData(vm,url,'response',function(){
			  if(vm.response.result== null) {
				  vm.data={}
				  alert('未查询到该房屋');
				  vm.$router.push("/addHouse");
				  // vm.canAddhouse=false;
			  }else {
				  vm.data = vm.response.result
				  vm.canAddhouse=true;
			  }
  		})
	  },
	  methods:{
	  	addHouse(){//添加房子 post 提交两个参数 
	  		let stmtId = this.axiosParams.number;
	  		let url2 = '/addhexiehouse/'+stmtId+'/'+this.data.mng_cell_id;
	  		vm.receiveData.getData(vm,url2,'res',function(){
	  			// vm.loadingShow = false;
	  			if(vm.res.success == true){
	  				if(vm.res.result !== null){
	  					MessageBox.alert('添加房子成功',vm.config.wuye_house.url1).then( action =>{
	  						vm.$router.push("/myHouse")
	  					})
	  				}else{
	  					MessageBox.alert('添加房子成功',vm.config.wuye_house.url1).then( action =>{
	  						vm.$router.push("/myHouse")
	  					})
	  				}
	  			}else{
	  				MessageBox.alert(vm.res.message).then( action =>{
						vm.$router.push("/addHouse")
	  				})
	  			}


	  		})

	  	}
	
	  }
	}
</script>