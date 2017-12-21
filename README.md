# quickpage

[![Greenkeeper badge](https://badges.greenkeeper.io/leizongmin/quickpage.svg)](https://greenkeeper.io/)

快速生成静态HTML的整站程序，代码来源于 jsxss.com，帮助开源项目作者简单快速地制作项目主页

## 创建初始项目

```bash
quickpage create [源码目录]
```

## 启动开发环境

```bash
quickpage start [源码目录]
```

## 生成静态页面

```bash
quickpage build [源码目录] [目标目录]
```

## 使用方法

### 配置路由

文件 `routes.js`

```JavaScript
module.exports = function (page) {

  ['zh', 'en'].forEach(function (lang) {
  
    // 载入配置
    var config = page.load(lang + '/config');
  
    // 访问/about.html时显示页面about.liquid
    page.add(lang + '/about', lang + '/about.liquid', config);
  
    // 访问/help.html时显示页面help.md
    page.add(lang + '/help', lang + '/help.md', config);
    
  });

};
```

#### page.load(name)

载入指定文件（省略后缀，后缀可以为`.js`或`.json`

#### page.add(pathname, filename, config)

添加页面
+ `pathname`为访问路径，会自动加上`.html`后缀；
+ `filename`为源文件，如果是`.html`后缀表示Liquid模板文件，如果为`.md`后缀表示Markdown文件，会先将其转成HTML代码，并存储到`content`变量，然后渲染模板`markdown`

#### page.engine(extname, handler)

设置处理指定后缀文件的渲染方法

```JavaScript
var md = require('markdown-it')();

page.engine('md', function (filename, callback) {
  // filename表示文件名
  // callback为回调函数

  fs.readFile(filename, function (err, content) {
    if (err) return callback(err);
    
    // template为用于显示的模板
    // content为内容
    callback(null, {
      template: 'markdown',
      content: md.render(content)
    });
  });
});
```
