# Git 工作流规范

> 项目分支规范说明.

## feature 分支

用于单一任务的开发分支，命名规范：feature/版本号/任务 id（如：feature/v3.1.0/6368）
版本号：v + 具体版本号
任务 id：对应 tfs 上的单一任务号，参考下图

## dev 分支

用于某一版本的开发分支，内容由该版本的 feature 分支合并得到
命名规范：dev/版本号（如：dev/v3.1.0）
版本号：v + 具体版本号

## test 分支

用于某一版本的测试分支，提测时由 dev 分支开出
命名规范：test/版本号（如：test/v3.1.0）
版本号：v + 具体版本号

## release 分支

用于某一版本的预发布分支，封版时由 test 分支开出
命名规范：release/版本号（如：release/v3.1.0）
版本号：v + 具体版本号

## hotfix 分支

用于某一版本的紧急修复分支，修复时由 master 分支开出
命名规范：hotfix/版本号（如：hotfix/v3.1.0）
版本号：v + 具体版本号

## master 分支

线上版本分支，版本上线后，版本负责人合并版本的预发布分支到 master 分支
提测/封版/hotfix 规范
提测/封版/hotfix 需通过邮件提出，邮件内容有仓库说明，提测/封版/hotfix 标签说明，提测/封版/hotfix 内容说明
提测/封版/hotfix 标签命名规范：test（release、hotfix）-v 版本号-yyyyMMddHHmm
