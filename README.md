<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, user-scalable=no">
<title>تاكسي أحمد - المنظومة الشاملة 🚕</title>
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/leaflet@1.9.4/dist/leaflet.css" />
<style>
/* =========================================================
   1. إعدادات المتغيرات الأساسية (Theme Variables)
   ========================================================= */
:root {
  --primary: #FFC107;
  --primary-hover: #E0A800;
  --dark: #0F1419;
  --gray: #6B7280;
  --light-gray: #E5E7EB;
  --bg: #F5F5F7;
  --white: #FFFFFF;
  --green: #10B981;
  --red: #EF4444;
  --blue: #3B82F6;
  
  --sh-sm: 0 2px 8px rgba(0,0,0,.08);
  --sh-md: 0 8px 24px rgba(0,0,0,.12);
  --sh-lg: 0 20px 60px rgba(0,0,0,.15);
  
  --radius-sm: 8px;
  --radius-md: 16px;
  --radius-lg: 24px;
  --radius-full: 999px;
}

/* =========================================================
   2. التهيئة العامة (Reset & Global Styles)
   ========================================================= */
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
  -webkit-tap-highlight-color: transparent;
  outline: none;
}

html, body {
  height: 100%;
  overflow: hidden;
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Helvetica, Arial, sans-serif;
  background-color: #000;
  color: var(--dark);
  display: flex;
  justify-content: center;
}

#app {
  width: 100%;
  max-width: 500px;
  height: 100vh;
  position: relative;
  overflow: hidden;
  background-color: var(--bg);
  box-shadow: 0 0 40px rgba(0,0,0,0.5);
}

/* =========================================================
   3. شاشة التحميل (Splash / Boot Screen)
   ========================================================= */
#boot-screen {
  position: absolute;
  inset: 0;
  background: linear-gradient(135deg, var(--primary), #FF9800);
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  z-index: 9999;
  color: var(--white);
  transition: opacity 0.4s ease;
}

.boot-logo {
  font-size: 80px;
  animation: bounce 1.5s infinite;
  margin-bottom: 20px;
  text-shadow: 0 10px 20px rgba(0,0,0,0.2);
}

.boot-text {
  font-size: 24px;
  font-weight: 800;
  letter-spacing: 1px;
}

.boot-spinner {
  width: 40px;
  height: 40px;
  border: 4px solid rgba(255,255,255,0.3);
  border-top-color: var(--white);
  border-radius: 50%;
  animation: spin 1s linear infinite;
  margin-top: 30px;
}

@keyframes bounce {
  0%, 100% { transform: translateY(0); }
  50% { transform: translateY(-15px); }
}

@keyframes spin {
  to { transform: rotate(360deg); }
}

/* =========================================================
   4. واجهة المصادقة (Auth UI)
   ========================================================= */
.auth-container {
  position: absolute;
  inset: 0;
  background: linear-gradient(135deg, var(--primary), #FF9800);
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 20px;
  z-index: 1000;
  overflow-y: auto;
}

.auth-card {
  width: 100%;
  max-width: 400px;
  background: var(--white);
  border-radius: var(--radius-lg);
  padding: 32px 24px;
  box-shadow: var(--sh-lg);
  margin: auto;
  animation: slideUpFade 0.4s ease-out;
}

@keyframes slideUpFade {
  from { opacity: 0; transform: translateY(30px); }
  to { opacity: 1; transform: translateY(0); }
}

.auth-header {
  text-align: center;
  margin-bottom: 24px;
}

.auth-header h1 {
  font-size: 26px;
  font-weight: 800;
  color: var(--dark);
  margin-bottom: 6px;
}

.auth-header p {
  font-size: 14px;
  color: var(--gray);
}

/* اختيار نوع الحساب (راكب / سائق) */
.role-selector {
  display: flex;
  gap: 12px;
  margin-bottom: 20px;
}

.role-option {
  flex: 1;
  border: 2px solid var(--light-gray);
  border-radius: var(--radius-md);
  padding: 16px 12px;
  text-align: center;
  cursor: pointer;
  transition: all 0.2s ease;
  background: var(--white);
}

.role-option.active {
  border-color: var(--primary);
  background: #FFF8E1;
}

.role-option .icon { font-size: 28px; margin-bottom: 6px; }
.role-option .title { font-size: 14px; font-weight: 800; color: var(--dark); }

/* التابات (دخول / تسجيل) */
.auth-tabs {
  display: flex;
  background: var(--bg);
  border-radius: 12px;
  padding: 4px;
  margin-bottom: 20px;
}

.auth-tab-btn {
  flex: 1;
  background: transparent;
  border: none;
  padding: 10px;
  font-size: 14px;
  font-weight: 700;
  color: var(--gray);
  border-radius: 8px;
  cursor: pointer;
  transition: 0.2s;
  font-family: inherit;
}

.auth-tab-btn.active {
  background: var(--white);
  color: var(--dark);
  box-shadow: var(--sh-sm);
}

/* حقول الإدخال */
.form-group { margin-bottom: 16px; }
.form-group label {
  display: block;
  font-size: 13px;
  font-weight: 800;
  color: var(--gray);
  margin-bottom: 6px;
}

.form-control {
  width: 100%;
  padding: 14px 16px;
  border: 2px solid transparent;
  background: var(--bg);
  border-radius: 12px;
  font-size: 15px;
  color: var(--dark);
  font-family: inherit;
  transition: 0.2s;
}

.form-control:focus {
  border-color: var(--primary);
  background: var(--white);
}

.remember-me {
  display: flex;
  align-items: center;
  gap: 8px;
  font-size: 13px;
  color: var(--gray);
  margin-bottom: 20px;
  cursor: pointer;
}

.remember-me input {
  width: 18px;
  height: 18px;
  accent-color: var(--primary);
  cursor: pointer;
}

.error-box {
  background: #FEE2E2;
  color: #991B1B;
  padding: 12px 16px;
  border-radius: 10px;
  font-size: 13px;
  font-weight: 600;
  margin-bottom: 16px;
  display: none;
}

/* =========================================================
   5. الأزرار العامة (Buttons)
   ========================================================= */
.btn {
  width: 100%;
  padding: 16px;
  border-radius: var(--radius-md);
  border: none;
  font-size: 16px;
  font-weight: 800;
  cursor: pointer;
  transition: transform 0.15s ease, opacity 0.15s ease;
  font-family: inherit;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
}

.btn:active { transform: scale(0.97); }
.btn:disabled { opacity: 0.6; cursor: not-allowed; transform: none; }

.btn-primary { background: var(--primary); color: var(--dark); }
.btn-secondary { background: transparent; color: var(--red); border: 2px solid var(--red); }
.btn-blue { background: var(--blue); color: var(--white); }
.btn-ghost { background: var(--bg); color: var(--dark); }
.btn-green { background: var(--green); color: var(--white); }

/* =========================================================
   6. الواجهة الرئيسية والخريطة (Main Layout)
   ========================================================= */
.app-view {
  position: absolute;
  inset: 0;
  z-index: 1;
  display: flex;
  flex-direction: column;
}

#map-container {
  position: absolute;
  inset: 0;
  z-index: 1;
  background: var(--light-gray);
}

/* الشريط العلوي */
.top-bar {
  position: absolute;
  top: 0; left: 0; right: 0;
  padding: 16px;
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  z-index: 10;
  pointer-events: none;
}

.top-bar > * { pointer-events: auto; }

.user-chip {
  display: flex;
  align-items: center;
  gap: 12px;
  background: var(--white);
  border-radius: var(--radius-full);
  padding: 6px 16px 6px 6px;
  box-shadow: var(--sh-md);
  border: none;
  cursor: pointer;
  font-family: inherit;
  transition: 0.2s;
}

.user-chip:active { transform: scale(0.95); }

.user-avatar {
  width: 40px;
  height: 40px;
  border-radius: 50%;
  background: var(--primary);
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 16px;
  font-weight: 800;
  color: var(--dark);
}

.user-chip.driver .user-avatar {
  background: var(--blue);
  color: var(--white);
}

.user-info { text-align: right; }
.user-name { font-size: 15px; font-weight: 800; color: var(--dark); line-height: 1.2; }
.user-meta { font-size: 11px; color: var(--gray); font-weight: 600; margin-top: 2px; }

/* =========================================================
   7. القائمة السفلية (Bottom Sheet)
   ========================================================= */
.bottom-sheet {
  position: absolute;
  bottom: 0; left: 0; right: 0;
  background: var(--white);
  border-radius: var(--radius-lg) var(--radius-lg) 0 0;
  padding: 12px 20px 24px;
  box-shadow: 0 -10px 40px rgba(0,0,0,0.1);
  z-index: 20;
  max-height: 85%;
  overflow-y: auto;
  transition: transform 0.3s cubic-bezier(0.4, 0, 0.2, 1);
  animation: sheetUp 0.3s ease-out;
}

