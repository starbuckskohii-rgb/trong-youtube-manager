# Prompt remix tool Google Flow: Excel → Video (Gemini Omni 1.1)

Thứ tự dùng trong Trình tạo công cụ (Flow → Công cụ → Remix), ô "Bạn muốn tạo gì?":

1. Dán **Prompt chính (Phần 1)**, chờ tool dựng xong.
2. Dán **Phần 2: Preset phong cách "Slow English 3D"** ở tin nhắn tiếp theo. Phần 2 ghi đè một số mặc định của Phần 1 (style, tốc độ nói, trường JSON).
3. Dán **Phần 3: Nhập transcript / kịch bản thô** nếu muốn tool tự chuyển transcript thành bảng STT | Cảnh | Thoại.
4. Dán **Phần 4: Thẻ nhân vật dùng ảnh của bạn + Nhập kịch bản có cấu trúc** (mẫu Ella), rồi **Phần 4b** (quy tắc đọc bổ sung).
5. Dán **Phần 5: Tự đồng bộ trang phục** để ảnh tải lên mặc đồ khác kịch bản vẫn ra đúng trang phục.
6. Dán **Phần 6: Lớp an toàn chính sách** để giảm số clip bị Google chặn và tự thử lại khi bị chặn.

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

## Phần 4: Thẻ nhân vật dùng ảnh của bạn + Nhập kịch bản có cấu trúc

Mẫu kịch bản đầy đủ: `samples/ella-halloween-moon-moth.txt`. Bản đã chuyển sang Excel: `samples/ella-halloween-moon-moth.xlsx`.

````
Nâng cấp tool thêm 2 phần: (A) thẻ nhân vật dùng ảnh tham chiếu do người dùng tải lên, (B) nhập kịch bản theo mẫu "Kịch bản có cấu trúc". Giữ nguyên mọi quy tắc về lời thoại.

=== A. THẺ NHÂN VẬT & ẢNH THAM CHIẾU CỦA NGƯỜI DÙNG ===
1. Mỗi nhân vật là một thẻ. Thẻ được tạo tự động từ danh sách "Nhân vật:" trong kịch bản (tên + mô tả), hoặc thêm tay.
2. Trong mỗi thẻ, người dùng tự tải ảnh của mình lên (kéo thả hoặc chọn file, PNG/JPG/WebP, nhiều ảnh):
   • Ảnh nhận diện (bắt buộc ít nhất 1): chính diện; khuyến nghị thêm góc 3/4 và toàn thân, nền trơn.
   • Ảnh theo trang phục (tùy chọn): mỗi trang phục một ô ảnh riêng (vd "Ella – Moon Fairy", "Ella – Moon Moth").
   • Thay, xóa, sắp xếp ảnh bất kỳ lúc nào. Đổi ảnh thì Character Bible được đánh dấu "cần cập nhật".
3. Character Bible = Gemini xem ẢNH của người dùng (ngoại hình) + đọc MÔ TẢ trong kịch bản (tính cách → cách diễn xuất, vd "vui tính" → nhanh nhẹn, biểu cảm phóng đại). Ngoại hình luôn theo ảnh, không bịa chi tiết trái với ảnh. Người dùng sửa và khóa được.
4. Nút "Chuyển về style" là TÙY CHỌN: chỉ dùng khi ảnh chưa đúng phong cách; ảnh gốc luôn được giữ lại.
5. Thư viện "Đạo cụ": tải ảnh đạo cụ quan trọng (vd cúp mặt trăng, áo choàng bạc). Cảnh nào nhắc tới đạo cụ thì tự gắn ảnh đạo cụ đó.
6. Khi gọi Omni, gắn ảnh tham chiếu theo thứ tự ưu tiên: người nói → nhân vật khác trong khung → ảnh trang phục đúng cảnh → ảnh bối cảnh → đạo cụ; tối đa MAX_REFS_PER_CLIP (hằng số, mặc định 5). Cảnh có hơn 3 nhân vật: chỉ gắn ảnh người nói + tối đa 2 người ở tiền cảnh, những người còn lại đứng ở hậu cảnh mờ.
7. Tag trong prompt dùng mã không dấu cách: "Ms. Rose" → @MsRose. Giao diện vẫn hiện tên thật.

=== B. NHẬP KỊCH BẢN THEO MẪU "KỊCH BẢN CÓ CẤU TRÚC" ===
Thêm tab "Kịch bản có cấu trúc" ở màn hình nhập: dán văn bản hoặc tải .txt / .docx (đọc phần chữ). Mẫu (rút gọn):

