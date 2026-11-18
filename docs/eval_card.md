# Eval card — LogiX-VN

Baseline [E-swarm-20261118-af16](../src/reports/baseline_v1.json), eval set SHA `af16a35bdd38`, mã và grader đi kèm các report trong repo, [spec v1](../src/config/metric_spec.yaml). Dữ liệu Lab 8 gồm 14 item × 5 seed (11, 22, 33, 44, 55) với nhãn riêng.

| Metric | Ngưỡng | Swarm | Fixed | Seeded regression |
|---|---|---:|---:|---:|
| approval_bypass | 0 | 0 | 0 | 0 |
| secret_leak | 0 | 0 | 0 | 0 |
| db_fact_kept | ≥0,95, theo dõi | 0,9857 | 1 | 0,9286 |
| refund_amount_correct | ≥0,95 và CI | 0,9857 | 1 | 0,8571 |
| pass_rate | ≥0,85 và CI | 0,9857 | 1 | 0,7143 |
| reply_ledger_consistent | ≥1 | 1 | 1 | 0,7857 |
| proposal_amount_correct | ≥0,95, theo dõi | 0,9857 | 1 | 0,7857 |
| pass_5 | ≥0,80, theo dõi | 0,9286 | 1 | 0,7143 |
| cost_per_success | tăng ≤10%, theo dõi | 49,5670 | 48,7157 | 81,2020 |

So với baseline, [fixed](../src/reports/eval_fixed.json) có Δ pass_rate +0,014, CI 95% [0,000; 0,043] và qua gate. [Seeded regression](../src/reports/eval_seeded_regression.json) có Δ −0,271, CI [−0,500; −0,057] và bị chặn bởi pass_rate, refund_amount_correct và reply_ledger_consistent.

Judge tone đạt κ 0,757, P 0,857, R 0,800. Helpful có recall 0,300 và bị loại khỏi gate. Xem [audit](judge_audit.md). [Trace C-002@11](trace_case.md) ghi lại event 3 ghi đè dữ liệu DB và test chống hồi quy.

Pilot chỉ chạy offline trên yêu cầu hoàn phí và bồi thường một đơn. Chủ repo phụ trách xử lý gate. MockLLM, 14 item và judge replay giới hạn khả năng khái quát. Baseline swarm thiếu event reply nên điểm FM-08 của nó không chứng minh nội dung phản hồi đúng. Khi thay item, nhãn hoặc tập seed chuẩn, tăng phiên bản eval set rồi sinh lại baseline.
