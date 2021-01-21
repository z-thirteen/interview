1. webpack 编译原理:

参数初始化:  从node_modules/.bin 目录中读取参数和配置参数合并

开始编译:   用上一步得到的参数初始化 Compiler 对象, 加载所有配置的插件, 执行对象的 run 方法开始执行编译;

确定入口:   根据配置中的 entry 找出所有的入口文件;

编译模块:   从入口文件出发，调用所有配置的 Loader 对模块进行翻译，再找出该模块依赖的模块，再递归此步骤直到所有入口依赖的文件都经过了处理;

完成模块编译:
        在经过第4步使用 Loader 翻译完所有模块后，得到了每个模块被翻译后的最终内容以及它们之间的依赖关系。

输出资源:  根据入口和模块之间的依赖关系, 组装成一个个包含多个模块的 Chunk，再把每个 Chunk 转换成一个单独的文件加入到输出列表，这步是可以修改输出内容的最后机会;

2. loader 和 plugin 有啥区别? 

  执行时机不同:
   
  loader 在固定的时间节点执行。 plugin在各自钩子时间节点执行。

3. 常用的loader: 
   
  less-loader     // 解析less 语法。
  postcss-loader  // 做浏览器兼容处理。 
  css-loader      // 解析import require 导入导出语法。
  style-loader    // 把 css 以标签形式插入页面中。

4. 常用 plugin:

   copy-webpack-plugin 
   friendly-errors-webpack-plugin 
   html-webpack-plugin
   mini-css-extract-plugin
   uglifyjs-webpack-plugin

5. webpack 优化

   1. 针对不同的loader采用了多进程编译，指定精确处理的目录和排除的目录，并开启缓存 极大的加快了编译速度。
   2. 使用webpack.DllReferencePlugin 根据环境自动 提取固定资源，加快编译与打包速度。
   3. 启用 scope hoisting。
   4. 大文件跳过编译 直接拷贝。
   5. nodemon 监听配置文件改动。
   6. 系统级的错误提示。

6. webpack HMR 原理:

   构建阶段: 创建 bundle 的时候, 加入一段 HMR runtime 的 js 和一段和服务沟通的 js 
   更新阶段: 服务器通过向浏览器发送更新消息，浏览器通过 jsonp 拉取更新的模块文件，jsonp 回调触发模块热替换逻辑

   核心 webSocket +   runtime。

7. 介绍下 webpack 的 treeshaking？

   treeshaking 是 rollup 的作者首先提出的, 用于剔除代码中无用的引用。

   要在项目中开启 treeshaking主要事项:

   1. 代码必须要遵循 ES6 模块化规范。
   2. 代码不要有副作用

   解决副作用的问题:

   package.json 的 sideEffects字段

8. webpack 编译文件的hash值都包括哪些？ 分别代表什么？

   * Hash         只要有文件更改hash就会发生变化。
   * chunkhash    针对公共依赖做hash。 比如项目中的框架包, react  react-dom。
   * contenthash  根据文件内容生成摘要。类似Etag;

