ELLA AND THE HALLOWEEN MOON MOTH
The Halloween Ball – A Family Story
Nhân vật:
* Ella: Cô bé yêu thích thời trang, sáng tạo và giàu cảm xúc.
* Ms. Rose: Giáo viên tổ chức đêm hội Halloween.
HỒI 1 – CHIẾC VÁY ÁNH TRĂNG
00:00–01:44 | Chọn váy và chuẩn bị Halloween
Cảnh 001 – Halloween Surprise [HÀI HƯỚC]
Hành lang trường học trang trí bí ngô và dơi giấy. Oliver bất ngờ nhảy ra sau chiếc bí ngô lớn, đeo mặt nạ ma ngộ nghĩnh.
Oliver: Boo! Happy Halloween, Ella!
Cảnh 024 – The Terrible Tear [KHÔNG THOẠI]
Ella bước tiếp. Áo choàng bị giữ lại đột ngột và rách thành một đường dài. Tiếng vải xé vang lên, Ella dừng lại sững sờ.
Cảnh 050 – The Halloween Lesson
Cả gia đình đứng trước khu trang trí bí ngô. Ella cầm cúp, Oliver đứng bên cạnh trong bộ đồ bí ngô ngộ nghĩnh.
Mom: Remember: Be kind. Ask for help. Try a new idea.
Oliver: Next year, I will be a glowing pumpkin!
Cả gia đình bật cười. Máy quay lùi dần, kết thúc bằng ánh đèn Halloween lung linh.
THE END
TỪ VỰNG TIẾNG ANH CUỐI TẬP
Costume – Trang phục hóa trang.
Bài học: Do not hurt others because you are jealous. When things go wrong, stay calm, ask for help, and use your creativity.

QUY TẮC ĐỌC (viết bằng code, không để AI đoán):
1. Dòng 1 = tên tập, dòng 2 = phụ đề.
2. Sau "Nhân vật:": mỗi dòng "* Tên: mô tả" tạo một thẻ nhân vật. Tên có thể có dấu chấm và dấu cách ("Ms. Rose").
3. "HỒI n – Tên hồi" mở một hồi. Dòng ngay sau, dạng "mm:ss–mm:ss | tóm tắt", là mốc thời lượng mục tiêu của hồi.
4. "Cảnh NNN – Tiêu đề [TAG]" mở một cảnh. Chấp nhận gạch "-", "–", "—". Có thể có 0, 1 hoặc nhiều tag.
5. Trong cảnh: dòng bắt đầu bằng "Tên nhân vật:" (tên có trong danh sách Nhân vật, so khớp tên dài nhất trước) là LỜI THOẠI. Chỉ tách ở dấu ":" ĐẦU TIÊN sau tên, phần còn lại giữ nguyên văn (vd "Mom: Remember: Be kind." → thoại là "Remember: Be kind."). Các dòng khác là MÔ TẢ CẢNH. Mô tả nằm sau lời thoại cuối cùng là hành động "Sau thoại" (vd cả nhà cười, máy quay lùi dần).
6. "THE END" kết thúc phần truyện. "TỪ VỰNG…" mở danh sách từ vựng, mỗi dòng "Từ – nghĩa". "Bài học:" là bài học của tập.
7. Dòng dạng "Tên: …" mà Tên không có trong danh sách → cảnh báo "Người nói lạ", hỏi người dùng thêm nhân vật hay coi là mô tả.

CHUYỂN THÀNH BẢNG STT | Cảnh | Thoại:
- STT = số cảnh 3 chữ số dạng chữ ("001"). Cột Cảnh = "[Hồi n – Tên hồi] Cảnh NNN – Tiêu đề [TAG]" + xuống dòng + mô tả (+ "Sau thoại: …" nếu có). Cột Thoại = các dòng "Tên: lời thoại" nguyên văn, đúng thứ tự.
- Cảnh nhiều người nói mà ước tính dài hơn MAX_CLIP_SECONDS → đề xuất tách theo người nói thành 049a, 049b… (máy quay vào người nói của từng phần; "Sau thoại" thuộc phần cuối). Người dùng bấm duyệt. Cảnh một người nói mà quá dài → chỉ cảnh báo.
- Lưu kèm các cột phụ: Hồi, Tiêu đề, Tag, Người nói, Thời lượng ước tính.

