<template>
	<el-container class="home-container">
		<!--頭部區-->
		<el-header>
			<div>
				<img src="../assets/深田咏美.jpg" alt="" />
				<span>电商后台管理系统</span>
			</div>
			<el-button type="info" @click="logout">退出</el-button>
		</el-header>
		<!--頁面主題區-->
		<el-container>
			<!--側邊欄-->
			<el-aside :width="isCollapse ? '64px' : '200px'">
				<div class="toggle-button" @click="toggleCollapse">|||</div>
				<!--侧边栏菜单区-->
				<el-menu background-color="#B5D2F1" text-color="#363E47" 
								active-text-color="#9136F1" :unique-opened="true"
								:collapse="isCollapse" :collapse-transition="false"
								:router="true" :default-active="activePath">
					<!--一级菜单-->
					<el-submenu :index="item.id+''" v-for="item in menuList" :key="item.id">
						<template slot="title">
							<i :class="iconsObj[item.id]"></i>
							<span>{{item.authName}}</span>
						</template>
						<!--二级菜单-->
						<el-menu-item :index="'/'+subItem.path" v-for="subItem in item.children" 
													:key="subItem.id" @click="saveNavState('/'+subItem.path)">
							<template slot="title">
								<i class="el-icon-menu"></i>
								<span>{{subItem.authName}}</span>
							</template>
						</el-menu-item>
					</el-submenu>
				</el-menu>
			</el-aside>
			<!--右側內容主體-->
			<el-main>
				<router-view></router-view>
			</el-main>
		</el-container>
	</el-container>
</template>

<script>
	export default {
		name: 'Home',
		data(){
			return{
				//左侧菜单数据
				menuList:[],
				iconsObj:{
					'125':'iconfont icon-user',
					'103':'iconfont icon-tijikongjian',
					'101':'iconfont icon-shangpin',
					'102':'iconfont icon-danju',
					'145':'iconfont icon-baobiao'
				},
//				是否折叠
				isCollapse:false,
//				被激活的链接地址
				activePath:''
			}
		},
		created(){
			this.getMenuList()
			this.activePath = window.sessionStorage.getItem('activePath')
		},
		methods: {
			logout() {
				//	退出 清除账号token并重定向到login
				window.sessionStorage.clear();
				this.$router.push('/login')
			},
			//获取所有的菜单
			async getMenuList(){
				const {data:res} = await this.$http.get("menus")
				if(res.meta.status !== 200) return this.$message.error(res.meta.msg)
				this.menuList = res.data;
//				console.log(res)
			},
			//	点击按钮，切换菜单的折叠与展开
			toggleCollapse(){
				this.isCollapse = !this.isCollapse
			},
			//保存链接的激活状态
			saveNavState(activePath){
				window.sessionStorage.setItem('activePath',activePath)
				this.activePath = activePath
			}
		}
	}
</script>

<style lang="less" scoped="scoped">
	.home-container {
		height: 100%;
	}
	
	.el-header {
		background-color: #409EFF;
		padding-left: 0px;
		display: flex;
		justify-content: space-between;
		align-items: center;
		/*background-color: #37d41;*/
		>div {
			display: flex;
			align-items: center;
			span {
				margin-left: 15px;
				color: #eaedf1;
			}
			img {
				height: 60px;
			}
		}
	}
	
	.el-aside {
		background-color: #B5D2F1;
		.el-menu{
			border-right: none;
		}
	}
	
	.el-main {
		background-color: #eaedf1;
	}
	
	.iconfont{
		margin-right: 10px;
	}
	
	.toggle-button{
		background-color: #69B0F9;
		font-size: 10px;
		line-height: 24px;
		color: white;
		text-align: center;
		letter-spacing: 0.2em;
		cursor: pointer;
	}
</style>