---
name: "animated_short_film"
description: "Skill dựng phim hoạt hình ngắn và quảng cáo hoạt hình từ đầu đến cuối trên Muse AI: thu thập yêu cầu (thời lượng, ngôn ngữ, tỷ lệ, chế độ Phim/Quảng cáo), character sheets, story bible, làm VO + nhạc TRƯỚC (audio-first), keyframes, mega prompt (video chỉ có SFX), generate video có QC đo đạc, dựng tự động bằng scripts/assemble.py (1 nhạc nền, ducking, overlay tiếng Việt, −14 LUFS) và QC cuối bằng scripts/qc.py. Tác giả: Đặng Hữu Sơn. Dùng khi user muốn làm phim hoạt hình ngắn hoặc video quảng cáo hoạt hình trên Muse AI."
---

# Animated Short Film & Ads Pipeline (Muse AI) — v2

**Tác giả:** Đặng Hữu Sơn
**Mô tả:** Từ một ý tưởng thô thành file MP4 hoàn chỉnh: nhân vật, kịch bản, âm thanh, hình ảnh, video, dựng phim và kiểm tra chất lượng có số đo.

## Purpose
Dẫn dắt toàn bộ quy trình sản xuất phim ngắn / quảng cáo hoạt hình trên Muse AI theo pipeline đã kiểm chứng ("The Last Lantern") và đã vá các lỗi phát hiện ở quảng cáo "Thinking Uni" (âm thanh chồng chéo, nhạc đổi mỗi cảnh, jump cut, chữ lạ do model vẽ, CTA nghe không rõ, loudness quá nhỏ).

## 3 luật nền (vi phạm = làm lại)
1. **Âm thanh 3 lớp:** clip video chỉ mang **SFX**; **1 bài nhạc** cho cả phim; **1 track VO**. Prompt video không bao giờ chứa MUSIC / VO / lời thoại.
2. **Không chữ trong hình do model sinh:** prompt hình/video không chứa tên riêng, tên hãng/studio, con số, chữ cần hiển thị. Mọi chữ là overlay ở khâu dựng.
3. **Đo, không tự khai:** mọi lần báo "đạt" phải kèm số đo + ảnh từ `scripts/qc.py`.

## Giai đoạn 0 — Thu thập đầu vào (BẮT BUỘC, hỏi trước mọi thứ)
Hỏi 4 câu. Chưa có đáp án thì chưa sang giai đoạn 1.

### 1. Thời lượng phim mong muốn?
- Mỗi clip video generate tối đa ~**10 giây**. **Số cảnh tạm tính = làm tròn lên (thời lượng ÷ 10)** — ads thường nhiều cảnh ngắn hơn (2–5s/shot, cắt từ clip 10s).
- Số cảnh và độ dài từng cảnh được **chốt lại ở Giai đoạn 3** sau khi đo VO thật.

### 2. Ngôn ngữ chính?
Quyết định ngôn ngữ VO, VO_SCRIPT.md, ngôn ngữ thảo luận. Chữ trên prompt hình luôn bằng tiếng Anh (và không chứa chữ cần hiển thị).

### 3. Tỷ lệ khung hình: 16:9 (ngang) hay 9:16 (dọc)?
- Ràng buộc **toàn bộ ref đầu vào**. Dán nguyên văn vào mọi prompt ảnh/video:
  > **STRICT ASPECT RATIO LOCK:** The final output video MUST be **[16:9 landscape | 9:16 vertical — chọn 1]**. All reference images — character sheets, context sheets, keyframes, first-frame and last-frame images — MUST share this exact aspect ratio. Reference images are not only style and character guides; they directly condition the output framing and composition. Any reference image with a mismatched aspect ratio MUST be regenerated or cropped to the locked ratio before use. NEVER mix aspect ratios within a single project.
- CLI `media-generation`: 16:9 → `--orientation landscape`; 9:16 → `--orientation vertical`.

### 4. Chế độ: Phim kể chuyện hay Quảng cáo?
- **Quảng cáo** (giới thiệu sản phẩm/sự kiện, kêu gọi đăng ký/mua) → đọc `references/09-ads-mode.md` ngay, hỏi thêm: CTA chính xác + ngày/giá, link, nền tảng, cách đọc tên thương hiệu, tối đa 3 thông điệp.

## Workflow
Làm đúng thứ tự. Mỗi giai đoạn có hướng dẫn + checklist trong `references/`. Kết thúc mỗi giai đoạn: đưa user duyệt kèm bằng chứng (ảnh/âm thanh/bảng số đo).

