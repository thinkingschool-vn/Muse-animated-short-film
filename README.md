# 🎬 Muse Animated Short Film

[![GitHub stars](https://img.shields.io/github/stars/sonlovinbot/Muse-animated-short-film?style=social)](https://github.com/sonlovinbot/Muse-animated-short-film/stargazers)
&nbsp;🌐 **Trang giới thiệu: https://app.danghuuson.com/Muse-animated-short-film/**

![The Last Lantern — phim mẫu làm bằng skill này](docs/assets/poster.jpg)

**Tác giả:** Đặng Hữu Sơn ([sonlovinbot](https://github.com/sonlovinbot)) — CEO & Co-Founder LovinBot AI

Skill dựng phim hoạt hình ngắn **từ đầu đến cuối trên Muse AI** — từ một ý tưởng thô thành file MP4 hoàn chỉnh: nhân vật, kịch bản, hình ảnh, video, thuyết minh và dựng phim.

*An end-to-end skill for producing animated short films on Muse AI — from a raw idea to a finished MP4.*

> [!TIP]
> Thấy skill có ích? Bấm **⭐ Star** ở góc trên repo — giúp nhiều người làm phim tìm thấy skill này hơn.

## 🎥 Video giới thiệu

[![Video giới thiệu Muse AI](docs/assets/video-thumbnail.jpg)](https://youtu.be/kysOeBo5Qtw)

Muse AI — tạo ảnh, video, audio — có cảnh từ phim mẫu "The Last Lantern". Xem trên YouTube: https://youtu.be/kysOeBo5Qtw

## Skill này làm gì?

![Quy trình 8 bước](docs/assets/pipeline.svg)

Pipeline v2 — đã kiểm chứng qua "The Last Lantern" (phim) và vá các lỗi từ quảng cáo "Thinking Uni":

| # | Giai đoạn | Kết quả |
|---|-----------|---------|
| 0 | Thu thập đầu vào | Thời lượng, ngôn ngữ, tỷ lệ 16:9/9:16, **chế độ Phim/Quảng cáo** |
| 1 | Character sheets | Khóa nhân vật (nhiều góc + biểu cảm), không chữ |
| 2 | Story bible | Kịch bản: hình (không chữ), kiểu nối từng mối, VO, overlay |
| 3 | **Âm thanh trước** | VO + từ điển phát âm + **1 bài nhạc** → bảng CUES chia cảnh |
| 4 | Keyframes | MATCH / CUT / TRANSITION cho từng mối nối |
| 5 | Mega prompt | Video chỉ có tiếng động — cấm nhạc, cấm lời, cấm chữ |
| 6 | Generate video | Chain frame cuối thật + `qc.py clips` (lời lạ, mối nối) |
| 7 | Dựng phim | `assemble.py`: xfade, overlay tiếng Việt, ducking, −14 LUFS |
| 8 | QC cuối | `qc.py final`: báo cáo số đo + ảnh mối nối |

Kèm theo: **luật realism**, **STRICT ASPECT RATIO LOCK**, **chế độ quảng cáo** và **19 bài học xương máu**.

## 🆕 Có gì mới ở v2

| Lỗi ở v1 (đo trên quảng cáo Thinking Uni) | v2 sửa thế nào |
|---|---|
| Âm thanh chồng nhau — nhân vật tự nói câu ngoài kịch bản đè lên thuyết minh | Prompt video chỉ có SFX; lời chỉ đi qua 1 track VO |
| Nhạc đổi đột ngột mỗi 10 giây, hụt tiếng ở mối nối | 1 bài nhạc cho cả phim, tự hạ khi có lời, crossfade |
| Quá nhỏ tiếng (−26 LUFS) | Chuẩn hóa −14 LUFS như TikTok/Reels/YouTube |
| Câu CTA ngày ra mắt nghe không rõ | Từ điển phát âm, CTA đọc chậm, kiểm tra độ rõ tự động |
| Jump cut ở mọi mối nối | Chain từ frame cuối thật hoặc cắt có chủ đích; đo SSIM |
| Model vẽ chữ lạ ("Disney/PIXAR", "Linh 24") | Vệ sinh prompt + TEXT BAN; mọi chữ là overlay |
| Agent tự báo "liền mạch" mà không đo | `scripts/qc.py` xuất bảng số đo + ảnh mối nối ở mọi chốt duyệt |

Chi tiết: [`examples/thinking-uni/POSTMORTEM.md`](examples/thinking-uni/POSTMORTEM.md)

## 🏮 Tác phẩm mẫu: "The Last Lantern"

Bộ file thật của dự án — mở ra là thấy mỗi giai đoạn trông như thế nào: [`examples/the-last-lantern/`](examples/the-last-lantern/)

| Milo | Lyra | Ember |
|---|---|---|
| ![Milo](docs/assets/characters/milo-sheet.jpg) | ![Lyra](docs/assets/characters/lyra-sheet.jpg) | ![Ember](docs/assets/characters/ember-sheet.jpg) |

| K1 · mở đầu | K2 | K3 | K7 |
|---|---|---|---|
| ![K1](docs/assets/keyframes/k1-city-dusk.jpg) | ![K2](docs/assets/keyframes/k2-spark-falls.jpg) | ![K3](docs/assets/keyframes/k3-ember-sneeze.jpg) | ![K7](docs/assets/keyframes/k7-door-light.jpg) |

| File | Nội dung |
|---|---|
| [`STORY.md`](examples/the-last-lantern/STORY.md) | Story bible: logline, thế giới, character lock, 8 nhịp cảnh, mô-típ nhạc |
| [`MEGA_PROMPTS.md`](examples/the-last-lantern/MEGA_PROMPTS.md) | 8 mega prompt copy-paste + thứ tự nạp ref + ghi chú dựng |
| [`VO_SCRIPT.md`](examples/the-last-lantern/VO_SCRIPT.md) | Thuyết minh Anh – Việt, mốc thời gian |
| [`FLOW.md`](examples/the-last-lantern/FLOW.md) | Dự án đi qua 8 giai đoạn: việc làm, file ra, điểm chốt, chuỗi keyframe |

## Cách dùng (cho người mới)

1. **Cài skill:** copy cả folder này vào thư mục skills của Muse AI, VD: `~/workspace/skills/animated-short-film/`
   - Hoặc: dán link repo này vào chat Muse, Muse sẽ tự cài.
2. **Mở chat và nói:** "Tôi muốn làm phim hoạt hình ngắn."
3. **Trả lời 3 câu hỏi đầu tiên:** phim dài bao lâu? ngôn ngữ nào? tỷ lệ 16:9 hay 9:16?
4. **Duyệt ở các điểm chốt:** mô tả nhân vật → story bible → keyframes → từng video → VO → phim cuối. Bạn chỉ việc xem và nói "ok" hoặc "sửa chỗ này".

Bạn không cần đọc các file reference — Muse tự đọc khi cần. Mọi prompt kỹ thuật, câu lệnh generate và dựng phim đã nằm trong skill.

## Cấu trúc

```
├── SKILL.md                  # Quy trình + luật vận hành (Muse đọc)
├── README.md                 # File này (người đọc)
├── references/               # Hướng dẫn chi tiết từng giai đoạn
│   ├── 01-character-sheets.md
│   ├── 02-story-and-script.md
│   ├── 03-audio-first.md     # VO + nhạc + CUES (làm trước hình)
│   ├── 04-keyframes.md
│   ├── 05-mega-prompts.md
│   ├── 06-video-generation.md
│   ├── 07-assembly.md
│   ├── 08-qc.md
│   ├── 09-ads-mode.md        # Chế độ quảng cáo
│   └── 10-lessons.md
├── scripts/
│   ├── assemble.py           # Dựng phim từ edit.json
│   └── qc.py                 # Kiểm tra chất lượng có số đo
├── templates/
│   └── edit.example.json     # Mẫu edit.json
├── examples/
│   ├── the-last-lantern/     # Tác phẩm mẫu: STORY, MEGA_PROMPTS, VO_SCRIPT, FLOW
│   └── thinking-uni/         # Post-mortem quảng cáo → lý do có v2
└── docs/                     # Trang giới thiệu (GitHub Pages) + ảnh minh hoạ
    └── assets/
```

Scripts cần Python 3.8+ và ffmpeg (bản có libass). Tùy chọn `pip install faster-whisper` để kiểm tra lời nói.

## ⭐ Ủng hộ

Nếu skill giúp bạn làm được bộ phim đầu tiên, hãy **[star repo](https://github.com/sonlovinbot/Muse-animated-short-film)** và chia sẻ phim của bạn. Các phần mềm, game 3D và tài liệu AI miễn phí khác: **[app.danghuuson.com](https://app.danghuuson.com)**

## Giấy phép

MIT — dùng tự do, ghi credit tác giả khi chia sẻ lại. Xem [LICENSE](LICENSE).