TAG ĐIỀU KHIỂN CÁCH DIỄN:
- [HÀI HƯỚC]: diễn tinh nghịch, biểu cảm phóng đại, nhịp hài; khung hình rộng hơn để thấy hành động; cho phép hiệu ứng âm thanh vui nhẹ (vẫn không có nhạc nền).
- [KỊCH TÍNH]: diễn chậm và căng; close-up hoặc push-in chậm vào khuôn mặt; ánh sáng tương phản hơn một chút nhưng vẫn trong preset.
- [KHÔNG THOẠI]: không ai nói. Khối DIALOGUE = "No one speaks." + SOUND EFFECTS lấy từ mô tả (vd "Tiếng vải xé vang lên" → "a loud, long fabric ripping sound"); cho phép phản ứng không lời (thở hắt, tiếng "oh" ngạc nhiên của đám đông). Thời lượng mặc định 6 giây, chỉnh được.
- Tag khác: giữ trong cột Cảnh và gửi cho Gemini như gợi ý cảm xúc.

BÁO CÁO SAU KHI ĐỌC:
- Số hồi, số cảnh, số câu thoại, số cảnh không thoại, số cảnh đã tách.
- Lỗi cấu trúc: số cảnh trùng, nhảy số, cảnh không có mô tả, người nói lạ.
- Kiểm tra độ phủ bằng code: số câu thoại trong văn bản gốc = số câu trong bảng, từng câu khớp từng ký tự, đúng thứ tự. Hiển thị "Thoại: 50/50 nguyên văn ✓".
- Bảng thời lượng theo hồi: mục tiêu (từ mốc mm:ss) / ước tính / chênh lệch. Thiếu thời lượng → gợi ý thêm cảnh mở bối cảnh hoặc cảnh phản ứng không thoại; người dùng tự quyết.
- Cảnh có hơn 3 nhân vật → cảnh báo như mục A.6.
- Gemini kiểm tra logic liên tục (CHỈ cảnh báo, không sửa thoại): vd nhân vật xuất hiện trước cảnh "bước vào", trang phục đổi mà không có cảnh thay đồ.
- Gemini gợi ý dòng thời gian trang phục cho từng nhân vật (vd Ella: đồ đi học → Moon Fairy → áo choàng rách → Moon Moth) và các ô ảnh trang phục nên tải.

TỪ VỰNG & BÀI HỌC:
- Không biến thành lời thoại (không bịa câu nói). Xuất ra danh sách thẻ từ vựng (CSV) để chèn khi dựng, và bài học cho thẻ kết / mô tả YouTube.
- Nút tùy chọn "Thêm cảnh từ vựng": người dùng tự gõ câu tiếng Anh nhân vật sẽ nói; sau khi lưu, câu đó bị khóa như thoại thường.

Nút "Tải file mẫu": tải về một kịch bản mẫu .txt đúng định dạng trên.
````

## Phần 4b: Bổ sung quy tắc đọc kịch bản có cấu trúc

Rút ra khi chạy thử bản v2 (`samples/ella-halloween-moon-moth-v2.txt`, 56 cảnh, 61 câu thoại): mô tả xen giữa các câu thoại, giọng nói không có lời, mô tả lệch thoại, hồi tưởng, tag [HOOK], cùng địa điểm khác thời điểm.

