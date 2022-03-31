# 新版husky实践

1. 安装husky
    `npm install -D husky @commitlint/cli @commitlint/config-conventional`

2. 在packgae.json中添加prepare脚本
    ```sh
    {
        "scripts": {
            "prepare": "husky install"
        }
    }
    ```
    prepare脚本会在npm install（不带参数）之后自动执行。也就是说当我们执行npm install安装完项目依赖后会执行 husky install命令，该命令会创建.husky/目录并指定该目录为git hooks所在的目录。

3. 添加git hooks->`pre-commit`

    `npx husky add .husky/pre-commit "npm run test"`

    运行完该命令后我们会看到.husky/目录下新增了一个名为pre-commit的shell脚本。也就是说在在执行git commit命令时会先执行pre-commit这个脚本。pre-commit脚本内容如下：

    ```sh
    #!/bin/sh
    . "$(dirname "$0")/_/husky.sh"
    
    npm run  test
    ```
    可以看到该脚本的功能就是执行npm run test这个命令

4. 添加git hooks->`commit-msg`
   
   `npx husk add .husky/commit-msg "npx --no-install commitlint --edit $1"`，windows创建不了就直接将下述内容放到`.husky/commit-msg`

    ```sh
    #!/bin/sh
    . "$(dirname "$0")/_/husky.sh"

    #--no-install 参数表示强制npx使用项目中node_modules目录中的commitlint包
    npx --no-install commitlint --edit $1
    ```

# git-cz规范提交，standard-version规范发布，changelog生成
参考：https://blog.csdn.net/weixin_39848007/article/details/112332246
