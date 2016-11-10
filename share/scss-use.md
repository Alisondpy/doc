SCSS 初级使用
=============

1. 安装 [ruby](https://www.ruby-lang.org/en/downloads/ "download page")

  On Windows machines, you can use [RubyInstaller](http://rubyinstaller.org/)

  NOTE: Centos install gem, run `sudo yum install ruby-rdoc ruby-devel`

  ```
  yum install rubygems
  ```

1. 替换 gem 镜像源 (ruby.taobao.org)

  ```sh
  $ gem sources -a https://ruby.taobao.org/ -r https://rubygems.org/
  $ gem sources -c
  $ gem sources -l
  *** CURRENT SOURCES ***

  https://ruby.taobao.org
  # 请确保只有 ruby.taobao.org
  ```

1. 安装compass,在命令行中输入

  ```sh
  gem install compass
  ```

1. windows compass 处理中文乱码

  `\Ruby\lib\ruby\gems\1.9.1\gems\sass-3.3.14\lib\sass\engine.rb`

  在这个文件里面 engine.rb，添加一行代码：

  ```rb
  Encoding.default_external = Encoding.find('utf-8')
  ```

1. 创建测试项目 'myproject'

  ```javascript
  compass create myproject --sass-dir "sass" --css-dir "css" --javascripts-dir "js" --images-dir "images"
  ```

1. 进入到 'myproject' 目录, 并打开 config.rb 文件。列出了常用的配置参数:

  ```javascript
  http_path = "/"
  css_dir = "css" #生成的css目录
  sass_dir = "sass" #需要编译的scss目录
  images_dir = "images" #需要编译的图片目录
  javascripts_dir = "js" #js目录,没什么用
  output_style = :expanded #(:nested、:compact和:compressed)
  line_comments = false;#是否生成注释
  #environment = :development  #（:production或者:development），智能判断编译模式。
  ```

1. 新建一个test.scss文件，然后输入以下命令就可以一边编译一边生成css文件了

  【ps:记得在当前窗口运行命令，否则会找不到 config.rb 配置文件】 

  ```javascript
  compass watch
  ``` 

  Linux, osx 用户可用这个脚本启用后台监听程序: [sass-watch](https://gist.github.com/965f3e1a0876592db33f)

1. scss语法[文档](http://sass-lang.com/documentation/file.SASS_REFERENCE.html) 
