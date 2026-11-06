# DATA SAFETY — Origami Pop (05h) · bản người đọc + audit

File import: `data_safety.csv` (chỉ cột Response value được điền; Question/Response ID + thứ tự giữ nguyên template — đã so khớp 782/782 dòng). Nguồn sự thật: `compliance_census.md` (trục Data & privacy / Data Safety) + `libs.versions.toml`/`build.gradle.kts` + merged manifest + Privacy Policy live (https://construacabados-ayc-sas.github.io/origami-pop/privacy/).

## Câu hỏi top-level
| Câu hỏi | Trả lời | Căn cứ |
|---|---|---|
| Collects or shares required user data types | **Yes** | AdMob + mediation, Firebase, Adjust/Meta/TikTok gửi dữ liệu rời máy |
| Encrypted in transit | **Yes** | mọi SDK/CDN dùng HTTPS/TLS (Privacy §7) |
| Account creation methods | **None** (`PSL_ACM_NONE`) | `grep FirebaseAuth|GoogleSignIn|signInWith|createUserWith|AccountManager` = 0; không đăng nhập |
| Users can request data deletion | **Yes** — URL = trang Privacy (mục 9) | Không có account để xoá; dữ liệu local xoá bằng gỡ app/clear storage; dữ liệu SDK qua email `santanaleticia3168661@gmail.com` (Privacy §9). *Chủ app cân nhắc: nếu không muốn nhận yêu cầu xoá qua email thì đổi sang "No" trên Console.* |
| Outside-app accounts | No | — |

## Data type đã khai (đều Required — người dùng không tắt được; ephemeral = No)
| Play data type | Collected | Shared | Purpose thu thập | Purpose chia sẻ | Bằng chứng SDK/manifest |
|---|---|---|---|---|---|
| **Device or other IDs** (`PSL_DEVICE_ID`) | ✔ | ✔ | Analytics · Advertising or marketing · Fraud prevention/security · App functionality (push token FCM) | Advertising · Analytics | `play-services-ads` + mediation (AppLovin/Facebook/TikTok) gửi Advertising ID; perms `AD_ID`, `ACCESS_ADSERVICES_AD_ID/ATTRIBUTION`; Firebase install ID + FCM token; Adjust/Meta/TikTok attribution (Privacy §2.1) |
| **App interactions** (`PSL_USER_INTERACTION`) | ✔ | ✔ | Analytics · Advertising | Analytics · Advertising | Firebase Analytics events (màn đã xem, tính năng dùng), ad impressions/clicks (Privacy §2.1) |
| **Approximate location** (`PSL_APPROX_LOCATION`) | ✔ | ✔ | Advertising · Analytics · Fraud prevention | Advertising · Analytics | suy từ IP bởi ad SDK/Firebase; app KHÔNG xin quyền location (Privacy §2.1 "IP address and approximate location derived from it") |
| **Crash logs** (`PSL_CRASH_LOGS`) | ✔ | — | Analytics · App functionality | — | `firebase-crashlytics` |
| **Diagnostics** (`PSL_PERFORMANCE_DIAGNOSTICS`) | ✔ | — | Analytics · App functionality | — | Crashlytics + thông tin thiết bị/app (model, OS, phiên bản, network) (Privacy §2.1) |

Crashlytics/Firebase do Google xử lý như service provider ⇒ khai "Collected" (không "Shared"). Dữ liệu gửi cho mạng quảng cáo/attribution bên thứ ba ⇒ "Shared".

## KHÔNG khai (và lý do)
- **Photos/Videos**: ảnh chụp bằng camera / chọn từ gallery chỉ xử lý & lưu **on-device** (`filesDir/creations`), không upload (Privacy §2.3) ⇒ không phải "collection".
- **User IDs / Email / Name / Phone / Address / Purchase history / Financial / Health / Contacts / Messages / Files / Audio / Calendar**: không thu thập (không account, không IAP, không quyền liên quan).
- **Installed apps**: `<queries>` chỉ liệt kê vending + Facebook/Instagram cho attribution; không `QUERY_ALL_PACKAGES`.
- **Precise location, Web browsing, SMS/Call log**: không.

## Đối chiếu Data Safety ↔ Privacy ↔ Description
| Data type | Privacy §2.1 nhắc? | Description nhắc? | Kết quả |
|---|---|---|---|
| Advertising/Device IDs | ✔ (Advertising ID, Firebase installation ID, push token, install/attribution) | "supported by ads" | PASS |
| App interactions | ✔ | — (không cần) | PASS |
| Approx location (IP) | ✔ | — | PASS |
| Crash logs / Diagnostics | ✔ | — | PASS |
| Camera photos on-device | ✔ §2.3 | ✔ "Photos stay on your phone" | PASS |
| Không account | ✔ | ✔ "no account or sign-in" | PASS |
**Kết luận: nhất quán — không lệch.**

## Việc người làm
Play Console → App content → Data safety → *Import from CSV* → chọn `data_safety.csv` → rà từng mục → Submit. Nếu Console báo thiếu block usage cho type nào, đối chiếu bảng trên.
