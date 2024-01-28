本项目已经集成了 github 自动部署，只需要运行根目录下的 push.sh 文件就可以上传 github，并且自动部署

其中配置项目在 .github/workflow 目录下的 pages.yml，


图片全部保存在 assets 目录下，typora 自动保存在当前目录下
但是hexo部署读取的目录在 source/assests 目录下的图片

使用 git hooks 实现，在 commit 的时候将改变的图片移动到 source/assets 目录下
 