# 《简单爬虫部署在heroku》

## 目标

当在浏览器中访问 `http://localhost:3000/` 时，输出 CNode社区首页的所有帖子标题和链接，以 json 的形式。

## 知识点

1. 学习使用 superagent 抓取网页
2. 学习使用 cheerio 分析网页

## 内容

还记得我们怎么新建一个项目吗？

1. 新建一个文件夹，进去之后 `npm init`
1. 安装依赖 `npm install --save PACKAGE_NAME`
1. 写应用逻辑

```js
app.get('/', function (req, res, next) {
  // 用 superagent 去抓取 https://cnodejs.org/ 的内容
  superagent.get('https://cnodejs.org/')
    .end(function (err, sres) {
      // 常规的错误处理
      if (err) {
        return next(err);
      }
      // sres.text 里面存储着网页的 html 内容，将它传给 cheerio.load 之后
      // 就可以得到一个实现了 jquery 接口的变量，我们习惯性地将它命名为 `$`
      // 剩下就都是 jquery 的内容了
      var $ = cheerio.load(sres.text);
      var items = [];
      $('#topic_list .topic_title').each(function (idx, element) {
        var $element = $(element);
        items.push({
          title: $element.attr('title'),
          href: $element.attr('href')
        });
      });

      res.send(items);
    });
});
```
## 部署在heroku

- 在项目下新建Procfile,输入 web: node app.js
- 去官网下载 Heroku-cli
- `heroku login`
- cd 项目下,`heroku create`
- `git add .` && `git commit -m "xxx"`
- `git remote -v` 检查项目源
- `git push heroku master`
- 查看结果 `heroku open`
