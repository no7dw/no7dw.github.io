### Markdown 中快速插入图片的处理方式

选项 :
- Marxico 同步 evernote 账号， 然后在Marxico 中插入数据
    ![2020-04-05-14-18-52](http://img.no1token.com/2020-04-05-14-18-52.png)
    适用于evernote 重度使用者，但以前Marxico sync to evernote 的免费的功能，现在需要付费

- vscode 插件（推荐）
    - paste image to aliyun
    - paste image to qiniu （推荐）

在试验 paste image to qiniu 的时候，发现提示

发现 https://github.com/favers/vscode-qiniu-upload-image 好久没维护了，有个 [issue & pr](https://github.com/favers/vscode-qiniu-upload-image/issues/13) 提及到这个 upload  error 

解决办法：
    https://github.com/Andyliwr/vscode-qiniu-upload-image
    用这个repo 替换掉 ~/.vscode/extensions/favers.paste-image-to-qiniu-0.0.1 

其余注意：
- 配置域名解析的CNAME （dnspod 为例）：
    ![2020-04-05-14-30-55](http://img.no1token.com/2020-04-05-14-30-55.png)

- 配置qiniu域名：
    ![2020-04-05-14-28-08](http://img.no1token.com/2020-04-05-14-28-08.png)


