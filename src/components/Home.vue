<template>
	<el-container class="home-container">
		<!-- 头部区域 -->
		<el-header>
			<span>电商后台管理系统</span>
			<el-button type="info" plain @click="logOut">退出</el-button>
		</el-header>
		<!-- 页面主体区域 -->
		<el-container>
			<!-- 侧边栏 -->
			<el-aside :width="isCollapse ? '64px':'200px'">
				<div class="toggle-button" @click="toggleCollapse">|||</div>
				<el-menu :default-active="activePath" class="el-menu-vertical-demo" 
					background-color="#97aece" text-color="#f1f1f1" active-text-color="#ff6459"
					:unique-opened="true" :collapse="isCollapse" :collapse-transition="false"
					:router="true">
					<!-- 一级菜单 -->
					<el-submenu :index="item.id+''" v-for="item in menuList" :key="item.id">
						<!-- 一级菜单的模板区 -->
						<template slot="title">
							<!-- 图标 -->
							<i :class="iconsObj[item.id]"></i>
							<!-- 文本区 -->
							<span>{{item.authName}}</span>
						</template>
						<!-- 二级菜单 -->
						<el-menu-item :index="'/' + subItem.path" v-for="subItem in item.children" :key="subItem.id"
						@click="saveNavState('/' + subItem.path)">
							<template slot="title">
								<i class="el-icon-menu"></i>
								<span>{{subItem.authName}}</span>
							</template>
						</el-menu-item>
					</el-submenu>
				</el-menu>
			</el-aside>
			<!-- 右侧内容 -->
			<el-main>
				<!-- 路由占位符 -->
				<router-view></router-view>
			</el-main>
		</el-container>
	</el-container>
</template>

<script>
	export default {
		data() {
			return {
				menuList:[],
				iconsObj:{
					'125':'el-icon-user-solid',
					'103':'el-icon-s-check',
					'101':'el-icon-shopping-cart-full',
					'102':'el-icon-chat-line-square',
					'145':'el-icon-s-data'
				},
				//是否折叠的状态参数
				isCollapse:false,
				//被激活的链接地址
				activePath:''
			}
		},
		
		created(){
			this.getMenuList()
			this.activePath = window.sessionStorage.getItem('activePath')
		},
		
		
		methods: {
			logOut() {
				window.sessionStorage.clear()
				this.$router.push('/login');
			},
			//获取所有的菜单
			async getMenuList() {
				const {data:res} = await this.$http.get('menus')
				console.log(res)
				if(res.meta.status !== 200) return this.$message.error(res.meta.msg)
				this.menuList = res.data
			},
			//点击按钮切换菜单的折叠与展开
			toggleCollapse(){
				 this.isCollapse = !this.isCollapse
			},
			//保存链接的激活状态
			saveNavState(activePath) {
				window.sessionStorage.setItem('activePath',activePath)
				this.activePath = activePath
			}
		}
	}
</script>

<style lang="less" scoped>
	.home-container {
		height: 100%;
	}

	.el-header {
		background-color: #798dbf;
		display: flex;
		justify-content: space-between;
		align-items: center;
		color: #f1f1f1;
		font-size: 24px;

	}

	.el-aside {
		background-color: #97aece;
		.el-menu {
			border-right: none;
		}
	}

	.el-main {
		background-color: #f1f1f1;
	}
	.el-submenu i {
		color: #ffffff;
	}
	.el-submenu span {
		margin-left: 10px;
	}
	.toggle-button {
		background-color: #a1bbda;
		font-size: 10px;
		line-height: 24px;
		color: #FFFFFF;
		text-align: center;
		letter-spacing: 0.2em;
		cursor: pointer;
	}
	.toggle-button:hover {
		background-color: #90a2d6;
	}

</style>
