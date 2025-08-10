# gitbook安装
npm config set registry http://registry.npm.taobao.org

npm install -g gitbook-cli --registry=http://registry.npm.taobao.org

gitbook init

gitbook serve
本地拉起一个服务

gitbook build

# Generate a PDF file
$ gitbook pdf ./ ./mybook.pdf

# Generate an ePub file
$ gitbook epub ./ ./mybook.epub

# Generate a Mobi file
$ gitbook mobi ./ ./mybook.mobi

#补充说明
1.github-page目前也是出于被封状态，内部机器是不能访问的，只能使用外部机器。
2.summary文件是管理页面结构的，需要删减内容时，得手动编辑。
3.github.io只支持docs目录静态页面，但是gitbook默认产生的是_book目录，所以需要把_book目录改名成docs目录。
4.gitbook serve默认会把根目录下所有的目录都加到_book目录中，所以需要把docs目录加到.bookignore中，避免重复目录。

