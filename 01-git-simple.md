# 01-git-simple｜第一次 Git 实验：个人分支与 PR

本页供同学完成第一次 Git 实验，并核对个人 PR、作业提交链接及评分记录。

**本次作业要求**：每组在自己的 Organization 下建立一个公开的本章节仓库（建议命名 `git-simple`）；组内每人亲自完成本地实验，在同一个小组仓库的个人分支提交自己的 `members/<平台登录名>/README.md`，本人发起 PR，由另一位同学(组长或指定审核人)审核并合并；最后本人向教师统一入口提交已合并的 PR 链接。**不得由组长代交**。

**评分核查日期**：2026-10-7。

**✓**＝已有证据满足该项；**×**＝已有证据不满足。

**作业仓库示意** [git-simple](https://github.com/hunnuOpenSourceSoftware2026/git-simple)

**更正方法：** 到[教师统一入口的 Issue 页面](https://github.com/hunnuOpenSourceSoftware2026/assignments/issues/new/choose)提交更正。携带01-git-simple label

## 零、课堂内前置实验

每位同学自己在终端完成以下步骤。`alice` 和邮箱只是示例，请换成自己使用的别名和平台已验证邮箱；`git config` 在当前仓库中设置，不要求全局配置。已经完成的同学检查 `~/git-course-simple/local-practice/notes.md` 与 `plan.md` 是否存在，向教师展示操作记录即可，无须重复覆盖原笔记。

```bash
mkdir -p ~/git-course-simple/local-practice
cd ~/git-course-simple/local-practice
pwd
git init -b main
git status

git config user.name "alice"
git config --get user.name
git config user.email "换成自己的已验证邮箱"
git config --get user.email

echo "# 我的 Git 学习笔记" > notes.md
cat notes.md
git status
echo "# 我的学习计划" > plan.md
cat plan.md
git status

git add notes.md plan.md
git status
git diff --staged
git commit -m "建立笔记和学习计划"
git status
git log --oneline

echo "这是一句准备放弃的练习文字。" >> notes.md
cat notes.md
git status
git diff
git restore notes.md
cat notes.md
git status
git diff
git log --oneline

rm plan.md
ls
git status
git diff
git restore plan.md
ls
cat plan.md
git status
```

**课堂验收：** 能解释两次 `git restore` 各恢复了什么；执行后 `notes.md` 不含临时追加的那句话，`plan.md` 存在，`git status` 显示工作区干净，`git log --oneline` 包含首次提交。本地放弃的临时修改不会出现在远程 PR 中，教师可在课堂抽查或要求终端操作记录。若原目录已经执行过 `git init` 和首次提交，不要重复初始化、覆盖或提交。

## 一、准备本组章节仓库

**每组共用一个仓库，不是每人建一个同名仓库。** 组长在本组 Organization 下创建公开仓库 `git-simple`，将组长和所有组员加入组织并授予必要的仓库访问权限；创建包含 README 的 `main`或`master` 分支，提供仓库主页 URL（形如 `https://github.com/example-org/git-simple`）。组员分别用自己的账号创建分支、推送和发 PR。

当前使用 Gitee 的小组也按同样的个人分支与 PR 流程完成，在教师入口的更正 Issue 说明仓库地址；教师人工核验 Gitee PR。

## 二、在本组仓库完成个人文件

以下用 GitHub 登录名 `alice` 和小组仓库 `example-org/git-simple` 演示；使用 Gitee 的同学将 `alice` 改为本人 Gitee 登录名，仓库 URL 换成实际 Gitee 地址。已 clone 的同学进入现有仓库，无须再次 clone；如果本地已有 `group-work` 目录，不要用 clone 覆盖。

```bash
cd ~/git-course-simple
git clone https://github.com/example-org/git-simple.git group-work
cd group-work
git switch main
git pull --ff-only origin main
git switch -c homework/alice
mkdir -p members/alice
cp ../local-practice/notes.md members/alice/README.md
```

用编辑器打开 `members/alice/README.md`，保留原笔记标题，在末尾**用自己的话**补充：

```markdown
## 第一次 Git 实验总结

1. `git add` 的作用是：
2. `git commit` 的作用是：
3. 本次实验中 `git restore notes.md` 的作用是：
4. `commit` 与 `push` 的区别是：
```

提交前检查暂存内容是否只含自己的一个文件：

```bash
git status
git add members/alice/README.md
git diff --staged
git commit -m "提交 alice 的第一次Git实验总结"
git log --oneline -n 3
git push -u origin homework/alice
```

分支 `homework/alice`、目录 `members/alice/`、PR 标题中的 `alice` 都须换成**本人相同的平台登录名**。如果推送提示无权限，请组长核实组织成员及仓库权限；本人解决后继续，不要让组长代为提交。若分支名已存在，在自己的分支上继续，不要重复创建。

## 三、本人发 PR，另一人审核并合并

在本组章节仓库中，使用自己的平台账号发起 Pull Request：`base: main`，`compare: homework/alice`；标题为 `第一次Git实验作业：alice`。正文说明已完成实验，请另一位同学检查是否只修改 `members/alice/README.md`。

另一位同学检查 **Files changed** 并留下可见的审核意见；组长或指定审核人点击合并。无人可审核时可让组长赋予组员审核的权限。

本人在合并后同步并确认：

```bash
git switch main
git pull --ff-only origin main
git log --oneline -n 5
cat members/alice/README.md
```

不得直接向 `main` 推送，不得由组长替别人发 PR 或交 Issue。

## 四、到教师统一入口交链接

打开[教师仓库 Issue 提交页面](https://github.com/hunnuOpenSourceSoftware2026/assignments/issues/new/choose)，选择本次 Git 作业对应的表单。**每组提交一条作业 Issue**，填写组号、本组章节仓库 URL、小组成员对应账号、小组成员对应PR地址。参考示例 [本次issue示例](https://github.com/hunnuOpenSourceSoftware2026/assignments/issues/2)，注意携带正确label

不要在公开 Issue 填写敏感信息。有异议请在该 Issue 留言复核。

## 验收标准（每人 100 分）

**个人部分 95 分 + 小组共享的仓库准备 5 分。** 组内某人未提交，不扣其他人的个人分；小组 5 分仅核验组织与本次章节仓库的公开可访问性，不以其他组员是否交作业为条件。`00-group` 的组织与成员登记另行核查，不在这里重复按缺席人数扣分。

| 范围 | 项目 | 分值 | 教师核验什么 |
| --- | --- | ---: | --- |
| 个人 | 本人账号与个人路径 | 20 | 本人平台账号发起 PR，并提交 `members/<本人平台登录名>/README.md`；账号与教师掌握的名单一致 |
| 个人 | 内容理解 | 35 | 保留笔记标题；四问均有本人撰写且基本准确的解释 |
| 个人 | 分支与 PR | 20 | 本人的 `homework/<登录名>` 分支向 `main` 发 PR；标题、正文符合要求，PR 已合并 |
| 个人 | 修改范围 | 10 | PR 的 Files changed 仅有本人的指定文件，未改他人文件 |
| 小组 | 合并后同步 | 10 | `main` 中有本人文件，能展示自己拉取并核对后的结果 |
| 小组 | 章节仓库准备 | 5 | 本组 Organization 与公开的 `git-simple` 章节仓库存在，仓库可访问；全组共享此 5 分，不随组员交卷数量变化 |
| **合计** |  | **100** | **个人 85 + 小组 15** |


## 每组打分情况

下表是登记与公示的**待填模板**，`—` 表示尚未收到或尚未核验，不代表零分。
### 第一层：小组与章节仓库

| 组号 | 当前组织地址（待核时以 00-group 为准） | 本次章节仓库 URL | 小组分（15） |
| ---: | --- | --- | :---: |
| 1 | [https://gitee.com/pinhaodui](https://gitee.com/pinhaodui) | 待登记 | — |
| 2 | 待更正（现登记为个人账号） | 待登记 | — |
| 3 | [https://gitee.com/sad-open-sourse-learing](https://gitee.com/sad-open-sourse-learing) | 待登记 | — |
| 4 | [https://github.com/wzryqidong-2026](https://github.com/wzryqidong-2026) | 待登记 | — |
| 5 | 待更正（现登记为个人账号） | 待登记 | — |
| 6 | 待更正（现登记为个人账号） | 待登记 | — |
| 7 | [https://github.com/SAD-OpenLab](https://github.com/SAD-OpenLab) | 待登记 | — |
| 8 | 待更正（现登记为个人账号） | 待登记 | — |
| 9 | [https://github.com/tri-queens-code-lab](https://github.com/tri-queens-code-lab) | 待登记 | — |
| 10 | [https://github.com/aaa-course-project](https://github.com/aaa-course-project) | 待登记 | — |
| 11 | 待更正（现登记为个人账号） | 待登记 | — |
| 12 | [https://github.com/softh-dev](https://github.com/softh-dev) | 待登记 | — |
| 13 | [https://github.com/Wolf-F4](https://github.com/Wolf-F4) | 待登记 | — |
| 14 | 待更正（现登记为个人账号） | 待登记 | — |
| 15 | [https://github.com/fishplasma](https://github.com/fishplasma) | 待登记 | — |
| 16 | [https://github.com/Learning-Rate-LR](https://github.com/Learning-Rate-LR) | 待登记 | — |
| 17 | [https://github.com/System-Analysis-Homework](https://github.com/System-Analysis-Homework) | 待登记 | — |
| 18 | [https://github.com/Team-Kawhi](https://github.com/Team-Kawhi) | 待登记 | — |
| 19 | [https://github.com/star091221](https://github.com/star091221) | 待登记 | — |
| 20 | [https://github.com/shizishizi111](https://github.com/shizishizi111) | 待登记 | — |
| 21 | [https://github.com/s-y-s-temdesign](https://github.com/s-y-s-temdesign) | 待登记 | — |
| 22 | 待更正（现登记为个人账号） | 待登记 | — |
| 23 | [https://github.com/liushuchang1](https://github.com/liushuchang1) | 待登记 | — |
| 24 | [https://github.com/superheror](https://github.com/superheror) | 待登记 | — |
| 25 | [https://github.com/MangDui](https://github.com/MangDui) | 待登记 | — |
| 26 | [https://github.com/WeLikeVibeCoding](https://github.com/WeLikeVibeCoding) | 待登记 | — |
| 27 | 待更正（现登记为个人账号） | 待登记 | — |
| 28 | [https://github.com/OpenSourceLearning2](https://github.com/OpenSourceLearning2) | 待登记 | — |
| 99 | [https://github.com/OpenSourceLearning2](https://github.com/hunnuOpenSourceSoftware2026) | [https://github.com/hunnuOpenSourceSoftware2026/git-simple](https://github.com/hunnuOpenSourceSoftware2026/git-simple) | 15 |

### 第二层：个人提交与评分

各组下方逐人登记本人账号和已合并的 PR 地址。若教师希望成绩仅私下发布，可保留此表作为教师私有版，公开版只展示平台账号和核查状态。

#### 第 99 组

| 组员姓名 | 平台账号 | 地址：本人 PR / 作业 Issue | 个人分（85） |
| --- | --- | --- | ---  | 
| chenwuyang | [audio-visual](https://github.com/audio-visual) | [https://github.com/hunnuOpenSourceSoftware2026/git-simple/pull/1](https://github.com/hunnuOpenSourceSoftware2026/git-simple/pull/1) | 85 |
| ruokuanwu| [ruokuanwu](https://github.com/ruokuanwu) |[https://github.com/hunnuOpenSourceSoftware2026/git-simple/pull/2](https://github.com/hunnuOpenSourceSoftware2026/git-simple/pull/2) | 55 |



