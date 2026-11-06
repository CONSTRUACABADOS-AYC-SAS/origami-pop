# PERMISSION DECLARATIONS — Origami Pop (05h)

Nguồn: merged manifest (prodDebug) + `compliance_census.md` trục API nhạy cảm + grep source, ngày 2026-10-29.

## Kết luận: **KHÔNG có permission/foreground-service nào đòi video demo hay declaration form bắt buộc** ("no sensitive-API video required")
Đã grep và KHÔNG thấy: `BIND_ACCESSIBILITY_SERVICE`, `SYSTEM_ALERT_WINDOW`, `QUERY_ALL_PACKAGES`, `PACKAGE_USAGE_STATS`, `BIND_NOTIFICATION_LISTENER_SERVICE`, `BIND_VPN_SERVICE`, `BIND_DEVICE_ADMIN`, `READ/SEND_SMS`, `READ/WRITE_CALL_LOG`, location (fine/coarse/background), `MANAGE_EXTERNAL_STORAGE`, `READ_MEDIA_*`, `RECORD_AUDIO`, `USE/SCHEDULE_EXACT_ALARM`, và `android:foregroundServiceType` = 0 kết quả; `startForeground` trong `app/src/main` = 0. ⇒ không cần `permission_videos/*.mp4`, không cần `permission_videos.json`, không cần host video.

## Foreground service permissions
- Không có type. Merged manifest chỉ có `android.permission.FOREGROUND_SERVICE` (normal, do thư viện kéo vào). Console "Foreground service permissions": khai **"My app doesn't use foreground service permissions"** nếu form hiện. *Khuyến nghị gỡ hẳn bằng `tools:node="remove"` (xem policy_audit R1).*

## Sensitive / runtime permissions (không cần declaration form — ghi để Console & reviewer đối chiếu)
| Permission | Feature người dùng thấy | Mô tả quyền (dán nếu Console hỏi) | Video | Listing |
|---|---|---|---|---|
| `android.permission.CAMERA` | Màn "Snap your model": chụp ảnh thành phẩm sau khi gấp xong, lưu vào Creations. Có rationale + runtime request; từ chối vẫn dùng "From gallery". `<uses-feature camera required=false>` | "Camera is used only when the user taps Take photo to photograph their finished origami. Photos are stored on the device and are never uploaded." | Không bắt buộc (runtime permission thường) | "Take a photo with the in-app camera or choose one from your gallery…" · "The camera is only used if you choose to photograph your finished model." |
| `android.permission.POST_NOTIFICATIONS` | Nhắc gấp hằng ngày (người dùng bật/tắt + chọn giờ trong Profile) + thông báo FCM | "Shows the optional daily folding reminder the user schedules, and occasional app messages." | Không (Play không đòi) | "Notifications are optional and only used for the daily folding reminder and occasional app messages" |
| `android.permission.RECEIVE_BOOT_COMPLETED` | Khôi phục lịch nhắc sau khi máy khởi động lại (`BootReceiver`) | "Re-schedules the daily reminder after the device restarts." | Không | như trên |
| `android.permission.VIBRATE` | Rung nhẹ khi gấp | normal | — | — |
| `AD_ID`, `ACCESS_ADSERVICES_*` | Quảng cáo/attribution | Khai Advertising ID trên Console (App content → Advertising ID = Yes: Advertising / Analytics) | — | "supported by ads" |
| `READ_BASIC_PHONE_STATE`, `com.amazon.privacypass.ATTEST`, `com.samsung.android.mapsagent.permission.READ_APP_INFO`, `com.google.android.finsky.*`, `c2dm.RECEIVE` | do thư viện ads/Firebase | không phải feature của app | — | — |
| `com.android.vending.BILLING`, `CHECK_LICENSE` | hạ tầng module-ads; KHÔNG có màn mua | không khai IAP | — | — |

## To REMOVE (thừa)
- `FOREGROUND_SERVICE` — không feature nào dùng (R1).
- `com.android.vending.BILLING` — chỉ là hạ tầng thư viện ads; giữ vì `AppPurchase` của module-ads cần (FRICTIONLESS_ACCESS §3) — KHÔNG khuyến nghị gỡ.

## Advertising ID (Console)
App dùng Advertising ID ⇒ App content → Advertising ID: **Yes**, mục đích Advertising + Analytics (đã có `AD_ID` trong manifest).

## TODO người làm
Không có video cần quay. Chỉ cần trả lời các form Console theo bảng trên (Advertising ID, Foreground service = không dùng, Data safety = import CSV).
