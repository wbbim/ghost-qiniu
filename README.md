Ghost with Qiniu Support
=================

介绍
------
Ghost 原版只维护了本地储存，该版本将其修改为上传至七牛云。  
本版本基于 Ghost 的官方稳定版，随官方版本进行更新。  
支持通过 npm 模块启动 `npm install ghost-qiniu` 。  
七牛云 Storage 代码修改自 [GhostChina 发布的 Ghost 0.4.2 七牛云集成](https://github.com/ghostchina/Ghost-0.4.2-qiniu) 。  
七牛云上传代码来自七牛云 Node.js SDK。

修改部分
------
* 修改了 `core/server/storage/index.js` 使其只支持七牛云上传  
  （由于 Storage 只会被初始化一次，配置文件内的内容尚未被被读取，无法判断配置是否存在）
* 修改了 `core/server/config/index.js` 判断七牛云配置是否存在，将读取到的七牛云配置插入 config 对象  
  （新版的配置载入并非直接载入所有配置，而且第一次初始化时并不会读取到七牛云配置）
* 添加了 `core/storage/qiniu.js` 实现七牛云支持

配置
------
以下为示例
```
qiniu: {
    bucketname: 'my-first-bucket', //空间名称
    ACCESS_KEY: 'xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx', //具体含义请参考七牛的文档
    SECRET_KEY: 'xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx',
    root: '/images/', //文件存储在哪个目录。可以设置为 `/` 表示存储在根目录
    prefix: 'http://cdn.my-domainname.com'  //上传的文件的 URL 前缀，可以是你自己绑定的二级域名或者七牛云默认分配的二级域名。
}
```

上传文件在七牛云上的存储结构
------
首先计算上传文件的 md5 值，然后取 md5 值的第1位字符作为一级目录名称，取第2和3位作为二级目录名称，剩余的字符作为文件名。例如：

某上传文件的 md5 值为：6fb2a38dc107eacb41cf1656e899cf70；扩展名为 .jpg ；目录及文件组织结构为：6/fb/2a38dc107eacb41cf1656e899cf70.jpg

注意
------
* `content/images` 目录必须设置正确的写权限。

版权和许可证
------
* Ghost 代码版权为 Ghost 基金会 所有，遵循 [MIT 许可证](https://github.com/TryGhost/Ghost/blob/master/LICENSE) 发布。
* 该版本代码中由本人修改的部分版权为本人所有，遵循 [MIT 许可证](https://github.com/sanddudu/ghost-0.5.2-qiniu/blob/master/LICENSE) 发布。
* [GhostChina 发布的 Ghost 0.4.2 七牛云集成](https://github.com/ghostchina/Ghost-0.4.2-qiniu) 代码中由 Ghost中文网 编写的代码版权为 Ghost中文网 所有，遵循 [MIT 许可证](https://github.com/ghostchina/Ghost-0.4.2-upyun/blob/master/LICENSE) 发布
* 七牛云 SDK 版权为 七牛云存储 所有，遵循 [MIT 许可证](http://www.opensource.org/licenses/MIT) 发布