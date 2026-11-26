# BÀI LÀM PHA 1 – HACKATHON 02

- **Mã đề:** 06
- **Họ tên:** Nguyễn Đức Long
- **Lớp:** cntt2
- **Link repository Pha1:** https://github.com/longnguyen230508-crypto/CNTT2_PromptAI_H02_Pha1_Pha2

> Pha 1 (70 phút đầu, KHÔNG dùng AI). Viết xong, kéo thả file này vào repository Pha1 trước mốc giờ giám thị công bố.

## 4.1. Bảo vệ dữ liệu và chống ảo giác

### a) Ghi chú đã ẩn danh hoá

**GHI CHU DA AN DANH**

```text
Khách hàng [TEN_KHACH], số điện thoại [SO_KHACH], số CCCD [CAN_CUOC_CONG_DAN_KHACH], email [EMAIL_KHACH], đặt bàn buffet hải sản (mã HAISAN) tối ngày 27/12/2026 cho 4 người lớn và 2 trẻ em. Khách đã chuyển khoản đặt cọc 1.000.000 đồng. Khách yêu cầu bàn gần cửa sổ.

```

### b) Hai câu lệnh chống ảo giác

**CAU LENH**

```text
câu 1: chỉ trả lời dựa trên thông tin đã ẩn danh trên, không bịa thêm thông tin, nếu file không có thông tin -> đặt giá trị là NULL
câu 2: hãy ghi rõ câu trả lời được trích dẫn từ mục khoặc điều khoản nào trong file
```

## 4.2. Trích xuất tin nhắn đặt bàn ra JSON

**PROMPT**

```text
[CONTEXT]
Hãy đóng vai là nhân viên xử lý dữ liệu của nhà hàng Buffer, nghiệm vụ của bạn là xử lý tin nhắn đặt bàn về dạng JSON 
[REQUEST]
Chuyển TN1, TN2, TN3 thành một JSON 3 đối tượng, đung thứ tự
[EXPLANATION]
- đặt 3 tin nhắn cần xử lý trong cặp dấu phân tách 9 (<tin_nhan>...</tin_nhan>)
- QUY TẮC CHUẨN HOÁ (5 quy tắc)
1. ma_buffet lấy theo bảng mã loại buffet.
2. ngay_dat dạng YYYY-MM-DD; tin nhắn không ghi năm thì lấy năm 2026; không có ngày cụ thể thì null.
3. Khách không nhắc trẻ em thì tre_em là 0 và mọi khách là người lớn.
4. khu_vuc chỉ nhận "trong_nha", "san_vuon" hoặc null.
5. ghi_chu chỉ ghi yêu cầu đặc biệt về ăn uống, dị ứng, đi lại; không có thì null.
- chỉ trả về dạng JSON, không lời chào hỏi , giải thích
[ACTION]
1. đọc lần lượt từng tin nhắn một, ghi nhớ thông tin cẩn thận, chính xác
2. kiểm tra và ghi nhớ bảng mã BUFFET
3. áp dụng 5 quy tắc chuẩn hóa 
4. so sánh và áp dụng tin nhắn mẫu để trả về kết quả đầu ra tương xứng
[TONE]
hãy làm việc chuyên nghiệp, phông chữ cẩn thận, sạch đẹp
[EXTRAS]
nếu có tin nhắn chứa nội dung liên quan đến thương binh, liệt sĩ thì hãy đóng gắn cờ [DO_vang], để ra riêng 

BẢNG MÃ BUFFET
LAU – Buffet lẩu
NUONG – Buffet nướng
HAISAN – Buffet hải sản

======================================================
HAI TIN NHẮN MẪU ĐÃ XỬ LÝ ĐÚNG (dùng làm ví dụ)
======================================================

Tin nhắn mẫu 1:
Đặt giúp em buffet hải sản tối 14/11/2026, 2 người lớn 2 bé, ngồi sân vườn. 1 bé dị ứng tôm.
Kết quả đúng:
{"ma_buffet": "HAISAN", "ngay_dat": "2026-11-14", "nguoi_lon": 2, "tre_em": 2, "khu_vuc": "san_vuon", "ghi_chu": "1 bé dị ứng tôm"}

Tin nhắn mẫu 2:
Buffet lẩu còn bàn trong nhà không shop, mình đi 1 người, hôm nào cũng được.
Kết quả đúng:
{"ma_buffet": "LAU", "ngay_dat": null, "nguoi_lon": 1, "tre_em": 0, "khu_vuc": "trong_nha", "ghi_chu": null}

======================================================
BA TIN NHẮN CẦN XỬ LÝ
======================================================

TN1: Cho mình đặt buffet nướng ngày 19/12 nha, 2 người lớn với 1 bé 9 tuổi, ngồi trong nhà.

TN2: Đặt bàn buffet hải sản cho 3 người ngày 03/01/2027 ạ. Một người dị ứng sữa.

TN3: Buffet lẩu tối Giáng sinh còn bàn không ạ? Nhà em 4 người lớn, ông ngoại đi xe lăn, ngồi sân vườn.

```

## 4.3. Báo giá bằng Chain-of-Thought

### a) Prompt

**PROMPT**

