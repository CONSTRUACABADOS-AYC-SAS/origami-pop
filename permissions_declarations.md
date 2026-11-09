# Permission declarations — Origami Pop (05h)

Nguồn: merged manifest `app/build/intermediates/merged_manifests/prodDebug/processProdDebugManifest/AndroidManifest.xml` (xem `compliance_census.md` § 4).

**Kết luận:** app **không** có permission nào thuộc diện form declaration bắt buộc của Play (không foreground-service-type, không All-files, không Accessibility/VPN/overlay/Usage-stats/QUERY_ALL_PACKAGES/SMS/Call-log/exact-alarm/location). Vẫn lập mục cho permission runtime mà reviewer có thể hỏi (CAMERA) và ghi rõ quyền không cần video.

## Foreground service permissions
- Không có `android:foregroundServiceType` nào (grep merged manifest = 0). `FOREGROUND_SERVICE` do thư viện kéo vào; app không có service foreground nào. **Console → App content → Foreground service permissions:** chọn "app không dùng foreground service" / trả lời "không có tác vụ foreground" (không tính năng nào cần).

## Sensitive permissions
### CAMERA (runtime)
- Feature: **Take photo** ở màn Congrats → màn "Snap your model" (`features/camera/CameraScreen.kt`, CameraX, ảnh vuông) để chụp thành phẩm, lưu vào Creations / Share.
- Declaration form: **không có form riêng** trên Play cho CAMERA (chỉ Data Safety + mô tả). Nếu hỏi: tính năng tuỳ chọn, không phải core.
- Mô tả quyền (dán nếu cần): *"Origami Pop dùng Camera chỉ khi người dùng bấm 'Take photo' để chụp tác phẩm gấp giấy của mình. Ảnh được xử lý và lưu trên thiết bị, không tải lên máy chủ; người dùng có thể từ chối và dùng 'From gallery' thay thế."*
- Video demo: **AI đã quay** `permission_videos/camera.mp4` (102 s, 540 px, 840 KB; emulator `re2`, airplane mode, bản build `com.construacabados.orgami.pop`).
  - Cảnh: 1) màn Home điện thoại → mở app drawer → bấm icon "Origami Pop"; 2) Language → English (United States) → xác nhận; 3) Onboarding → Home; 4) Resume Crown → chọn bước 26 → Finish & celebrate; 5) Congrats → Take photo; 6) hộp thoại giải thích in-app → "Got it"; 7) hộp thoại hệ thống "Allow … to take pictures" → While using the app; 8) viewfinder camera chạy thật.
  - Direct link sau khi host vào repo store public (`tools/video_publish.sh --app <dir> --storeCode a021`): Pages `https://construacabados-ayc-sas.github.io/origami-pop/permission_videos/camera.mp4` · raw `https://raw.githubusercontent.com/CONSTRUACABADOS-AYC-SAS/origami-pop/main/permission_videos/camera.mp4` — **trạng thái: CHƯA host** (orchestrator publish cùng landing/legal; ghi trong `permission_videos.json`).
  - Listing đối chiếu (full_description): "The Camera permission is used only if you choose to photograph your finished fold; photos stay on your device unless you share them."
- Real-device-only: không (camera ảo của emulator cho viewfinder).

### POST_NOTIFICATIONS (runtime, Android 13+)
- Feature: nhắc gấp hằng ngày (tuỳ chọn) + FCM. Play **không** đòi declaration/video. Listing: "The Notifications permission is used for your optional daily reminder and for messages from us".

### RECEIVE_BOOT_COMPLETED (normal)
- Không cần form. `core/BootReceiver` đặt lại nhắc sau khi khởi động. Listing: "the app restores the reminder after your phone restarts".

### AD_ID / ACCESS_ADSERVICES_* (ads)
- **App content → Advertising ID = Yes** (Advertising or marketing + Analytics). Không cần video. Listing nói rõ ads + Advertising ID.

## To REMOVE (thừa, không feature dùng)
- Không đề xuất gỡ permission nào của app. `com.android.vending.BILLING` + `CHECK_LICENSE` đến từ thư viện ads (kiểm cờ ad-free), không có IAP; không gỡ dependency lib ads (ngoại lệ hạ tầng FRICTIONLESS §3).

## Việc người dùng (Console, không làm hộ được)
1. App content → Data safety → Import `data_safety.csv`.
2. App content → Advertising ID = Yes.
3. (Tuỳ chọn) nếu Console hỏi lý do CAMERA: dán mô tả trên + link video khi đã host.