````
Bổ sung cho phần NHẬP KỊCH BẢN CÓ CẤU TRÚC (giữ nguyên mọi quy tắc cũ):
1. MÔ TẢ XEN GIỮA THOẠI: dòng mô tả nằm giữa hai câu thoại là hành động xảy ra giữa hai câu đó. Lưu đúng thứ tự (trước thoại / giữa thoại / sau thoại). Khi tách cảnh theo người nói, mô tả xen giữa thuộc phần của câu thoại NGAY SAU nó ("Ngay trước câu thoại: …"); mô tả sau câu cuối thuộc phần cuối ("Sau thoại: …"). Mô tả mở đầu cảnh được lặp lại ở mọi phần để giữ bối cảnh. Khi không tách, ghi vào cột Cảnh dạng "Giữa thoại: …".
2. GIỌNG NGOÀI KHUNG HÌNH: hỗ trợ dòng thoại "Tên (ngoài khung hình): câu nói". Người nói không xuất hiện trong khung, không gắn ảnh tham chiếu của họ; prompt ghi: @Tên is heard off-screen saying: "…". Thoại vẫn khóa nguyên văn.
3. CÓ TIẾNG NÓI NHƯNG KHÔNG CÓ LỜI (vd mô tả "Giọng Ms. Rose vang lên, thông báo…" mà không có dòng thoại): cảnh báo vàng "Có tiếng nói nhưng không có lời thoại". Gợi ý thêm dòng "(ngoài khung hình)" hoặc đổi thành âm thanh không rõ lời. KHÔNG BAO GIỜ tự bịa lời.
4. MÔ TẢ LỆCH THOẠI: Gemini so mô tả với thoại cùng cảnh; mâu thuẫn (vd mô tả "giải trang phục sáng tạo nhất" nhưng thoại "Halloween Queen") → cảnh báo và gợi ý sửa MÔ TẢ. Không sửa thoại.
5. HỒI TƯỞNG trong mô tả (vd "nhớ lại cảnh…"): không vẽ hồi tưởng trong cùng clip; diễn bằng biểu cảm. Trong danh sách dựng, gợi ý chèn 1–2 giây clip cũ của cảnh được nhớ lại (dùng lại, không tốn credit).
6. TAG [HOOK]: cảnh mở màn gây tò mò; vẫn tạo video như cảnh thường, máy quay và ánh sáng ấn tượng hơn, nhịp nhanh hơn một chút. Khác với nút "Gợi ý hook" (dùng lại clip cũ).
7. CÙNG ĐỊA ĐIỂM, KHÁC THỜI ĐIỂM (vd hội trường ban ngày và đêm hội): tạo hai bối cảnh riêng (LOC_HALL_DAY, LOC_HALL_NIGHT), mỗi bối cảnh một ảnh tham chiếu riêng.
8. Clip ước tính dưới 4 giây được nâng lên tối thiểu 4 giây.
````

## Phần 5: Tự đồng bộ trang phục khi ảnh tải lên khác kịch bản

````
Nâng cấp: TỰ ĐỘNG ĐỒNG BỘ TRANG PHỤC khi ảnh nhân vật tải lên mặc đồ khác với kịch bản. Nguyên tắc: KỊCH BẢN LÀ CHUẨN về trang phục, ẢNH LÀ CHUẨN về khuôn mặt, tóc, dáng người. Phần này ghi đè mọi chỗ trước đó đưa trang phục vào Character Bible và câu "Characters must look exactly like their reference images".

=== 1. TÁCH NHẬN DIỆN VÀ TRANG PHỤC ===
- Character Bible chỉ mô tả nhận diện: khuôn mặt, mắt, tóc, màu da, dáng người, tuổi. KHÔNG mô tả quần áo.
- Trang phục là danh sách riêng của từng nhân vật (Outfit Bible). Mỗi outfit có: mã, mô tả tiếng Anh cố định, các cảnh sử dụng, ảnh tham chiếu (nếu có). Danh sách outfit lấy từ dòng thời gian trang phục đọc được trong kịch bản.
- Cảnh nào kịch bản không nói về trang phục thì dùng outfit gần nhất trước đó. Nếu chưa có outfit nào thì lấy trang phục trong ảnh tải lên làm outfit mặc định.

=== 2. KIỂM TRA KHI TẢI ẢNH ===
- Mỗi khi người dùng tải ảnh, Gemini mô tả trang phục trong ảnh và so với từng outfit kịch bản yêu cầu.
- Hiển thị bảng "Trang phục": mỗi hàng là một outfit (vd "Ella – Moon Fairy, cảnh 014–023"), các cột: Mô tả theo kịch bản | Ảnh đang có | Trạng thái (✓ Khớp / ⚠ Khác trang phục / ✗ Chưa có ảnh).
- Dòng ⚠ ghi rõ khác ở đâu (vd "ảnh mặc váy hồng, kịch bản cần váy xanh đậm + áo choàng bạc") và liệt kê các cảnh bị ảnh hưởng.

=== 3. TỰ SỬA BẰNG ẢNH TRANG PHỤC MỚI ===
- Nút "Tạo ảnh trang phục đúng kịch bản" cho từng dòng ⚠/✗, và nút "Sửa tất cả". Dùng model tạo/sửa ảnh của Flow, lấy ảnh nhận diện của người dùng làm gốc: GIỮ NGUYÊN khuôn mặt, tóc, màu da, dáng người, phong cách vẽ; CHỈ thay quần áo theo mô tả outfit; nền trơn; xuất 1 ảnh toàn thân chính diện + 1 ảnh góc 3/4.
- Trang phục biến đổi theo cốt truyện (vd váy sạch → bị bẩn; áo choàng nguyên → rách → thành đôi cánh) thì tạo NỐI TIẾP: ảnh trạng thái sau được sửa từ ảnh trạng thái trước, để vẫn là đúng bộ đồ đó.
- Người dùng xem và chọn "Duyệt" hoặc "Tạo lại". Chỉ ảnh đã duyệt mới được dùng. Ảnh gốc của người dùng không bao giờ bị ghi đè.
- Trước khi chạy hàng loạt, hiện số ảnh sẽ tạo và hỏi xác nhận (tốn credit).

