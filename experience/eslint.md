# 优化eslint中debugger设置问题

## 一、需求

	本地开发时，由于不止一名开发人员需要在代码写入`debugger`，提交代码时，代码中不要有`debugger`，以免影响其他开发人员调试。

## 二、技术方案

### eslint：

	关闭`eslint`自动`fix debugger`错误功能；
	
	开启命令行检查`debugger`错误功能；



### git：

	在`git commit`时调用eslint检查`debugger`，抛出错误，停止`commit`。

## 三、相关插件

vscode：关闭`debugger`自动fix功能；

eslint：全局安装`eslint`支持命令行模式；

husky：`githook`，代码提交时，挂载`eslint`命令行；

## 四、详细配置

### 全局安装以下插件：

```javascript
npm install -g 
eslint@4.15.0
eslint-plugin-vue@4.0.0
eslint-config-standard@10.2.1
babel-eslint@8.2.1
eslint-plugin-import@2.7.0
eslint-plugin-node@5.2.0
eslint-plugin-promise@3.4.0
eslint-plugin-standard@3.0.1
```

### 配置.vscode/settings.json：

```javascript
"eslint.validate": [
    "javascript",
    "javascriptreact",
    {
      "language": "html",
      "autoFix": true
    },
    {
      "language": "vue",
      "autoFix": true
    }
  ],
  "eslint.options": {
    "rules": {
      "no-debugger": 0
    }
  }
```

### 配置package.json：

```javascript
"lint": "eslint ./src/**/*.js ./src/**/*.vue no-debugger:2",
"precommit": "npm run lint"

"devDependencies": {
    "husky": "^1.2.1"
  }
```

### 配置.eslintrc.js：

```javascript
rules: {
  'no-debugger': process.env.NODE_ENV === 'dev' ? 0 : 2
}
```

	配置成功后，当代码中有`debugger`，执行`git commit`控制台会报错，需要手工清除掉`debugger`后再提交。
