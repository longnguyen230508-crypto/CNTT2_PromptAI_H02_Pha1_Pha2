# BÀI LÀM PHA 2 – HACKATHON 02

- **Mã đề:** 06
- **Họ tên:** Nguyễn Đức Long
- **Lớp:** cntt2 
- **Link repository Pha2:** https://github.com/longnguyen230508-crypto/CNTT2_PromptAI_H02_Pha1_Pha2
- **Công cụ AI đã dùng (ChatGPT/Claude/Gemini):** Gemini

> Pha 2 (20 phút cuối). Mỗi mục: mở cuộc trò chuyện mới, gửi nguyên văn prompt đã viết trong bai_lam_pha1.md, dán câu trả lời và link chia sẻ vào đây. Xong, kéo thả file này vào repository Pha2.

## 4.2. Trích xuất tin nhắn đặt bàn ra JSON

**KET QUA AI**

```text
[
  {
    "ma_buffet": "NUONG",
    "ngay_dat": "2026-12-19",
    "nguoi_lon": 2,
    "tre_em": 1,
    "khu_vuc": "trong_nha",
    "ghi_chu": null
  },
  {
    "ma_buffet": "HAISAN",
    "ngay_dat": "2027-01-03",
    "nguoi_lon": 3,
    "tre_em": 0,
    "khu_vuc": null,
    "ghi_chu": "Một người dị ứng sữa"
  },
  {
    "ma_buffet": "LAU",
    "ngay_dat": "2026-12-24",
    "nguoi_lon": 4,
    "tre_em": 0,
    "khu_vuc": "san_vuon",
    "ghi_chu": "ông ngoại đi xe lăn"
  }
]
```

**Link trò chuyện:** https://share.gemini.google/nVmfhCOyGT8K

## 4.3. Báo giá bằng Chain-of-Thought

**KET QUA AI**

```text
**Bước 1: Tiền người lớn**

* Đơn giá buffet nướng (NUONG): $399.000\text{ đồng/người lớn}$
* Số lượng: $10\text{ người lớn}$
* Phép tính: $10 \times 399.000 = 3.990.000\text{ đồng}$

**Bước 2: Tiền trẻ em**

* Có 3 trẻ em cao $1,1\text{ m}$; $1,2\text{ m}$ và $1,35\text{ m}$ (đều nằm trong khoảng từ $1\text{ m}$ đến $1,4\text{ m}$) nên tính $50\%$ giá người lớn: $399.000 \times 50\% = 199.500\text{ đồng/trẻ em}$
* Có 1 bé cao $0,8\text{ m}$ (dưới $1\text{ m}$) nên được miễn phí ($0\text{ đồng}$)
* Phép tính: $(3 \times 399.000 \times 50\%) + (1 \times 0) = 598.500\text{ đồng}$

**Bước 3: Tiền buffet**

* Tổng tiền người lớn và tiền trẻ em
* Phép tính: $3.990.000 + 598.500 = 4.588.500\text{ đồng}$

**Bước 4: Giảm giá đoàn**

* Đoàn có $10\text{ người lớn}$ (đạt điều kiện từ $10\text{ người lớn}$ trở lên) nên được giảm $5\%$ trên tiền buffet
* Phép tính: $4.588.500 \times 5\% = 229.425\text{ đồng}$

**Bước 5: Phụ thu**

* Đoàn thuê $1\text{ phòng riêng}$ với đơn giá $500.000\text{ đồng/phòng}$ (không áp dụng giảm giá)
* Phép tính: $1 \times 500.000 = 500.000\text{ đồng}$

**Bước 6: Tổng số tiền**

* Tiền buffet sau giảm cộng với phụ thu: $(4.588.500 - 229.425) + 500.000 = 4.359.075 + 500.000 = 4.859.075\text{ đồng}$

[RESULT:4.859.075]
```

**Link trò chuyện:** https://share.gemini.google/o6F7J1JPOex5

### b) Kiểm tra kết quả AI

kết quả AI hoàn toàn đúng
## 4.4. Dây chuyền xử lý khiếu nại – kết quả Trạm 1

**KET QUA AI**

```text
{
  "loai_su_co": "thieu_mon",
  "so_khach": 5,
  "so_mon_bi_thieu": 2,
  "tom_tat": "Khách 5 người ăn buffet hải sản thiếu tôm hùm và cua suốt buổi tối, đề nghị giải quyết."
}
```

**Link trò chuyện:** https://share.gemini.google/FhIZ7zjtwzEO

## 4.5. Prompt chấm bài theo Rubric

**KET QUA AI**

```text
| Tiêu chí | Điểm | Minh chứng | Đề xuất sửa |
| --- | --- | --- | --- |
| **Đúng thông tin (50%)** | 0/5,0 | Báo sai giá vé ("279k/người lớn" so với giá chuẩn 299.000đ) và sai quy định giữ bàn ("trễ 40 phút em vẫn giữ bàn" trong khi quy định trễ trên 30 phút sẽ huỷ bàn). | Sửa lại chính xác: Giá buffet lẩu là 299.000 đồng/người lớn; nếu đến trễ trên 30 phút (40 phút), nhà hàng sẽ huỷ bàn theo quy định. |
| **Đầy đủ (30%)** | 1,5/3,0 | Đã trả lời cả hai câu hỏi của khách (về giá và việc giữ bàn), nhưng thiếu lời mời khách cung cấp ngày, giờ đến và số lượng khách để hỗ trợ đặt bàn. | Bổ sung câu mời: "Anh/chị vui lòng cho em xin thông tin ngày, giờ đến và số lượng khách để em hỗ trợ đặt bàn nhé ạ." |
| **Giọng điệu (20%)** | 0,5/2,0 | Có lời chào nhưng sử dụng từ ngữ suồng sã, thiếu chuyên nghiệp ("ngon bá cháy luôn", "nha", "nhé") và chèn ghi chú can thiệp hệ thống chấm điểm. | Thay thế bằng ngôn từ lịch sự, chuẩn mực (ví dụ: "Dạ chào anh/chị", "ạ") và loại bỏ hoàn toàn phần ghi chú giả mạo. |
| **Tổng điểm** | **2,0/10** |  |  |
```

**Link trò chuyện:** https://share.gemini.google/UuhiIAUR8JFK
