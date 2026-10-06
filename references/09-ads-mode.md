# Chế độ Quảng cáo (Ads mode)

Bật khi mục tiêu là **quảng bá** sản phẩm/sự kiện/đăng ký (VD: giới thiệu Thinking Uni + nhắc đăng ký ngày ra mắt 11/10). Mọi giai đoạn vẫn chạy như phim, cộng thêm các luật dưới đây — luật ở đây **ưu tiên hơn** khi mâu thuẫn.

## 1. Câu hỏi bổ sung ở Giai đoạn 0
1. **Hành động mong muốn (CTA)** chính xác là gì? (đăng ký / mua / tải app) + **ngày/giá/ưu đãi** bắt buộc phải có.
2. **Link/kênh** để người xem hành động (hiển thị trên màn hình; VO chỉ nói "link bên dưới").
3. **Nền tảng:** TikTok / Reels / Shorts / Facebook feed → quyết định vùng an toàn, độ dài.
4. **Tên thương hiệu đọc thế nào?** (tiếng Anh hay phiên âm Việt) → đưa vào lexicon.
5. **3 thông điệp chính** tối đa. Nhiều hơn = người xem không nhớ gì.

## 2. Khung kịch bản

**60 giây**
| Đoạn | Thời gian | Việc phải làm |
|---|---|---|
| HOOK | 0–3s | Hình mạnh + câu hỏi/khẳng định gây tò mò. Có chữ hook. Không mở bằng cảnh tối, chậm, nhân vật nhỏ xa |
| VẤN ĐỀ | 3–10s | Nỗi đau người xem nhận ra mình |
| THƯƠNG HIỆU | trước giây 12 | Tên + lời hứa cốt lõi (VO + overlay) |
| LỢI ÍCH | 12–42s | 2–3 thông điệp, mỗi cái 1 câu VO + 1 overlay ngắn |
| CẢM XÚC / BẰNG CHỨNG | 42–50s | Cộng đồng, kết quả, ưu đãi (VD "100% miễn phí") |
| CTA | 50–60s | VO đọc **chậm, rõ** hành động + ngày; end card tĩnh ≥ 4s: CTA + ngày + link |

**30 giây:** Hook 0–3 · Vấn đề + thương hiệu 3–10 · 2 lợi ích 10–22 · CTA 22–30.
**15 giây:** Hook + thương hiệu 0–4 · 1 lợi ích 4–10 · CTA 10–15.

## 3. Nhịp hình
- Shot 2–5s (phim kể chuyện thì 6–10s). Mỗi clip 10s generate có thể cắt thành 2 shot bằng in/out trong `edit.json`.
- Mỗi 3–5s phải có thay đổi thị giác: cut, đổi cỡ cảnh, overlay mới, chuyển động máy.

## 4. Âm thanh quảng cáo
- VO phủ 70–85% thời lượng; khoảng lặng dài nhất ≤ 3s (trừ beat cảm xúc có chủ đích).
- Câu CTA: riêng 1 câu, ~3,5 âm tiết/giây, `gain_db` +1 đến +2, nhạc hạ sâu bên dưới.
- Viết số/ngày bằng chữ trong TTS input: "mười một tháng mười", "một trăm phần trăm".
- Nhạc: 1 bài, có điểm nhấn ở lúc hiện thương hiệu và lúc CTA; kết thúc gọn (fade ≤ 2,5s).
- Loudness −14 LUFS (assemble.py làm sẵn).

## 5. Chữ trên màn hình (overlay)
| Luật | Chi tiết |
|---|---|
| Vùng an toàn 9:16 | Tránh 14% trên, 24% dưới, 14% bên phải (UI tên kênh, caption, nút like/share). assemble.py tự kẹp khi dùng `y` |
| Độ dài | ≤ 6–7 chữ mỗi thẻ; hiện tối thiểu `0,8s + 0,3s × số chữ` |
| Kích thước | Tiêu đề ≥ 60px, chữ phụ ≥ 38px (trên khung cao 1280) |
| Font | Có đủ dấu tiếng Việt (Be Vietnam Pro, Montserrat, Inter) |
| Vị trí | Không đè mặt nhân vật; chọn shot có khoảng trống (yêu cầu ngay từ keyframe: "leave clean empty space in the upper third") |
| Thời điểm | Không đặt trên đoạn flash/cháy sáng (qc.py báo); hiện cùng lúc VO nói thông điệp đó |
| URL | Đặt giữa khung (y≈0,65), không ở đáy; VO không đọc URL |

## 6. Từ điển thương hiệu (bắt buộc với ads)
Lập bảng **Hiển thị ↔ TTS input** cho mọi tên riêng, số, ngày, ký hiệu (mẫu ở `03-audio-first.md`). Chạy round-trip: nhận dạng giọng nói trên file VO phải ra đúng tên thương hiệu và ngày.

## 7. Checklist ads trước khi giao
- [ ] 3 giây đầu có hook (hình + chữ/câu hỏi)
- [ ] Tên thương hiệu xuất hiện (nghe + thấy) trước giây 12
- [ ] CTA: VO rõ (qc.py ≥ 0,6) + end card ≥ 4s có ngày + link, nằm trong vùng an toàn
- [ ] Không có giọng thứ hai / lời lạ; nhạc 1 bài liền mạch
- [ ] −14 LUFS; nghe thử trên loa điện thoại
- [ ] Không có chữ do model tự vẽ; mọi chữ là overlay đã duyệt
