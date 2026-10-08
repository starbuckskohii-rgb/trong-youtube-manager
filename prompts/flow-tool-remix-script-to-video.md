# Prompt remix tool Google Flow: Excel → Video (Gemini Omni 1.1)

Dán **Prompt chính** vào ô "Bạn muốn tạo gì?" trong Trình tạo công cụ (Flow → Công cụ → Remix).
Nếu ô nhập báo quá dài, gửi phần MỤC TIÊU + QUY TẮC + BƯỚC 1–3 trước, rồi gửi BƯỚC 4–8 + GIAO DIỆN ở tin nhắn sau.

## Prompt chính

````
Remix công cụ này thành "MC ANIM STUDIO – SCRIPT TO VIDEO". Công cụ đọc kịch bản từ file Excel, tự sinh prompt cho từng cảnh và dựng video bằng model Gemini Omni 1.1. Lời thoại phải giữ nguyên 100%. Toàn bộ giao diện bằng tiếng Việt. Giữ phong cách hiện tại: nền tối, viền xanh lá, font pixel.

=== QUY TẮC TỐI THƯỢNG: LỜI THOẠI KHÔNG ĐƯỢC ĐỘNG VÀO ===
1. Lời thoại ở cột "Thoại" phải nằm NGUYÊN VĂN 100% trong prompt video. Không dịch, không sửa chính tả, không đổi dấu câu, không đổi chữ hoa/thường, không thêm/bớt/tóm tắt. Chỉ được bỏ khoảng trắng thừa ở đầu và cuối ô.
2. AI (Gemini) KHÔNG BAO GIỜ được viết hoặc viết lại lời thoại. AI chỉ viết phần hình ảnh: bối cảnh, hành động, biểu cảm, góc máy, âm thanh nền. Lời thoại do CODE chèn vào prompt qua template, lấy thẳng từ dữ liệu Excel.
3. Trước khi tạo video, code kiểm tra từng dòng: chuỗi thoại gốc phải xuất hiện trong prompt cuối, khớp từng ký tự. Dòng nào không khớp thì tô đỏ và khóa nút tạo video của dòng đó.
4. Ô thoại luôn ở chế độ chỉ đọc, có biểu tượng 🔒 và huy hiệu "Thoại khớp 100% ✓".

=== BƯỚC 1: ĐỌC KỊCH BẢN EXCEL ===
- Cho tải lên .xlsx, .xls, .csv. Đọc ngay trên trình duyệt (dùng SheetJS; nếu không tải được thư viện thì tối thiểu hỗ trợ .csv UTF-8).
- Đọc sheet đầu tiên, tự nhận dòng tiêu đề, không phân biệt hoa/thường và có dấu/không dấu: "STT" | "Cảnh" (Canh, Scene) | "Thoại" (Thoai, Dialogue). Nếu không nhận ra thì cho người dùng tự chọn cột.
- Sắp xếp theo STT. Cảnh báo nếu STT trùng, STT trống, hoặc dòng thiếu mô tả cảnh. Dòng có Thoại trống là cảnh không lời.
- Cách hiểu cột Thoại:
  • Dòng trong ô có dạng "Tên: câu nói" và Tên trùng một nhân vật đã tải lên → Tên là người nói, phần sau dấu ":" đầu tiên là lời thoại nguyên văn.
  • Ô có nhiều dòng → nhiều câu thoại, giữ đúng thứ tự.
  • Không ghi tên người nói → cho người dùng chọn người nói trong bảng (mặc định: nhân vật đầu tiên).
- Bảng xem trước gồm: STT | Cảnh | Thoại 🔒 | Người nói | Nhân vật trong cảnh | Bối cảnh | Thời lượng ước tính | Trạng thái.
- Có nút "Tải file Excel mẫu" (3 cột STT | Cảnh | Thoại, 3 dòng ví dụ).

