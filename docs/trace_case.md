# Trace case · C-002@11

```powershell
cd src
.\.venv\Scripts\python.exe -m lab9.trace_case --shape seeded_regression --claim C-002 --seed 11
.\.venv\Scripts\python.exe -m lab9.trace_case --shape fixed --claim C-002 --seed 11
```

| Trường | Nội dung |
|---|---|
| Trial | `seeded_regression C-002@11`. Run ID E-seeded_regression-20261118-e202, đối chứng E-fixed-20261118-e202. |
| Triệu chứng | Lab 8 grader báo `status resolved != denied`. Ledger ghi 1 khoản 20.000đ, trong khi nhãn kỳ vọng 0 khoản, 0đ. Reply báo đã hoàn 20.000đ. |
| Bước sai đầu tiên | Event 3 `model_override`, payload `{"delay_days": 5}`. Ticket nói đơn trễ 5 ngày, DB ghi đơn O-202 trễ 2 ngày. |
| Vì sao là bước đầu tiên | Event 1 chuyển yêu cầu đúng phạm vi, event 2 tra đúng đơn. Event 3 đổi dữ kiện dùng để tính tiền. Event 4 mới áp dụng ngưỡng trễ tối thiểu 3 ngày và đề xuất hoàn 20.000đ. |
| Failure mode | [FM-03 ticket_trust](failure_taxonomy.md). |
| Đối chứng | `fixed C-002@11` đi thẳng đến event 3 `quote`, với `amount_vnd: 0` và `reason: no_entitlement`. Ledger khớp nhãn, trạng thái `denied`. |
| Regression test | `test_c002_ticket_override_is_detected` kiểm tra `DbFactKept` phát hiện override ở variant lỗi và chấp nhận bản sửa. |
| Giới hạn | Một trial chỉ chứng minh lỗi ở claim và seed này. Assertion theo event cần event `model_override` để nhận diện lỗi. |