| # | Giai đoạn | File | Đầu ra |
|---|---|---|---|
| 1 | Character sheets | `references/01-character-sheets.md` | Sheet mỗi nhân vật (chính + phụ), đúng tỷ lệ, không chữ |
| 2 | Story bible / Ad script | `references/02-story-and-script.md` | `STORY.md`: hình (không chữ), kiểu nối từng mối, VO, overlay |
| 3 | **Âm thanh trước** | `references/03-audio-first.md` | `VO_SCRIPT.md` + lexicon phát âm, file VO, 1 file nhạc, **bảng CUES** |
| 4 | Keyframes | `references/04-keyframes.md` | Keyframe theo CUES; MATCH/CUT/TRANSITION cho từng mối |
| 5 | Mega prompt | `references/05-mega-prompts.md` | `MEGA_PROMPTS.md`: chỉ SFX, AUDIO RULES, TEXT BAN, end-hold |
| 6 | Generate video + QC clip | `references/06-video-generation.md` | Clip đạt `qc.py clips` (không lời lạ, mối nối đúng loại) |
| 7 | Dựng | `references/07-assembly.md` | `edit.json` → `scripts/assemble.py` → MP4 |
| 8 | QC cuối | `references/08-qc.md` | `qc.py final` PASS + báo cáo gửi user |

> Trước khi bắt đầu dự án mới, đọc `references/10-lessons.md` — 19 bài học xương máu (11 từ "The Last Lantern", 8 từ "Thinking Uni").

## Scripts (Python 3.8+, chỉ cần ffmpeg/ffprobe; `faster-whisper` tùy chọn)
```bash
python scripts/assemble.py edit.json --plan     # xem timeline
python scripts/assemble.py edit.json            # dựng phim
python scripts/qc.py lastframe clip.mp4 out.png # frame cuối thật (chain MATCH)
python scripts/qc.py clips c1.mp4 c2.mp4 ... --aspect 9:16
python scripts/qc.py final final.mp4 --aspect 9:16 --duration 60 --script vo_lines.txt
```
Mẫu `edit.json`: `templates/edit.example.json`. Nếu môi trường thiếu `faster-whisper`, thử `pip install faster-whisper`; không cài được thì báo user phần kiểm tra lời nói phải nghe thủ công.

## Output Contract
- `STORY.md` — đã chốt với user (có kiểu nối từng mối, cột Hình không chứa chữ)
- `character_sheets/`, `keyframes/` — đúng tỷ lệ, không chữ lạ
- `VO_SCRIPT.md` (Hiển thị / TTS input / âm tiết) + `audio/` (VO từng câu + 1 file nhạc)
- `MEGA_PROMPTS.md` — không MUSIC/VO/tên riêng
- `videos/` — clip đã qua `qc.py clips`
- `edit.json` + file MP4 cuối: đúng tỷ lệ, 24fps, −14 LUFS, đã qua `qc.py final`
- `qc_final/qc_report.md` + `boundaries.jpg` — gửi kèm khi giao phim

## Operating Rules
1. **Giai đoạn 0 bắt buộc:** thời lượng + ngôn ngữ + tỷ lệ + chế độ.
2. **STRICT ASPECT RATIO LOCK** cho mọi ref; sai tỷ lệ = vẽ lại.
3. **Khóa nhân vật:** character sheet nạp TRƯỚC TIÊN trong mọi lệnh generate; khóa cả chi tiết dễ trôi của nhân vật phụ (kính, râu, tóc).
4. **Âm thanh 3 lớp** (luật nền 1). Prompt video kết thúc bằng: *"SOUND (SFX only): … AUDIO RULES: No music. No dialogue. No narration. No singing. Characters do not speak."*
5. **Audio-first:** VO + nhạc làm ở Giai đoạn 3, đo thời lượng thật, rồi mới vẽ keyframe. Độ dài cảnh lấy từ bảng CUES.
6. **Mối nối có chủ đích:** mỗi mối là MATCH (chain từ frame cuối thật, SSIM ≥ 0,75), CUT (khác hẳn, SSIM < 0,25) hoặc TRANSITION (xfade). Không để "gần giống".
7. **Không chữ do model sinh** (luật nền 2). Phát hiện chữ lạ = làm lại, không giữ "vì nhìn cũng hợp".
8. **Realism:** mọi hành động vật lý trong MOTION phải đúng cơ chế thực tế (mở cửa kính trước khi châm bấc…).
9. **Dựng bằng assemble.py**, không viết tay lệnh ffmpeg nối/mix — để có ducking, crossfade, loudnorm, kiểm tra VO chồng nhau.
10. **Đo, không tự khai** (luật nền 3): báo user bằng bảng `qc_report.md` + ảnh `boundaries.jpg`. Chưa PASS thì chưa giao; nếu buộc phải giao khi còn WARN, nói rõ từng WARN.
11. Mọi con số user sẽ hành động theo (số cảnh, thời lượng, độ phân giải, LUFS) lấy từ output tool thực tế, không đoán.
