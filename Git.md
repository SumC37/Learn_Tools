---
tags:
  - permanent
  - Git
---

## 参考
[git-scm](https://git-scm.cn/)
[Pro Git](https://git-scm.cn/book/en/v2)
[Git教程@廖雪峰](https://liaoxuefeng.com/books/git/introduction/index.html)
[[Git备忘录]]

## 版本控制系统
版本控制系统分为集中式和分布式。
集中式：由中央服务器控制。有SVN、CVS等。
分布式：每个人的电脑都具有完整的版本库。
Git管理的是修改不是文件。
```mermaid
flowchart TD
    A[版本控制系统]
    
    subgraph B [集中式]
        direction LR
        B1[中央服务器]
        B2[客户端1] --> B1
        B3[客户端2] --> B1
        B4[客户端3] --> B1
    end
    
    subgraph C [分布式]
        direction LR
        C1[客户端1<br/>完整版本库]
        C2[客户端2<br/>完整版本库]
        C3[客户端3<br/>完整版本库]
        C1 <--> C2 <--> C3
    end
    
    A --> B
    A --> C
    
    B --> D[SVN, CVS<br/>中央服务器存储所有历史]
    C --> E[Git<br/>每个节点都是完整仓库]
```

## Git工作流
```mermaid
flowchart TD
    subgraph A [工作区]
        A1[修改文件]
        A2[新建文件]
        A3[删除文件]
    end
    
    A -- git add --> B[暂存区<br>Stage/Index<br>临时存储区]
    
    subgraph B_Details [暂存区功能]
        B1[记录文件快照]
        B2[准备下次提交]
        B3[文件状态跟踪]
    end
    
    B --> B_Details
    
    B -- git commit --> C[本地版本库<br>.git 目录]
    
    subgraph C_Details [版本库内容]
        C1[提交对象<br>Commit Objects]
        C2[树对象<br>Tree Objects]
        C3[数据对象<br>Blob Objects]
        C4[引用<br>References]
    end
    
    C --> C_Details
    
    C_Details --> D[分支指针<br>HEAD -> master]
    
    E[远程仓库<br>Remote] <-- git push/pull --> C
```

## 分支管理
分支就是指针
HEAD指针指向当前分支指针

```mermaid
gitGraph
    commit id: "项目初始化"
    branch master
    checkout master
    commit id: "基础代码"
    
    branch dev
    checkout dev
    commit id: "开发环境搭建"
    
    branch feature1
    checkout feature1
    commit id: "功能1开发"
    commit id: "功能1测试"
    
    checkout dev
    branch feature2
    checkout feature2
    commit id: "功能2开发"
    commit id: "功能2测试"
    
    checkout dev
    merge feature1 id: "合并功能1"
    commit id: "集成测试1"
    
    checkout master
    commit id: "生产修复"
    
    checkout dev
    merge feature2 id: "合并功能2"
    commit id: "集成测试2"
    
    checkout master
    merge dev id: "发布版本"
    commit id: "版本更新"
```


## Fork-Pull Request 工作流
```mermaid
flowchart TD
    U[原始作者仓库<br>upstream/master]
    
    subgraph F [你的Fork副本仓库]
        F_M[你的master分支]
    end
    
    subgraph L [你的本地电脑]
        L_U[本地跟踪分支<br>upstream/master]
        L_F[本地工作分支<br>feature]
    end
    
    U -- 你手动Pull/Fetch同步 --> L_U
    L_U -- 你合并到本地master --> F_M
    F_M <-- 你推送Push --> L_F
    L_F -- 用于发起Pull Request --> U
```
> [!warning]
> 不要将账号、密码等隐私信息提交、上传、推送到版本库

