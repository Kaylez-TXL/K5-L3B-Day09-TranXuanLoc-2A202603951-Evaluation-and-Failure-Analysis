# Judge audit · M2

Replay hai CSV trong `src/data/eval/` bằng `python -m lab9.judge --csv <file> --audit 5`. Lớp dương là `FAIL`. TP là lỗi được phát hiện, FP là báo nhầm, FN là lỗi bị bỏ sót, TN là lượt đạt được nhận đúng.

| Tiêu chí | n | TP / FP / FN / TN | κ | Precision | Recall | Tỉ lệ lỗi replay → Rogan–Gladen | Quyết định |
|---|---:|---|---:|---:|---:|---|---|
| tone_ok | 100 | 24 / 4 / 6 / 66 | 0,757 | 0,857 | 0,800 | 28% → 30% | Đạt ba ngưỡng. Dùng cho giọng văn. |
| helpful | 100 | 9 / 0 / 21 / 70 | 0,375 | 1,000 | 0,300 | 9% → 30% | Trượt κ và recall. Loại khỏi gate. |

Ngưỡng là κ ≥ 0,60, precision ≥ 0,80 và recall ≥ 0,70. Exit code của `tone_ok` là 0 và của `helpful` là 1.

## Audit bất đồng

| Case | Judge | Người chấm | Kết luận và bằng chứng | Sửa rubric |
|---|---|---|---|---|
| H-008 | PASS | FAIL | Đồng ý với người chấm. Cụm “nha, anh check giúp em” quá suồng sã trong ngữ cảnh hỗ trợ hoàn tiền. | Thêm ví dụ về cách xưng hô và từ đệm suồng sã. Chấm cả câu trong ngữ cảnh dịch vụ. |
| H-017 | PASS | FAIL | Đồng ý với người chấm theo ngữ cảnh CSV. “Bạn ơi” chưa phù hợp với khách lớn tuổi. | Cung cấp ngữ cảnh xưng hô nếu có. Khi chưa biết, dùng anh/chị hoặc cách gọi trung tính. |
| H-029 | PASS | FAIL | Đồng ý với người chấm. “Không cần nhắn thêm” mang tính gạt đi dù câu mở đầu có “Dạ, vui lòng”. | Yêu cầu đọc trọn câu. Câu ra lệnh hoặc gạt đi tính FAIL dù câu có từ lịch sự. |
| H-042 | FAIL | PASS | Đồng ý với người chấm. Reply ngắn, lịch sự, có số tiền và thời gian nhận. | Chỉ yêu cầu lời xin lỗi khi tình huống cần xin lỗi. Cho phép câu xác nhận hoàn tiền ngắn. |
| H-051 | FAIL | PASS | Đồng ý với người chấm. “Refund” được chấp nhận theo quy ước CS LogiX trong ghi chú. | Bổ sung từ nghiệp vụ được phép cùng ví dụ PASS. |
| HL-015 | PASS | FAIL | Đồng ý với người chấm. Reply xác nhận 55.000đ, ledger ghi 40.000đ. | Kiểm tra số tiền bằng code với ledger trước khi chấm độ hữu ích. Mọi chênh lệch tiền là FAIL. |
| HL-047 | PASS | FAIL | Đồng ý với người chấm. “Ok bạn” chưa phù hợp ngữ cảnh hỗ trợ và “xử lý rồi” thiếu kết quả cụ thể. | Yêu cầu cách xưng hô phù hợp và nêu rõ trạng thái xử lý cho khách. |

Judge helpful gắn cờ đúng 9 trong 30 ca lỗi. Trong 21 ca bỏ sót có 18 ca sai tiền và 3 ca sai xưng hô theo nhãn CSV. Precision 1,00 phản ánh 9 lượt gắn cờ đều đúng, trong khi recall 0,30 cho thấy judge bỏ sót phần lớn ca lỗi. Tỉ lệ sau hiệu chỉnh là ước lượng tổng hợp từ replay. Gate cần grader code cho số tiền và approval.

Hai CSV đã công khai. Sau khi chỉnh rubric, cần nhãn người mới trên tập held-out để đo lại trước khi mở rộng vai trò của judge.
