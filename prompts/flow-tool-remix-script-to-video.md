# Prompt remix tool Google Flow: Excel → Video (Gemini Omni 1.1)

Thứ tự dùng trong Trình tạo công cụ (Flow → Công cụ → Remix), ô "Bạn muốn tạo gì?":

1. Dán **Prompt chính (Phần 1)**, chờ tool dựng xong.
2. Dán **Phần 2: Preset phong cách "Slow English 3D"** ở tin nhắn tiếp theo. Phần 2 ghi đè một số mặc định của Phần 1 (style, tốc độ nói, trường JSON).
3. Dán **Phần 3: Nhập transcript / kịch bản thô** nếu muốn tool tự chuyển transcript thành bảng STT | Cảnh | Thoại.

Nếu ô nhập báo quá dài, gửi từ đầu đến hết BƯỚC 3 trước, rồi gửi phần còn lại ở tin nhắn sau.

## Prompt chính (Phần 1)

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

## Phần 2: Preset phong cách "Slow English 3D"

Phong cách tham chiếu: video hoạt hình 3D hội thoại học tiếng Anh A1–A2 (vd. https://www.youtube.com/watch?v=f3-HaMjMx7E).
Những gì phân tích được từ 3 phút đầu video đó:

- 3D chất lượng phim chiếu rạp, không viền, da mềm, tóc chi tiết, vải có chất liệu (ren, kim sa).
- Nhân vật tuổi teen cách điệu: mắt to, mũi nhỏ, tỉ lệ đầu/thân khoảng 1:5.
- Bối cảnh trong nhà sạch, nhiều chi tiết: lớp học, cửa hàng quần áo, hội trường trang trí bóng bay.
- Màu tươi, bão hòa cao, ánh sáng ấm có bloom nhẹ, xóa phông khi cận.
- Máy tĩnh, cỡ trung và cận, shot/reverse-shot khi hội thoại, mỗi shot 2–4 giây, thỉnh thoảng push-in chậm.
- Nhân vật tự nói trên hình (lip-sync), không có người dẫn chuyện. Giọng Mỹ chậm, rõ, ngắt nghỉ giữa các câu.
- Phụ đề trắng chữ to, hộp tím/xanh bán trong suốt, giữa đáy màn hình. Huy hiệu "A2 LEVEL" góc dưới phải.
- Nhạc acoustic-pop vui, âm lượng thấp. Mở đầu bằng hook "Later in this episode", rồi giới thiệu nhân vật, title card "Earlier that day", sau đó kể theo trình tự.

````
Nâng cấp tool: chuyên sản xuất phim hoạt hình 3D phong cách "SLOW ENGLISH 3D" (truyện hội thoại học tiếng Anh A1–A2, nhân vật tuổi teen và người lớn, đời sống thường ngày: trường học, gia đình, cửa hàng, tiệc). Giữ nguyên mọi quy tắc về lời thoại ở phần trước. Phần này ghi đè các mặc định cũ về phong cách, tốc độ nói và trường JSON.

=== 1. PRESET PHONG CÁCH "SLOW ENGLISH 3D" (mặc định, khóa) ===
- Thay Style Bible mặc định bằng đoạn sau và dán NGUYÊN VĂN vào mọi prompt video:

VISUAL STYLE: High-end 3D animated feature-film look. Stylized characters with large expressive eyes, small noses, soft rounded faces and a head-to-body ratio of about 1:5; detailed strand-based hair; soft skin shading with subtle subsurface scattering; realistic fabric textures such as cotton, denim, lace and sequins. No outlines, no toon shading, not anime, not 2D, not photorealistic. Clean, richly detailed everyday environments with tidy, believable props. Bright, warm, highly saturated colors; soft key light with gentle bloom on highlights; shallow depth of field with a softly blurred background. Eye-level camera, static or very slow push-in, 16:9 frame. Smooth, expressive acting with natural hand gestures, head tilts, blinking and subtle idle movement; precise lip-sync. Clean image: no text, no subtitles, no captions, no logos, no watermark.

- Preset hiển thị ở header dạng huy hiệu "SLOW ENGLISH 3D". Chỉ sửa được khi bấm "Mở khóa preset".

=== 2. NGÔN NGỮ MÁY QUAY (Gemini bắt buộc tuân theo khi viết trường camera) ===
1. Clip đầu tiên ở một bối cảnh mới: establishing shot (wide hoặc medium-wide), thấy rõ không gian và mọi nhân vật có mặt.
2. Mỗi câu thoại: máy quay vào NGƯỜI NÓI, medium close-up, ngang tầm mắt. Có người nghe thì quay over-the-shoulder từ sau vai người nghe (shot/reverse-shot).
3. Luật 180°: trong cùng một bối cảnh, mỗi nhân vật luôn đứng cùng một phía khung hình. Gemini trả thêm trường "screen_positions" (vd {"Maya":"left","Sara":"right"}); code lưu theo bối cảnh và gửi lại cho các cảnh sau để giữ nguyên.
4. Cảm xúc mạnh (bất ngờ, xấu hổ, vui, giận): close-up khuôn mặt.
5. Cảnh không thoại: reaction close-up hoặc shot hành động.
6. Chỉ dùng máy tĩnh hoặc push-in chậm. Không lia nhanh, không rung tay, không góc nghiêng, không zoom gắt.
7. Mỗi clip là đúng một shot, không cắt cảnh bên trong clip.
- JSON Gemini trả về thêm các trường: "shot_type", "screen_positions", "outfits" (trang phục từng nhân vật trong cảnh).

=== 3. GIỌNG NÓI ===
- Câu dẫn của khối DIALOGUE trong preset này: "spoken slowly and clearly in natural American English, about 100 words per minute, with a short pause between sentences, warm friendly tone, precise lip-sync".
- Ước tính thời lượng mới: số từ ÷ 1,7 từ/giây + 0,5 giây cho mỗi lần ngắt câu + 1,5 giây đệm.
- Mỗi nhân vật có một mô tả giọng cố định bằng tiếng Anh (tool gợi ý theo tuổi và giới tính, vd "teenage girl, bright and gentle American voice"). Khóa lại và dán nguyên văn vào mọi clip nhân vật đó nói.
- Mọi prompt luôn có "No background music." Nhạc nền được thêm lúc dựng để không bị lệch giữa các clip.

=== 4. NHÂN VẬT, TRANG PHỤC, BỐI CẢNH ĐÚNG STYLE ===
- Character Bible viết theo khuôn: face, eyes, hair, skin tone, stylized body proportions (~1:5), base outfit. Không mô tả kiểu ảnh thật.
- Trang phục theo phân đoạn: mỗi nhân vật có danh sách trang phục (vd Outfit A – đi học, Outfit B – váy dạ hội). Gemini đọc toàn bộ kịch bản và đề xuất cảnh nào đổi trang phục; người dùng duyệt. Mỗi cảnh gán outfit cho từng nhân vật; prompt dán nguyên văn mô tả outfit. Mặc định giữ nguyên trang phục trong cùng bối cảnh và cùng ngày.
- Nút "Chuyển về style" cho từng nhân vật: dùng model tạo ảnh của Flow vẽ lại nhân vật đúng preset trên nền trơn (1 ảnh chính diện toàn thân + 1 ảnh 3/4 bán thân), giữ nguyên đặc điểm nhận dạng. Người dùng chọn ảnh ưng ý làm ảnh tham chiếu. Hỏi xác nhận trước vì tốn credit.
- Nút "Tạo ảnh bối cảnh" cho từng bối cảnh: tạo 1 ảnh tham chiếu không có người, theo Location Bible + preset. Ảnh này được gắn làm ingredient cho MỌI clip ở bối cảnh đó để nền đồng nhất. Mỗi clip gắn tối đa: ảnh các nhân vật trong cảnh (≤3) + 1 ảnh bối cảnh.

=== 5. XUẤT CHO HẬU KỲ ===
- Phụ đề .srt và .ass: dùng lời thoại nguyên văn. Thời gian tính theo thời lượng THỰC của từng clip (đọc metadata video sau khi tạo; chưa có video thì dùng ước tính), nối theo thứ tự STT. Một clip nhiều câu thì chia thời gian theo số từ; mỗi câu bắt đầu sau 0,3 giây.
- File .ass có sẵn kiểu chữ: trắng, sans-serif đậm, cỡ lớn, hộp nền tím bán trong suốt, căn giữa đáy khung, tối đa 2 dòng mỗi lần hiện.
- Danh sách dựng (CSV): thứ tự, tên file clip, thời lượng, mốc bắt đầu trên timeline, loại (hook / giới thiệu nhân vật / title card / cảnh chính).
- Hiển thị "Ghi chú hậu kỳ" cố định trong tool: thêm nhạc nền acoustic-pop vui nhẹ ở âm lượng thấp; huy hiệu "A2 LEVEL" góc dưới phải; title card "Earlier that day..." và tên nhân vật làm trong phần mềm dựng, KHÔNG tạo bằng AI (AI vẽ chữ hay bị lỗi).

=== 6. HOOK & GIỚI THIỆU NHÂN VẬT (TÙY CHỌN) ===
- Nút "Gợi ý hook": Gemini chọn 3–5 dòng kịch tính nhất (theo STT) làm đoạn "Later in this episode..." ở đầu video. Hook dùng lại clip đã tạo (không tốn thêm credit) và được đưa lên đầu danh sách dựng.
- Nút "Tạo clip giới thiệu nhân vật" (tốn credit, hỏi xác nhận): mỗi nhân vật 1 clip 4 giây không thoại. Nhân vật đứng trong bối cảnh quen thuộc, quay về phía máy quay, mỉm cười, vẫy tay nhẹ; chừa khoảng trống một bên khung hình để chèn tên khi dựng.
````

## Phần 3: Nhập transcript / kịch bản thô → kịch bản video

Dùng khi chỉ có kịch bản dạng thô, ví dụ transcript YouTube có mốc thời gian, không có tên người nói và không có mô tả cảnh.

````
Thêm chế độ "NHẬP KỊCH BẢN THÔ" đứng trước Bước 1. Tool biến văn bản thô thành bảng STT | Cảnh | Thoại rồi nạp vào quy trình hiện có.

=== ĐẦU VÀO ===
- Dán văn bản hoặc tải file .txt / .srt. Nhận các dạng: "[00:01:23] câu nói", SRT, văn bản trơn, "Tên: câu nói".
- Bỏ mốc thời gian nhưng lưu lại cho từng câu (cột "Mốc gốc" để người dùng nghe lại bản gốc). Dấu ">>" là gợi ý ĐỔI NGƯỜI NÓI.

=== GIAI ĐOẠN NHÁP (thoại CHƯA khóa, mọi thay đổi phải hiện rõ) ===
1. Ghép câu: nối các dòng bị cắt ngang theo thời gian thành câu hoàn chỉnh.
2. Phát hiện đoạn HOOK: đoạn mở đầu lặp lại nguyên văn một đoạn ở giữa truyện thì đánh dấu HOOK, KHÔNG tạo dòng mới. Ghi lại hook dùng lại clip của những STT nào.
3. Phát hiện lời chào kênh ("Welcome to ..."): đánh dấu INTRO, thay tên kênh cũ bằng tên kênh trong phần Cài đặt.
4. Sửa lỗi nhận dạng giọng nói rõ ràng (vd "h" → "hmm", tên riêng nghe nhầm). Mỗi chỗ sửa hiện dạng so sánh (chữ cũ gạch đỏ, chữ mới xanh) và phải được người dùng duyệt. Không bao giờ sửa ngầm.
5. Đoán người nói cho từng câu theo ngữ cảnh, kèm mức tự tin (cao/thấp). Câu tự tin thấp tô vàng để người dùng chọn lại. Tên chưa rõ thì đặt tên tạm và ghi chú.
6. Chia bối cảnh (LOC_xx) và viết cột Cảnh bằng tiếng Việt: địa điểm, ai có mặt, hành động, cảm xúc, trạng thái trang phục.
7. Chia dòng: mỗi dòng ≤ MAX_CLIP_SECONDS theo công thức thời lượng hiện có; chỉ cắt ở ranh giới câu; mỗi dòng một người nói. Riêng câu cảm thán rất ngắn (≤ 2 từ, vd "Yes.", "Wow.", "Huh?") được ghép với câu liền kề của người khác thành clip 2 người.
8. Dòng thời gian trang phục: phát hiện khi trang phục đổi hoặc biến đổi theo cốt truyện (vd váy sạch → bị bẩn → đã sửa) và gán cho từng dòng.

=== KIỂM TRA ĐỘ PHỦ (bắt buộc, chạy bằng code) ===
- Chuẩn hóa văn bản gốc (bỏ mốc thời gian, ">>", dấu câu, chữ hoa) và trừ đoạn HOOK. Ghép toàn bộ thoại trong bảng (bỏ tiền tố "Tên:") và chuẩn hóa tương tự.
- So sánh từng từ. Mọi khác biệt phải nằm trong danh sách sửa đã duyệt ở bước 4. Thiếu từ, thừa từ hoặc đảo thứ tự → báo lỗi đỏ, không cho chốt.
- Hiển thị: "Độ phủ thoại: 492/492 từ ✓, 4 chỗ sửa đã duyệt".

=== CHỐT ===
- Nút "Chốt kịch bản": xuất Excel (sheet Kich ban: STT | Cảnh | Thoại; kèm các sheet Ghi chu, Nhan vat, Boi canh, Hook) và nạp thẳng vào Bước 1.
- Từ lúc chốt, QUY TẮC TỐI THƯỢNG về lời thoại áp dụng: thoại bị khóa 100%.
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
