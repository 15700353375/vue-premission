  // 0. 如果使用模块化机制编程，导入Vue和VueRouter，要调用 Vue.use(VueRouter)

  // 1. 定义 (路由) 组件。
  // 可以从其他文件 import 进来
  const Foo = { template: '<div>foo</div>' }
  const Bar = { template: '<div>bar</div>' }
  const premission1 = { template: '<div>premission1</div>' }
  const premission2 = { template: '<div>premission2</div>' }
  const premission = { template: '<div>无权限</div>' }


  // 2. 定义路由
  <!-- 配置不需要权限的基础路由 -->
  const baseRoutes = [
    { path: '/foo', component: Foo },
    { path: '/bar', component: Bar }
  ]

  <!-- 定义需要权限的主路由 -->
  const mainRoutes = [
    { path: '/premission1', component: checkPremission(1) ? premission1 : premission },
    { path: '/premission2', component: checkPremission() ? premission2 : premission }
  ]

  // 3. 创建 router 实例，然后传 `routes` 配置
  const router = new VueRouter({
    routes: baseRoutes
  })

  // 4. 创建和挂载根实例。
  // 记得要通过 router 配置参数注入路由，
  const app = new Vue({
    router,
  }).$mount('#app')

  // 现在，应用已经启动了！


  // 判断是否有权限  此处根据自己的业务写函数
  function checkPremission(type) {
    let flag = false;
    if (type) {
      flag = true
    }
    return flag
  }

  <!-- 登录成功之后去拉取主路由 -->
  document.getElementById('login').onclick = function(){
    app.isLogin = true
    addAdminRoutes()
  }

  <!-- 方式一  登录之后添加路由 -->
  function resetRouter() {
    let newRouter = new VueRouter({
      routes: baseRoutes
    });
    app.$router.matcher = newRouter.matcher;
  }

  function addAdminRoutes() {
    resetRouter();
    app.$router.addRoutes(mainRoutes)
  }


  <!-- 方式二 登录之后控制导航菜单 -->
