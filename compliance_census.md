# compliance_census — Origami Pop (05h) · Google Play

- app: Origami Pop · displayName `Origami Pop` · applicationId `com.construacabados.orgami.pop` (namespace code `com.paper.folding.origamilab`) · versionName 1.0.0 · targetSdk 36 · minSdk 24
- store: a021 — CONSTRUACABADOS AYC SAS · santanaleticia3168661@gmail.com
- nguồn bằng chứng: `app/src/main/AndroidManifest.xml`, **manifest merged thật** `app/build/intermediates/merged_manifests/prodDebug/processProdDebugManifest/AndroidManifest.xml`, `app/build.gradle.kts`, `gradle/libs.versions.toml`, source `app/src/main/java/**`, `blueprint/legal/URLS.json`, trang Privacy live (curl 2026-11-08).

## 1. IAP / billing
- Grep merged manifest: `com.android.vending.BILLING` **có**; `CHECK_LICENSE` có.
- Grep source: `GlobalApp.kt:42-43` `config.adFreeInAppProductIds = arrayOf(Iap.lifetimeId)` / `adFreeSubscriptionProductIds = arrayOf(Iap.weeklyId, Iap.monthlyId)` (`features/iap/Iap.kt` chỉ khai 3 id, không UI). Grep `launchBillingFlow|queryProductDetails|BillingClient|\.purchase\(` trong `app/src/main/java` → **0 kết quả**. `features/store/CoinStoreScreen.kt` chỉ tiêu coin trong game (`PlayerRepo.buyTheme`), không tiền thật.
- Kết luận: BILLING đến từ thư viện ads (`AppPurchase` kiểm cờ ad-free), **không có màn/nút mua nào** ⇒ KHÔNG phải IAP. Quyết định: **không** viết refund policy, **không** khai IAP trong listing, mô tả **không** nhắc gói/giá, Data Safety **không** khai Purchase history. (Privacy Policy đã nêu đúng: "does not currently offer any purchases".)

## 2. Nội dung do người dùng cung cấp (app-là-công-cụ)
- Không phải app-công-cụ: nội dung (ảnh bước, video clip, dữ liệu mẫu) do chủ app phục vụ từ CDN (manifest BBL CDN, `files/manifest.json`); người dùng không nhập/tải nội dung bản quyền vào app.
- Ảnh người dùng tự chụp (CameraX) / chọn từ thư viện (PhotoPicker) lưu **cục bộ** (Creations), chia sẻ chỉ qua share-sheet do người dùng chủ động. Không host/chia sẻ phía server ⇒ **không cần trang DMCA**.
- Terms (live) có mục nội dung/IP: licence cá nhân phi thương mại; "Photos you take ... remain yours" (đã đọc). Disclosure "user-supplied content/không affiliated" **không áp dụng** (không có ROM/playlist...).

## 3. Data & privacy
- Permission (merged manifest, đã sort): `INTERNET`, `ACCESS_NETWORK_STATE`, `ACCESS_WIFI_STATE`, `CAMERA`, `POST_NOTIFICATIONS`, `RECEIVE_BOOT_COMPLETED`, `VIBRATE`, `WAKE_LOCK`, `FOREGROUND_SERVICE`, `com.google.android.gms.permission.AD_ID`, `ACCESS_ADSERVICES_AD_ID`, `ACCESS_ADSERVICES_ATTRIBUTION`, `ACCESS_ADSERVICES_CUSTOM_AUDIENCE`, `ACCESS_ADSERVICES_TOPICS`, `READ_BASIC_PHONE_STATE`(lib), `com.android.vending.BILLING`, `CHECK_LICENSE`, `com.google.android.c2dm.permission.RECEIVE`, `BIND_GET_INSTALL_REFERRER_SERVICE`, `com.amazon.privacypass.ATTEST`, `com.samsung.android.mapsagent.permission.READ_APP_INFO`, `<pkg>.DYNAMIC_RECEIVER_NOT_EXPORTED_PERMISSION`.
- SDK (libs.versions.toml / build.gradle.kts): Firebase BOM 34.7.0 → **Analytics, Crashlytics, Remote Config, Cloud Messaging**; `com.bbl:module-ads` (AdMob + **mediation AppLovin MAX / Meta Audience Network / ByteDance-Pangle / Vungle** — merged manifest có `com.applovin.mediation`, `com.bytedance.sdk`, `com.vungle.ads`, `com.facebook.ads`); **Adjust 5.7.0**, **Facebook SDK 18.3.0**, **TikTok Business SDK 1.5.0**, **UMP** (consent), Play In-App Review, Media3, CameraX.
- Account/login: **không có** (không SDK auth, không màn đăng nhập) ⇒ không cần delete-account page.
- Dữ liệu rời máy (SDK): Advertising ID/device IDs, IP→vị trí gần đúng, device/app info, tương tác, crash/diagnostics, FCM token, install attribution. Dữ liệu on-device (favourites, tiến độ, coin, ảnh thành phẩm) **không rời máy** ⇒ không khai.
- Privacy Policy live đã đối chiếu: liệt kê đủ các nhóm trên + Adjust/Meta/TikTok/AdMob/Firebase/UMP/Play Review (xem `data_safety.md` § đối chiếu).
- **Lệch cần sửa:** cả Privacy lẫn Terms ghi `package com.paper.folding.origamipop` (package cũ), applicationId hiện tại là `com.construacabados.orgami.pop` → nên cập nhật trang legal (không chặn ASO, nhưng Play/reviewer có thể đối chiếu package).

