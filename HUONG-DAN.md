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
