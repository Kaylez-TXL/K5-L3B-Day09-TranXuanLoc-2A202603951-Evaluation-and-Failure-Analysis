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

## Eval set v2

[Baseline E-swarm-20261118-1d04](../src/reports/baseline_v2.json) gồm 28 item × 5 seed, SHA `1d047deb4902`. Build báo 0 lỗi. A/A với seed 66, 77, 88, 99, 110 cho Δ = 0 trên mọi metric. Cả hai lượt có pass_hat_5 = 0,8929. Sàn 0,80 nằm dưới mức A/A quan sát được. Với model thật, cần đo lại độ dao động trên nhiều lượt lặp trước khi chọn sàn.

`slice_regression` có pass rate tổng 0,9357 và chỉ bị chặn ở slice `money_movement` (sàn 0,95). Slice `adversarial` (sàn 0,85) chặn `seeded_regression`. `pass_hat_5` chặn `flaky_regression` cùng với pass_rate và money_movement. Lỗi hiếm chỉ xuất hiện ở khoảng một phần ba seed có thể lọt hoặc bị phát hiện thiếu ổn định với 5 seed.

Scorecard đạt 3/3, 0 báo nhầm. Gate chặn cả bốn trap công khai gồm NaN sau lỗi evaluator, bỏ dòng fail, chọn seed và nới spec. V1 đo detection 100% và false alarms 0% sau khi bổ sung grader số tiền đề xuất.