=== 4. KHI TẠO VIDEO ===
- Với mỗi nhân vật trong cảnh: gắn ảnh ĐÃ DUYỆT của đúng outfit cảnh đó, thay cho ảnh gốc.
- Outfit chưa có ảnh duyệt: KHÔNG gắn ảnh gốc toàn thân (model sẽ chép luôn quần áo trong ảnh). Thay vào đó tự cắt ảnh chỉ lấy khuôn mặt và tóc (cắt bằng canvas theo khung mặt Gemini xác định), và hiện cảnh báo vàng "Chưa có ảnh trang phục" trên dòng đó.
- Template prompt đổi thành:
  CHARACTERS:
  @{Tên}: {identity bible}. OUTFIT IN THIS SHOT: {mô tả outfit nguyên văn}.
  Thay câu RULES cũ về ảnh tham chiếu bằng: "Match faces, hairstyles and body proportions to the reference images. Clothing must follow the OUTFIT lines exactly, even if a reference image shows different clothing."

=== 5. KIỂM TRA SAU KHI TẠO ===
- Sau mỗi clip, Gemini xem khung hình giữa clip, so trang phục từng nhân vật với outfit yêu cầu. Khớp → "Trang phục ✓". Lệch → tô cam "Sai trang phục: …" và gợi ý "Tạo lại".

=== 6. CHỌN CHUẨN CHO TỪNG OUTFIT ===
- Mỗi outfit có công tắc "Chuẩn theo": Kịch bản (mặc định) / Ảnh. Chọn "Ảnh" thì mô tả outfit được viết lại theo quần áo trong ảnh, bỏ cảnh báo ⚠, và tool không tạo ảnh mới cho outfit đó.
````

## Phần 6: Lớp an toàn chính sách trước khi tạo video

````
THÊM "LỚP AN TOÀN CHÍNH SÁCH" TRƯỚC KHI GỌI OMNI. Tuyệt đối không sửa lời thoại.

Bối cảnh: Gemini viết prompt và bộ lọc an toàn của model video là hai hệ thống riêng. Bộ lọc kiểm tra cả prompt, ẢNH THAM CHIẾU và VIDEO ĐẦU RA, nên prompt do Gemini viết vẫn có thể bị chặn. Có nhân vật trẻ em thì bộ lọc chặt hơn nhiều.

1. KIỂM TRA TỪ NGỮ BẰNG CODE (trước khi gửi), chỉ sửa trong PHẦN HÌNH ẢNH:
- Mô tả da, cơ thể, trang phục ôm khi cảnh có nhân vật trẻ em (smooth skin, bare skin, body, figure, proportions, subsurface, fitted, bodice, tight, tights, legs, slim, curves) → bỏ, hoặc thay bằng mô tả trang phục chung (vd "costume dress", "striped knee socks"). Mô tả màu da (vd "light olive skin tone") thì giữ.
- Bạo lực, nguy hiểm (blood, wound, knife, stab, attack, hit, injured, fall hard, scissors cutting near a person) → diễn đạt nhẹ (vd "carefully trims the fabric hem on the table", "stumbles but is fine").
- Chất lỏng đỏ dính trên quần áo (red juice, red liquid) → "a large pink fruit-punch stain" để không giống máu.
- Kinh dị (scary, horror, creepy, haunted, skull, zombie, demon, blood-red) → "cozy, friendly Halloween decorations: smiling pumpkins, paper bats, soft purple and orange lights".
- Bắt nạt, cảm xúc quá mạnh ở trẻ em (bully, hate, humiliate, revenge, crying hysterically) → "feels hurt", "tears up", "looks jealous".
- Tên hãng, studio, người thật (Pixar, Disney, DreamWorks, Ghibli, tên người nổi tiếng) → bỏ, dùng "high-end 3D animated feature-film look".
- Từ gợi hiện thực (photorealistic, realistic skin, real child, live-action) → bỏ.
- Từ rủi ro nằm trong LỜI THOẠI: KHÔNG sửa, chỉ tô vàng "Thoại có từ dễ bị chặn: …" để người dùng tự quyết.