=== BƯỚC 2: NHÂN VẬT, BỐI CẢNH, PHONG CÁCH (ĐỒNG BỘ HÌNH ẢNH) ===
- Tải ảnh nhân vật. Mỗi nhân vật gồm: Tên (khớp tên trong kịch bản), 1–3 ảnh tham chiếu (nền trơn là tốt nhất), mô tả giọng nói (vd: "young female, warm, clear American accent").
- Sau khi tải ảnh, dùng Gemini xem ảnh và viết "Character Bible" tiếng Anh cho từng nhân vật: khuôn mặt, tóc, màu da, trang phục, phụ kiện, dáng người. Người dùng sửa được rồi bấm "Khóa". Mô tả đã khóa được dán NGUYÊN VĂN vào mọi prompt có nhân vật đó, không rút gọn, không diễn đạt lại.
- "Location Bible": trước khi sinh prompt, Gemini đọc TOÀN BỘ kịch bản một lần và gom các cảnh cùng địa điểm thành danh sách bối cảnh (vd: LOC_01 – Tom's kitchen, morning). Mỗi bối cảnh có một đoạn mô tả cố định bằng tiếng Anh: không gian, đồ vật chính, bảng màu, ánh sáng, thời điểm trong ngày. Người dùng xem/sửa/khóa được. Mọi cảnh cùng bối cảnh dùng cùng một đoạn mô tả nguyên văn. Cho tải ảnh tham chiếu bối cảnh (tùy chọn).
- "Style Bible" chung cho cả phim: chọn phong cách (mặc định: hoạt hình 3D khối kiểu phim chiếu rạp, màu tươi, ánh sáng mềm), tỉ lệ khung hình (16:9 / 9:16), ngôn ngữ thoại (mặc định English), tốc độ nói (mặc định "Slow English": chậm, rõ từng từ). Dán nguyên văn vào mọi prompt.
- Tự phát hiện nhân vật trong từng cảnh (theo tên trong cột Cảnh và người nói), cho sửa tay bằng checkbox. Cảnh có hơn 3 nhân vật thì cảnh báo, vì Omni dễ lẫn nhân vật.

=== BƯỚC 3: TỰ SINH PROMPT ===
- Với mỗi dòng, gọi Gemini và bắt trả về JSON (không phải văn bản tự do):
  { "location_id": "", "characters": [], "action": "", "expressions": "", "camera": "", "ambient_sound": "", "continuity_note": "" }
  Chỉ dẫn cho Gemini: viết tiếng Anh, bám sát cột Cảnh; KHÔNG viết lời thoại, KHÔNG dùng dấu ngoặc kép; hành động và biểu cảm phải hợp với nội dung câu thoại; nếu cùng bối cảnh với cảnh trước thì giữ liên tục (vị trí đứng, đồ vật đang cầm, ánh sáng, trang phục).
- CODE ghép prompt cuối theo template cố định:

STYLE: {style bible}
SETTING: {location bible của location_id}
CHARACTERS:
@{Tên}: {character bible}   (lặp cho từng nhân vật trong cảnh)
ACTION: {action}. Expressions: {expressions}.
CAMERA: {camera}
DIALOGUE (spoken word for word exactly as written, in {ngôn ngữ thoại}, {tốc độ nói}, accurate lip-sync):
@{Người nói 1} says: "{thoại nguyên văn 1}"
@{Người nói 2} says: "{thoại nguyên văn 2}"
AUDIO: {ambient_sound}. Voice of @{Tên}: {mô tả giọng}. No background music.
RULES: Only the lines above are spoken, word for word, in this order. No other speech, no narration, no subtitles, no on-screen text. Characters must look exactly like their reference images.

- Cảnh không thoại: thay khối DIALOGUE bằng "No one speaks. Ambient sound only."
- Khi gọi Omni, gắn ảnh tham chiếu của đúng các nhân vật có trong cảnh (và ảnh bối cảnh nếu có) làm ingredients.

=== BƯỚC 4: THỜI LƯỢNG ===
- Ước tính thời gian nói = số từ ÷ tốc độ ("Slow English" = 2 từ/giây, "Bình thường" = 2,5 từ/giây) + 1,5 giây đệm. Thời lượng clip = làm tròn lên, tối thiểu 4 giây, tối đa MAX_CLIP_SECONDS (hằng số, mặc định 10).
- Thoại dài hơn giới hạn: tô vàng, KHÔNG tự cắt. Cho chọn: (a) "Chia thành nhiều clip liên tiếp": chia theo ranh giới câu, ghép các phần lại phải đúng bằng chuỗi gốc (code kiểm tra), các clip giữ nguyên bối cảnh, nhân vật, góc máy; (b) giữ nguyên.

=== BƯỚC 5: XEM LẠI & CHỈNH ===
- Mỗi dòng xem được prompt đầy đủ; sửa được phần hình ảnh (action, camera…), không sửa được thoại.
- Nút "Tạo lại prompt" cho từng dòng và "Tạo lại tất cả".
- Thanh tổng quan: tổng số cảnh, số cảnh đạt kiểm tra thoại, tổng thời lượng ước tính, số lượt tạo video dự kiến.

=== BƯỚC 6: DỰNG VIDEO VỚI GEMINI OMNI 1.1 ===
- Model cố định: Gemini Omni 1.1 (có âm thanh và khẩu hình).
- Nút "Tạo video" từng dòng, "Tạo các dòng đã chọn", "Tạo tất cả". Trước khi chạy hàng loạt, hiện hộp xác nhận số lượt tạo (vì tốn credit).
- Chạy theo hàng đợi, tối đa 2 video cùng lúc, tự thử lại tối đa 2 lần khi lỗi. Trạng thái: Chờ / Đang tạo / Xong / Lỗi (kèm lý do). Xem trước video ngay trong bảng.
- Hai cảnh liền nhau cùng bối cảnh: tùy chọn dùng khung hình cuối của clip trước làm khung hình đầu clip sau (nếu nền tảng hỗ trợ).

=== BƯỚC 7: KIỂM TRA LỜI NÓI SAU KHI TẠO ===
- Nếu nền tảng cho phép: sau khi có video, dùng Gemini nghe và chép lại lời nói trong clip, so với thoại gốc (bỏ qua dấu câu và chữ hoa/thường). Khớp → "Nói đúng ✓". Lệch → tô cam, hiện các từ sai/thiếu/thừa, gợi ý "Tạo lại".

=== BƯỚC 8: XUẤT ===
- Xuất Excel/CSV: STT | Cảnh | Thoại (nguyên văn) | Prompt cuối | Thời lượng | Trạng thái.
- Xuất/nhập JSON toàn bộ dự án (bible + prompt) để dùng lại lần sau.
- Tải video đặt tên theo thứ tự: 001_ten-canh.mp4, 002_ten-canh.mp4… để ghép dựng đúng thứ tự.
- Tự lưu trạng thái làm việc, tải lại trang không bị mất.

=== GIAO DIỆN ===
- Thanh tiến trình 4 bước trên cùng: ① Kịch bản → ② Nhân vật & bối cảnh → ③ Prompt → ④ Video.
- Bảng cảnh là trung tâm; mỗi dòng mở rộng được để xem prompt và video.
````

## Prompt sửa lỗi (dùng sau khi tool đã dựng xong)

Khi thoại bị thay đổi:

```
Dòng STT {số} có thoại trong prompt khác với Excel. Sửa lại để lời thoại chỉ được chèn bằng code từ dữ liệu Excel gốc, Gemini không được trả về hay chỉnh sửa thoại, và thêm bước kiểm tra khớp từng ký tự trước khi cho tạo video.
```

Khi nhân vật bị lệch hình giữa các cảnh:

```
Nhân vật {Tên} bị khác nhau giữa các cảnh. Đảm bảo mọi prompt có {Tên} đều dán nguyên văn Character Bible đã khóa và luôn gắn đúng ảnh tham chiếu của {Tên} khi gọi Omni 1.1.
```

Khi không đọc được Excel:

```
File .xlsx của tôi không đọc được. Hãy hiện lỗi cụ thể, hỗ trợ cả .xlsx/.xls/.csv, và cho tôi tự chọn cột STT, Cảnh, Thoại nếu không nhận ra tiêu đề.
```

## Mẫu file Excel

| STT | Cảnh | Thoại |
|-----|------|-------|
| 1 | Tom bước vào bếp buổi sáng, Anna đang pha cà phê | Tom: Good morning, Anna. |
| 2 | Anna quay lại, mỉm cười, đưa Tom cốc cà phê | Anna: Good morning, Tom. Would you like some coffee? |
| 3 | Cận cảnh Tom cầm cốc cà phê, gật đầu | Tom: Yes, please. Thank you. |
