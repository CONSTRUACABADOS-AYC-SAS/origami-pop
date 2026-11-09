# review_notes — Origami Pop (05h) · Google Play Console "Testing instructions" / App access

(Map từ mẫu App Store Connect Review Notes sang Play Console: **App access**, **Testing instructions**, **Contact**.)

## URLs
- Marketing/landing: https://construacabados-ayc-sas.github.io/origami-pop/ (sẽ là landing mới sau publish `landing.json`)
- Privacy Policy: https://construacabados-ayc-sas.github.io/origami-pop/privacy/
- Terms: https://construacabados-ayc-sas.github.io/origami-pop/terms/
- Support: santanaleticia3168661@gmail.com (CONSTRUACABADOS AYC SAS)

## App access
Toàn bộ app dùng được ngay, **không đăng nhập, không tài khoản, không paywall**; mọi mẫu mở sẵn. Không cần tài khoản test.

## Testing instructions (dán vào Console)
Origami Pop is a step-by-step paper-folding guide (origami, kirigami, 3D origami) for hobbyists and beginners who want to fold a specific model without getting stuck: each step has a photo and a short clip of exactly that fold, progress is saved, and a relaxing Mini Fold game is included.
1. Launch the app. Pick a language and swipe through the short onboarding (Next → Get Started). You land on Home — no sign-in.
2. Discover tab → open any model (e.g. Crown) → Start fold. Use Next/Previous or the step grid; tap "Watch this step" to see the clip for that fold; the "?" button opens Basic folds.
3. Jump to the last step via the grid and tap "Finish & celebrate". On the Congrats screen tap **Take photo** to test the Camera permission (optional — "From gallery" works without it).
4. Game tab → Play level → swipe or tap the arrows to fold the four corners and reveal the picture.
5. Profile tab → Language, Appearance, Daily reminder (Notifications permission is requested on Android 13+).
Requires an internet connection to load guide images/clips. The app shows ads (AdMob + mediation); Advertising ID is declared in Data safety.

## Khác biệt/định vị (nội bộ)
- USP: "không bao giờ kẹt ở một bước" — clip cắt đúng đoạn từng bước + tiếp tục đúng bước đang dừng; ba catalog (origami, kirigami, 3D) + Mini Fold. Listing, screenshot và landing cùng hook này.
- Không có mã ghép đôi/tài khoản test cần cấp (app không có tương tác 2 người).

## Lưu ý trước khi submit
- Policy WARN-1 (`policy_audit.md`): bỏ câu/phần thưởng "300 coins khi rate" ở màn Rate.
- Cập nhật Privacy/Terms cho package `com.construacabados.orgami.pop`.
