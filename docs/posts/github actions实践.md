---
title: github actions实践
date: 2021-09-07 09:11:51
permalink: /pages/b2d598/
categories:
  - 工具
tags:
  - github actions
  - ci
  - cd
---

![github](https://s2.loli.net/2022/05/31/B4JcMuPfEwxlVtN.png)

[GitHub Actions](https://docs.github.com/cn/actions) 是一个持续集成和持续交付(CI/CD)的平台，它允许你自动化构建、测试和部署管道。您可以创建工作流来构建和测试存储库中的每个pull请求，或者将合并的pull请求部署到生产中。

## 基本概念

### workflow

workflow 是指由多个步骤所组成的一个完整的工作流程，例如：安装依赖 > 项目打包 > 项目部署。

### name

工作流程的名称，此名称会在 actions 运行时显示在 actions 面板中。

```
# 例子
name: CI
```

### on

触发工作流程的事件，可以是一个，也可以是多个。此外，可指定触发事件的代码分支。

```
# 监听一个事件
on: push

# 监听多个事件
on: [push, pull_request]

# 监 main 分支的 push 事件
on:
  push:
    branches:
      - main
```

### jobs

任务，一个工作流由一个或多个任务组成，是一些列步骤的集合。

```
jobs:
  main: # 任务名称
    runs-on: ubuntu-latest # 运行环境
    steps: # 任务的步骤
    
    - name: Checkout # 步骤1
      uses: actions/checkout@v2 # 检出代码到工作流环境中
      with: # 参数配置
        persist-credentials: false
        
    - name: Install # 步骤2
      run: npm install
      
    - name: Build # 步骤3
      run: npm run build
      
    - name: Deploy # 步骤4
      uses: JamesIves/github-pages-deploy-action@releases/v3
      with:
        ACCESS_TOKEN: ${{ secrets.ACCESS_TOKEN }} # 项目环境参数。在项目的 settings > Security > secrets > Actions 中配置。
        BRANCH: gh-pages
        FOLDER: dist
    ...
```

## 市场

在 [Github Actions 市场](https://github.com/marketplace?category=&query=&type=actions&verification=) 中可以找到他人发布的 Actions 供自己使用。

## 常用模板

### Vue 项目上传并部署 pages

```
name: CI # 工作流名称
on:
  push: # 触发事件
    branches:
      - main # 指定分支

jobs:
  main:
    runs-on: ubuntu-latest # 运行环境
    steps:
    - name: Checkout
      uses: actions/checkout@v2 # 检出代码到运行环境
      with:
        persist-credentials: false # 是否使用本地git config配置令牌或SSH密钥
        
    # 安装依赖
    - name: Install
      run: npm install
    
    # 打包
    - name: Build
      run: npm run build
     
    # 部署 pages  
    - name: Deploy
      uses: JamesIves/github-pages-deploy-action@releases/v3
      with:
        ACCESS_TOKEN: ${{ secrets.ACCESS_TOKEN }} # 在项目中配置 ACCESS_TOKEN
        BRANCH: gh-pages
        FOLDER: dist
```

### 定时任务

```
name: CheckIn

# 环境变量
env:
  COOKIE: ${{ secrets.COOKIE }}
  PASS: ${{ secrets.PASS }}
  EMAIL: ${{ secrets.EMAIL }}
  SERVICE: ${{ secrets.SERVICE }}

on:
  # 手动触发
  workflow_dispatch:

  # 定时执行
  schedule:
    # 每天9点执行 时间格式 minute hour day month week 设置的时间是UTC 不是北京时间 需要+8
    - cron: "0 1 * * *"

jobs:
  start:
    # 运行环境为最新版的Ubuntu
    runs-on: ubuntu-latest

    steps:
      - uses:
          actions/checkout@v2
      
      # 安装node.js
      - name: Setup Node.js
        uses: actions/setup-node@v2
        with:
          node-version: "14"

      # 安装依赖
      - name: npm install
        run: npm install
        
      # 执行脚本
      - name: Start task
        run: node index.js
```
