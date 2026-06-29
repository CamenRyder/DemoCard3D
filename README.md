# MindAR Demo 01 — Scan thẻ bài → mô hình 3D động bám theo thẻ

Demo web AR thuần (Three.js + MindAR). Quét một thẻ bài bằng camera → hiện model 3D
có animation, **bám dính và xoay theo thẻ**. Nghiêng/xoay thẻ trong không gian để
thấy các mặt khác nhau của model 3D.

## Chạy thử (bắt buộc HTTPS hoặc localhost vì web cần quyền camera)

```bash
cd demo01
npx serve            # hoặc: python3 -m http.server 8000
```

Mở `http://localhost:3000` (serve) hoặc `http://localhost:8000` trên trình duyệt,
bấm **▶️ Bật AR**, cho phép dùng camera.

> Muốn test trên điện thoại: máy tính và điện thoại cùng mạng LAN, mở `https://<IP-máy-tính>:<port>`.
> Camera trên mạng LAN thường cần HTTPS — dùng `npx local-ssl-proxy` hoặc deploy lên Netlify/Vercel/GitHub Pages.

## Thẻ để scan (asset mẫu)

Mở/in ảnh thẻ mẫu này rồi chĩa camera vào nó:
https://cdn.jsdelivr.net/gh/hiukim/mind-ar-js@1.2.5/examples/image-tracking/assets/card-example/card.png

(Có thể mở ảnh trên 1 màn hình khác và quét trực tiếp.)

## Thay bằng thẻ bài & model của bạn

1. **Tạo file target riêng:** vào https://hiukim.github.io/mind-ar-js-doc/tools/compile/
   → upload ảnh thẻ bài của bạn → tải về `targets.mind` → đặt vào thư mục này.
   - Ảnh nên rõ nét, nhiều chi tiết, ít vùng trơn/lặp lại để track tốt.
2. **Chuẩn bị model:** file `.glb` có sẵn animation, đặt tên `model.glb` vào thư mục này.
3. Trong `index.html`, sửa 2 hằng số:
   ```js
   const TARGET_MIND = './targets.mind';
   const MODEL_GLB   = './model.glb';
   ```
4. Tinh chỉnh `model.scale`, `model.position`, `model.rotation` cho khớp kích thước thẻ.

## "Xem các mặt khác nhau" — demo này làm theo kiểu nào?

Kiểu **nghiêng thẻ, model 3D quay theo**: model được gắn vào hệ tọa độ của thẻ
(`anchor.group`), nên khi bạn nghiêng/xoay thẻ thật, model nghiêng/xoay theo và lộ ra
mặt bên, mặt sau của *model*. Không cần code thêm.

> Nếu sau này muốn kiểu **lật mặt trước/sau thẻ ra nội dung khác**: compile 2 ảnh
> (trước + sau) vào cùng 1 file `.mind`, rồi thêm `mindarThree.addAnchor(1)` cho mặt sau.

## Lỗi thường gặp

- **Không xin được camera:** chưa chạy qua https/localhost, hoặc trình duyệt chặn quyền.
- **Không nhận thẻ:** ảnh target ít chi tiết, thiếu sáng, hoặc thẻ nghiêng quá ~60°.
- **Model quá to/nhỏ hoặc lệch:** chỉnh `scale` / `position` trong `index.html`.
