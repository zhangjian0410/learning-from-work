# 保存、重复处理与候选状态

只在需要持久记录、可靠重跑或应用候选时使用；普通对话复盘不要求先填写完整表格。已有业务格式优先，下面是没有专用合同的默认形式。

## 记录身份和增量

首次保存分配稳定 `task_ref`、`record_id` 和观察/候选 ID。后续先查旧记录，同一主张沿用 ID；相同来源版本及相同判断只返回原记录。观察时间更新不构成新事件，文件变新也不自动表示知识生效。

来源新增、更正、现行做法变化或判断修订时追加不可变版本，注明 `supersedes`、变化原因和受影响项，保留旧版和旧失败。文件可用 `closeout-v001.md`、`sources-v001.json`，或沿现有系统的版本机制。再次运行无法访问先前记录时明确“本次未验证跨次去重”。

普通任务的记录包含：

- `task_ref`、`record_id`、`version`、真实 `created_at`、`supersedes`。
- `scope`：实际材料、读取范围、允许保存位置；`independent_task_count` 按真实任务计数。
- `existing_record_refs`：原复盘或业务记录；`source_manifest`：本版来源清单或内嵌来源表。
- 每条观察的 `id`、`event_refs`、`claim`、`kind`（fact/inference/proposal）、`source_refs`、`applicability`、`limits`、`disposition`、`authority_refs`、`next_action` 和 `promotion`。

用户已有表达不必为匹配字段重复翻译；保持上述含义可查即可。`authority_refs` 可以明确“未提供/未访问”，不能空缺却声称已完成对照。

## 来源清单

保存来源路径或稳定链接、实际读取位置、读取时间、原事件 ID 和可取得的版本。文件型可靠重跑默认使用 JSON 对象 `{task_ref, captured_at, files}`；每项含 `id`、`path`、`locator`、`sha256`、`digest_scope`（full_file/excerpt）、`role`（event_evidence/current_authority）和可用的 `event_id`。

SHA-256 取实际文件字节或实际读到片段的 UTF-8 字节；片段须标范围与编码。只算了全文件摘要却仅读片段时，两种范围分列。聊天、云文档或当前工具无法取得可验证版本时写 unavailable，保留消息/页面定位；这种记录可支持有限复盘，但不能宣称具备文件级身份或可安全自动覆盖目标。

重复处理逐项比较事件/任务身份、定位、版本及判断。报告与原文对同一事件的转述不另计独立证据。旧来源不可用或版本冲突时保留缺口，暂停依赖该来源的应用。

## 晋升与复用

`promotion` 可用 not_requested/proposed/authorized/applied/reuse_verified；每次变化追加版本并引用实际依据。

- proposed：准确目标与版本、建议差异、证据、适用范围、验证与回退方法。
- authorized：覆盖该目标及差异的真实批准；更早材料中的批准只记录其原范围。
- applied：写前/写后版本、实际动作与验证回执；不能以“已有覆盖”冒充本轮晋升。
- reuse_verified：后续独立任务身份、实际采用行为和观察结果；仅文件存在、再次读到或合成测试不够。

源更新、应用与候选关闭都保留原记录，不删除证据。写入前后复核版本；发现并发变化就保留当前结果并处理冲突，不覆盖别人的修改。恢复从既有输出与回执判断已完成步骤，不重建 ID 或重写不可变版本。无法满足目标自身的可靠写入要求时停在可审阅差异。
