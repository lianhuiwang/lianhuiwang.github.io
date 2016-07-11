The source of [http://lianhuiwang.github.io](http://lianhuiwang.github.io).

Powered by [hexo](https://hexo.io/zh-cn/). Themed by [next](https://github.com/iissnan/hexo-theme-next).


### INSTALL GUIDE

#### Install nodejs
Follow [nodejs](https://nodejs.org/en/) docs.
In China, use <https://npm.taobao.org/> to speed up.

#### Install hexo
    npm install -g hexo-cli
See more information at [hexo docs](https://hexo.io/docs/).

#### Clone this branch
    git clone -b source https://github.com/lianhuiwang/lianhuiwang.github.io.git

#### Install node modules
    cd lianhuiwang.github.io
    npm install

#### Support search
    npm install hexo-generator-search --save

#### Support RSS
    npm install hexo-generator-feed --save

#### Support SiteMap
    npm install hexo-generator-sitemap --save

#### Support comments
Register duoshuo's shortname for it.
Set doushuo_shortname in ./themes/next/_config.yml

#### Generate pages
    hexo g

#### Browse the pages
    hexo s
See http://localhost:4000/ .

### Install deployer-git 
    npm install hexo-deployer-git --save
    
### Deploy

