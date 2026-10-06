# Post-mortem — Quảng cáo "Thinking Uni" (60s, 9:16, tiếng Việt)

Phim giới thiệu Thinking University + nhắc đăng ký ngày ra mắt 11/10, làm bằng skill v1. Phân tích file `thinking-uni-60s-9x16.mp4` bằng `scripts/qc.py final` + nhận dạng giọng nói. Đây là lý do có bản v2.

## Kết quả QC bản v1

| Hạng mục | Đo được | Đánh giá |
|---|---|---|
| Loudness | −26,3 LUFS (LRA 12,1) | ❌ quá nhỏ (chuẩn −14) |
| Khoảng hụt tiếng | 20,03–21,31s · 30,05–30,62s | ❌ nối audio thô |
| Nhạc | 6 khối phổ khác nhau, đổi tại mỗi điểm cắt | ❌ 6 bài nhạc |
| Lời lạ | 40,1s *"Chúng ta cùng nhau, học hỏi và chia sẻ kiến thức nhé!"* · 53,8s *"Welcome to the community platform."* · 57,4s *"We did it together."* | ❌ nhân vật tự nói, đè VO |
| Độ rõ VO câu CTA | 0,25 (nghe ra *"…mắc kệ mũi thằng Mùi"*) | ❌ thông tin quan trọng nhất bị mất |
| Mối nối | SSIM 0,33 · 0,10 · 0,18 · (flash) · 0,12 | ❌ định match-cut nhưng lệch cả 5 |
| Flash | 40,1–41,5s, đúng lúc hiện "100% MIỄN PHÍ" | ⚠️ thông điệp chính bị chìm |
| Chữ lạ do model | "Disney/PIXAR" (3 clip phải render lại), "Linh 24", "HỌC TẬP SÁNG TẠO" | ⚠️ |
| Overlay | Chữ mặc định, đè đầu nhân vật, URL sát đáy (vùng UI che) | ⚠️ |

## Gốc rễ → bản vá v2

| Gốc rễ (v1) | Bản vá (v2) |
|---|---|
| Mega prompt có `SOUND / MUSIC / VO` → mỗi clip tự sinh nhạc + lời | Prompt video chỉ SFX + AUDIO RULES; 1 nhạc nền; 1 track VO (`05-mega-prompts.md`, `03-audio-first.md`) |
| Lệnh dựng `amix` giữ nguyên tiếng gốc + VO, không duck, không crossfade, không loudnorm | `scripts/assemble.py`: sidechain ducking, xfade, loudnorm 2 lượt −14 LUFS, chặn VO chồng nhau |
| VO đặt cứng "+1s mỗi cảnh" | Audio-first: đo VO → bảng CUES → chia cảnh |
| Match-cut dựa vào keyframe vẽ trước, model trôi | MATCH = chain từ frame cuối thật (`qc.py lastframe`); hoặc CUT/TRANSITION có chủ đích |
| "Disney/Pixar", tên + tuổi, chữ trên tường trong prompt | Vệ sinh prompt + TEXT BAN; chữ chỉ là overlay |
| Agent tự khai "liền mạch, on-model" | `qc.py clips` / `qc.py final` + ảnh `boundaries.jpg` ở mọi chốt duyệt |
| Không có khái niệm quảng cáo | `09-ads-mode.md`: hook 3s, CTA đọc rõ + end card, vùng an toàn 9:16, lexicon thương hiệu |

## Làm lại quảng cáo này với v2 — gợi ý
1. Giữ character sheets (đã ổn), viết lại prompt bỏ "Disney/Pixar", bỏ tên/tuổi.
2. VO_SCRIPT có cột TTS input: "mười một tháng mười", "một trăm phần trăm"; thử 2–3 cách đọc "Thinking University" + round-trip.
3. 1 bài nhạc không lời ~62s, nhấn ở 10s (cửa mở) và 50s (CTA).
4. Cảnh 1 mở bằng hook mạnh (cận mặt + tia sáng) thay vì wide tối.
5. Mối c1→c2 MATCH (chain), các mối còn lại CUT/TRANSITION.
6. Dựng bằng `templates/edit.example.json`; end card c6 ≥ 4s, URL ở y≈0,68.