## 4. API nhạy cảm (grep merged manifest)
| Mẫu grep | Kết quả |
|---|---|
| `BIND_ACCESSIBILITY_SERVICE`/`AccessibilityService` | không |
| `SYSTEM_ALERT_WINDOW` | không |
| `PACKAGE_USAGE_STATS` | không |
| `QUERY_ALL_PACKAGES` | không (chỉ `<queries>` vài package cụ thể: vending, facebook.katana/lite, instagram + intent ads/billing/referrer) |
| `BIND_NOTIFICATION_LISTENER_SERVICE`, `BIND_VPN_SERVICE`, `BIND_DEVICE_ADMIN` | không |
| `READ/SEND_SMS`, `READ_CALL_LOG`, `PROCESS_OUTGOING_CALLS` | không |
| `ACCESS_*_LOCATION`, background location | không |
| `MANAGE_EXTERNAL_STORAGE`, `READ_MEDIA_*` | không (ảnh qua PhotoPicker hệ thống) |
| `RECORD_AUDIO` | không |
| `SCHEDULE_EXACT_ALARM`/`USE_EXACT_ALARM` | không |
| `foregroundServiceType` | **không có** service nào khai type; `FOREGROUND_SERVICE` đến từ thư viện ⇒ không cần form FGS |
| **`CAMERA`** | **có, runtime** — `features/camera/CameraScreen.kt` (CameraX) chụp ảnh thành phẩm; có rationale in-app + "Use gallery instead" |
| **`POST_NOTIFICATIONS`** | có — nhắc hằng ngày + FCM |
| **`RECEIVE_BOOT_COMPLETED`** | có — `core/BootReceiver` đặt lại nhắc sau khởi động |
| **`VIBRATE`** | có — haptic khi gấp |
| `READ_BASIC_PHONE_STATE` | từ thư viện (không cần form) |

→ Không có capability thuộc diện form declaration + video bắt buộc của Play. `CAMERA`/`POST_NOTIFICATIONS` được mô tả trong listing; CAMERA có video minh hoạ phòng khi reviewer hỏi.
- **Call Log/SMS handler-gate:** không có perm ⇒ N/A. **targetSdk 36 (<37)** ⇒ minimize-gate N/A (không dùng READ_CONTACTS/Location/READ_MEDIA).

## 5. Ads
- Có ads (AdMob + mediation + UMP). Listing nói rõ "free to download and contains ads". Play Console: "Contains ads = Yes".
- **Phát hiện policy (ngoài ASO, cần chủ sản phẩm quyết):** màn Rate hiển thị **"Rate us on Google Play — your first rating adds 300 coins"** (`features/congrats/RatePromptDialog.kt`, `claimRateReward`) ⇒ **đánh giá có thưởng** — vi phạm chính sách Google Play về ratings/reviews (cấm khuyến khích/đổi thưởng cho rating). Khuyến nghị: bỏ phần thưởng + câu "adds 300 coins" trước khi phát hành (`policy_audit.md` WARN-1).

## 6. Đối tượng
- Không nhắm trẻ em (Privacy §11: không hướng tới <13); không social/UGC/chat; content-rating dự kiến Everyone. **Không** cần trang `child-safety`. KHÔNG chọn "Designed for Families" (ads SDK).

## 7. Appearance variants
- `design_variants: 1` nhưng code có hệ `AppVariant` (PAPER_ATELIER, SKY_STUDIO, MEADOW, GRAPE_POP) trong `ui/theme/Variants.kt`. **Mặc định khi cài mới: `AppDefaults.VARIANT = 3` ⇒ GRAPE_POP** (`core/AppDefaults.kt:9`; tím #7D2AE7, hồng #E4318D, accent cam #FF9E1B). Toàn bộ screenshot/feature graphic chụp từ GRAPE_POP trên bản build `com.construacabados.orgami.pop` (emulator, airplane mode, không ad); icon = launcher icon GRAPE_POP (`drawable-nodpi/m3_icon.png`).

## 8. Kết luận census
- Trang legal cần: **privacy + terms** (đã live, 200). Không refund, không DMCA, không delete-account, không child-safety.
- Disclosure bắt buộc trong description: **ads + Advertising ID**, **Camera (tuỳ chọn, ảnh ở lại máy)**, **Notifications/boot (nhắc tuỳ chọn)**, **cần internet để tải hướng dẫn** — đã có trong `full_description.txt` (mục PERMISSIONS AND ADS).
- Mục audit: Data Safety CSV, ma trận sensitive-API↔listing, Privacy URL, ads/"free", rating-incentive WARN.
- Submission assets: `data_safety.csv`, `permissions_declarations.md` (CAMERA + video), `review_notes.md`.
