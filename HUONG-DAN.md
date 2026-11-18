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
- **Di chuyển bi trắng lúc bắt đầu**: giữ chuột vào bi rồi kéo (chỉ trong vùng sáng sau vạch đầu bàn), sau đó ngắm và đánh. Online: vị trí được gửi cùng cú đánh nên 2 máy luôn khớp.
- **Bi lăn mượt hơn**: nội suy vị trí giữa các bước vật lý, làm mượt thời gian khung hình, canvas nét theo màn hình Retina, bi xoay theo đúng quãng đường đã đi, bi rơi lỗ có hiệu ứng (bản 8 bi).
