<template>
	<div>
		<el-card class="box-card">
			<div slot="header" class="clearfix">白名单首借额度设置</div>
			<el-form :model="addForm" :rules="rules" label-width="140px" ref="addForm">
				<el-form-item label="白名单首借额度" prop="Amount">
		          	<el-input type="text" v-model.number="addForm.Amount"clearable placeholder="请输入100的整数倍金额" style="width:288px;">
		          		<template slot="append">元</template>
		          	</el-input>
		          	<i class="el-icon-info" style="font-size:18px; color:#108EE9; margin:0 20px;"></i>
		          	<span style="padding:11px 20px; color:#1989FA; border:1px solid #108EE9; border-radius:5px;">白名单用户通过风控系统默认获得的借款额度。</span>
		        </el-form-item>
				<el-form-item style="width:400px; text-align:center; padding-top:20px;">
					<el-button type="primary" @click="addSubmit">确定</el-button>
					<el-button @click.native="historyBack">返回</el-button>
				</el-form-item>
			</el-form>
		</el-card>
	</div>
</template>

<script>
	import { getwhitebalance,setwhitebalance } from 'api/api.js';
	export default {
		data() {
			return {
				addForm:{
					Amount:null
				},
				rules: {
					Amount: [{
						required:true,
						message:'白名单首借额度',
						trigger:'blur'
					}]
				}
			}
		},
		mounted() {
			this.getdtl();
		},
		methods: {
			getdtl(){
				getwhitebalance({}).then((res) => {
					if( res.Ret === 200 ){
						this.addForm.Amount=res.Amount;
					}
				});
			},
	     	addSubmit(){
	     		this.$refs.addForm.validate((valid) => {
					if( valid ){
						if( this.addForm.Amount%100!=0 || this.addForm.Amount<=0 ){
							this.$message.error( "请输入100的整数倍金额!" );
							return false;
						}
						setwhitebalance(this.addForm).then((res) => {
							if( res.Ret === 200 ){
								this.$message.success( res.Msg );
								let that=this;
								setTimeout(function(){
									that.historyBack();
								},1000);
							}
						});
					}
				});
	     	},
	     	historyBack(){  //返回上一页
		    	history.back();
		    }
		}
	}
</script>

<style scoped lang="scss"></style>