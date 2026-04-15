# Experiment Report: Data Quality Impact on AI Agent

**Student ID:** AI20K-0336
**Name:** Võ Thiên Phú
**Date:** 15/04/2026
---

## 1. Kết quả thí nghiệm

| Scenario | Agent Response | Accuracy (1-10) | Notes |
|----------|----------------|-----------------|-------|
| Clean Data (`processed_data.csv`) | Agent: Based on my data, the best choice is Laptop at $1200. | 9 | Dữ liệu sau ETL giữ lại record hợp lệ, category được chuẩn hóa, giá trị price hợp lý nên agent chọn đúng sản phẩm electronics có giá cao nhất trong tập dữ liệu sạch. |
| Garbage Data (`garbage_data.csv`) | Agent: Based on my data, the best choice is Nuclear Reactor at $999999. | 2 | Agent bị "đánh lừa" bởi outlier giá trị quá lớn, không có cơ chế xử lý anomaly/validation trước khi suy luận nên đưa ra kết quả phi thực tế. |


---

## 2. Phan tich & nhan xet

### Tai sao Agent tra loi sai khi dung Garbage Data?

Agent trả lời sai vì dữ liệu rác làm hỏng hướng suy luận đơn giản của agent. Thứ nhất, có outlier "Nuclear Reactor" với price = 999999 trong category electronics, nên khi agent lấy giá cao nhất thì record này luôn thắng và che lấp các sản phẩm hợp lý như Laptop. Thứ hai, có duplicate id (id=1 xuất hiện 2 lần) làm giảm độ tin cậy của tập dữ liệu. Thứ ba, có wrong data type ("ten dollars") có thể gây lỗi tính toán hoặc làm sai kết quả thống kê nếu pipeline xử lý số học. Cuối cùng, null/missing values (id rỗng, category rỗng, price = 0) cho thấy dữ liệu không được validation chặt chẽ. Tổng hợp lại, agent không sai về mặt code truy vấn, nhưng sai do "garbage in, garbage out": đầu vào kém chất lượng dẫn đến đầu ra phi thực tế.

---

## 3. Kết luận

**Quality Data > Quality Prompt?** (Đồng ý hay không? Giải thích ngắn gọn.)

Đồng ý. Prompt tốt chỉ giúp agent diễn đạt hoặc suy luận trên những gì nó "nhìn thấy"; nếu dữ liệu nền bị nhiễm outlier, sai kiểu, trùng lặp, thiếu trường quan trọng thì câu trả lời vẫn sai. Trong thí nghiệm này, cùng một prompt nhưng clean data cho kết quả hợp lý (Laptop $1200), còn garbage data đẩy agent đến kết luận vô lý (Nuclear Reactor $999999). Vì vậy, đảm bảo data quality là điều kiện tiên quyết trước khi tối ưu prompt.
