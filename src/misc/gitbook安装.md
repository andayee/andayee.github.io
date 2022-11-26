# gitbook安装
npm config set registry http://registry.npm.taobao.org

npm install -g gitbook-cli --registry=http://registry.npm.taobao.org

gitbook init

gitbook serve

gitbook build

# Generate a PDF file
$ gitbook pdf ./ ./mybook.pdf

# Generate an ePub file
$ gitbook epub ./ ./mybook.epub

# Generate a Mobi file
$ gitbook mobi ./ ./mybook.mobi

