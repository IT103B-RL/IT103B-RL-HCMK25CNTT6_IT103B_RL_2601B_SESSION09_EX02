
### Phân tích lỗi

1. **Lỗi truy cập thuộc tính động qua biến:**

* **Dòng lệnh:** `const basePrice = bookingReservation.priceKey;`
* **Nguyên nhân:** Dùng dot notation khiến JS tìm thuộc tính tĩnh `"priceKey"` (không tồn tại $\rightarrow$ `undefined`) thay vì giá trị của biến `priceKey` (`"roomPrice"`). Dẫn đến `earlySurcharge` và `totalAmount` bị `NaN`. Sửa thành: `bookingReservation[priceKey]`.

2. **Lỗi xóa thuộc tính không triệt để:**

* **Dòng lệnh:** `bookingReservation.discountCode = undefined;`
* **Nguyên nhân:** Gán `undefined` chỉ làm rỗng giá trị nhưng khóa `'discountCode'` vẫn tồn tại trong đối tượng (`'discountCode' in obj` vẫn trả về `true`, vẫn bị duyệt qua trong vòng lặp). Sửa thành: `delete bookingReservation.discountCode;`.

3. **Lỗi truy cập giá trị trong vòng lặp `for...in`:**

* **Dòng lệnh:** `console.log(key + ": " + bookingReservation.key);`
* **Nguyên nhân:** `key` ở đây là biến đại diện cho tên thuộc tính ở mỗi lượt duyệt. Dùng `.key` sẽ tìm kiếm thuộc tính tên là `"key"` (trả về `undefined` cho mọi dòng). Sửa thành: `bookingReservation[key]`.

---

### Bảng Test Cases



| **Trường hợp kiểm thử**                                        | **Dữ liệu đầu vào**                                | **Kết quả sai thực tế**                                                                                     | **Kết quả đúng mong đợi**                                                                                                           |
| ------------------------------------------------------------------------- | ------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------- |
| **TC01:**Voucher không hợp lệ (`isVoucherValid: false`), check-in 9h | `roomPrice: 1500000checkInHour: 9``isVoucherValid: false` | -`totalAmount: NaN`-`'discountCode' in obj`là`true`- In ra: mọi thuộc tính đều có giá trị`undefined` | -`earlySurcharge: 450000`-`totalAmount: 1950000`-`'discountCode' in obj`là`false`- In ra chính xác từng cặp key-value tương ứng |
| **TC02:**Voucher hợp lệ (`isVoucherValid: true`), check-in 14h        | `roomPrice: 2000000checkInHour: 14``isVoucherValid: true` | -`totalAmount: NaN`- In ra: mọi thuộc tính đều có giá trị`undefined`                                      | -`earlySurcharge: 0`-`totalAmount: 2000000`- Giữ nguyên`discountCode: "SUMMER10"`- In ra chính xác từng cặp key-value               |