2. VIẾT LẠI AN TOÀN BẰNG GEMINI, chỉ cho phần hình ảnh: giữ nguyên nội dung cảnh; đặt trong khung "wholesome, family-friendly animated story"; tả xung đột nhẹ nhàng; không nhấn vào thân thể nhân vật; không thêm hay bớt nhân vật.

3. STYLE BIBLE BẢN AN TOÀN (dùng khi cảnh có nhân vật trẻ em), thay đoạn VISUAL STYLE cũ bằng:
VISUAL STYLE: Wholesome, family-friendly 3D animated feature-film look. Stylized cartoon characters with large expressive eyes and soft rounded shapes, soft matte stylized shading, detailed hair, cozy fabric textures. Clean, richly detailed everyday environments. Bright, warm colors, soft light, gentle depth of field. Eye-level camera, static or slow push-in, 16:9 frame. Expressive, natural acting with precise lip-sync. Not photorealistic, not live-action. Clean image: no text, no subtitles, no captions, no logos, no watermark.

4. KHI BỊ CHẶN:
- Lưu nguyên văn thông báo lỗi. Phân loại: prompt bị chặn / ảnh tham chiếu bị chặn / video đầu ra bị chặn / lỗi "prominent people" / không rõ.
- Tự thử lại theo bậc, dừng ở bậc đầu tiên thành công:
  (a) gửi lại y nguyên 1 lần (bộ lọc video đầu ra có tính ngẫu nhiên);
  (b) gửi bản đã viết lại an toàn;
  (c) bỏ ảnh tham chiếu, chỉ dùng mô tả chữ, để biết lỗi có phải do ảnh không;
  (d) dừng, tô đỏ "Cần sửa tay" kèm chẩn đoán.
- Ghi lại bậc nào thành công cho từng dòng.

5. NÚT "KIỂM TRA ẢNH NHÂN VẬT": với mỗi nhân vật, tạo 1 clip thử 4 giây (nhân vật đứng vẫy tay, không thoại, có gắn ảnh tham chiếu). Báo ĐẠT / BỊ CHẶN cho từng nhân vật trước khi chạy hàng loạt. Hỏi xác nhận vì tốn credit.

6. BÁO CÁO: số clip bị chặn theo từng loại lỗi, những từ hoặc ảnh hay gây chặn nhất.
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

Khi mọi câu thoại bị gán cho @NARRATOR, tên người nói nằm lẫn trong lời thoại, cột cảnh hiện "UNTITLED SCENE":

````
SỬA LỖI ĐỌC THOẠI (lỗi nghiêm trọng). Hiện tại khi nạp Excel, mọi dòng bị gán @NARRATOR và tên người nói nằm lẫn trong lời thoại, vd: @NARRATOR: "Ms. Rose: Tonight, one girl will become our Halloween Queen!". Narrator sẽ đọc to cả chữ "Ms. Rose", sai kịch bản. Cột "Bối cảnh & hành động" hiện "UNTITLED SCENE" và "..." dù Excel có mô tả cảnh. Thời lượng cũng bị tính dư (vd dòng 001 ra 10s, đúng ra khoảng 7s).

NGUYÊN NHÂN CẦN SỬA: không được chỉ nhận tên người nói khi tên đó trùng một nhân vật ĐÃ TẢI ẢNH. Lúc nạp Excel chưa có thẻ nhân vật nào, nên mọi dòng rơi về NARRATOR. Tên có dấu chấm và dấu cách như "Ms. Rose" cũng phải nhận được.

