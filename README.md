CSS 缩放器
=========================
什么是css缩放器？
> 其实就是针对背景图片进行缩放，因为是跟样式捆绑的，所以我们叫样式缩放器。

## 背景

主要针对无线端，目前无线端高清屏幕越来越多，为了让网页得到更好的用户体验，我们往往需要为高清
屏幕提供2倍大小的背景图片。

但是，有时候用户所在的网络环境不理想，加载非高清版本，能够让内容更快的呈现出来，
于是我们又需要提供一份非高清版本的样式，根据用户网络情况进行切换。

于是，我们需要维护两份相似度很高的样式表，这样便带来了一定的维护成本。

## 解决方案

> 如何解决样式表针对不同的终端带来的维护成本问题？
>> 默认只提供高清版本，普通版本自动生成如何？
>>> Excellent, 此程序就是用来实现此方案的。

## 如何使用？

* 安装npm包。

    ```bash
    npm install -g fis-prepackager-css-scale
    ```
* 配置`fis-conf.js`，对scale.css进行条件缩放。

    ```javascript
    // 启用此css-scale插件
    fis.config.set('modules.prepackager', fis.config.get('modules.prepackager') + ',css-scale');

    settings: {
            prepackager: {
                'css-scale':
            }
        }

    fis.config.set( 'settings.prepackager.css-scale', {
        include: /scale\.css$/i,
        condition: '$condition'
    });
    ```

## 规则说明



## 具体细节

针对高清屏幕的样式，我们往往会这么写。

```css
.ruler {
    background: url('xxx_200x200.png');
    background-size: 100px 100px;
}
```

把它转成普通版本的样式，需要两步。

1. 把图片`xxx_200x200.png`，通过`photoshop`缩小一倍， 变成`xxx_100x100.png`。
2. 去掉`background-size`一条。

最终变成。

```css
.ruler {
    background: url('xxx_100x100.png');
}
```

当然还有更多细节处理，这里不列出来！

## 担心图片自动缩放效果不好？

完全不用担心，效果与`photoshop`缩放的效果非常接近。

scale 0.2倍。

系统：![系统缩放](./scale.png)
Photoshop: ![photoshop缩放](./photoshop.png)

## 如果不想让某个背景图片自动缩放，怎么办？
默认样式表中所有图片，在此样式缩放的时候都会跟着缩放。如果某个图片不想被缩放，怎么办？

设置一个noScale属性就ok了。如下：

```css
.ruler {
    background: url(xxx.png?__noscale);
}
```