@keyframes sheetUp {
  from { transform: translateY(100%); }
  to { transform: translateY(0); }
}

.sheet-handle {
  width: 40px;
  height: 5px;
  background: var(--light-gray);
  border-radius: var(--radius-full);
  margin: 0 auto 16px;
}

.close-sheet-btn {
  position: absolute;
  top: 20px; left: 20px;
  width: 32px; height: 32px;
  background: var(--bg);
  border: none;
  border-radius: 50%;
  color: var(--gray);
  font-size: 18px;
  font-weight: bold;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: 0.2s;
}

.close-sheet-btn:active { background: var(--light-gray); transform: scale(0.9); }

.open-sheet-btn {
  position: absolute;
  bottom: 24px; left: 50%;
  transform: translateX(-50%);
  z-index: 15;
  width: 85%; max-width: 380px;
  padding: 18px;
  border-radius: var(--radius-full);
  font-size: 17px;
  font-weight: 800;
  box-shadow: 0 10px 30px rgba(255, 193, 7, 0.4);
  display: none;
}

/* =========================================================
   8. عناصر اختيار المواقع
   ========================================================= */
.sheet-title {
  font-size: 20px;
  font-weight: 800;
  margin-bottom: 16px;
  color: var(--dark);
}

.location-row {
  display: flex;
  align-items: center;
  gap: 12px;
  background: var(--bg);
  padding: 12px 16px;
  border-radius: var(--radius-md);
  margin-bottom: 12px;
  border: 2px solid transparent;
  transition: 0.2s;
}

.location-row:focus-within { border-color: var(--primary); background: var(--white); }

.loc-dot {
  width: 14px; height: 14px;
  border-radius: 50%;
  flex-shrink: 0;
}

.loc-dot.green { background: var(--green); box-shadow: 0 0 0 4px rgba(16,185,129,0.2); }
.loc-dot.red { background: var(--red); box-shadow: 0 0 0 4px rgba(239,68,68,0.2); }

.loc-text-area { flex: 1; min-width: 0; cursor: pointer; }
.loc-label { font-size: 11px; color: var(--gray); font-weight: 700; margin-bottom: 2px; }
.loc-value { font-size: 15px; font-weight: 800; color: var(--dark); white-space: nowrap; overflow: hidden; text-overflow: ellipsis; }

.loc-map-btn {
  width: 40px; height: 40px;
  background: var(--white);
  border: 1px solid var(--light-gray);
  border-radius: 10px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 18px;
  cursor: pointer;
  box-shadow: var(--sh-sm);
  transition: 0.2s;
}

.loc-map-btn:active { background: var(--bg); transform: scale(0.95); }

/* =========================================================
   9. اختيار السيارة والكلفة
   ========================================================= */
.section-title {
  font-size: 15px;
  font-weight: 800;
  margin: 20px 0 12px;
  color: var(--dark);
}

.cars-grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 10px;
}

.car-card {
  background: var(--bg);
  border: 2px solid transparent;
  border-radius: var(--radius-md);
  padding: 12px 6px;
  text-align: center;
  cursor: pointer;
  transition: 0.2s;
}