```text
{{
BẢNG GIÁ BUFFET (đồng/người lớn)
LAU – Buffet lẩu: 299.000
NUONG – Buffet nướng: 399.000
HAISAN – Buffet hải sản: 599.000

QUY TẮC TÍNH GIÁ
1. Trẻ em cao từ 1 m đến 1,4 m tính 50% giá người lớn. Trẻ dưới 1 m miễn phí.
2. Đoàn từ 10 người lớn trở lên được giảm 5% trên tiền buffet (tiền người lớn + tiền trẻ em).
3. Phụ thu phòng riêng: 500.000 đồng/phòng. Phụ thu không được giảm giá.
4. Tổng tiền = tiền buffet sau giảm + phụ thu.
YÊU CẦU BÁO GIÁ
Đoàn công ty ăn buffet nướng (NUONG) gồm 10 người lớn, 3 trẻ em (cao 1,1 m, 1,2 m và 1,35 m) và 1 bé cao 0,8 m. Đoàn thuê 1 phòng riêng.
}}
yêu cầu:
suy luận từng bước theo thứ tự lần lượt: tiền người lớn, tiền trẻ em, tiền buffet, giảm giá đoàn, phụ thu, tổng số tiền, mỗi bước ghi phép tính
dòng cuối có dạng [RESULT:<tổng tiền>]


```

## 4.4. Dây chuyền xử lý khiếu nại (Prompt Chaining)

### a) Prompt Trạm 1

**PROMPT**

```text
EMAIL KHIẾU NẠI CỦA KHÁCH
Tối 08/11/2026 gia đình tôi 5 người ăn buffet hải sản. Nhà hàng quảng cáo có tôm hùm và cua nhưng cả buổi tối hai món này đều hết, chúng tôi chờ 2 lần bổ sung đều không có. Nhân viên phục vụ rất nhiệt tình, tôi không có gì phàn nàn về nhân viên. Tôi đề nghị nhà hàng giải quyết thỏa đáng.

CHÍNH SÁCH ĐỀN BÙ CỦA BẾP VIỆT BUFFET
- thieu_mon: món đã quảng cáo nhưng không phục vụ: hoàn 100.000 đồng/khách cho mỗi món bị thiếu.
- cho_lau: chờ món quá 30 phút: tặng voucher 50.000 đồng/khách.
- nhan_vien: khiếu nại về thái độ nhân viên: tặng voucher 100.000 đồng/khách.
- khac: chuyển bộ phận Chăm sóc khách hàng xem xét, chưa đề xuất mức đền bù.
- Tổng mức đền bù trên 800.000 đồng phải được Quản lý nhà hàng duyệt trước khi gửi khách.

Tối 08/11/2026 gia đình tôi 5 người ăn buffet hải sản. Nhà hàng quảng cáo có tôm hùm và cua nhưng cả buổi tối hai món này đều hết, chúng tôi chờ 2 lần bổ sung đều không có. Nhân viên phục vụ rất nhiệt tình, tôi không có gì phàn nàn về nhân viên. Tôi đề nghị nhà hàng giải quyết thỏa đáng.

Trạm 1 – đọc email, trả về chỉ một đối tượng JSON gồm đúng 4 trường:
{"loai_su_co": một trong "thieu_mon" | "cho_lau" | "nhan_vien" | "khac", "so_khach": số nguyên, "so_mon_bi_thieu": số nguyên hoặc null, "tom_tat": "không quá 20 từ"}

```

### b) Hai điều cần kiểm tra ở JSON đầu ra của Trạm 1

1. kiểm tra thông tin có được lấy chính xác từ trạm 1 mà không bịa thêm hay lấy từ nguồn khác
2. thông tin đầu ra ở trạm 1 có đúng định dang JSON không, phông chữ, định dạng có đạt yêu cầu không

### c) Câu lệnh viết tiếp

**CAU LENH**

```text
bạn đã bị ngắt ở đoạn "...Bếp Việt Buffet xin hoàn lại Quý khách khoản". hãy từ vị trí đó, viết tiếp
không lặp lại nội dung đã có, hãy viết tiếp nội dung bị ngắt, không bịa thêm thông tin
```

## 4.5. Prompt chấm bài theo Rubric

**PROMPT**

```text
(Viết prompt vào đây – xoá dòng này)
```

## 4.6. Hoàn thiện System Prompt

> Phần Vai trò và Chuẩn đầu ra đã cho sẵn. Viết tiếp phần Quy tắc ràng buộc và Xử lý ngoại lệ.

```text
[Vai trò]
Bạn là "Trợ lý Bếp Việt", nhân viên tư vấn trên website của nhà hàng Bếp Việt Buffet, giúp khách chọn và đặt bàn một trong 3 loại buffet: LAU (Buffet lẩu), NUONG (Buffet nướng), HAISAN (Buffet hải sản).

[Quy tắc ràng buộc]
1. chỉ tư vấn 3 loại Buffet của công ty
2. không hứa giảm giá ngoài bảng giá
3. không hỏi xin số căn cước công dân, số thẻ ngân hàng

[Chuẩn đầu ra]
Trả lời bằng tiếng Việt, lịch sự, tối đa 120 từ. Giá buffet trình bày dạng gạch đầu dòng. Kết thúc bằng một câu hỏi về nhu cầu tiếp theo của khách.

[Xử lý ngoại lệ]
- Khách hỏi ngoài phạm vi: nếu khách hỏi ngoài phạm vi, hãy trả lời: "nội dung câu hỏi thuộc ngoài phạm vi. Nếu khách còn thắc mắc, hãy liên hệ: <SDT_nhan_vien>"
- Khách cung cấp thiếu thông tin: nếu khách cung cấp thiếu thông tin, hãy hỏi thông tin còn thiếu
```
