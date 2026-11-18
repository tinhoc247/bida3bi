# Bida 3 Bi (3 băng) 🎱 (game độc lập)
Chỉ cần `index.html` + `vk.js` (bàn phím ảo) cùng thư mục – mở trực tiếp / Netlify / GitHub Pages đều được.

- **Chơi với máy**: dùng được ngay, không cần mạng.
- **Online 2 người**: dùng chung Firebase với game bida 8 bi (dòng `const K=` / `DB=` trong `index.html`), cùng Rules:
```json
{ "rules": { "bida": { "rooms": { "$code": { ".read": "auth != null", ".write": "auth != null" } } } } }
```
Luật: bi của bạn chỉ cần chạm cả 2 bi kia là được 1 điểm và đánh tiếp (không yêu cầu ăn băng).

## Danh sách người chơi online + lời mời (mới)
- Tên chỉ nhập **một lần** (lưu trong trình duyệt), sau đó hiện "Xin chào, <tên>" và nút **Đổi tên**.
- Ở chế độ **2 người** có danh sách **🟢 Người chơi đang online**, có ô **Tìm theo tên**. Bấm **Mời** → tự tạo phòng và gửi lời mời; bên kia thấy hộp **Đồng ý / Từ chối**.
- Cần thêm rule Firebase này **bên trong** `"bida": { ... }` (cạnh `"rooms"`), rồi bấm Publish:
"online": {
  "$g": {
    ".read": "auth != null",
    "$uid": {
      ".write": "auth != null && auth.uid === $uid",
      "n": { ".validate": "newData.isString() && newData.val().length <= 40" },
      "t": { ".validate": "newData.isNumber()" },
      "s": { ".validate": "newData.isNumber()" },
      "inv": {
        ".write": "auth != null",
        ".validate": "newData.hasChildren(['f','n','r','t']) && newData.child('n').isString() && newData.child('n').val().length <= 40 && newData.child('r').isString() && newData.child('r').val().length <= 10"
      },
      "$other": { ".validate": false }
    }
  }
}

## Cập nhật giao diện (bàn mới)
- **Bàn mới**: thành gỗ đỏ có chấm trắng, băng xanh, nỉ xanh sáng ở giữa, lỗ có nắp kim loại, vạch đầu bàn.
- **Tâm ngắm**: vòng bi ảo có dấu **+** ở tâm, **chấm đỏ** ở điểm chạm, đường vàng = hướng bi bị chạm, đường trắng ngắn = hướng bi trắng lệch đi. Cây cơ vuốt nhọn, kéo lùi khi lấy lực.
- **Bi đặt sẵn ở vị trí chuẩn**: không được dời bi ở cú đầu tiên (đã bỏ chế độ kéo bi). Vạch đầu bàn chỉ hiện trước cú đánh đầu tiên, sau đó biến mất.
- **Bi lăn mượt hơn**: nội suy vị trí giữa các bước vật lý, làm mượt thời gian khung hình, canvas nét theo màn hình Retina, bi xoay theo đúng quãng đường đã đi, bi rơi lỗ có hiệu ứng (bản 8 bi).

## Âm thanh
Tiếng bi va nhau, tiếng đánh bi, cú phá (bida 8 bi) và tiếng reo hò khi thắng dùng mẫu âm thật nhúng sẵn trong `index.html` (không cần file ngoài). Nếu trình duyệt chưa giải mã xong, game dùng âm tổng hợp như trước.

## Tiếng vỗ tay khi ghi điểm (mới)
- Ngay khi bi của bạn **chạm đủ 2 bi kia** (lúc va chạm thứ hai, không đợi bi dừng hẳn) là phát **tiếng vỗ tay ~2,6 giây**, âm lượng vừa phải (mẫu nhúng sẵn trong `index.html`, khoá `cheer` của `SMP`; chỉnh to/nhỏ ở `g.gain.value=.38` trong hàm `clap()`). Ghi điểm liên tiếp thì tiếng cũ tắt nhanh rồi phát lại, không chồng to lên. Nút 🔊 tắt tiếng như cũ.
- Dòng **"Băng: x • Đã chạm: y/2"** đã chuyển ra **ngoài bàn** (ngay dưới bàn) để không che bi.