.car-card.active { background: #FFF8E1; border-color: var(--primary); }
.car-icon { font-size: 28px; margin-bottom: 4px; }
.car-name { font-size: 12px; font-weight: 800; color: var(--dark); }
.car-mult { font-size: 10px; color: var(--gray); font-weight: 600; }

.price-summary {
  background: linear-gradient(135deg, #FFF8E1, #FFF3C4);
  border-radius: var(--radius-md);
  padding: 16px;
  margin: 20px 0;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.price-col .p-label { font-size: 12px; color: #92400E; font-weight: 700; margin-bottom: 4px; }
.price-col .p-value { font-size: 22px; font-weight: 800; color: var(--dark); }
.price-col .p-value small { font-size: 13px; color: var(--gray); font-weight: 700; }

/* =========================================================
   10. شاشات الانتظار والرحلة
   ========================================================= */
.searching-view { text-align: center; padding: 20px 0; }

.radar-anim {
  width: 100px; height: 100px;
  margin: 0 auto 20px;
  position: relative;
  display: flex;
  align-items: center;
  justify-content: center;
}

.radar-anim::before, .radar-anim::after {
  content: '';
  position: absolute;
  inset: 0;
  border-radius: 50%;
  background: var(--primary);
  opacity: 0.3;
  animation: pulse 2s ease-out infinite;
}

.radar-anim::after { animation-delay: 1s; }
.radar-anim .emoji { font-size: 44px; z-index: 1; animation: bounce 1s infinite; }

@keyframes pulse {
  0% { transform: scale(0.5); opacity: 0.8; }
  100% { transform: scale(2); opacity: 0; }
}

.searching-view h3 { font-size: 18px; font-weight: 800; margin-bottom: 8px; color: var(--dark); }
.searching-view p { font-size: 14px; color: var(--gray); }

.driver-info-card {
  display: flex;
  align-items: center;
  gap: 14px;
  background: var(--bg);
  padding: 16px;
  border-radius: var(--radius-md);
  margin-bottom: 16px;
}

.driver-avatar-lg {
  width: 60px; height: 60px;
  border-radius: 50%;
  background: var(--primary);
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 30px;
}

.driver-details { flex: 1; }
.driver-name-lg { font-size: 17px; font-weight: 800; color: var(--dark); }
.driver-car-lg { font-size: 13px; color: var(--gray); margin-top: 2px; }
.driver-plate { font-size: 12px; font-weight: 800; background: var(--white); padding: 2px 8px; border-radius: 6px; display: inline-block; margin-top: 4px; }

.progress-container {
  height: 8px;
  background: var(--light-gray);
  border-radius: var(--radius-full);
  overflow: hidden;
  margin: 16px 0;
}

.progress-bar {
  height: 100%;
  background: var(--primary);
  width: 0%;
  transition: width 0.2s linear;
  border-radius: var(--radius-full);
}

.rating-stars {
  display: flex;
  justify-content: center;
  gap: 8px;
  font-size: 36px;
  margin: 16px 0;
  cursor: pointer;
}

.rating-stars span {
  opacity: 0.3;
  filter: grayscale(1);
  transition: 0.2s;
}

.rating-stars span.active {
  opacity: 1;
  filter: none;
  transform: scale(1.1);
}

/* =========================================================
   11. واجهة السائق
   ========================================================= */
.driver-toggle-row {
  display: flex;
  align-items: center;
  justify-content: space-between;
  background: var(--bg);
  padding: 20px;
  border-radius: var(--radius-md);
  margin-bottom: 20px;
}

.toggle-title { font-size: 16px; font-weight: 800; color: var(--dark); }
.toggle-sub { font-size: 12px; color: var(--gray); margin-top: 4px; }

.switch {
  position: relative;
  display: inline-block;
  width: 56px;
  height: 32px;
}

.switch input { opacity: 0; width: 0; height: 0; }

.slider {
  position: absolute;
  cursor: pointer;
  inset: 0;
  background-color: #ccc;
  transition: .4s;
  border-radius: 34px;
}

.slider:before {
  position: absolute;
  content: "";
  height: 24px; width: 24px;
  left: 4px; bottom: 4px;
  background-color: white;
  transition: .4s;
  border-radius: 50%;
}

input:checked + .slider { background-color: var(--green); }
input:checked + .slider:before { transform: translateX(24px); }

.incoming-request {
  background: var(--white);
  border: 2px solid var(--blue);
  border-radius: var(--radius-md);
  padding: 16px;
  margin-bottom: 16px;
  animation: slideUpFade 0.4s;
}

.req-header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 12px; }
.req-passenger { font-size: 16px; font-weight: 800; }
.req-price { font-size: 22px; font-weight: 800; color: var(--green); }
.req-route { font-size: 14px; font-weight: 700; margin-bottom: 8px; line-height: 1.5; color: var(--dark); }
.req-dist { font-size: 12px; color: var(--gray); font-weight: 600; margin-bottom: 16px; }

/* =========================================================
   12. النوافذ المنبثقة (Modals)
   ========================================================= */
.modal-overlay {
  position: fixed;
  inset: 0;
  background: rgba(0,0,0,0.5);
  z-index: 1000;
  display: none;
  align-items: flex-end;
  justify-content: center;
  backdrop-filter: blur(4px);
}

.modal-overlay.show { display: flex; }

.modal-content {
  width: 100%;
  max-width: 500px;
  background: var(--white);
  border-radius: var(--radius-lg) var(--radius-lg) 0 0;
  padding: 20px;
  max-height: 85%;
  overflow-y: auto;
  animation: sheetUp 0.3s ease-out;
}

.list-modal-header {
  font-size: 18px;
  font-weight: 800;
  text-align: center;
  margin-bottom: 16px;
}

.loc-list { list-style: none; padding: 0; margin: 0; }
.loc-list-item {
  padding: 16px;
  border-bottom: 1px solid var(--bg);
  display: flex;
  justify-content: space-between;
  align-items: center;
  font-size: 15px;
  font-weight: 700;
  cursor: pointer;
  transition: 0.2s;
  color: var(--dark);
}

.loc-list-item:active { background: var(--bg); }
.loc-list-item .arrow { color: var(--gray); font-size: 12px; }

/* =========================================================
   13. الخريطة الكاملة (Full Screen Map Picker)
   ========================================================= */
#full-map-overlay {
  position: fixed;
  inset: 0;
  z-index: 2000;
  background: var(--white);
  display: none;
  flex-direction: column;
}

#full-map { flex: 1; width: 100%; z-index: 1; }

.full-map-header {
  position: absolute;
  top: 0; left: 0; right: 0;
  padding: 20px;
  background: rgba(255,255,255,0.95);
  z-index: 10;
  box-shadow: var(--sh-md);
  display: flex;
  align-items: center;
  gap: 16px;
  border-radius: 0 0 20px 20px;
}

.back-btn {
  width: 40px; height: 40px;
  border-radius: 50%;
  background: var(--bg);
  border: none;
  font-size: 18px;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
}

.full-map-title { font-size: 16px; font-weight: 800; color: var(--dark); }

.center-pin {
  position: absolute;
  top: 50%; left: 50%;
  transform: translate(-50%, -100%);
  font-size: 44px;
  z-index: 10;
  pointer-events: none;
  filter: drop-shadow(0 4px 4px rgba(0,0,0,0.4));
  animation: bounce 2s infinite;
}

.full-map-footer {
  position: absolute;
  bottom: 0; left: 0; right: 0;
  padding: 24px;
  background: var(--white);
  z-index: 10;
  border-radius: 24px 24px 0 0;
  box-shadow: 0 -8px 24px rgba(0,0,0,0.1);
  text-align: center;
}

.closest-area-text {
  font-size: 18px;
  font-weight: 800;
  margin-bottom: 16px;
  color: var(--dark);
}

/* =========================================================
   14. الملف الشخصي والإشعارات
   ========================================================= */
.profile-header { text-align: center; padding-bottom: 20px; border-bottom: 1px solid var(--bg); margin-bottom: 20px; }
.profile-avatar {
  width: 80px; height: 80px;
  border-radius: 50%;
  background: var(--primary);
  display: flex; align-items: center; justify-content: center;
  font-size: 32px; font-weight: 800;
  margin: 0 auto 12px;
  color: var(--dark);
}
.profile-avatar.driver { background: var(--blue); color: var(--white); }
.profile-name { font-size: 20px; font-weight: 800; }
.profile-phone { font-size: 14px; color: var(--gray); margin-top: 4px; }
.profile-badge {
  display: inline-block;
  background: #FFF8E1; color: #92400E;
  padding: 4px 12px;
  border-radius: var(--radius-full);
  font-size: 12px; font-weight: 700;
  margin-top: 8px;
}

.stats-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 12px; margin-bottom: 20px; }
.stat-box { background: var(--bg); padding: 16px; border-radius: var(--radius-md); text-align: center; }
.stat-val { font-size: 18px; font-weight: 800; color: var(--dark); }
.stat-lbl { font-size: 12px; color: var(--gray); margin-top: 4px; }

.toast-msg {
  position: fixed;
  bottom: 30px; left: 50%;
  transform: translate(-50%, 100px);
  background: var(--dark);
  color: var(--white);
  padding: 14px 24px;
  border-radius: 12px;
  font-size: 14px; font-weight: 700;
  z-index: 9999;
  opacity: 0;
  transition: 0.3s;
  pointer-events: none;
  max-width: 90%;
  text-align: center;
  box-shadow: var(--sh-lg);
}

.toast-msg.show { opacity: 1; transform: translate(-50%, 0); }
.toast-msg.success { background: var(--green); }
.toast-msg.error { background: var(--red); }

/* =========================================================
   15. تنسيقات خرائط Leaflet
   ========================================================= */
.leaflet-container { font-family: inherit; }
.custom-marker { display: flex; align-items: center; justify-content: center; }
.marker-dot { width: 24px; height: 24px; border-radius: 50%; border: 3px solid var(--white); box-shadow: var(--sh-sm); }
.car-marker-icon { font-size: 32px; filter: drop-shadow(0 4px 4px rgba(0,0,0,0.3)); text-align: center; }
</style>
</head>
<body>

<!-- شاشة التحميل -->
<div id="boot-screen">
  <div class="boot-logo">🚕</div>
  <div class="boot-text" id="boot-text">جاري التحميل...</div>
  <div class="boot-spinner"></div>
</div>

<!-- الحاوية الرئيسية -->
<div id="app"></div>

<!-- الإشعارات -->
<div class="toast-msg" id="toast"></div>

<!-- نافذة الخريطة الكاملة -->
<div id="full-map-overlay">
  <div class="full-map-header">
    <button class="back-btn" onclick="closeMapPicker()">✕</button>
    <div class="full-map-title" id="map-picker-title">حدد الموقع</div>
  </div>
  <div id="full-map"></div>
  <div class="center-pin">📍</div>
  <div class="full-map-footer">
    <div class="closest-area-text" id="map-picker-text">جاري التحديد...</div>
    <button class="btn btn-primary" onclick="confirmMapLocation()">تأكيد الموقع</button>
  </div>
</div>

<!-- نافذة اختيار المنطقة بالقائمة -->
<div class="modal-overlay" id="loc-list-modal">
  <div class="modal-content">
    <div class="sheet-handle"></div>
    <div class="list-modal-header" id="loc-list-title">اختر المحافظة</div>
    <ul class="loc-list" id="loc-list-container"></ul>
    <button class="btn btn-ghost" style="margin-top:16px" onclick="document.getElementById('loc-list-modal').classList.remove('show')">إلغاء</button>
  </div>
</div>

<!-- نافذة الحساب -->
<div class="modal-overlay" id="profile-modal">
  <div class="modal-content" id="profile-modal-content"></div>
</div>

<script src="https://cdn.jsdelivr.net/npm/leaflet@1.9.4/dist/leaflet.js"></script>
<script>
/* =========================================================
   البيانات الأساسية (Data Core)
   ========================================================= */
const GOVS = [
  { name:'بغداد', lat:33.3152, lng:44.3661, areas:['الكرادة','الجادرية','المنصور','الكاظمية','الأعظمية','زيونة','الحارثية','العلاوي','الباب الشرقي','الطالبية','الشعلة','الدورة','المحمودية','أبو غريب','الغزالية','اليرموك','العامرية','البياع','السيدية','حي أور','مدينة الصدر','بغداد الجديدة'] },
  { name:'البصرة', lat:30.5081, lng:47.7835, areas:['العشار','الجنينة','الموفقية','الزبير','أبو الخصيب','شط العرب','القرنة','الطويسة','كرمة علي','الهارثة','الفاو','سفوان','المدينة','البراضعية'] },
  { name:'نينوى', lat:36.3350, lng:43.1189, areas:['الموصل القديمة','الدواسة','الزهور','الجامعة','الرسالة','المتنبي','الدوميز','برطلة','تلكيف','الحمدانية','القيارة','سنجار','تلعفر','بعشيقة'] },
  { name:'أربيل', lat:36.1911, lng:44.0092, areas:['مركز أربيل','عنكاوا','شقلاوة','سوران','كويه','رواندز','جومان','خبات','سيدكان','ميركه سور'] },
  { name:'السليمانية', lat:35.5550, lng:45.4350, areas:['مركز السليمانية','حلبجة','رانية','دوكان','جمجمال','دربنديخان','بنجوين','سيد صادق','شهربازار','بازيان'] },
  { name:'دهوك', lat:36.8669, lng:42.9503, areas:['مركز دهوك','زاخو','سميل','العمادية','عقرة','شيخان','بامرني','سرسنك'] },
  { name:'كركوك', lat:35.4681, lng:44.3922, areas:['مركز كركوك','الحويجة','داقوق','دبس','الرياض','التون كوبري','ليلان','يايجة'] },
  { name:'صلاح الدين', lat:34.6000, lng:43.6833, areas:['تكريت','سامراء','بيجي','الشرقاط','الدور','بلد','طوزخورماتو','الدجيل','الإسحاقي'] },
  { name:'الأنبار', lat:33.4200, lng:43.3000, areas:['الرمادي','الفلوجة','هيت','حديثة','الرطبة','القائم','الحبانية','الخالدية','الكرمة','عامرية الفلوجة','البغدادي'] },
  { name:'بابل', lat:32.4682, lng:44.4231, areas:['الحلة','المحاويل','الهاشمية','المسيب','الحمزة','القاسم','المدحتية','الاسكندرية','الشوملي','الطليعة'] },
  { name:'كربلاء', lat:32.6160, lng:44.0249, areas:['مركز كربلاء','الحر','الحسينية','الخيرات','عين التمر','الهندية','الجدول الغربي','الاسكان'] },
  { name:'النجف', lat:31.9976, lng:44.3296, areas:['النجف القديمة','الكوفة','الحيرة','المناذرة','المشخاب','القادسية','السلام','الميسان','العباسية','الغري'] },
  { name:'القادسية', lat:31.9892, lng:44.9250, areas:['الديوانية','الشامية','عفك','الحمزة','البدير','الدغارة','الشنافية','السلام','المهادية'] },
  { name:'المثنى', lat:31.3000, lng:45.2833, areas:['السماوة','الرميثة','الخضر','السلمان','الوركاء','الهلال','النجمي','الدراجي'] },
  { name:'ذي قار', lat:31.0425, lng:46.2561, areas:['الناصرية','الشطرة','الرفاعي','سوق الشيوخ','الجبايش','قلعة سكر','الغراف','النصر','سيد دخيل','البطحاء'] },
  { name:'ميسان', lat:31.8400, lng:47.1500, areas:['العمارة','المجر الكبير','قلعة صالح','علي الغربي','الكحلاء','الميمونة','العزير','كميت','المشرح'] },
  { name:'واسط', lat:32.5000, lng:45.8167, areas:['الكوت','الحي','النعمانية','بدرة','الصويرة','العزيزية','شيخ سعد','الزبيدية'] },
  { name:'ديالى', lat:33.7500, lng:44.6167, areas:['بعقوبة','المقدادية','الخالص','بلدروز','خانقين','كفري','المنصورية','العبارة','بني سعد'] },
];

const CARS = {
  economy: { name: 'اقتصادي', icon: '🚗', mult: 1.0 },
  comfort: { name: 'مريح',    icon: '🚙', mult: 1.3 },
  vip:     { name: 'VIP',     icon: '🚘', mult: 1.9 },
  van:     { name: 'فان',     icon: '🚐', mult: 1.5 },
};

const DRIVERS_DB = [
  { name: 'أحمد كريم', rating: 4.9, phone: '07701234567', car: 'تويوتا كورولا', plate: '٢٤٧٨١', emoji: '👨' },
  { name: 'حسين علي', rating: 4.8, phone: '07809876543', car: 'كيا سيراتو', plate: '١٥٦٣٢', emoji: '🧔' },
  { name: 'مصطفى جبار', rating: 5.0, phone: '07512345678', car: 'هيونداي إلنترا', plate: '٩٨٤٥٦', emoji: '👨‍🦱' },
  { name: 'علي حسن', rating: 4.7, phone: '07705556677', car: 'نيسان صني', plate: '٣٣٤٤٥', emoji: '👨‍🦰' },
];

/* =========================================================
   قاعدة البيانات المحلية (LocalStorage DB)
   ========================================================= */
const DB = {
  KEY: 'ahmed_taxi_v10',
  
  load() {
    try { 
      const d = JSON.parse(localStorage.getItem(this.KEY)); 
      if (d && Array.isArray(d.users)) return d; 
    } catch (e) {}
    return { users: [], session: null, activeRide: null };
  },
  
  save(data) { localStorage.setItem(this.KEY, JSON.stringify(data)); },
  
  hash(str) { 
    let h = 5381; 
    for (let i = 0; i < str.length; i++) h = ((h << 5) + h) ^ str.charCodeAt(i); 
    return 'h_' + (h >>> 0).toString(36); 
  },
  
  register(u) {
    const db = this.load();
    if (db.users.find(x => x.phone === u.phone)) throw new Error('رقم الهاتف مسجل من قبل');
    if (!/^07\d{9}$/.test(u.phone)) throw new Error('رقم الهاتف يجب أن يبدأ بـ 07 ويكون 11 رقماً');
    if (u.password.length < 4) throw new Error('كلمة المرور يجب أن تكون 4 أحرف على الأقل');
    
    const newUser = { 
      id: 'u_' + Date.now().toString(36), 
      name: u.name.trim(), 
      phone: u.phone, 
      password: this.hash(u.password), 
      gov: u.gov, 
      role: u.role, 
      createdAt: Date.now(), 
      rides: [] 
    };
    
    db.users.push(newUser); 
    this.save(db); 
    return newUser;
  },
  
  login(phone, password) {
    const db = this.load(); 
    const u = db.users.find(x => x.phone === phone);
    if (!u) throw new Error('لا يوجد حساب بهذا الرقم');
    if (u.password !== this.hash(password)) throw new Error('كلمة المرور غير صحيحة');
    return u;
  },
  
  startSession(userId, remember) {
    const db = this.load(); 
    const dur = remember ? 365*24*60*60*1000 : 24*60*60*1000;
    db.session = { userId, remember, expiresAt: Date.now() + dur }; 
    this.save(db);
  },
  
  getSession() {
    const db = this.load(); 
    if (!db.session) return null;
    if (db.session.expiresAt < Date.now()) { db.session = null; this.save(db); return null; }
    return db.users.find(x => x.id === db.session.userId) || null;
  },
  
  logout() { 
    const db = this.load(); 
    db.session = null; 
    this.save(db); 
  },
  
  createRide(r) { 
    const db = this.load(); 
    db.activeRide = { id: 'r_'+Date.now().toString(36), ...r, status: 'pending', createdAt: Date.now() }; 
    this.save(db); 
  },
  
  getRide() { return this.load().activeRide; },
  
  updateRide(status) { 
    const db = this.load(); 
    if (db.activeRide) { db.activeRide.status = status; this.save(db); } 
  },
  
  clearRide() { 
    const db = this.load(); 
    db.activeRide = null; 
    this.save(db); 
  },
  
  addHistory(userId, ride) {
    const db = this.load(); 
    const u = db.users.find(x => x.id === userId);
    if (u) { u.rides = u.rides || []; u.rides.unshift(ride); this.save(db); }
  }
};

/* =========================================================
   حالة التطبيق (State Management)
   ========================================================= */
const state = {
  user: null, 
  role: 'passenger', 
  stage: 'book',
  pickGov: null, pickArea: null, pickLat: null, pickLng: null,
  dropGov: null, dropArea: null, dropLat: null, dropLng: null,
  carType: 'economy', 
  distance: 0, 
  price: 0, 
  driver: null,
  progress: 0, 
  arrived: false, 
  isOnline: false
};

let mainMap = null, pickerMap = null;
let pickMarker = null, dropMarker = null, carMarker = null, routePolyline = null;
let pickerTarget = null, pickerStep = 'gov', selectedMapCoords = null;
let driverTimeout = null, rideInterval = null;

/* =========================================================
   الدوال المساعدة (Utilities)
   ========================================================= */
function toast(msg, type = '') {
  const t = document.getElementById('toast');
  if (!t) return;
  t.className = 'toast-msg show ' + type; 
  t.textContent = msg;
  clearTimeout(t._timer); 
  t._timer = setTimeout(() => t.classList.remove('show'), 3000);
}

function fmt(n) { return Number(n).toLocaleString('en-US'); }

function initials(name) { 
  const p = (name || 'مستخدم').trim().split(' '); 
  return ((p[0]?.[0]||'') + (p[1]?.[0]||'')).toUpperCase(); 
}

function getAreaCoords(govName, areaName) {
  const g = GOVS.find(x => x.name === govName);
  if (!g) return { lat: 33.3152, lng: 44.3661 };
  
  const i = g.areas.indexOf(areaName);
  if (i === -1) return { lat: g.lat, lng: g.lng };
  
  const a = ((i * 137.5) % 360) * Math.PI / 180;
  const r = 0.05 + (i % 5) * 0.02;
  
  return { lat: g.lat + Math.cos(a) * r, lng: g.lng + Math.sin(a) * r };
}

function haversine(lat1, lon1, lat2, lon2) {
  const R = 6371, 
        dLat = (lat2-lat1)*Math.PI/180, 
        dLon = (lon2-lon1)*Math.PI/180;
        
  const a = Math.sin(dLat/2)**2 + 
            Math.cos(lat1*Math.PI/180) * Math.cos(lat2*Math.PI/180) * Math.sin(dLon/2)**2;
            
  return R * 2 * Math.atan2(Math.sqrt(a), Math.sqrt(1-a));
}

function calcPrice(km, type) { 
  return Math.round(((2000 + km * 800) * CARS[type].mult) / 250) * 250; 
}

function resetState() {
  state.stage = 'book'; 
  state.driver = null; 
  state.progress = 0; 
  state.arrived = false;
  
  if (rideInterval) clearInterval(rideInterval);
  if (routePolyline && mainMap) { mainMap.removeLayer(routePolyline); routePolyline = null; }
  if (carMarker && mainMap) { mainMap.removeLayer(carMarker); carMarker = null; }
}

/* =========================================================
   واجهة المصادقة (Auth Render)
   ========================================================= */
function renderAuth() {
  const govOptions = GOVS.map(g => `<option value="${g.name}">${g.name}</option>`).join('');
  
  document.getElementById('app').innerHTML = `
    <div class="auth-container">
      <div class="auth-card">
        <div class="auth-header">
          <h1>تاكسي أحمد 🚕</h1>
          <p>أهلاً بك، يرجى تسجيل الدخول</p>
        </div>
        
        <div class="role-selector" id="role-selector">
          <div class="role-option active" data-role="passenger">
            <div class="icon">🙋‍♂️</div><div class="title">راكب</div>
          </div>
          <div class="role-option" data-role="driver">
            <div class="icon">🚕</div><div class="title">سائق</div>
          </div>
        </div>

        <div class="auth-tabs">
          <button class="auth-tab-btn active" data-tab="login">دخول</button>
          <button class="auth-tab-btn" data-tab="register">حساب جديد</button>
        </div>
        
        <div class="error-box" id="auth-error"></div>
        
        <form id="auth-form">
          <div class="form-group" id="group-name" style="display:none">
            <label>الاسم الكامل</label>
            <input type="text" id="auth-name" class="form-control" placeholder="مثال: أحمد كريم">
          </div>
          
          <div class="form-group">
            <label>رقم الهاتف</label>
            <input type="tel" id="auth-phone" class="form-control" placeholder="07XX XXX XXXX" maxlength="11" inputmode="numeric">
          </div>
          
          <div class="form-group">
            <label>كلمة المرور</label>
            <input type="password" id="auth-password" class="form-control" placeholder="••••••••">
          </div>
          
          <div class="form-group" id="group-gov" style="display:none">
            <label>المحافظة</label>
            <select id="auth-gov" class="form-control">${govOptions}</select>
          </div>
          
          <label class="remember-me">
            <input type="checkbox" id="auth-remember" checked>
            <span>تذكرني (ابقى مسجل دخول)</span>
          </label>
          
          <button type="submit" class="btn btn-primary" id="auth-submit">دخول</button>
        </form>
      </div>
    </div>`;
    
  // إخفاء شاشة التحميل
  const boot = document.getElementById('boot-screen');
  if (boot) {
    boot.style.opacity = '0';
    setTimeout(() => boot.style.display = 'none', 400);
  }

  let currentRole = 'passenger', currentMode = 'login';
  
  document.querySelectorAll('.role-option').forEach(el => {
    el.onclick = () => { 
      document.querySelectorAll('.role-option').forEach(x => x.classList.remove('active')); 
      el.classList.add('active'); 
      currentRole = el.dataset.role; 
    };
  });

  document.querySelectorAll('.auth-tab-btn').forEach(btn => {
    btn.onclick = () => {
      document.querySelectorAll('.auth-tab-btn').forEach(x => x.classList.remove('active')); 
      btn.classList.add('active'); 
      currentMode = btn.dataset.tab;
      
      document.getElementById('group-name').style.display = currentMode === 'register' ? 'block' : 'none';
      document.getElementById('group-gov').style.display = currentMode === 'register' ? 'block' : 'none';
      document.getElementById('auth-submit').textContent = currentMode === 'register' ? 'إنشاء حساب' : 'دخول';
      document.getElementById('auth-error').style.display = 'none';
    };
  });

  document.getElementById('auth-form').onsubmit = (e) => {
    e.preventDefault(); 
    const err = document.getElementById('auth-error'); 
    err.style.display = 'none';
    
    try {
      let user;
      if (currentMode === 'register') {
        user = DB.register({ 
          name: document.getElementById('auth-name').value.trim(), 
          phone: document.getElementById('auth-phone').value.trim(), 
          password: document.getElementById('auth-password').value, 
          gov: document.getElementById('auth-gov').value, 
          role: currentRole 
        });
        toast('تم إنشاء الحساب 🎉', 'success');
      } else {
        user = DB.login(document.getElementById('auth-phone').value.trim(), document.getElementById('auth-password').value);
        if (user.role !== currentRole) throw new Error('هذا الحساب مسجل كـ ' + (user.role === 'driver' ? 'سائق' : 'راكب'));
        toast('أهلاً بعودتك 👋', 'success');
      }
      
      DB.startSession(user.id, document.getElementById('auth-remember').checked);
      state.user = user; 
      state.role = user.role; 
      renderMainApp();
      
    } catch (ex) { 
      err.textContent = ex.message; 
      err.style.display = 'block'; 
    }
  };
}

/* =========================================================
   الواجهة الرئيسية (Main App Render)
   ========================================================= */
function renderMainApp() {
  try {
    if (!state.user) return renderAuth();
    
    const uName = state.user.name || 'مستخدم';
    const uGov = state.user.gov || 'بغداد';
    const uRole = state.role || 'passenger';

    // تهيئة المواقع الافتراضية
    if (!state.pickGov) {
      const g = GOVS.find(x => x.name === uGov) || GOVS[0];
      state.pickGov = g.name; state.pickArea = g.areas[0];
      state.dropGov = g.name; state.dropArea = g.areas[1] || g.areas[0];
      
      const pc = getAreaCoords(state.pickGov, state.pickArea), dc = getAreaCoords(state.dropGov, state.dropArea);
      state.pickLat = pc.lat; state.pickLng = pc.lng; state.dropLat = dc.lat; state.dropLng = dc.lng;
    }

    document.getElementById('app').innerHTML = `
      <div class="app-view">
        <div class="top-bar">
          <button class="user-chip ${uRole === 'driver' ? 'driver' : ''}" onclick="showProfile()">
            <div class="user-avatar">${initials(uName)}</div>
            <div class="user-info">
              <div class="user-name">${uName.split(' ')[0]}</div>
              <div class="user-meta">📍 ${uGov} (${uRole === 'driver' ? 'سائق' : 'راكب'})</div>
            </div>
          </button>
        </div>
        
        <div id="map-container"></div>
        
        <button class="btn btn-primary open-sheet-btn" id="open-sheet-btn" onclick="toggleSheet(true)">🚕 عرض التفاصيل</button>
        
        <div class="bottom-sheet" id="bottom-sheet"></div>
      </div>`;

    setTimeout(() => {
      try {
        if (!window.L) {
          const mc = document.getElementById('map-container');
          if (mc) mc.innerHTML = '<div style="padding:20px;text-align:center;color:var(--red);font-weight:bold;background:#fff;margin:20px;border-radius:12px;">تعذر تحميل الخريطة، تأكد من اتصال الإنترنت ثم أعد تحميل الصفحة.</div>';
          renderSheet();
          return;
        }
        
        if (mainMap) { try { mainMap.remove(); } catch(e){} }
        const container = document.getElementById('map-container');
        if (!container) return;
        
        mainMap = L.map(container, { zoomControl: false, attributionControl: false }).setView([state.pickLat, state.pickLng], 12);
        L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', { maxZoom: 19 }).addTo(mainMap);
        
        setTimeout(() => {
          mainMap.invalidateSize();
          if (uRole === 'passenger') updateMarkers();
        }, 300);
        
        renderSheet();
      } catch (err) {
        console.error("Map init error:", err);
        renderSheet();
      }
    }, 150);
    
    // إخفاء شاشة التحميل
    const boot = document.getElementById('boot-screen');
    if (boot) {
      boot.style.opacity = '0';
      setTimeout(() => boot.style.display = 'none', 400);
    }
    
  } catch (e) {
    console.error("Render main app error:", e);
    document.getElementById('app').innerHTML = `<div style="padding:20px;text-align:center;color:white;background:#333;height:100vh;display:flex;align-items:center;justify-content:center;">حدث خطأ غير متوقع، يرجى إعادة تحميل الصفحة.</div>`;
    document.getElementById('boot-screen').style.display = 'none';
  }
}

/* =========================================================
   إدارة الخريطة والعلامات
   ========================================================= */
function updateMarkers() {
  if (!mainMap) return;
  try {
    if (pickMarker) mainMap.removeLayer(pickMarker);
    if (dropMarker) mainMap.removeLayer(dropMarker);
    
    pickMarker = L.marker([state.pickLat, state.pickLng], { icon: L.divIcon({ className: 'custom-marker', iconSize: [24,24], iconAnchor:[12,12], html: `<div class="marker-dot" style="background:var(--green)"></div>` }) }).addTo(mainMap);
    
    dropMarker = L.marker([state.dropLat, state.dropLng], { icon: L.divIcon({ className: 'custom-marker', iconSize: [24,24], iconAnchor:[12,12], html: `<div class="marker-dot" style="background:var(--red)"></div>` }) }).addTo(mainMap);
    
    if (!routePolyline) {
      const b = L.featureGroup([pickMarker, dropMarker]).getBounds();
      if (b.isValid()) mainMap.fitBounds(b, { padding: [80, 80], maxZoom: 14 });
    }
  } catch(e) { console.warn("Marker error:", e); }
}

/* =========================================================
   إدارة القائمة السفلية (Bottom Sheet Control)
   ========================================================= */
function toggleSheet(isOpen) {
  const s = document.getElementById('bottom-sheet'), b = document.getElementById('open-sheet-btn');
  if (s && b) {
    if (isOpen) { 
      s.style.transform = 'translateY(0)'; 
      b.style.display = 'none'; 
    } else { 
      s.style.transform = 'translateY(100%)'; 
      b.style.display = 'block'; 
    }
  }
}

function renderSheet() {
  if (state.role === 'passenger') {
    if (state.stage === 'book') renderPassengerBook();
    else if (state.stage === 'searching') renderPassengerSearching();
    else if (state.stage === 'driver_found') renderPassengerDriverFound();
    else if (state.stage === 'in_ride') renderPassengerInRide();
    else if (state.stage === 'done') renderPassengerDone();
  } else {
    if (state.stage === 'book') renderDriverBook();
    else if (state.stage === 'driver_ride') renderDriverActiveRide();
  }
}

/* =========================================================
   واجهات الراكب (Passenger UI)
   ========================================================= */
function renderPassengerBook() {
  toggleSheet(true);
  const sheet = document.getElementById('bottom-sheet');
  if (!sheet) return;
  
  sheet.innerHTML = `
    <button class="close-sheet-btn" onclick="toggleSheet(false)">✕</button>
    <div class="sheet-handle"></div>
    <h2 class="sheet-title">وين نوصلك؟</h2>
    
    <div class="location-row">
      <div class="loc-dot green"></div>
      <div class="loc-text-area" onclick="openLocationList('pick')">
        <div class="loc-label">نقطة الانطلاق</div>
        <div class="loc-value">${state.pickArea}، ${state.pickGov}</div>
      </div>
      <button class="loc-map-btn" onclick="openMapPicker('pick')">📍</button>
    </div>
    
    <div class="location-row">
      <div class="loc-dot red"></div>
      <div class="loc-text-area" onclick="openLocationList('drop')">
        <div class="loc-label">الوجهة</div>
        <div class="loc-value">${state.dropArea}، ${state.dropGov}</div>
      </div>
      <button class="loc-map-btn" onclick="openMapPicker('drop')">📍</button>
    </div>

    <h3 class="section-title">اختر السيارة</h3>
    <div class="cars-grid">
      ${Object.entries(CARS).map(([k,c]) => `
        <div class="car-card ${state.carType === k ? 'active' : ''}" onclick="setCarType('${k}')">
          <div class="car-icon">${c.icon}</div>
          <div class="car-name">${c.name}</div>
          <div class="car-mult">×${c.mult}</div>
        </div>
      `).join('')}
    </div>
    
    <div class="price-summary">
      <div class="price-col">
        <div class="p-label">المسافة</div>
        <div class="p-value" style="font-size:16px">${state.distance.toFixed(1)} كم</div>
      </div>
      <div class="price-col" style="text-align:left">
        <div class="p-label">السعر التقديري</div>
        <div class="p-value">${fmt(state.price)}<small> د.ع</small></div>
      </div>
    </div>
    
    <button class="btn btn-primary" onclick="requestTaxi()">🚕 اطلب التكسي</button>`;
  
  if (state.distance === 0) drawRouteOnMainMap();
}

function setCarType(type) { 
  state.carType = type; 
  state.price = calcPrice(state.distance, type); 
  renderPassengerBook(); 
}

function renderPassengerSearching() {
  toggleSheet(true);
  document.getElementById('bottom-sheet').innerHTML = `
    <div class="sheet-handle"></div>
    <div class="searching-view">
      <div class="radar-anim"><span class="emoji">🚕</span></div>
      <h3>جاري البحث عن سائق...</h3>
      <p>هذا يأخذ لحظات فقط ⏳</p>
    </div>`;
}

function renderPassengerDriverFound() {
  toggleSheet(true);
  document.getElementById('bottom-sheet').innerHTML = `
    <div class="sheet-handle"></div>
    <div style="background:var(--dark); color:var(--white); padding:16px; border-radius:16px; display:flex; justify-content:space-between; align-items:center; margin-bottom:16px">
      <div>
        <div style="font-size:13px; opacity:0.7">${state.arrived ? 'السائق وصل!' : 'وقت الوصول'}</div>
        <div style="font-size:22px; font-weight:800; color:var(--primary)">${state.arrived ? '🎉' : '5 دقائق'}</div>
      </div>
      <div style="font-size:36px">🚕</div>
    </div>
    
    <div class="driver-info-card">
      <div class="driver-avatar-lg">${state.driver.emoji}</div>
      <div class="driver-details">
        <div class="driver-name-lg">${state.driver.name} ⭐ ${state.driver.rating}</div>
        <div class="driver-car-lg">${state.driver.car}</div>
        <div class="driver-plate">${state.driver.plate}</div>
      </div>
      <button class="loc-map-btn" onclick="window.location.href='tel:${state.driver.phone}'" style="background:var(--green); color:white; border:none">📞</button>
    </div>
    
    <div class="price-summary">
      <div class="price-col">
        <div class="p-label">الوجهة</div>
        <div class="p-value" style="font-size:15px">${state.dropArea}</div>
      </div>
      <div class="price-col" style="text-align:left">
        <div class="p-label">السعر</div>
        <div class="p-value">${fmt(state.price)}<small> د.ع</small></div>
      </div>
    </div>
    
    ${state.arrived 
      ? `<button class="btn btn-primary" onclick="startPassengerRide()">🚀 بدء الرحلة</button>`
      : `<button class="btn btn-secondary" onclick="cancelPassengerRide()">إلغاء الطلب</button>`}`;
}

function renderPassengerInRide() {
  toggleSheet(true);
  const remain = Math.max(1, Math.round((1 - state.progress / 100) * Math.max(3, state.distance * 2)));
  
  document.getElementById('bottom-sheet').innerHTML = `
    <div class="sheet-handle"></div>
    <h2 class="sheet-title" style="font-size:18px">🚕 في الطريق إلى ${state.dropArea}</h2>
    <p style="font-size:14px; color:var(--gray); margin-bottom:12px">الوصول المتوقع بعد ${remain} دقيقة</p>
    
    <div class="progress-container">
      <div class="progress-bar" id="ride-progress" style="width:${state.progress}%"></div>
    </div>
    
    <div class="price-summary" style="margin-top:20px">
      <div class="price-col">
        <div class="p-label">الكلفة النهائية</div>
        <div class="p-value">${fmt(state.price)}<small> د.ع</small></div>
      </div>
    </div>`;
}

function renderPassengerDone() {
  toggleSheet(true);
  document.getElementById('bottom-sheet').innerHTML = `
    <div class="sheet-handle"></div>
    <div class="searching-view" style="padding-top:0">
      <div style="font-size:64px">🎉</div>
      <h3 style="margin-top:12px">وصلت بالسلامة!</h3>
    </div>
    
    <div class="price-summary">
      <div class="price-col">
        <div class="p-label">الكلفة الإجمالية</div>
        <div class="p-value">${fmt(state.price)}<small> د.ع</small></div>
      </div>
      <div class="price-col" style="text-align:left">
        <div class="p-label">المسافة</div>
        <div class="p-value" style="font-size:16px">${state.distance.toFixed(1)} كم</div>
      </div>
    </div>
    
    <div style="text-align:center; font-size:14px; font-weight:800; margin-bottom:8px">قيّم السائق</div>
    <div class="rating-stars" id="rating-stars">
      ${[1,2,3,4,5].map(i => `<span data-val="${i}" onclick="setRating(${i})">⭐</span>`).join('')}
    </div>
    
    <button class="btn btn-primary" onclick="finishPassengerRide()" style="margin-top:16px">تم ✅</button>`;
}

function setRating(val) { 
  document.querySelectorAll('#rating-stars span').forEach(s => 
    s.classList.toggle('active', Number(s.dataset.val) <= val)
  ); 
}

/* =========================================================
   منطق رحلة الراكب (Passenger Ride Logic)
   ========================================================= */
function requestTaxi() {
  if (state.pickArea === state.dropArea && state.pickGov === state.dropGov) {
    return toast('اختر منطقتين مختلفتين', 'error');
  }
  
  state.stage = 'searching'; 
  renderPassengerSearching();
  
  const rideId = 'r_' + Date.now();
  DB.createRide({ 
    id: rideId, 
    passengerName: state.user.name, 
    pickArea: state.pickArea, pickGov: state.pickGov, 
    dropArea: state.dropArea, dropGov: state.dropGov, 
    price: state.price, distance: state.distance 
  });
  
  // محاكاة قبول السائق في حال لم يكن هناك سائق حقيقي
  setTimeout(() => {
    const r = DB.getRide();
    if (r && r.id === rideId && r.status === 'pending') {
      DB.updateRide('accepted');
      startDriverFoundSimulation();
    }
  }, 5000);
}

function startDriverFoundSimulation() {
  if (state.stage !== 'searching') return;
  
  state.driver = DRIVERS_DB[Math.floor(Math.random() * DRIVERS_DB.length)];
  state.stage = 'driver_found'; 
  state.arrived = false; 
  renderPassengerDriverFound();
  
  if (mainMap) {
    const sLat = state.pickLat + 0.02, sLng = state.pickLng + 0.02;
    if (carMarker) mainMap.removeLayer(carMarker);
    
    carMarker = L.marker([sLat, sLng], { icon: L.divIcon({ className:'custom-marker', iconSize:[44,44], iconAnchor:[22,22], html:`<div class="car-marker-icon">🚕</div>` }) }).addTo(mainMap);
    
    const t0 = Date.now(), dur = 4000;
    const tm = setInterval(() => {
      const t = Math.min(1, (Date.now() - t0) / dur);
      carMarker.setLatLng([sLat + (state.pickLat - sLat) * t, sLng + (state.pickLng - sLng) * t]);
      if (t >= 1) { 
        clearInterval(tm); 
        state.arrived = true; 
        if (state.stage === 'driver_found') renderPassengerDriverFound(); 
        toast('السائق وصل! 🎉', 'success'); 
      }
    }, 50);
  }
}

function startPassengerRide() {
  state.stage = 'in_ride'; 
  state.progress = 0; 
  renderPassengerInRide();
  
  const t0 = Date.now(), dur = Math.max(8000, state.distance * 1000);
  rideInterval = setInterval(() => {
    const t = Math.min(1, (Date.now() - t0) / dur); 
    state.progress = Math.round(t * 100);
    const bar = document.getElementById('ride-progress'); 
    if (bar) bar.style.width = state.progress + '%';
    
    if (routePolyline && carMarker) {
      const coords = routePolyline.getLatLngs();
      if (coords.length > 1) {
        const idx = t * (coords.length - 1), i = Math.floor(idx), f = idx - i;
        if (i < coords.length - 1) {
          carMarker.setLatLng([
            coords[i].lat + (coords[i+1].lat - coords[i].lat) * f, 
            coords[i].lng + (coords[i+1].lng - coords[i].lng) * f
          ]);
        }
      }
    }
    
    if (t >= 1) { 
      clearInterval(rideInterval); 
      state.stage = 'done'; 
      DB.clearRide(); 
      renderPassengerDone(); 
    }
  }, 100);
}

function cancelPassengerRide() { 
  DB.clearRide(); 
  resetState(); 
  renderPassengerBook(); 
  updateMarkers(); 
}

function finishPassengerRide() {
  const rating = document.querySelectorAll('#rating-stars span.active').length || 5;
  DB.addHistory(state.user.id, { 
    from: `${state.pickArea}، ${state.pickGov}`, 
    to: `${state.dropArea}، ${state.dropGov}`, 
    price: state.price, 
    km: state.distance.toFixed(1), 
    driver: state.driver ? state.driver.name : 'سائق', 
    rating, 
    date: new Date().toLocaleDateString('en-GB') 
  });
  resetState(); 
  renderPassengerBook(); 
  updateMarkers(); 
  toast('شكراً لك! تم حفظ الرحلة 🌟', 'success');
}

/* =========================================================
   اختيار الموقع من القائمة (Location List)
   ========================================================= */
function openLocationList(target) { 
  pickerTarget = target; 
  pickerStep = 'gov'; 
  document.getElementById('loc-list-title').textContent = target === 'pick' ? 'اختر محافظة الانطلاق' : 'اختر محافظة الوجهة'; 
  renderLocList(); 
  document.getElementById('loc-list-modal').classList.add('show'); 
}

function renderLocList() {
  const list = document.getElementById('loc-list-container'), cGov = pickerTarget === 'pick' ? state.pickGov : state.dropGov;
  
  if (pickerStep === 'gov') {
    list.innerHTML = GOVS.map(g => `
      <li class="loc-list-item" onclick="selectGovList('${g.name}')">
        <span>${g.name}</span><span class="arrow">◀</span>
      </li>
    `).join('');
  } else {
    const g = GOVS.find(x => x.name === cGov);
    list.innerHTML = `
      <li class="loc-list-item" style="background:#f0f0f0; font-size:13px; color:#666" onclick="pickerStep='gov';renderLocList();document.getElementById('loc-list-title').textContent='اختر المحافظة'">
        <span>🔙 رجوع للمحافظات</span>
      </li>
      ${g.areas.map(a => `
        <li class="loc-list-item" onclick="selectAreaList('${a}')">
          <span>${a}</span>
        </li>
      `).join('')}`;
  }
}

function selectGovList(govName) { 
  if (pickerTarget === 'pick') state.pickGov = govName; 
  else state.dropGov = govName; 
  
  pickerStep = 'area'; 
  document.getElementById('loc-list-title').textContent = 'اختر المنطقة'; 
  renderLocList(); 
}

function selectAreaList(areaName) {
  const gov = pickerTarget === 'pick' ? state.pickGov : state.dropGov;
  const c = getAreaCoords(gov, areaName);
  
  if (pickerTarget === 'pick') { 
    state.pickArea = areaName; state.pickLat = c.lat; state.pickLng = c.lng; 
  } else { 
    state.dropArea = areaName; state.dropLat = c.lat; state.dropLng = c.lng; 
  }
  
  document.getElementById('loc-list-modal').classList.remove('show'); 
  updateMarkers(); 
  drawRouteOnMainMap(); 
  renderPassengerBook();
}

/* =========================================================
   اختيار الموقع من الخريطة (Full Map Picker)
   ========================================================= */
function openMapPicker(target) {
  pickerTarget = target; 
  document.getElementById('map-picker-title').textContent = target === 'pick' ? 'حدد نقطة الانطلاق' : 'حدد الوجهة';
  document.getElementById('full-map-overlay').style.display = 'flex';
  
  setTimeout(() => {
    try {
      if (!window.L) { toast('الخريطة غير متاحة، تحقق من النت', 'error'); closeMapPicker(); return; }
      
      if (!pickerMap) {
        pickerMap = L.map('full-map', { zoomControl: false, attributionControl: false }).setView([33.3152, 44.3661], 14);
        L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', { maxZoom: 19 }).addTo(pickerMap);
        pickerMap.on('moveend', updateClosestArea);
      }
      
      pickerMap.invalidateSize();
      pickerMap.setView([target === 'pick' ? state.pickLat : state.dropLat, target === 'pick' ? state.pickLng : state.dropLng], 15);
      updateClosestArea();
    } catch(e) { console.error(e); }
  }, 300);
}

function updateClosestArea() {
  if (!pickerMap) return;
  const c = pickerMap.getCenter(); 
  if (!c) return;
  selectedMapCoords = { lat: c.lat, lng: c.lng };
  
  let min = Infinity, gName = 'بغداد', aName = 'الكرادة';
  GOVS.forEach(g => g.areas.forEach(a => {
    const coords = getAreaCoords(g.name, a), d = haversine(c.lat, c.lng, coords.lat, coords.lng);
    if (d < min) { min = d; gName = g.name; aName = a; }
  }));
  
  document.getElementById('map-picker-text').innerText = `${aName}، ${gName}`;
}

function confirmMapLocation() {
  if (!selectedMapCoords) return;
  const text = document.getElementById('map-picker-text').innerText;
  const parts = text.split('، ');
  if (parts.length < 2) return toast('يرجى تحريك الخريطة لتحديد المنطقة', 'error');
  
  const aName = parts[0], gName = parts[1];
  
  if (pickerTarget === 'pick') { 
    state.pickGov = gName; state.pickArea = aName; state.pickLat = selectedMapCoords.lat; state.pickLng = selectedMapCoords.lng; 
  } else { 
    state.dropGov = gName; state.dropArea = aName; state.dropLat = selectedMapCoords.lat; state.dropLng = selectedMapCoords.lng; 
  }
  
  closeMapPicker(); 
  updateMarkers(); 
  toast('جاري حساب المسار...', 'success'); 
  drawRouteOnMainMap(); 
  renderPassengerBook();
}

function closeMapPicker() { document.getElementById('full-map-overlay').style.display = 'none'; }

async function drawRouteOnMainMap() {
  try {
    const url = `https://router.project-osrm.org/route/v1/driving/${state.pickLng},${state.pickLat};${state.dropLng},${state.dropLat}?overview=full&geometries=geojson`;
    const res = await fetch(url), data = await res.json();
    
    if (data.routes && data.routes[0]) {
      const dist = data.routes[0].distance / 1000; 
      state.distance = dist; 
      state.price = calcPrice(dist, state.carType);
      
      if (mainMap) {
        if (routePolyline) mainMap.removeLayer(routePolyline);
        routePolyline = L.polyline(data.routes[0].geometry.coordinates.map(c => [c[1], c[0]]), { color: '#FFC107', weight: 6, opacity: 0.9 }).addTo(mainMap);
        mainMap.fitBounds(routePolyline.getBounds(), { padding: [80, 80] });
      }
    }
  } catch (e) {
    state.distance = haversine(state.pickLat, state.pickLng, state.dropLat, state.dropLng) * 1.35; 
    state.price = calcPrice(state.distance, state.carType);
    
    if (mainMap) { 
      if (routePolyline) mainMap.removeLayer(routePolyline); 
      routePolyline = L.polyline([[state.pickLat, state.pickLng], [state.dropLat, state.dropLng]], { color: '#FFC107', weight: 6, dashArray: '10, 10' }).addTo(mainMap); 
    }
  }
  if (state.stage === 'book') renderPassengerBook();
}

/* =========================================================
   واجهة السائق (Driver App Logic)
   ========================================================= */
function renderDriverBook() {
  toggleSheet(true);
  document.getElementById('bottom-sheet').innerHTML = `
    <div class="sheet-handle"></div>
    <h2 class="sheet-title">مرحباً كابتن ${state.user.name.split(' ')[0]}</h2>
    <p style="font-size:14px; color:var(--gray); margin-bottom:20px">جاهز لاستلام الطلبات؟</p>
    
    <div class="driver-toggle-row">
      <div>
        <div class="toggle-title">حالة الاتصال</div>
        <div class="toggle-sub">${state.isOnline ? 'أنت متصل الآن' : 'غير متصل'}</div>
      </div>
      <label class="switch">
        <input type="checkbox" id="driver-online-toggle" ${state.isOnline ? 'checked' : ''} onchange="toggleDriverOnline(this.checked)">
        <span class="slider"></span>
      </label>
    </div>
    
    ${state.isOnline ? `
      <div class="searching-view">
        <div class="radar-anim" style="width:80px; height:80px"><span class="emoji" style="font-size:32px">📡</span></div>
        <h3>جاري البحث عن طلبات قريبة...</h3>
        <p>انتظر، سيصلك طلب قريباً</p>
      </div>
    ` : `
      <div class="searching-view" style="opacity:0.5">
        <div style="font-size:48px">💤</div>
        <h3 style="margin-top:12px">غير متصل</h3>
        <p>قم بتفعيل الاتصال لتصلك الطلبات</p>
      </div>
    `}`;
}

function toggleDriverOnline(isOnline) {
  state.isOnline = isOnline; 
  renderDriverBook();
  
  if (isOnline) {
    driverTimeout = setTimeout(() => {
      if (!state.isOnline) return;
      let r = DB.getRide();
      if (!r || r.status !== 'pending') {
        r = { passengerName: 'راكب تجريبي', pickArea: 'الكرادة', pickGov: 'بغداد', dropArea: 'المنصور', dropGov: 'بغداد', price: 8500, distance: 7.5 };
      }
      showIncomingDriverRequest(r);
    }, 5000);
  } else { 
    clearTimeout(driverTimeout); 
  }
}

function showIncomingDriverRequest(r) {
  toggleSheet(true);
  document.getElementById('bottom-sheet').innerHTML = `
    <div class="sheet-handle"></div>
    <h2 class="sheet-title">🚨 طلب جديد!</h2>
    
    <div class="incoming-request">
      <div class="req-header">
        <div class="req-passenger">🙋‍♂️ ${r.passengerName}</div>
        <div class="req-price">${fmt(r.price)} <span style="font-size:12px">د.ع</span></div>
      </div>
      <div class="req-route">
        <div>📍 من: ${r.pickArea}، ${r.pickGov}</div>
        <div>🏁 إلى: ${r.dropArea}، ${r.dropGov}</div>
      </div>
      <div class="req-dist">المسافة: ${r.distance} كم</div>
    </div>
    
    <button class="btn btn-blue" onclick="acceptDriverRequest()" style="margin-bottom:12px">قبول الطلب</button>
    <button class="btn btn-secondary" onclick="rejectDriverRequest()">تجاهل</button>`;
}

function acceptDriverRequest() { 
  state.stage = 'driver_ride'; 
  DB.updateRide('accepted'); 
  renderDriverActiveRide(); 
  toast('تم قبول الطلب، توجه للراكب', 'success'); 
}

function rejectDriverRequest() { 
  DB.clearRide(); 
  state.stage = 'book'; 
  renderDriverBook(); 
  toast('تم تجاهل الطلب'); 
}

function renderDriverActiveRide() {
  toggleSheet(true);
  document.getElementById('bottom-sheet').innerHTML = `
    <div class="sheet-handle"></div>
    <div style="background:var(--blue); color:var(--white); padding:16px; border-radius:16px; display:flex; justify-content:space-between; align-items:center; margin-bottom:16px">
      <div><div style="font-size:13px; opacity:0.8">أنت في رحلة</div><div style="font-size:20px; font-weight:800">جاري التوصيل</div></div>
      <div style="font-size:36px">🚕</div>
    </div>
    
    <div class="price-summary">
      <div class="price-col"><div class="p-label">الوجهة</div><div class="p-value" style="font-size:15px">المنصور، بغداد</div></div>
      <div class="price-col" style="text-align:left"><div class="p-label">الأجرة</div><div class="p-value">${fmt(8500)}<small> د.ع</small></div></div>
    </div>
    
    <button class="btn btn-primary" onclick="finishDriverRide()">🏁 إنهاء الرحلة</button>`;
}

function finishDriverRide() { 
  DB.clearRide(); 
  state.stage = 'book'; 
  renderDriverBook(); 
  toast('تم إنهاء الرحلة بنجاح 💸', 'success'); 
}

/* =========================================================
   الملف الشخصي (Profile Modal)
   ========================================================= */
function showProfile() {
  const u = state.user, r = u.rides || [];
  document.getElementById('profile-modal-content').innerHTML = `
    <div class="sheet-handle"></div>
    <div class="profile-header">
      <div class="profile-avatar ${state.role === 'driver' ? 'driver' : ''}">${initials(u.name)}</div>
      <div class="profile-name">${u.name}</div>
      <div class="profile-phone">📱 ${u.phone}</div>
      <div class="profile-badge">📍 ${u.gov} | ${state.role === 'driver' ? 'سائق 🚕' : 'راكب 🙋‍♂️'}</div>
    </div>
    
    <div class="stats-grid">
      <div class="stat-box"><div class="stat-val">${r.length}</div><div class="stat-lbl">رحلة</div></div>
      <div class="stat-box"><div class="stat-val">${fmt(r.reduce((s, x) => s + x.price, 0))}</div><div class="stat-lbl">د.ع مصروف</div></div>
    </div>
    
    <button class="btn btn-secondary" onclick="logoutUser()">تسجيل الخروج</button>
    <button class="btn btn-ghost" style="margin-top:10px" onclick="document.getElementById('profile-modal').classList.remove('show')">إغلاق</button>`;
  document.getElementById('profile-modal').classList.add('show');
}

function logoutUser() {
  if (!confirm('هل تريد تسجيل الخروج؟')) return;
  DB.logout(); 
  state.user = null;
  document.getElementById('profile-modal').classList.remove('show');
  if (mainMap) { try { mainMap.remove(); } catch(e){} mainMap = null; }
  renderAuth();
}

/* =========================================================
   المزامنة بين النوافذ (Cross-Tab Sync)
   ========================================================= */
window.addEventListener('storage', (e) => {
  if (e.key === DB.KEY && state.user) {
    const ride = DB.getRide();
    // سائق يستلم طلب جديد من راكب في نافذة أخرى
    if (state.role === 'driver' && state.isOnline && ride && ride.status === 'pending' && state.stage === 'book') {
      clearTimeout(driverTimeout); 
      showIncomingDriverRequest(ride);
    }
    // راكب يعرف إن السائق قبل الطلب
    if (state.role === 'passenger' && state.stage === 'searching' && ride && ride.status === 'accepted') {
      startDriverFoundSimulation();
    }
  }
});

/* =========================================================
   التشغيل (Boot)
   ========================================================= */
setTimeout(() => {
  try {
    const user = DB.getSession();
    if (!window.L) {
      document.getElementById('boot-text').innerText = "تعذر تحميل الخريطة";
      document.getElementById('boot-screen').innerHTML += '<p style="margin-top:10px;font-size:14px">تأكد من اتصال الإنترنت وأعد التحميل</p>';
      return;
    }
    if (user) { 
      state.user = user; 
      state.role = user.role || 'passenger'; 
      renderMainApp(); 
    } else { 
      renderAuth(); 
    }
  } catch (e) {
    console.error(e);
    document.getElementById('boot-text').innerText = "حدث خطأ";
    document.getElementById('boot-screen').innerHTML += '<button onclick="location.reload()" style="margin-top:10px;padding:8px 16px;border:none;background:#fff;border-radius:8px;font-weight:bold;">إعادة المحاولة</button>';
  }
}, 1000);
</script>
</body>
</html>
