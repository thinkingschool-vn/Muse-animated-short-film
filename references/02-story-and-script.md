# Giai đoạn 2 — Thảo luận & Chốt kịch bản (Story Bible)

## Mục đích
Biến ý tưởng mơ hồ của user thành một **Story Bible** duy nhất, chốt cứng mọi thứ trước khi vẽ một nét nào. Đây là "hiến pháp" của phim — mọi prompt sau này đều trích từ đây.

## Lưu ý từ Giai đoạn 0
- Số cảnh đã chốt từ công thức thời lượng (thời lượng ÷ 10, làm tròn lên). Không hỏi lại "mấy cảnh" — chỉ xác nhận.
- Ngôn ngữ thảo luận và VO = ngôn ngữ đã chọn ở Giai đoạn 0.

## Prompt thảo luận (hỏi user, từng câu một, ngắn gọn)
1. Phim về ai? (nhân vật chính, tuổi, tính cách, ngoại hình)
2. Câu chuyện một câu là gì? (logline: ai + muốn gì + vật cản + cái giá) — *ads:* thông điệp chính + CTA (xem `09-ads-mode.md`)
3. Thế giới diễn ra ở đâu? Phong cách hình ảnh? (2D vẽ tay, 3D hoạt hình, anime...)
4. VO ngôn ngữ gì? Giọng kể chuyện hay nhân vật tự thoại? (khuyến nghị: người kể chuyện — nhân vật không nói trong clip)
5. Nhạc: mood + có điểm nhấn ở đâu? (1 bài cho cả phim)
6. Cảm xúc từng cảnh đi lên hay xuống? Đâu là đỉnh cảm xúc?

## Template STORY.md (viết xong đưa user duyệt)
```markdown
# <TÊN PHIM> — Story Bible
*Phong cách (2D/3D — khớp character sheet), số cảnh, tỷ lệ, ngôn ngữ VO, chế độ: Phim | Quảng cáo*

## Logline
<1 câu>

## Characters (CANONICAL LOCK — chép nguyên văn phần NGOẠI HÌNH vào mọi prompt)
- **<TÊN> (vai):** NGOẠI HÌNH (dùng trong prompt, không tên, tuổi viết bằng chữ): <...>
  · Tính cách + arc (chỉ để tham khảo, không đưa vào prompt)

## World & Style Lock
<Thế giới, phong cách hình ảnh mô tả bằng tính từ (không tên hãng/studio), tỷ lệ + độ phân giải>

## Audio Plan
- Nhạc: 1 bài — <mood, BPM nếu biết, điểm nhấn>
- VO: <giọng>, ngân sách âm tiết/cảnh
- SFX: chỉ tiếng động từ clip; nhân vật không nói

### Scene N — "<Tên cảnh>"
- Beat: <cảm xúc chủ đạo>
- Hình (KHÔNG chứa chữ): first frame <...> · last frame <...> · shot <cỡ cảnh>
- Nối sang cảnh N+1: MATCH | CUT | TRANSITION
- VO: "<câu thoại>" (<số âm tiết>)
- Overlay (chữ thêm khi dựng): "<...>" hoặc —
```

## Quy tắc chốt
- Phần **Characters (CANONICAL LOCK)** phải viết đủ chi tiết để paste nguyên văn vào prompt mà không cần sửa.
- Mỗi cảnh bắt buộc có first frame + last frame mô tả rõ + kiểu nối — nguyên liệu cho keyframes.
- **Ý nghĩa bằng chữ** (học phí, slogan, ngày, %) chỉ nằm ở VO hoặc Overlay — không bao giờ ở cột Hình. Ví dụ sai: "tường ghi chữ 'học phí'". Đúng: Hình "tường đá xám cao" + Overlay/VO nói về học phí.
- VO mỗi cảnh trong ngân sách âm tiết (`03-audio-first.md`); số cảnh/độ dài cảnh chốt lại sau khi đo VO thật.
- User duyệt STORY.md xong mới sang giai đoạn 3. Đổi kịch bản sau khi đã vẽ = làm lại từ đầu.
