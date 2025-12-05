## vue 项目中的优化

1. 代码层面的优化

   - 根据业务场景拆分组件， 提高代码复用率
   - 使用 v-for 进行列表渲染时，给 key 属性添加唯一标识
   - 路由懒加载
   - 使用 keep-alive 缓存路由
   - 图片懒加载
   - 使用异步组件配合 v-if 实现按需加载，优化首屏加载速度

2. 构建打包优化
   - 按需加载第三方组件库（unplugin-vue-components），使用时自动引入和注册组件（unplugin-auto-import, unplugin-vue-components）
   - 代码打包设定分块策略，将相关联的第三方库打包到一起；将同类型的文件打包到指定文件夹
   - 配置 esbuild.drop，移除生产环境的 log 跟 debugger
   - 配置 vite-plugin-imagemin，压缩图片
   - 配置 vite-plugin-compression，开启 gzip 压缩，减小文件体积（需要后端开启 gizp 压缩，并在响应头设置 Content-Encoding: gzip，告知客户端解压）
