# Prompt tạo ảnh nhân vật: Ivy và Ms. Rose (cùng style với Ella)

Dùng trong Flow, ở phần tạo ảnh. Ghép **KHỐI STYLE CHUNG** với prompt của từng nhân vật.

- **Ảnh nhận diện:** gắn ảnh 3 góc nhìn của Ella làm ảnh tham chiếu **phong cách**.
- **Ảnh trang phục:** gắn ảnh nhận diện của **chính nhân vật đó**, không gắn ảnh Ella.

## Khối style chung (dán đầu mọi prompt nhận diện)

```
Character turnaround sheet in exactly the same art style as the reference image: high-end 3D animated feature-film look, wholesome family-friendly look, soft rounded stylized forms, soft matte stylized shading and soft rosy cheeks, detailed strand-based hair, realistic fabric textures (knit, cotton, tartan, felt). Three full-body views of the SAME character side by side on a pure white seamless background, evenly spaced and at the same scale: front view, side profile facing right, back view. Neutral relaxed standing pose, arms slightly away from the body, empty hands. Soft even studio lighting with soft contact shadows. No text, no lettering, no labels, no watermark.
```

## Ivy – ảnh nhận diện (đồng phục, cảnh 002–006)

```
Character: Ivy, Ella's classmate. Same age and same height as the girl in the reference image, drawn in the same cartoon style (large head, big eyes), but clearly a different girl. Do not copy the reference girl's face, hair color, braids or freckles.
Face: slightly narrower heart-shaped face with a small pointed chin, dark brown almond-shaped eyes with long lashes, thin arched eyebrows, light olive skin, no freckles, small neat nose.
Expression: confident, slightly proud closed-mouth smile, one eyebrow a little raised.
Hair: sleek straight jet-black hair with blunt straight bangs, pulled into a high ponytail tied with a purple satin ribbon bow; a small hand-made purple felt star clip above her left ear.
Outfit: the same school uniform as the reference: navy blue short-sleeve knit cardigan with a cream Peter Pan collar and a small orange shield crest with a book icon on the chest, green-and-navy tartan pleated skirt, white knee-high socks, black Mary Jane shoes.
Personal touch: a lavender backpack decorated with small hand-stitched silver star patches.
```

## Ivy – trang phục Halloween (cảnh 015–056)

Gắn ảnh nhận diện Ivy vừa chọn, không gắn ảnh Ella.

```
Use the attached Ivy turnaround as the only character reference. Keep her face, skin tone, eyes, hair (high jet-black ponytail, straight bangs, purple ribbon bow) and cartoon style exactly the same. Change only the clothing.
Halloween costume: a cute purple-and-black witch costume dress with short puffed sleeves and tiny silver star embroidery, a knee-length layered black tulle skirt with a jagged hem, purple-and-black striped knee socks, black ankle boots with small silver buckles.
On her head: a hand-made tall purple felt witch hat with a wide floppy brim, a black band, hand-sewn silver stars with visible stitches, and a thin purple ribbon chin strap tied under her chin.
Same layout: front view, side profile facing right, back view, pure white seamless background, same lighting. No text.
```

## Mũ phù thủy của Ivy – 3 trạng thái (đạo cụ)

```
Prop sheet in the same 3D animated feature-film style, pure white seamless background, soft studio lighting. Three versions of the SAME hand-made witch hat side by side, same size and angle:
1) Intact: tall purple felt witch hat, wide floppy brim, black band, hand-sewn silver stars with visible stitches, thin purple ribbon chin strap.
2) Damaged: the same hat, the chin strap's stitching has come loose on one side so the strap hangs down, and the tip of the hat droops sideways.
3) Repaired: the same hat, the strap re-attached with a shiny silver voile ribbon tied in a neat bow on the side.
No text, no labels.
```

Dùng: 1 cho cảnh 002–033, 2 cho cảnh 034–039, 3 cho cảnh 040–056.

## Ms. Rose – ảnh nhận diện (đồ đi dạy, cảnh 001–003)

```
Character: Ms. Rose, a kind school teacher in her mid-fifties. Same art style as the reference image, drawn as an adult: taller, about one and a half times the height of the children. Do not copy any feature of the reference girl except the art style.
Face: warm medium-brown skin, soft round face with gentle smile lines, warm brown eyes, round thin gold-rimmed glasses, soft natural makeup.
Hair: silver-gray hair in a neat rounded bun, a few soft curls framing the face.
Expression: warm, gentle closed-mouth smile; kind but composed.
Outfit: dusty-rose knit cardigan over a cream blouse with a soft bow at the collar, a small red rose-shaped brooch on the cardigan, mid-calf navy pleated skirt, low-heeled brown loafers, a navy lanyard with a small school ID card showing the orange shield crest with a book icon (no text on the card).
```

## Ms. Rose – trang phục đêm hội (cảnh 045–051)

```
Use the attached Ms. Rose turnaround as the only character reference. Keep her face, skin tone, glasses, silver-gray bun and cartoon style exactly the same. Change only the clothing.
Halloween-night outfit: an elegant long-sleeve plum-purple velvet dress reaching the ankles, a sheer silver shawl printed with small crescent moons draped over her shoulders, the same small red rose brooch pinned on the shawl, small black low-heeled shoes.
Same layout: front view, side profile facing right, back view, pure white seamless background, same lighting. No text.
```

## Vì sao đã bỏ một số từ

Bộ lọc an toàn của Google chặt hơn nhiều khi có nhân vật trẻ em. Các từ mô tả da và cơ thể (smooth skin, subsurface scattering, body proportions), trang phục ôm (fitted bodice, tights) có thể làm prompt hoặc video bị chặn, nên đã được thay bằng mô tả trang phục chung.

## Mẹo khi dùng

1. **Tạo 2–4 phương án rồi chọn bản đẹp nhất.** Bản đã chọn làm gốc cho mọi ảnh trang phục sau, nên khuôn mặt sẽ giữ được giống nhau.
2. **Cắt riêng từng góc nhìn trước khi tải vào thẻ nhân vật** (nhất là ảnh chính diện). Nếu đưa nguyên ảnh có 3 dáng đứng cạnh nhau làm tham chiếu cho video, model đôi khi vẽ ra 3 nhân vật giống hệt nhau trong cùng cảnh.
3. **Xếp ảnh vào đúng ô trong thẻ nhân vật:** ảnh đồng phục vào ô "Ảnh nhận diện"; ảnh Halloween vào ô trang phục tương ứng. Như vậy Phần 5 dùng đúng bộ đồ cho từng cảnh.
