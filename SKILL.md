---
name: bilibili-submission-draft-check-free
description: 使用用户指定的 Easy WebBridge browserId，只读检查本人已打开的 B站投稿草稿及当前可见栏目，整理封面、标题、简介、分区、素材的缺项和修改建议，并逐条保留页面可见来源。适合发布前快速核对；账号、稿件归属或页面不能确认时立即停止，不保存、不提交、不发布，缺料如实标注。
---

# B站投稿草稿缺项检查

在不改动稿件的前提下，给出一份能直接人工处理的草稿缺项清单：封面、标题、简介、分区、素材状态、修改建议和对应的可见页面来源。

## 输入

- 用户明确指定的 `browserId`，以及用户本人已打开的 B站投稿草稿页面。
- 可选的本次页面范围说明；一次最多核对 100 行素材或表格。超出时要求用户分批，不能声称已经读完。

## 结果

按字段输出“已见内容 / 缺项 / 修改建议 / 来源”。每个字段都必须绑定至少一条来源，且来源包含非空的 `field`、可见位置和所见文字；不可确认的字段写为“未确认”，不能猜测或套用样例。素材数组及任何 `tables` 表格均最多 100 行。

## 前置依赖：Easy WebBridge

安装并连接 Easy WebBridge 后再运行。GitHub 主下载地址：<https://github.com/xxjrq/easy-webbridge>；Gitee 备用地址：<https://gitee.com/xxjrq/easy-webbridge>。在浏览器扩展页加载仓库中的 `extension/`，确认目标环境已连接并由用户提供 `browserId`。本 Skill 每次只操作一个指定环境，绝不广播到全部账号。

## 执行流程（默认只读）

1. 先阅读本机 Easy WebBridge CLI 的帮助，确认参数：

   ```bash
   node /path/to/easy-webbridge/cli/easy-webbridge.mjs --help
   ```

2. 仅用用户提供的 `browserId` 列出标签页，定位用户已经打开的草稿。不得新建或接管无关页面；不要公开 `browserId`、session 或本机路径。

   ```bash
   node /path/to/easy-webbridge/cli/easy-webbridge.mjs command <browserId> list_tabs '{}'
   ```

3. 读取页面前先确认账号显示信息、草稿标题或其他归属线索。如果账号、稿件归属或当前页面无法确认，停止并说明原因。登录、验证码、风控或安全验证出现时暂停，不绕过。

4. 对确认的草稿页先做文本快照；快照必须显式带 `--text-mode page`。每次导航、刷新或切换页面后重新获取 refs，不能复用旧 refs。

   ```bash
   node /path/to/easy-webbridge/cli/easy-webbridge.mjs snapshot <browserId> <tabId> --text-mode page --compact
   ```

5. 只读取可见表单内容与栏目：封面预览状态、标题、简介、分区、已列素材及字段提示。记录每条资料的可见位置和所见文本；不读取隐藏数据、不调用非页面可见接口。

6. 输出检查表并结束任务。全程只读：不填写、不保存、不提交、不发布，避免平台自动保存造成稿件改动。

## 任务分组与边界

若确需打开对照页，使用 `<skill-slug>-<run-id>` 的单一 session 和单一标签组，只复用本任务的组；结束前仅关闭本 Skill 创建且所有权已核对的 session，绝不关闭用户原有页面。对用户已打开的草稿只借用、只读，不将它登记为可关闭资产。

## 离线自测

`fixtures/` 是明确标注的离线页面资料模拟，绝非在线采集。自测验证结构化检查器的成功与安全停止分支，不验证浏览器登录或真实页面：

```bash
node scripts/self-test.mjs
```

## 参考来源

业务用途与元数据检查问题的参考：<https://github.com/social-media-skills/skills> 和 <https://github.com/social-media-skills/skills/blob/main/skills/youtube-publishing-and-metadata/SKILL.md>。本 Skill 的说明、脚本和示例均为原创；未复制其实现或文本。
