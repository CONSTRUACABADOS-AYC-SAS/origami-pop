# Data Safety — Origami Pop (05h) · bản người đọc + audit

Import: Play Console → App content → Data safety → **Import** `blueprint/aso/data_safety.csv` (47 dòng đã điền, giữ nguyên Question/Response ID và thứ tự template `instructions/aso/data_safety_template.csv`). Rà lại rồi Submit.

## Câu hỏi cấp cao
| Câu | Khai | Bằng chứng |
|---|---|---|
| Collects/shares user data | **Yes** | SDK ads/analytics (census §3) |
| Encrypted in transit | **Yes** | Firebase/AdMob/CDN đều HTTPS (Privacy §7) |
| Account creation | **None** (`PSL_ACM_NONE`) | không SDK auth, không màn đăng nhập |
| Data deletion | **Yes** + URL = trang Privacy (mục 9: email yêu cầu xoá + xoá dữ liệu cục bộ) | Privacy §9 |
| Family policy | để trống (app không dành cho trẻ em) | Privacy §11 |

## Data types khai
| Data type Play | Collected / Shared | Mục đích | User control | Bằng chứng (SDK/permission) |
|---|---|---|---|---|
| **Approximate location** (IP→khu vực) | Collected + **Shared** | Advertising, Analytics | Required | `module-ads` (AdMob+MAX/Meta/Pangle/Vungle), Firebase; Privacy §2.1 "IP address and approximate location derived from it" |
| **Device or other IDs** (Advertising ID, FCM token, Firebase installation ID) | Collected + **Shared** | Advertising, Analytics, Fraud prevention, App functionality | Required | `AD_ID`, `ACCESS_ADSERVICES_AD_ID`, Adjust/Meta/TikTok SDK, `firebase-messaging` |
| **App interactions** | Collected + **Shared** | Analytics, Advertising | Required | `firebase-analytics`, ad SDK (impressions/clicks) |
| **Crash logs** | Collected | Analytics, App functionality | Required | `firebase-crashlytics` |
| **Diagnostics** (performance) | Collected | Analytics, App functionality | Required | Crashlytics/Firebase diagnostics |

"Ephemeral" = No cho tất cả (SDK lưu trữ). User control = **Required** (không có toggle opt-out trong app; người dùng chỉ có thể đặt lại Advertising ID ở cài đặt Android).

## KHÔNG khai (on-device / không rời máy)
- Photos (ảnh thành phẩm chụp bằng CameraX / chọn từ PhotoPicker): xử lý & lưu **cục bộ**, chỉ chia sẻ khi người dùng bấm Share (Privacy §2.3) ⇒ không phải "collection".
- Favourites, tiến độ gấp, coin, cài đặt (DataStore cục bộ).
- Purchase history / Financial info: không có IAP (census §1).
- Name/email/phone/contacts/precise location/SMS/audio/files: không có permission/feature.

## Đối chiếu Data Safety ↔ Privacy Policy (https://construacabados-ayc-sas.github.io/origami-pop/privacy/)
| Type khai | Privacy Policy nhắc? |
|---|---|
| Approx. location | Có — "IP address and approximate location derived from it (country/region level)" (§2.1) |
| Device/other IDs | Có — "Advertising ID ... device identifiers", "Firebase installation ID and push notification token" (§2.1) |
| App interactions | Có — "App interactions (screens viewed, features used, ad impressions and clicks...)" (§2.1) |
| Crash logs / diagnostics | Có — "Crash logs and performance diagnostics — Firebase Crashlytics" (§2.1) |
| Shared với bên thứ ba | Có — §4 liệt kê AdMob + mediation, Firebase, Adjust, Meta, TikTok, UMP, Play Review, CDN; §6 "share only with service providers" |
| Không bán dữ liệu | Có — "We do not sell this information" |

→ **Nhất quán**. Điểm cần người duyệt: (1) Privacy ghi package cũ `com.paper.folding.origamipop`; (2) "Data deletion Yes + URL" dùng chính trang Privacy §9 — nếu muốn chắc nên thêm URL/anchor riêng; (3) khi `ads.sh` chốt danh sách mediation cuối cùng, đối chiếu lại các mạng (merged manifest hiện: AppLovin MAX, Meta, ByteDance/Pangle, Vungle).
