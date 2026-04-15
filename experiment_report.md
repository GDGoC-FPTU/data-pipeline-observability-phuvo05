# Experiment Report: Data Quality Impact on AI Agent

**Student ID:** AI20K-0336
**Name:** Võ Thiên Phú
**Date:** 15/04/2026

---

## 1. Kết quả thí nghiệm

Chạy `agent_simulation.py` với 2 bộ dữ liệu và ghi lại kết quả:

| Scenario | Agent Response | Accuracy (1-10) | Notes |
|----------|----------------|-----------------|-------|
| Clean Data (`processed_data.csv`) | "Based on my data, the best choice is Laptop at $1200" | 9/10 | Agent trả lời chính xác, chọn sản phẩm electronics có giá cao nhất (Laptop $1200 vs Monitor $300) |
| Garbage Data (`garbage_data.csv`) | "Based on my data, the best choice is Nuclear Reactor at $999999" | 2/10 | Agent trả lời sai hoàn toàn, chọn sản phẩm không thực tế (Nuclear Reactor) với giá trị outlier bất thường |

---

## 2. Phân tích & nhận xét

### Tại sao Agent trả lời sai khi dùng Garbage Data?

Dữ liệu garbage có nhiều vấn đề nghiêm trọng về chất lượng, dẫn đến Agent đưa ra quyết định sai lệch:

**1. Duplicate IDs:** Có 2 records cùng ID = 1 (Laptop và Banana), gây nhiễu loạn trong việc xác định sản phẩm duy nhất. Hệ thống không biết nên chọn record nào.

**2. Wrong Data Types:** Giá của "Broken Chair" là "ten dollars" (string) thay vì số, khiến việc so sánh giá trị không thể thực hiện chính xác. Agent không thể tính toán hoặc sắp xếp theo giá.

**3. Outliers:** "Nuclear Reactor" có giá $999,999 - một giá trị bất thường và không thực tế cho sản phẩm electronics. Agent chọn sản phẩm này vì nó có giá cao nhất, nhưng đây không phải là lựa chọn hợp lý trong thực tế.

**4. Null/Empty Values:** Record cuối cùng có ID rỗng, product là "Ghost Item" với giá = 0 và category rỗng. Dữ liệu này vô nghĩa nhưng vẫn tồn tại trong dataset, gây nhiễu thông tin.

**5. Inconsistent Format:** Category viết thường ("electronics", "fruit", "furniture") thay vì Title Case như dữ liệu clean, gây khó khăn trong việc filter và group dữ liệu.

Tất cả các vấn đề trên khiến Agent không thể đưa ra quyết định đúng đắn. Thay vì gợi ý sản phẩm thực tế như Laptop hay Monitor, Agent chọn "Nuclear Reactor" - một outlier vô lý.

---

## 3. Kết luận

**Quality Data > Quality Prompt?** Tôi hoàn toàn đồng ý.

Dù prompt có tốt đến đâu, nếu dữ liệu đầu vào kém chất lượng (duplicate, sai kiểu dữ liệu, outliers, null values), Agent sẽ trả lời sai. Trong thí nghiệm này, cùng một prompt "What is the best electronic product?", nhưng kết quả hoàn toàn khác biệt:
- Clean data → Agent trả lời chính xác (Laptop $1200)
- Garbage data → Agent trả lời vô lý (Nuclear Reactor $999,999)

**Bài học:** Data quality là nền tảng của mọi hệ thống AI. Trước khi tối ưu prompt, cần đảm bảo dữ liệu sạch, chuẩn hóa và hợp lệ. Một ETL pipeline tốt (Extract-Transform-Load) với validation chặt chẽ là bước quan trọng đầu tiên để xây dựng AI Agent hiệu quả.
