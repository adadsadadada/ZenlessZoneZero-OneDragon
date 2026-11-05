# 上游备份与每日安全同步

- 本仓库：[adadsadadada/ZenlessZoneZero-OneDragon](https://github.com/adadsadadada/ZenlessZoneZero-OneDragon)，GitHub Fork parent 必须为 [OneDragon-Anything/ZenlessZoneZero-OneDragon](https://github.com/OneDragon-Anything/ZenlessZoneZero-OneDragon)。
- 固定版本：[snapshot-2026-11-05](https://github.com/adadsadadada/ZenlessZoneZero-OneDragon/tree/snapshot-2026-11-05)；分支和同名标签都固定在 8da39a457384311066118282e756fd754b5999e0。
- 快照分支和标签受 active ruleset 保护，禁止更新和删除，无 bypass；同步从不写入这两个 ref。
- main 保留上游代码及本仓备份配置；upstream-checkpoint 只记录已安全合并的上游提交。二者禁止强制改写历史和删除。

工作流 .github/workflows/safe-upstream-sync.yml 每天 UTC 19:17（北京时间次日 03:17）检查，也可手动 workflow_dispatch。GitHub schedule 可能延迟，不保证精确到分钟。

先验证仓库数字 ID、Fork parent、上游可访问且未归档、分支名、历史连续性和快照 SHA，再调用 GitHub 普通合并 API。发生冲突、上游删除/不可访问/改写历史或身份变化时立即失败，不使用强制同步、强推或覆盖式重置。

上游 .github/workflows 或本仓保留的备份配置路径发生变化时，先停止并要求人工审查；原有上游工作流保持禁用。同步无需 checkout，不安装依赖，不执行任何上游构建、脚本、Action 或项目代码。

默认无令牌权限。同步 job 仅 contents:write；失败报告 job 仅 issues:write，在本仓创建去重告警 Issue，并保留失败的 Actions 日志和 summary。解决原因后手动重新运行验证，可关闭告警。每日检查会继续，未解决的问题不会触发强制覆盖。

每 28 天用 contents API 更新一次 .github/upstream-backup-audit.json 检查记录，以保持仓库活动，避免 GitHub 因公开仓库 60 天无活动而停用 schedule。该记录只在 main 更新，不涉及固定版本。若 GitHub 账户或平台另外禁用 Actions，需由账户所有者恢复。

上游后续代码仍需使用者自行审查后运行。此备份配置不改变上游程序行为。未创建 ChatGPT 提醒任务。
