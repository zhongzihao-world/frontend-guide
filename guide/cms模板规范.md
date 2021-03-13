## 引进 cms 模板

用 yeoman 搭建活动模板脚手架

1，安装 Yeoman 工具集

```
npm install --global yo
```

2，执行 npm install -g generator-cms-module，把脚手架放在你自己的 yo 上

3，在项目上运行 yo，会出现可以看到 Cms Module 的个人 generator，选择并按提示输入信息就可以把该脚手架的代码 create 到项目里

## 模板规范

- {workplace}/config：配置文件
- {workplace}/src/components：公共组件
- {workplace}/src/entry：每个页面的入口文件
- {workplace}/src/page：每个页面的 html 文件和对应的 css/less 文件
- {workplace}/src/public：公共文件存放位置，包括 css，img 和 js 文件
- {workplace}/build/calculate：每个页面的数据处理存放位置
- {workplace}/preview：预览图片存放位置
- {workplace}/preview.json：预览数据信息
- {workplace}/config.json：网站配置信息
