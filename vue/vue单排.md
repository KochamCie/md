### installation

1. 安装node.js     node -v , npm -v
2. 安装淘宝镜像 
   - npm install -g cnpm --registry=[https://registry.npm.taobao.org](https://link.jianshu.com/?t=https://registry.npm.taobao.org/)
   - cnpm -v   不行就配置环境变量
3. 安装webpack
   - npm install webpack -g
   - 当前项目npm install webpack --save-dev
   - webpack后提示npm install webpack-cli -D
   - 然后npm i -g webpack-cli -D --save
   - webpack -v
4. 安装vue-cli
   - npm install vue-cli -g
   - vue -V

### init

1. 新建一个文件夹并cd进入

2. vue init webpack manager      // manager为项目名

   - install vue-router			yes
   - use ESLint to lint your code     no
   - setup unit tests....               yes
   - setup e2e....      yes 
   - to get start
     - cd manager
     - npm install
     - npm run dev

   ​

3. 安装vue路由模块 vue-router和网络请求模块vue-resource，页面跳转

   - cnpm install vue-router vue-resource --save

4. 后端请求交互

   - npm install --save axios

5. cnpm i element-ui -S

6. ​

   ​

7. 如果npm install 卡在chromedriver ，则使用淘宝

   - ```shell
     npm install chromedriver --chromedriver_cdnurl=http://cdn.npm.taobao.org/dist/chromedriver
     ```

