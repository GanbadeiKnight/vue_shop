<template>
	<div class="login_container">
		<div class="login_box">
			<div class="login_avatar">
				<img src="../assets/image/myavatar.png">
			</div>
			<div class="login_form">
				<el-form :model="loginForm"
				 :rules="loginFormRules"
				label-width="40px"
				ref="loginFormRef">
				  <el-form-item prop="name" label="账号">
				    <el-input
					placeholder="请输入账号"
					 v-model="loginForm.username" 
					 prefix-icon="el-icon-user-solid">
					 </el-input>
				  </el-form-item>
				  <el-form-item prop="password" label="密码">
				    <el-input 
					placeholder="请输入密码"
					v-model="loginForm.password" 
					prefix-icon="el-icon-more" 
					show-password></el-input>
				  </el-form-item>
				</el-form>
				<el-form class="login_btn">
					<el-button type="info" plain @click="checkForms()">登录</el-button>
				</el-form>
			</div>
		</div>
	</div>
</template>

<script>
	export default{
		name:'Login',
		data(){
			return {
				//登录表单的数据
				loginForm: {
					username:'',
					password:''
				},
				//验证规则对象
				loginFormRules: {
					name:[
						{min:3,max:10,message:'长度在3到10个字符',trigger:'blur'}
					],
					password:[
						{min:6,max:10,message:'长度在6到10个字符',trigger:'blur'}
					]
				}
			}
		},
		methods:{
			checkForms(){
				this.$refs.loginFormRef.validate(async valid => {
					if (!valid) return;
					const {data:res} = await this.$http.post("login",this.loginForm)
					console.log(res)
					if (res.meta.status !== 200) return this.$message.error('登录失败！请检查您的账号与密码！')
					this.$message.success('登陆成功！')
					//保存token到seesionStorage当中
					window.sessionStorage.setItem('token',res.data.token)
					this.$router.push("/home")
				})
			}
		}
	}
</script>

<style lang="less" scoped>
	.login_container{
		height:100%;
		display: flex;
		background-image: url(../assets/image/barbaraikuyo.jpg);
		background-repeat:no-repeat;
		background-size: 100%,100%;	
		// filter: blur(2px);
		justify-content:center;
		align-items:center;
		background-position: 0,0;
		z-index: -100;
	}
	.login_container:after{
		content: "";
		position: absolute;
		height: 100%;
		width: 100%;
		background: inherit;
		filter: blur(2px);
		// z-index: -99;
	}
	.login_box{
		height:60%;
		width:40%;
		background-color: rgba(216,226,237,0.4);
		z-index:4;
		display: flex;
		justify-content:center;
		flex-direction:column;
		justify-content:space-around;
		align-items:center;
	}
	.login_avatar{
		height: 100px;
		width: 100px;
		border-radius: 50%;
		img{
			width: 100px;
			height: 100px;
			border-radius: 50%;
		}
		margin-top: 10px;
	}
	.login_form{
		height: 200px;
		width: 340px;
	}
	.login_btn{
		display: flex;
		justify-content: center;
	}
</style>
