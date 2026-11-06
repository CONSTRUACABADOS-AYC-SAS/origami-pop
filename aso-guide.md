# REVIEW NOTES — Origami Pop (05h) · Play Console "App access" / "Testing instructions"

**Marketing URL (landing)**: https://construacabados-ayc-sas.github.io/origami-pop/ (landing do orchestrator publish — URL deterministic; hiện trả trang mục lục legal HTTP 200)
**Privacy Policy**: https://construacabados-ayc-sas.github.io/origami-pop/privacy/ · **Terms**: https://construacabados-ayc-sas.github.io/origami-pop/terms/ (Support URL = trang legal của store)
**Developer / contact**: CONSTRUACABADOS AYC SAS · santanaleticia3168661@gmail.com
**Package**: com.paper.folding.origamipop

## App access
Toàn bộ chức năng dùng được KHÔNG cần tài khoản/mã/thanh toán: không có màn đăng nhập, không paywall, mọi mẫu mở sẵn. Không cần credential test hay mã ghép đôi (không có tính năng nhiều người).

## Định vị (a)
Origami Pop là app hướng dẫn gấp giấy (origami · kirigami · 3D origami) dành cho người thích thủ công mọi lứa tuổi — người mới, phụ huynh gấp cùng con, giáo viên. Mục tiêu: gấp xong một mẫu mà không bị lạc bước. Pain-point: ảnh tĩnh từng bước thường không đủ nên người gấp bị kẹt; Origami Pop cho **mỗi bước một clip video cắt đúng đoạn**, xem lại một chạm, kèm lưới nhảy bước.

## Khác biệt (b)
- Clip video cắt theo từng bước (nút "Watch this step", thời lượng đúng đoạn) thay vì một video dài hoặc ảnh tĩnh.
- Mọi mẫu mở sẵn, không đăng nhập; không khoá bằng quảng cáo.
- Lưới chọn bước (thumbnail) + Basic Folds guide (6 kiểu gấp) + game thư giãn "Mini Fold" (250 level) + thư viện ảnh thành phẩm tự chụp (lưu local).
- Selling-point trong listing ("Stuck on a step? Watch that clip") đồng hook với định vị trên.

## Hướng dẫn test (c)
1. Mở app → funnel Splash / chọn ngôn ngữ / onboarding → Home (không paywall). Có thể có hộp thoại xin quyền thông báo (tuỳ chọn, bấm Allow/Don't allow đều được).
2. **Discover** → chọn một mẫu (vd *Cute Cat*, 20 bước, 20 clip) → **Start fold**. Ở Viewer: bấm **Next step**; bấm **Watch this step** để xem clip của đúng bước (cần mạng để tải ảnh/clip — nếu offline, ảnh/clip chưa mở sẽ không tải được); bấm ô lưới (góc phải dưới dải thumbnail) để nhảy bước; bấm **?** để xem Basic Folds.
3. Đi tới bước cuối → **Finish & celebrate** → **Take photo** (cấp quyền CAMERA khi được hỏi; hoặc **From gallery**) → **Save to Creations**; xem tại tab **Creations**.
4. Tab **Game** (nút bướm giữa thanh dưới) → **Play level 1**: vuốt/bấm 4 mũi tên để gấp 4 góc → lộ hình → +10 coin.
5. Profile → bật/tắt Daily reminder, Dark mode, Language.
Ghi chú: app hiển thị quảng cáo AdMob (có consent UMP ở EEA/UK); coin trong game là tiền ảo kiếm miễn phí, KHÔNG có giao dịch tiền thật.

## Permissions
CAMERA (chụp ảnh thành phẩm, tuỳ chọn) · POST_NOTIFICATIONS + RECEIVE_BOOT_COMPLETED (nhắc hằng ngày, tuỳ chọn). Không có quyền nhạy cảm cần video demo — xem `permissions_declarations.md`.

## Liên hệ
santanaleticia3168661@gmail.com — CONSTRUACABADOS AYC SAS.
