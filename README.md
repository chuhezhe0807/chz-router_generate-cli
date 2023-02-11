# chz-router_generate-cli 

### 介绍 

chz-router_generate-cli 是一个JS编写的，根据你的 "config.json" 路由配置文件自动生成路由表的命令行工具。

读取你指定的 router-config.json 配置，从入口文件夹开始遍历所有 "config.json" 指向的文件，并根据 "config.json"中的身份生成不同的路由，并最终将路由表写入到你指定的输出文件中。

### 一、如何使用

1. $ npm install chz-router_generate-cli --dev
2. 在你执行 chz-router_generate 命令的目录下（一般是项目根目录）创建 router-config.json 文件，写入生成路由所需的配置。
3. 编写"config.json"文件，指定需要读取目录，生成路由的文件夹路径。
4. 执行 chz-router_generate 命令。

### 二、配置文件介绍 

- router-config.json

  router-config.json 是你需要在执行 chz-router_generate 命令的目录下创建的一个整体的配置文件，包含以下内容：

  ```json
  // 这是一个 router-config.json 文件示例（以下内容都是必选）
  {
    "entry": "./src/views/",					// 读取 config.json 文件 入口
    "entry_alias": "@/views",					// 入口路径的别名, 输出路由表时写component路径时使用
    "project_alias": "@/",					// 项目 src 路径的别名输出路由表时用于替换使用了项目根路径的路径
    "output": "./src/router/router-list.js",	// 输出路由配置文件路径
    "identities": ["sys", "teacher", "student"]	// 所有的身份，用于判断"config.json"文件中身份数组是否写错
  }
  ```

- config.json

  每一个 "config.json" 文件描述的是该文件所在文件夹下所有需要生成路由的信息。

  ```json
  // 这是一个 config.json 文件示例
  {
    "name": "home-component",		// 该 "config.json" 文件的名称，用于 "component" 父路由命名
    "component": "@/layout/basic-first-header.vue",  // "list" 列表中所有路由的父路由组件，该vue文件中需要有 <router-view /> 标签，用于显示子路由
    "exclude": ["mgr-organization-two", "user-student-one"],  // 子路由文件夹名称 某种情况下可能希望 "list" 中的路由组件不使用 "component" 指定的父路由组件，而是使用上一层级的路由组件（也就是说提升子路由层级，与父路由平级）
    "list": [
      {
        "name": "mgr-organization-one",		// 子路由文件夹名称 告诉代码需要解析哪一个文件夹，该文件夹下需要有 index.vue 文件来填充 "component" 指定 vue 文件的 <router-view /> 标签
        "meta": {								// 记录到路由表中的路由信息
          "icon": "iconfont xx-question",
          "title": "管理端组织机构一",
          "requireAuth": false
        },
        "identities": ["sys"],				// 确定哪些身份可以访问该文件夹下的路由
        "platform": "classes"					// 子路由属于哪一个平台 注意 只有解析入口的第一个 "config.json" 文件中有 "platform" 配置项生效，后续所有的子路由都会延续这个配置 不写为默认值
      },
      {
        "name": "mgr-organization-two",
        "meta": {
          "icon": "iconfont xx-question",
          "title": "管理端组织机构二",
          "requireAuth": true
        },
        "identities": ["sys"],
        "platform": "organization"
      },
      {
          "name": "mgr-organization-three",
          "meta": {
            "icon": "iconfont xx-question",
            "title": "管理端组织机构三",
            "requireAuth": true
          },
          "identities": ["sys"]
        },
      {
        "name": "user-teacher-one",
        "meta": {
          "icon": "iconfont xx-question",
          "title": "用户端教师一",
          "requireAuth": true
        },
        "identities": ["teacher"],
        "platform": "teaching"
      },
      {
        "name": "user-teacher-two",
        "meta": {
          "icon": "iconfont xx-question",
          "title": "用户端教师二",
          "requireAuth": true
        },
        "identities": ["sys", "teacher"],
        "platform": "arrangement"
      },
      {
        "name": "user-student-one",
        "meta": {
          "icon": "iconfont xx-question",
          "title": "用户端学生一",
          "requireAuth": true
        },
        "identities": ["student"],
        "platform": "studying"
      },
      {
        "name": "user-student-two",
        "meta": {
          "icon": "iconfont xx-question",
          "title": "用户端学生二",
          "requireAuth": true
        },
        "identities": ["sys", "student"]
      }
    ]
  }
  ```

### 三、生成路由项目目录参考 📑

```
├─App.vue
├─main.ts
├─views
|   ├─config.json								# 指定/views目录下所有需要解析的目录
|   ├─user-teacher-two
|   |        └index.vue
|   ├─user-teacher-one
|   |        └index.vue
|   ├─user-student-two
|   |        └index.vue
|   ├─user-student-one
|   |        └index.vue
|   ├─mgr-organization-two
|   |          ├─config.json					# 指定/views/mgr-organization-two 目录下需要解析的目录
|   |          ├─index.vue
|   |          ├─mgr-two-children
|   |          |        └index.vue
|   ├─mgr-organization-three
|   |           └index.vue
|   ├─mgr-organization-one
|   |          ├─config.json
|   |          ├─index.vue
|   |          ├─sub-aside-two
|   |          |       └index.vue
|   |          ├─sub-aside-three
|   |          |        └index.vue
|   |          ├─sub-aside-one
|   |          |       └index.vue
|   |          ├─sub-aside-four
|   |                  └index.vue
├─router
|   ├─index.ts
|   └router-list.js
├─layout
|   ├─404.vue
|   ├─basic-empty-route.vue
|   ├─basic-first-header.vue
|   └basic-sub-aside.vue
```



### 