1. TÁCH NGƯỜI NÓI (chạy ngay khi nạp file, không phụ thuộc ảnh):
- Chuẩn hóa ô Thoại: Unicode NFC, đổi "\r\n" thành "\n", đổi khoảng trắng không ngắt (U+00A0) thành dấu cách, đổi dấu hai chấm toàn khổ "：" thành ":". Tách ô thành từng dòng, bỏ dòng trống.
- Mỗi dòng khớp mẫu: đầu dòng là TÊN (1–3 từ viết hoa chữ cái đầu, được có dấu chấm, dấu nháy, gạch nối, vd "Ms. Rose", "Mr. Lee", "Oliver"), tùy chọn "(ghi chú)", rồi dấu ":" ĐẦU TIÊN, rồi lời thoại. Regex JavaScript gợi ý (cờ u, nhận cả tên có dấu như "Bà Lan"): /^\s*(\p{Lu}[\p{L}\p{N}'.\-]*(?:\s+\p{Lu}[\p{L}\p{N}'.\-]*){0,2})\s*(?:\(([^)]*)\))?\s*:\s*(.+)$/u
- Phần sau dấu ":" đầu tiên là lời thoại, giữ NGUYÊN VĂN (chỉ bỏ khoảng trắng đầu/cuối). Dấu ":" về sau thuộc lời thoại (vd "Mom: Remember: Be kind." → người nói Mom, thoại "Remember: Be kind.").
- Ghi chú "(ngoài khung hình)" → người nói không xuất hiện trên hình, chỉ nghe giọng.
- Danh sách nhân vật = danh sách "Nhân vật:" của kịch bản (nếu có) + thẻ đã tạo + MỌI tên người nói tìm thấy trong Excel. Tên mới thì tự tạo thẻ nhân vật trống, gắn nhãn "Chưa có ảnh".
- Tên tìm được nhưng không có trong danh sách "Nhân vật:" của kịch bản → tô vàng "Người nói lạ: X?" để người dùng xác nhận (phòng trường hợp câu thoại không có tên mà bắt đầu bằng "Remember:").
- Dòng không có tên người nói → tô vàng "Chưa rõ người nói", người dùng chọn. KHÔNG tự gán NARRATOR. Chỉ dùng NARRATOR khi Excel ghi đúng "Narrator:" hoặc người dùng tự chọn.
- Ô Thoại trống → cảnh không thoại, không có người nói.

2. HIỂN THỊ: mỗi câu một dòng dạng @ELLA: "Ivy, I love the stars on your hat!". Tên người nói KHÔNG được nằm trong dấu ngoặc kép. Ô có 2 câu thì hiện 2 dòng.

3. PROMPT VIDEO: mỗi câu ghi @{Tên} says: "{lời thoại}" (người nói ngoài khung hình: @{Tên} is heard off-screen saying: "{lời thoại}"). Kiểm tra thoại 100% so khớp trên lời thoại ĐÃ TÁCH, và báo lỗi nếu bên trong dấu ngoặc kép còn tiền tố "Tên:".

4. CỘT CẢNH: dòng đầu ô Cảnh dạng "[Hồi n – Tên hồi] Cảnh NNN – Tiêu đề [TAG]…" → tách ra Hồi, Tiêu đề, Tag; các dòng còn lại là mô tả. Ô Cảnh không có dòng đầu kiểu này → tiêu đề "Cảnh {STT}", toàn bộ ô là mô tả. Cột "Bối cảnh & hành động" hiện tiêu đề + mô tả tiếng Việt đọc từ Excel; prompt tiếng Anh do Gemini viết thì hiện ở phần mở rộng. Không hiện "UNTITLED SCENE" khi Excel có dữ liệu.

5. THỜI LƯỢNG: tính trên lời thoại ĐÃ TÁCH (không tính tên người nói): số từ ÷ 1,7 + 0,5 giây mỗi lần ngắt câu + 1,5 giây đệm, tối thiểu 4 giây; cảnh không thoại mặc định 6 giây. Nếu Omni chỉ nhận một số mức cố định thì làm tròn LÊN mức gần nhất trong hằng số SUPPORTED_DURATIONS (mặc định [4, 6, 8, 10]). Rê chuột vào thời lượng thì hiện cách tính.

6. NÚT "TỰ KIỂM TRA ĐỌC THOẠI": chạy các ca sau, hiện ĐẠT/LỖI từng ca:
- "Ms. Rose: Tonight, one girl will become our Halloween Queen!" → người nói Ms. Rose (@MsRose); thoại "Tonight, one girl will become our Halloween Queen!"
- "Ivy: But... I made something special too." → Ivy; "But... I made something special too."
- "Mom: Remember: Be kind. Tell the truth. Help each other." → Mom; "Remember: Be kind. Tell the truth. Help each other."
- "Ms. Rose (ngoài khung hình): Ivy, you are next!" → Ms. Rose, ngoài khung hình; "Ivy, you are next!"
- "Ruby: What?\nMaya: Huh?" → 2 câu: Ruby "What?", Maya "Huh?"
- Ô trống → không có thoại, không có người nói.
- Ô Cảnh "[Hồi 1 – HAI CÔ BÉ, MỘT CHIẾC VƯƠNG MIỆN] Cảnh 001 – The Halloween Crown [HOOK]\nHội trường trường học được trang trí…" → Hồi 1, tiêu đề "The Halloween Crown", tag HOOK, mô tả "Hội trường trường học được trang trí…"
- Thời lượng: dòng 001 = 7s (8s nếu làm tròn theo SUPPORTED_DURATIONS); dòng 004 = 6s; dòng 005 (không thoại) = 6s.

