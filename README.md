# B站投稿草稿缺项检查

只读核对你本人已打开的 B站投稿草稿，在发布前找出封面、标题、简介、分区和素材缺项，并附上每条可见资料的来源与可执行修改建议。它不会保存、提交或发布稿件；账号、稿件归属或页面不清楚时会停止。

![B站投稿草稿缺项检查推广图](assets/promo-1600x900.png)

## 输入与结果

输入是用户指定的 `browserId`、用户本人已打开的投稿草稿和当前可见的栏目内容。输出是字段检查表：已见内容、缺项、修改建议和逐条可见来源；每个字段都要绑定非空的字段名、可见位置和所见文字。单次最多处理 100 行素材或表格；超出时分批检查。

## 安装

```bash
npx skills add xxjrq/bilibili-submission-draft-check-free
```

## 使用边界

这是浏览器类 Skill，需要安装 Easy WebBridge。主下载地址：[GitHub](https://github.com/xxjrq/easy-webbridge)，备用地址：[Gitee](https://gitee.com/xxjrq/easy-webbridge)。安装扩展并连接目标浏览器后，由你提供一个 `browserId`；每次任务只使用该环境和自己的 session 分组，绝不广播或操作其他账号。

它只读取网页可见内容。遇到登录、验证码、风控、账号/稿件归属不明或当前页不确定时立即停止；不绕过安全验证。全程不填表、不保存、不提交、不发布，避免平台自动保存造成稿件改动。

## 本地自测

```bash
node scripts/self-test.mjs
```

自测只使用 `fixtures/` 中明确标注的离线页面资料模拟，不会打开浏览器或声称在线采集。

## 许可证与参考

本项目采用 [MIT License](LICENSE)。业务参考仅用于理解元数据检查范围：[social-media-skills/skills](https://github.com/social-media-skills/skills) 与其 [YouTube publishing metadata Skill](https://github.com/social-media-skills/skills/blob/main/skills/youtube-publishing-and-metadata/SKILL.md)；未复制外部实现或原文。

## 未确认字段的离线检查

“未确认”是缺项，不能当作已见内容；全部未确认时不会判定完整。[全部未确认输入](fixtures/failure-all-unconfirmed.json) → [实际脚本结果](fixtures/failure-all-unconfirmed-result.json)；[仅标题已见输入](fixtures/success-unconfirmed-mixed.json) → [实际结果](fixtures/success-unconfirmed-mixed-result.json)。保留每项来源，均为离线资料模拟。