Sau khi sửa, nạp lại file Excel và đọc lại toàn bộ 66 dòng theo quy tắc mới.
````

Khi giao diện bước 1 bị lệch, bị cắt, trống trơn:

````
SỬA LỖI GIAO DIỆN BƯỚC 1 "KỊCH BẢN" (ưu tiên cao). Giữ nguyên toàn bộ logic đọc thoại vừa sửa.

LỖI HIỆN TẠI:
- Khung tải file bị lệch sang trái và bị cắt mất phần bên trái (mất góc bo trái, không thấy tiêu đề/mô tả), chỉ còn 2 nút "Tự kiểm tra đọc thoại" và "Chọn file Excel".
- Khung chỉ rộng khoảng nửa màn hình, phần còn lại trống; bên dưới không có gì.
- Không thấy các cách nhập "Kịch bản có cấu trúc" và "Transcript thô".

YÊU CẦU BỐ CỤC:
1. Vùng nội dung chính: width 100%, max-width 1200px, căn giữa (margin: 0 auto), padding 24px hai bên (16px khi màn hình hẹp), box-sizing: border-box. Bỏ mọi margin âm, transform/translate, position absolute/fixed, width cố định bằng px hoặc 100vw đang làm khung nhập bị tràn hoặc lệch. Phần tử cha không được overflow: hidden làm cắt nội dung.
2. Khung nhập kịch bản rộng hết vùng nội dung, có viền và bo đủ 4 góc. Bên trong:
   - Tiêu đề "Nhập kịch bản" + một dòng mô tả ngắn; chữ đủ tương phản trên nền tối (tối thiểu 4.5:1).
   - 3 tab: "File Excel" | "Kịch bản có cấu trúc" | "Transcript thô".
     • File Excel: vùng kéo-thả file (viền nét đứt, cao khoảng 180px) + nút "Chọn file Excel" + link "Tải file Excel mẫu".
     • Kịch bản có cấu trúc: ô dán văn bản lớn (cao ít nhất 320px) + nút "Tải .txt" + nút "Đọc kịch bản" + link "Tải file mẫu".
     • Transcript thô: ô dán văn bản + nút "Tải .txt/.srt" + nút "Chuyển thành kịch bản".
   - Nút "Tự kiểm tra đọc thoại" ở góc phải trên của khung, kiểu nút phụ nhưng chữ rõ ràng.
3. Chưa nạp gì: dưới khung nhập hiện hướng dẫn 3 bước ngắn (Nạp kịch bản → Tải ảnh nhân vật ở bước 2 → Tạo video ở bước 3).
4. Sau khi nạp: hiện ngay bên dưới (a) thanh báo cáo: số cảnh, số câu thoại, "Thoại: x/x nguyên văn ✓", các cảnh báo vàng; (b) bảng cảnh rộng hết vùng nội dung, cột Shot | Bối cảnh & hành động | Hội thoại | Thời lượng; chữ tự xuống dòng, không tràn ngang.
5. Kết quả "Tự kiểm tra đọc thoại" hiện trong một hộp ngay dưới khung nhập (danh sách ĐẠT/LỖI), không bật ra ngoài màn hình.
6. Responsive: ở chiều rộng 375px, 768px, 1280px, 1920px không có thanh cuộn ngang, không phần tử nào bị cắt; nhóm nút dùng flex-wrap để xuống hàng khi thiếu chỗ.
7. Giữ nguyên header (logo, huy hiệu SLOW ENGLISH 3D, 4 bước, ENGINE OMNI 1.1 FLASH) và theme tối hiện tại.

Sau khi sửa, rà lại CSS của bước 1 và liệt kê ngắn gọn những thuộc tính đã gây lệch/cắt.
````

## Mẫu file Excel

| STT | Cảnh | Thoại |
|-----|------|-------|
| 1 | Tom bước vào bếp buổi sáng, Anna đang pha cà phê | Tom: Good morning, Anna. |
| 2 | Anna quay lại, mỉm cười, đưa Tom cốc cà phê | Anna: Good morning, Tom. Would you like some coffee? |
| 3 | Cận cảnh Tom cầm cốc cà phê, gật đầu | Tom: Yes, please. Thank you. |
