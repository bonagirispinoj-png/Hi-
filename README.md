<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0">
<title>RideGo — Smart Rides</title>
<link href="https://fonts.googleapis.com/css2?family=Nunito:wght@400;600;700;800;900&display=swap" rel="stylesheet">
<style>
:root {
  --primary: #FF6B35;
  --primary-dark: #e55a25;
  --secondary: #1A1A2E;
  --accent: #16213E;
  --green: #00C853;
  --green-dark: #00A344;
  --red: #FF3D57;
  --yellow: #FFD600;
  --purple: #7C4DFF;
  --blue: #2196F3;
  --bg: #F5F5F5;
  --card: #ffffff;
  --text: #1A1A2E;
  --muted: #888;
  --border: #E8E8E8;
  --shadow: 0 4px 20px rgba(0,0,0,0.08);
}
* { box-sizing: border-box; margin: 0; padding: 0; -webkit-tap-highlight-color: transparent; }
body { font-family: 'Nunito', sans-serif; background: var(--bg); color: var(--text); min-height: 100vh; overflow-x: hidden; }
.screen { display: none; }
.screen.active { display: block; }

.app-header { background: var(--secondary); color: #fff; padding: 14px 16px; display: flex; align-items: center; justify-content: space-between; position: sticky; top: 0; z-index: 100; }
.header-sub { font-size: 11px; opacity: 0.6; font-weight: 400; }
.card { background: var(--card); border-radius: 16px; padding: 16px; margin: 10px; box-shadow: var(--shadow); }
.card-title { font-size: 16px; font-weight: 800; margin-bottom: 12px; }

input, select, textarea {
  display: block; width: 100%; padding: 13px 14px; margin: 6px 0 12px;
  border: 1.5px solid var(--border); border-radius: 12px; font-size: 15px;
  font-family: 'Nunito', sans-serif; background: #fff; color: var(--text); transition: border-color 0.2s;
}
input:focus, select:focus, textarea:focus { outline: none; border-color: var(--primary); }
label { font-size: 13px; font-weight: 700; color: var(--muted); text-transform: uppercase; letter-spacing: 0.5px; display: block; margin-bottom: 2px; }
textarea { resize: vertical; min-height: 70px; }

.btn {
  display: block; width: 100%; padding: 14px; margin: 6px 0;
  border: none; border-radius: 12px; font-size: 15px; font-weight: 800;
  cursor: pointer; font-family: 'Nunito', sans-serif; transition: all 0.2s; text-align: center;
}
.btn:active { transform: scale(0.97); }
.btn-primary { background: var(--primary); color: #fff; }
.btn-secondary { background: var(--secondary); color: #fff; }
.btn-green { background: var(--green); color: #fff; }
.btn-red { background: var(--red); color: #fff; }
.btn-gray { background: #E8E8E8; color: var(--text); }
.btn-outline { background: transparent; border: 2px solid var(--primary); color: var(--primary); }
.btn-sm { padding: 8px 14px; font-size: 13px; margin: 4px; width: auto; display: inline-block; border-radius: 8px; }
.btn-purple { background: var(--purple); color: #fff; }
.btn-blue { background: var(--blue); color: #fff; }

#screen-landing { min-height: 100vh; background: linear-gradient(160deg, var(--secondary) 0%, var(--accent) 60%, #0F3460 100%); }
.landing-hero { padding: 50px 20px 30px; text-align: center; }
.landing-hero .logo { font-size: 42px; font-weight: 900; color: #fff; letter-spacing: -1px; }
.landing-hero .logo span { color: var(--primary); }
.landing-hero .tagline { color: rgba(255,255,255,0.65); font-size: 15px; margin-top: 8px; }
.landing-hero .hero-icon { font-size: 72px; margin: 20px 0; animation: float 3s ease-in-out infinite; }
@keyframes float { 0%,100% { transform: translateY(0); } 50% { transform: translateY(-10px); } }
.landing-cards { padding: 0 16px 30px; }
.role-card { background: rgba(255,255,255,0.08); backdrop-filter: blur(10px); border: 1px solid rgba(255,255,255,0.15); border-radius: 20px; padding: 20px; margin: 12px 0; cursor: pointer; transition: all 0.2s; display: flex; align-items: center; gap: 16px; }
.role-card:active { transform: scale(0.97); background: rgba(255,255,255,0.15); }
.role-card .role-icon { font-size: 36px; }
.role-card .role-info h3 { color: #fff; font-size: 17px; font-weight: 800; }
.role-card .role-info p { color: rgba(255,255,255,0.55); font-size: 13px; margin-top: 2px; }
.role-card .arrow { margin-left: auto; color: rgba(255,255,255,0.4); font-size: 20px; }

/* Google Maps container */
.map-container { height: 240px; border-radius: 14px; overflow: hidden; margin: 10px; background: #e0e0e0; position: relative; }
.locate-btn { position: absolute; bottom: 10px; right: 10px; background: white; border: none; border-radius: 50%; width: 40px; height: 40px; font-size: 18px; cursor: pointer; box-shadow: 0 2px 8px rgba(0,0,0,0.2); z-index: 500; }
#gmap-booking, #gmap-active, #gmap-driver { width: 100%; height: 100%; border-radius: 14px; }

.vehicle-grid { display: grid; grid-template-columns: repeat(3, 1fr); gap: 8px; margin: 10px 0; }
.vehicle-option { background: #f8f8f8; border: 2px solid transparent; border-radius: 12px; padding: 12px 6px; text-align: center; cursor: pointer; transition: all 0.2s; }
.vehicle-option.selected { border-color: var(--primary); background: #fff3ee; }
.vehicle-option .v-icon { font-size: 26px; display: block; }
.vehicle-option .v-name { font-size: 12px; font-weight: 700; margin-top: 4px; }
.vehicle-option .v-price { font-size: 11px; color: var(--muted); }

.fare-box { background: linear-gradient(135deg, #fff3ee, #ffe8dc); border: 1.5px solid #ffcdb5; border-radius: 14px; padding: 14px; margin: 8px 0; }
.fare-box .fare-amount { font-size: 28px; font-weight: 900; color: var(--primary); }
.fare-box .fare-label { font-size: 13px; color: var(--muted); }

.ride-tracking { text-align: center; padding: 20px 10px; }
.tracking-steps { display: flex; flex-direction: column; gap: 0; margin: 20px 10px; }
.tracking-step { display: flex; align-items: flex-start; gap: 14px; padding: 12px 0; position: relative; }
.tracking-step:not(:last-child)::after { content: ''; position: absolute; left: 15px; top: 50px; width: 2px; height: calc(100% - 20px); background: var(--border); }
.step-dot { width: 32px; height: 32px; border-radius: 50%; display: flex; align-items: center; justify-content: center; font-size: 15px; flex-shrink: 0; }
.step-dot.done { background: var(--green); }
.step-dot.active { background: var(--primary); animation: pulse 1.5s infinite; }
.step-dot.pending { background: var(--border); }
@keyframes pulse { 0%,100% { box-shadow: 0 0 0 0 rgba(255,107,53,0.4); } 50% { box-shadow: 0 0 0 8px rgba(255,107,53,0); } }
.step-info h4 { font-size: 15px; font-weight: 700; }
.step-info p { font-size: 13px; color: var(--muted); margin-top: 2px; }

.driver-info-card { background: var(--secondary); color: #fff; border-radius: 20px; padding: 16px; margin: 10px; }
.driver-info-card .d-name { font-size: 18px; font-weight: 800; }
.driver-info-card .d-detail { font-size: 13px; opacity: 0.7; margin-top: 2px; }
.driver-info-card .d-rating { color: var(--yellow); font-size: 13px; margin-top: 4px; }
.driver-avatar { width: 56px; height: 56px; border-radius: 50%; background: rgba(255,255,255,0.15); font-size: 28px; display: flex; align-items: center; justify-content: center; flex-shrink: 0; }
.contact-row { display: flex; gap: 8px; margin-top: 12px; }
.contact-btn { flex: 1; background: rgba(255,255,255,0.12); border: none; color: #fff; border-radius: 10px; padding: 10px; font-size: 12px; font-weight: 700; font-family: 'Nunito', sans-serif; cursor: pointer; }

.badge { display: inline-block; padding: 4px 10px; border-radius: 20px; font-size: 12px; font-weight: 800; }
.badge-pending { background: #FFF3E0; color: #E65100; }
.badge-accepted { background: #E3F2FD; color: #1565C0; }
.badge-completed { background: #E8F5E9; color: #1B5E20; }
.badge-cancelled { background: #FFEBEE; color: #B71C1C; }
.badge-approved { background: #E8F5E9; color: #1B5E20; }
.badge-rejected { background: #FFEBEE; color: #B71C1C; }
.badge-review { background: #EDE7F6; color: #4A148C; }
.badge-online { background: #E8F5E9; color: #1B5E20; }
.badge-trial { background: #E3F2FD; color: #1565C0; }

.tabs { display: flex; gap: 6px; padding: 10px 10px 0; overflow-x: auto; }
.tabs::-webkit-scrollbar { display: none; }
.tab-btn { padding: 8px 16px; border-radius: 20px; border: none; font-size: 13px; font-weight: 700; font-family: 'Nunito', sans-serif; cursor: pointer; white-space: nowrap; transition: all 0.2s; background: #E8E8E8; color: var(--text); }
.tab-btn.active { background: var(--primary); color: #fff; }

.ride-card { background: #fff; border-radius: 14px; padding: 14px; margin: 8px 10px; box-shadow: var(--shadow); border-left: 4px solid var(--border); }
.ride-card.pending { border-left-color: var(--yellow); }
.ride-card.accepted { border-left-color: var(--primary); }
.ride-card.completed { border-left-color: var(--green); }
.ride-card.cancelled { border-left-color: var(--red); }
.ride-card h4 { font-size: 15px; font-weight: 800; margin-bottom: 6px; }
.ride-card p { font-size: 13px; color: var(--muted); margin: 2px 0; }
.ride-card .fare { font-size: 20px; font-weight: 900; color: var(--primary); }
.route-line { display: flex; flex-direction: column; gap: 4px; background: #f8f8f8; border-radius: 10px; padding: 10px 12px; margin: 8px 0; }
.route-line .pickup { font-size: 13px; font-weight: 700; color: var(--green-dark); }
.route-line .drop { font-size: 13px; font-weight: 700; color: var(--red); }

.duty-toggle { background: var(--secondary); border-radius: 16px; padding: 16px; margin: 10px; display: flex; align-items: center; justify-content: space-between; }
.duty-toggle .toggle-info h3 { color: #fff; font-size: 16px; font-weight: 800; }
.duty-toggle .toggle-info p { color: rgba(255,255,255,0.5); font-size: 12px; }
.toggle-switch { position: relative; width: 56px; height: 28px; }
.toggle-switch input { opacity: 0; width: 0; height: 0; }
.toggle-slider { position: absolute; top: 0; left: 0; right: 0; bottom: 0; background: #555; border-radius: 28px; transition: 0.3s; cursor: pointer; }
.toggle-slider:before { position: absolute; content: ''; width: 22px; height: 22px; top: 3px; left: 3px; background: white; border-radius: 50%; transition: 0.3s; }
input:checked + .toggle-slider { background: var(--green); }
input:checked + .toggle-slider:before { transform: translateX(28px); }

.earnings-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 10px; margin: 10px; }
.earnings-item { background: #fff; border-radius: 14px; padding: 14px; box-shadow: var(--shadow); text-align: center; }
.earnings-item .amount { font-size: 22px; font-weight: 900; color: var(--primary); }
.earnings-item .label { font-size: 12px; color: var(--muted); font-weight: 600; }

.admin-stat-grid { display: grid; grid-template-columns: repeat(2,1fr); gap: 10px; margin: 10px; }
.stat-card { background: var(--card); border-radius: 14px; padding: 16px; box-shadow: var(--shadow); text-align: center; }
.stat-card .stat-num { font-size: 28px; font-weight: 900; color: var(--primary); }
.stat-card .stat-label { font-size: 12px; color: var(--muted); font-weight: 700; }

.kyc-section { background: #f8f8f8; border-radius: 12px; padding: 14px; margin: 10px 0; border: 2px dashed var(--border); }
.kyc-section.uploaded { border-color: var(--green); background: #f0fff4; border-style: solid; }
.kyc-section.verified { border-color: var(--green); background: #e8f5e9; border-style: solid; }
.kyc-section.rejected { border-color: var(--red); background: #ffebee; border-style: solid; }
.kyc-section h4 { font-size: 14px; font-weight: 800; margin-bottom: 8px; }
.kyc-section .upload-status { font-size: 12px; color: var(--muted); }

.spinner { display: inline-block; width: 20px; height: 20px; border: 3px solid rgba(255,255,255,0.3); border-top-color: #fff; border-radius: 50%; animation: spin 0.8s linear infinite; vertical-align: middle; }
@keyframes spin { to { transform: rotate(360deg); } }

#toast { position: fixed; bottom: 80px; left: 50%; transform: translateX(-50%) translateY(20px); background: rgba(26,26,46,0.95); color: #fff; padding: 12px 20px; border-radius: 12px; font-size: 14px; font-weight: 600; z-index: 9999; opacity: 0; transition: all 0.3s; white-space: nowrap; pointer-events: none; max-width: 90vw; text-align: center; }
#toast.show { opacity: 1; transform: translateX(-50%) translateY(0); }

.searching-animation { text-align: center; padding: 40px 20px; }
.search-ring { width: 80px; height: 80px; border: 4px solid var(--border); border-top-color: var(--primary); border-radius: 50%; animation: spin 1.2s linear infinite; margin: 0 auto 16px; }

.bottom-nav { position: fixed; bottom: 0; left: 0; right: 0; background: #fff; border-top: 1px solid var(--border); display: flex; z-index: 99; box-shadow: 0 -4px 20px rgba(0,0,0,0.08); }
.nav-item { flex: 1; padding: 10px 4px; text-align: center; cursor: pointer; transition: color 0.2s; color: var(--muted); font-size: 10px; font-weight: 700; display: flex; flex-direction: column; align-items: center; gap: 2px; }
.nav-item.active { color: var(--primary); }
.nav-item .nav-icon { font-size: 22px; }
.page-with-nav { padding-bottom: 70px; }

.section-label { font-size: 11px; font-weight: 800; color: var(--muted); text-transform: uppercase; letter-spacing: 1px; padding: 8px 16px 4px; }

.location-input-wrap { position: relative; }
.location-input-wrap input { padding-right: 44px; }
.location-input-wrap .loc-icon { position: absolute; right: 12px; top: 50%; transform: translateY(-50%); font-size: 20px; cursor: pointer; }

.payment-row { display: flex; gap: 8px; margin: 8px 0; }
.payment-opt { flex: 1; border: 2px solid var(--border); border-radius: 12px; padding: 12px 6px; text-align: center; cursor: pointer; transition: all 0.2s; }
.payment-opt.selected { border-color: var(--primary); background: #fff3ee; }
.payment-opt .p-icon { font-size: 22px; }
.payment-opt .p-label { font-size: 12px; font-weight: 700; margin-top: 4px; }

.sos-btn { background: var(--red); color: #fff; border: none; border-radius: 50%; width: 60px; height: 60px; font-size: 24px; cursor: pointer; animation: pulse-sos 2s infinite; }
@keyframes pulse-sos { 0%,100% { box-shadow: 0 0 0 0 rgba(255,61,87,0.4); } 50% { box-shadow: 0 0 0 12px rgba(255,61,87,0); } }

.modal-overlay { position: fixed; top: 0; left: 0; right: 0; bottom: 0; background: rgba(0,0,0,0.6); z-index: 500; display: flex; align-items: flex-end; }
.modal-sheet { background: #fff; border-radius: 24px 24px 0 0; padding: 24px 16px; width: 100%; max-height: 80vh; overflow-y: auto; animation: slideUp 0.3s ease; }
@keyframes slideUp { from { transform: translateY(100%); } to { transform: translateY(0); } }
.modal-handle { width: 40px; height: 4px; background: var(--border); border-radius: 4px; margin: 0 auto 16px; }

.approval-banner { background: #FFF3E0; border: 1.5px solid #FFB300; border-radius: 12px; padding: 14px; margin: 10px; }
.approval-banner h4 { color: #E65100; font-size: 15px; font-weight: 800; }
.approval-banner p { color: #BF360C; font-size: 13px; margin-top: 4px; }

.trial-banner { background: linear-gradient(135deg,#e3f2fd,#bbdefb); border: 1.5px solid var(--blue); border-radius: 12px; padding: 14px; margin: 10px; }
.trial-banner h4 { color: #1565C0; font-size: 15px; font-weight: 800; }
.trial-banner p { color: #1976D2; font-size: 13px; margin-top: 4px; }

.free-badge { background: linear-gradient(135deg, #00C853, #00A344); color: #fff; border-radius: 12px; padding: 12px 16px; margin: 10px; display: flex; align-items: center; gap: 12px; }
.free-badge .fb-icon { font-size: 28px; }
.free-badge h4 { font-size: 15px; font-weight: 800; }
.free-badge p { font-size: 12px; opacity: 0.9; margin-top: 2px; }

.pickup-mode-row { display: flex; gap: 8px; margin: 0 0 8px; }
.pickup-mode-btn { flex: 1; padding: 8px; border-radius: 10px; border: 2px solid var(--border); background: #f8f8f8; font-family: 'Nunito', sans-serif; font-size: 12px; font-weight: 700; cursor: pointer; text-align: center; transition: all 0.2s; }
.pickup-mode-btn.active { border-color: var(--primary); background: #fff3ee; color: var(--primary); }

.notif-banner { background: linear-gradient(135deg, var(--purple), #5c35cc); color: #fff; border-radius: 12px; padding: 12px 16px; margin: 10px; display: flex; align-items: center; justify-content: space-between; gap: 12px; }
.notif-banner p { font-size: 13px; font-weight: 700; flex: 1; }
.notif-banner button { background: #fff; color: var(--purple); border: none; border-radius: 8px; padding: 6px 12px; font-size: 12px; font-weight: 800; font-family: 'Nunito', sans-serif; cursor: pointer; white-space: nowrap; }

.live-dot { display: inline-block; width: 8px; height: 8px; background: var(--green); border-radius: 50%; animation: pulse-dot 1.5s infinite; margin-right: 5px; }
@keyframes pulse-dot { 0%,100% { box-shadow: 0 0 0 0 rgba(0,200,83,0.5); } 50% { box-shadow: 0 0 0 6px rgba(0,200,83,0); } }

.route-info-bar { background: linear-gradient(135deg,#fff3ee,#ffe8dc); border-radius: 12px; padding: 10px 14px; margin: 4px 10px; display: flex; align-items: center; justify-content: space-between; border: 1px solid #ffcdb5; }
.route-info-bar .ri-item { text-align: center; }
.route-info-bar .ri-val { font-size: 18px; font-weight: 900; color: var(--primary); }
.route-info-bar .ri-lbl { font-size: 10px; color: var(--muted); font-weight: 700; text-transform: uppercase; }

/* Chat / Messaging */
.chat-overlay { position: fixed; top: 0; left: 0; right: 0; bottom: 0; background: rgba(0,0,0,0.7); z-index: 600; display: flex; align-items: flex-end; }
.chat-sheet { background: #fff; border-radius: 24px 24px 0 0; width: 100%; height: 70vh; display: flex; flex-direction: column; animation: slideUp 0.3s ease; }
.chat-header { padding: 16px; border-bottom: 1px solid var(--border); display: flex; align-items: center; justify-content: space-between; }
.chat-messages { flex: 1; overflow-y: auto; padding: 12px 16px; display: flex; flex-direction: column; gap: 8px; }
.chat-msg { max-width: 75%; padding: 10px 14px; border-radius: 16px; font-size: 14px; }
.chat-msg.sent { background: var(--primary); color: #fff; border-radius: 16px 16px 4px 16px; align-self: flex-end; }
.chat-msg.received { background: #f0f0f0; color: var(--text); border-radius: 16px 16px 16px 4px; align-self: flex-start; }
.chat-msg .msg-time { font-size: 10px; opacity: 0.7; margin-top: 4px; }
.chat-input-row { padding: 12px 16px; border-top: 1px solid var(--border); display: flex; gap: 8px; align-items: center; padding-bottom: max(12px, env(safe-area-inset-bottom)); }
.chat-input-row input { margin: 0; border-radius: 24px; background: #f5f5f5; }
.chat-send-btn { background: var(--primary); border: none; border-radius: 50%; width: 44px; height: 44px; color: #fff; font-size: 18px; cursor: pointer; flex-shrink: 0; }

/* Quick message chips */
.quick-msgs { display: flex; gap: 6px; overflow-x: auto; padding: 8px 16px; }
.quick-msgs::-webkit-scrollbar { display: none; }
.quick-chip { background: #f0f0f0; border: none; border-radius: 20px; padding: 6px 14px; font-size: 12px; font-weight: 700; cursor: pointer; font-family: 'Nunito', sans-serif; white-space: nowrap; }

/* Driver incoming ride alert */
.incoming-alert { background: linear-gradient(135deg, var(--green), var(--green-dark)); color: #fff; border-radius: 16px; padding: 16px; margin: 10px; animation: pulse-alert 1.5s infinite; }
@keyframes pulse-alert { 0%,100% { box-shadow: 0 0 0 0 rgba(0,200,83,0.5); } 50% { box-shadow: 0 0 0 12px rgba(0,200,83,0); } }
.incoming-alert h3 { font-size: 18px; font-weight: 900; }
.incoming-alert .timer-bar { background: rgba(255,255,255,0.3); border-radius: 10px; height: 6px; margin: 10px 0; overflow: hidden; }
.incoming-alert .timer-fill { background: #fff; height: 100%; border-radius: 10px; transition: width 1s linear; }

/* Verification docs */
.doc-verify-row { display: flex; align-items: center; gap: 10px; padding: 12px; background: #f8f8f8; border-radius: 10px; margin: 6px 0; }
.doc-verify-row .doc-icon { font-size: 24px; }
.doc-verify-row .doc-info { flex: 1; }
.doc-verify-row .doc-info h5 { font-size: 14px; font-weight: 700; }
.doc-verify-row .doc-info p { font-size: 12px; color: var(--muted); }
.doc-deadline { background: #fff3e0; border: 1px solid #ffb300; border-radius: 10px; padding: 10px 14px; margin: 8px 0; font-size: 13px; color: #e65100; font-weight: 700; }
.doc-deadline.urgent { background: #ffebee; border-color: var(--red); color: var(--red); }

/* OTP input */
.otp-row { display: flex; gap: 10px; justify-content: center; margin: 16px 0; }
.otp-box { width: 50px; height: 56px; text-align: center; font-size: 22px; font-weight: 900; border: 2px solid var(--border); border-radius: 12px; font-family: 'Nunito', sans-serif; }
.otp-box:focus { outline: none; border-color: var(--primary); }

/* Ride progress bar */
.ride-progress { background: #f0f0f0; border-radius: 10px; height: 8px; margin: 8px 0; overflow: hidden; }
.ride-progress-fill { background: linear-gradient(90deg, var(--primary), var(--yellow)); height: 100%; border-radius: 10px; transition: width 0.5s ease; }

/* Driver location chip */
.driver-eta-chip { background: var(--secondary); color: #fff; border-radius: 12px; padding: 8px 14px; font-size: 13px; font-weight: 700; display: inline-flex; align-items: center; gap: 6px; }

/* Suggestions dropdown */
.suggestions-box { background: #fff; border: 1.5px solid var(--border); border-radius: 12px; margin-top: -8px; margin-bottom: 8px; overflow: hidden; box-shadow: var(--shadow); max-height: 200px; overflow-y: auto; }
.suggestion-item { padding: 12px 14px; cursor: pointer; border-bottom: 1px solid var(--border); font-size: 14px; display: flex; align-items: center; gap: 8px; }
.suggestion-item:last-child { border-bottom: none; }
.suggestion-item:active { background: #f5f5f5; }
</style>
</head>
<body>

<div id="toast"></div>

<!-- ============================== LANDING ============================== -->
<div id="screen-landing" class="screen active">
  <div class="landing-hero">
    <div class="logo">Ride<span>Go</span></div>
    <div class="tagline">Fast · Safe · Reliable rides at your doorstep</div>
    <div class="hero-icon">🚀</div>
  </div>
  <div class="landing-cards">
    <div class="role-card" onclick="App.goUserBooking()">
      <div class="role-icon">🙋</div>
      <div class="role-info"><h3>Book a Ride</h3><p>Find bikes, autos, cars instantly</p></div>
      <div class="arrow">›</div>
    </div>
    <div class="role-card" onclick="App.showScreen('screen-driver-home')">
      <div class="role-icon">🏍️</div>
      <div class="role-info"><h3>I'm a Driver</h3><p>Register free, start earning today</p></div>
      <div class="arrow">›</div>
    </div>
    <div class="role-card" onclick="App.showAdminLogin()">
      <div class="role-icon">🛠</div>
      <div class="role-info"><h3>Admin Panel</h3><p>Manage rides, drivers &amp; payments</p></div>
      <div class="arrow">›</div>
    </div>
  </div>
</div>

<!-- ============================== DRIVER HOME (No login required) ============================== -->
<div id="screen-driver-home" class="screen">
  <div class="app-header">
    <div style="font-size:18px;font-weight:800;color:#fff">Driver Portal</div>
    <button class="btn btn-gray btn-sm" onclick="App.showScreen('screen-landing')">← Back</button>
  </div>
  <div style="padding:16px">
    <div style="background:linear-gradient(135deg,var(--primary),var(--primary-dark));border-radius:20px;padding:24px;color:#fff;margin-bottom:16px">
      <div style="font-size:32px;margin-bottom:8px">🏍️</div>
      <h2 style="font-size:22px;font-weight:900">Start Earning with RideGo</h2>
      <p style="opacity:0.85;font-size:14px;margin-top:6px">No upfront fees. Register free, start immediately. Verify documents within 1 week.</p>
    </div>
    <div style="display:grid;grid-template-columns:1fr 1fr;gap:10px;margin-bottom:16px">
      <div style="background:#fff;border-radius:14px;padding:14px;box-shadow:var(--shadow);text-align:center">
        <div style="font-size:24px">💰</div><div style="font-weight:800;margin-top:4px">₹1,200/day</div><div style="font-size:12px;color:var(--muted)">Avg Earnings</div>
      </div>
      <div style="background:#fff;border-radius:14px;padding:14px;box-shadow:var(--shadow);text-align:center">
        <div style="font-size:24px">⭐</div><div style="font-weight:800;margin-top:4px">20% Cut</div><div style="font-size:12px;color:var(--muted)">Platform Fee</div>
      </div>
      <div style="background:#fff;border-radius:14px;padding:14px;box-shadow:var(--shadow);text-align:center">
        <div style="font-size:24px">📅</div><div style="font-weight:800;margin-top:4px">7 Days</div><div style="font-size:12px;color:var(--muted)">Doc Verify Time</div>
      </div>
      <div style="background:#fff;border-radius:14px;padding:14px;box-shadow:var(--shadow);text-align:center">
        <div style="font-size:24px">🆓</div><div style="font-weight:800;margin-top:4px">Free Join</div><div style="font-size:12px;color:var(--muted)">No Entry Fee</div>
      </div>
    </div>
    <button class="btn btn-green" onclick="App.showScreen('screen-driver-signup')">🚀 Register as Driver — Free</button>
    <button class="btn btn-outline" onclick="App.showScreen('screen-driver-login')">Already registered? Login</button>
  </div>
</div>

<!-- ============================== DRIVER LOGIN ============================== -->
<div id="screen-driver-login" class="screen">
  <div class="app-header">
    <div style="font-size:18px;font-weight:800;color:#fff">Driver Login</div>
    <button class="btn btn-gray btn-sm" onclick="App.showScreen('screen-driver-home')">← Back</button>
  </div>
  <div class="card" style="margin-top:16px">
    <div class="card-title">🏍️ Welcome Back, Driver</div>
    <label>Email</label>
    <input id="d-login-email" type="email" placeholder="driver@email.com">
    <label>Password</label>
    <input id="d-login-pass" type="password" placeholder="••••••••">
    <button class="btn btn-primary" onclick="App.driverLogin()">Login</button>
    <button class="btn btn-outline" onclick="App.showScreen('screen-driver-signup')">New Driver? Register Here</button>
    <div style="text-align:center;margin-top:8px"><span style="font-size:13px;color:var(--muted);cursor:pointer" onclick="App.forgotPassword('driver')">Forgot password?</span></div>
  </div>
</div>

<!-- ============================== DRIVER SIGNUP ============================== -->
<div id="screen-driver-signup" class="screen">
  <div class="app-header">
    <div style="font-size:18px;font-weight:800;color:#fff">Driver Registration</div>
    <button class="btn btn-gray btn-sm" onclick="App.showScreen('screen-driver-home')">← Back</button>
  </div>
  <div style="background:linear-gradient(135deg,#00C853,#00A344);padding:14px 16px;color:#fff">
    <p style="font-size:15px;font-weight:800">⚡ Start Driving Instantly — 7-Day Free Trial!</p>
    <p style="font-size:13px;opacity:0.9;margin-top:4px">Register now and start earning TODAY. Upload documents anytime within 7 days to keep your account active.</p>
  </div>
  <div class="card">
    <div class="card-title">Personal Details</div>
    <label>Full Name</label>
    <input id="s-name" placeholder="As on Aadhaar card">
    <label>Email</label>
    <input id="s-email" type="email" placeholder="your@email.com">
    <label>Phone Number</label>
    <input id="s-phone" type="tel" placeholder="+91 XXXXX XXXXX">
    <label>Password</label>
    <input id="s-pass" type="password" placeholder="Min 8 characters">
    <label>Vehicle Type</label>
    <select id="s-vehicle">
      <option value="">Select vehicle type</option>
      <option>Bike</option><option>Auto</option><option>Car</option>
    </select>
    <label>Vehicle Number</label>
    <input id="s-vnum" placeholder="e.g. TS09AB1234">
    <label>UPI ID (for payouts)</label>
    <input id="s-upi" placeholder="name@upi or phone@bank">
    <label>Bank Account Number</label>
    <input id="s-bank-acc" placeholder="Account number (optional now)">
    <label>IFSC Code</label>
    <input id="s-ifsc" placeholder="e.g. SBIN0001234 (optional now)">
  </div>
  <div class="card">
    <div class="card-title">📄 KYC Documents <span style="background:#e3f2fd;color:#1565c0;font-size:11px;font-weight:800;padding:3px 8px;border-radius:8px;margin-left:6px">Upload within 7 days</span></div>
    <p style="font-size:13px;color:var(--muted);margin-bottom:12px">You can start driving immediately. All documents required within 7 days to maintain active status and get paid.</p>

    <div class="doc-deadline" id="doc-deadline-info">
      ⏰ Upload deadline: <span id="deadline-text">7 days from registration</span>. Required: Licence · RC · Aadhaar · Bank Account · Auto Insurance (if auto)
    </div>

    <div class="kyc-section" id="kyc-licence">
      <h4>🪪 Driving Licence <span style="color:var(--red);font-size:11px">REQUIRED within 7 days</span></h4>
      <input type="file" id="file-licence" accept="image/*,application/pdf" onchange="App.handleFileUpload('licence', this)">
      <p class="upload-status" id="status-licence">Not uploaded — required within 7 days</p>
    </div>
    <div class="kyc-section" id="kyc-rc">
      <h4>📋 Vehicle RC (Registration Certificate) <span style="color:var(--red);font-size:11px">REQUIRED</span></h4>
      <input type="file" id="file-rc" accept="image/*,application/pdf" onchange="App.handleFileUpload('rc', this)">
      <p class="upload-status" id="status-rc">Not uploaded — required within 7 days</p>
    </div>
    <div class="kyc-section" id="kyc-aadhaar">
      <h4>🪪 Aadhaar Card <span style="color:var(--red);font-size:11px">REQUIRED</span></h4>
      <input type="file" id="file-aadhaar" accept="image/*,application/pdf" onchange="App.handleFileUpload('aadhaar', this)">
      <p class="upload-status" id="status-aadhaar">Not uploaded — required within 7 days</p>
    </div>
    <div class="kyc-section" id="kyc-bank">
      <h4>🏦 Bank Passbook / Cancel Cheque <span style="color:var(--red);font-size:11px">REQUIRED for payments</span></h4>
      <input type="file" id="file-bank" accept="image/*,application/pdf" onchange="App.handleFileUpload('bank', this)">
      <p class="upload-status" id="status-bank">Not uploaded — required for withdrawals</p>
    </div>
    <div class="kyc-section" id="kyc-insurance">
      <h4>🛡️ Vehicle Insurance <span style="color:var(--blue);font-size:11px">Required for Autos/Cars</span></h4>
      <input type="file" id="file-insurance" accept="image/*,application/pdf" onchange="App.handleFileUpload('insurance', this)">
      <p class="upload-status" id="status-insurance">Optional for bikes</p>
    </div>
    <div class="kyc-section" id="kyc-selfie">
      <h4>🤳 Profile Selfie</h4>
      <input type="file" id="file-selfie" accept="image/*" capture="user" onchange="App.handleFileUpload('selfie', this)">
      <p class="upload-status" id="status-selfie">Upload for better trust</p>
    </div>
  </div>
  <div style="padding:0 10px 16px">
    <button class="btn btn-green" id="signup-btn" onclick="App.driverSignup()">🚀 Register &amp; Start Driving Now →</button>
    <p style="font-size:12px;color:var(--muted);text-align:center;margin-top:8px">By registering, you agree to RideGo's Terms &amp; Privacy Policy</p>
  </div>
</div>

<!-- ============================== ADMIN LOGIN ============================== -->
<div id="screen-admin-login" class="screen">
  <div class="app-header">
    <div style="font-size:18px;font-weight:800;color:#fff">Admin Login</div>
    <button class="btn btn-gray btn-sm" onclick="App.showScreen('screen-landing')">← Back</button>
  </div>
  <div class="card" style="margin-top:16px">
    <div class="card-title">🛠 Admin Access</div>
    <label>Admin Email</label>
    <input id="a-login-email" type="email" placeholder="admin email" value="bonagirispinoj@gmail.com">
    <label>Password</label>
    <input id="a-login-pass" type="password" placeholder="••••••••">
    <button class="btn btn-secondary" onclick="App.adminLogin()" id="admin-login-btn">Login as Admin</button>
    <div style="text-align:center;margin-top:8px"><span style="font-size:13px;color:var(--muted);cursor:pointer" onclick="App.forgotPassword('admin')">Forgot password?</span></div>
  </div>
</div>

<!-- ============================== USER BOOKING ============================== -->
<div id="screen-user-booking" class="screen page-with-nav">
  <div class="app-header">
    <div style="display:flex;align-items:center;gap:10px">
      <button class="btn btn-gray btn-sm" style="margin:0;padding:6px 10px;font-size:18px;background:rgba(255,255,255,0.15);color:#fff;border:none" onclick="App.showScreen('screen-landing')">←</button>
      <div>
        <div style="font-size:18px;font-weight:800;color:#fff">Book Ride</div>
        <div class="header-sub" id="user-location-sub">Getting your location...</div>
      </div>
    </div>
    <button class="sos-btn" style="width:40px;height:40px;font-size:14px;border-radius:8px" onclick="App.triggerSOS()">SOS</button>
  </div>
  <div id="notif-banner" class="notif-banner" style="display:none">
    <p>🔔 Allow notifications for real-time ride updates</p>
    <button onclick="App.requestNotifPermission()">Allow</button>
  </div>
  <!-- Google Maps container for booking -->
  <div class="map-container" id="booking-map-container" style="height:260px">
    <div id="gmap-booking" style="width:100%;height:100%"></div>
    <button class="locate-btn" onclick="App.recenterMap()">🎯</button>
  </div>
  <!-- Route info bar -->
  <div id="route-info-bar" style="display:none" class="route-info-bar">
    <div class="ri-item"><div class="ri-val" id="ri-km">—</div><div class="ri-lbl">Distance</div></div>
    <div class="ri-item"><div class="ri-val" id="ri-time">—</div><div class="ri-lbl">Est. Time</div></div>
    <div class="ri-item"><div class="ri-val" id="ri-fare">—</div><div class="ri-lbl">Fare</div></div>
  </div>
  <div class="card" style="margin-top:0">
    <label>📍 Pickup Location</label>
    <div class="pickup-mode-row">
      <button class="pickup-mode-btn active" id="pm-live" onclick="App.setPickupMode('live')">📡 Live GPS</button>
      <button class="pickup-mode-btn" id="pm-manual" onclick="App.setPickupMode('manual')">✏️ Manual</button>
    </div>
    <div class="location-input-wrap">
      <input id="pickup-input" placeholder="Your pickup address" oninput="App.pickupInputChanged()">
      <span class="loc-icon" onclick="App.recenterMap()">🎯</span>
    </div>
    <div id="pickup-suggestions" class="suggestions-box" style="display:none"></div>
    <label>🏁 Drop Location</label>
    <div class="location-input-wrap">
      <input id="drop-input" placeholder="Where are you going?" oninput="App.dropInputChanged()">
      <span class="loc-icon" onclick="App.searchDropOnMaps()">🗺</span>
    </div>
    <div id="drop-suggestions" class="suggestions-box" style="display:none"></div>
  </div>
  <div class="section-label">Choose Vehicle</div>
  <div style="padding:0 10px">
    <div class="vehicle-grid" id="vehicle-grid">
      <div class="vehicle-option selected" onclick="App.selectVehicle(this,'Bike')">
        <span class="v-icon">🏍️</span><div class="v-name">Bike</div><div class="v-price">₹14/km</div>
      </div>
      <div class="vehicle-option" onclick="App.selectVehicle(this,'Auto')">
        <span class="v-icon">🛺</span><div class="v-name">Auto</div><div class="v-price">₹22/km</div>
      </div>
      <div class="vehicle-option" onclick="App.selectVehicle(this,'Car')">
        <span class="v-icon">🚗</span><div class="v-name">Car</div><div class="v-price">₹30/km</div>
      </div>
    </div>
  </div>
  <div class="card" id="fare-preview">
    <div class="fare-box">
      <div class="fare-label">Estimated Fare</div>
      <div class="fare-amount" id="fare-display">₹—</div>
      <div style="font-size:12px;color:var(--muted);margin-top:4px" id="dist-display">Enter pickup &amp; drop to see estimate</div>
    </div>
    <label>Payment Method</label>
    <div class="payment-row">
      <div class="payment-opt selected" id="pay-cash" onclick="App.selectPayment('cash')">
        <div class="p-icon">💵</div><div class="p-label">Cash</div>
      </div>
      <div class="payment-opt" id="pay-upi" onclick="App.selectPayment('upi')">
        <div class="p-icon">📱</div><div class="p-label">UPI</div>
      </div>
      <div class="payment-opt" id="pay-wallet" onclick="App.selectPayment('wallet')">
        <div class="p-icon">💳</div><div class="p-label">Wallet</div>
      </div>
    </div>
  </div>
  <div class="card">
    <div class="card-title">Your Details</div>
    <label>Your Name</label>
    <input id="user-name-input" placeholder="Full name">
    <label>Phone Number</label>
    <input id="user-phone-input" type="tel" placeholder="10-digit phone number">
    <label>Note for Driver (Optional)</label>
    <textarea id="user-note" placeholder="e.g. Call on arrival, gate number..." style="min-height:50px"></textarea>
    <button class="btn btn-primary" id="book-btn" onclick="App.bookRide()">🚀 Book Now</button>
  </div>
</div>

<!-- USER BOTTOM NAV -->
<div id="user-bottom-nav" class="bottom-nav" style="display:none">
  <div class="nav-item active" onclick="App.goUserBooking()" id="unav-book">
    <span class="nav-icon">🏠</span>Book
  </div>
  <div class="nav-item" onclick="App.showUserRides()" id="unav-rides">
    <span class="nav-icon">🕒</span>Rides
  </div>
  <div class="nav-item" onclick="App.showUserProfile()" id="unav-profile">
    <span class="nav-icon">👤</span>Profile
  </div>
</div>

<!-- ============================== SEARCHING ============================== -->
<div id="screen-searching" class="screen">
  <div class="app-header">
    <div style="font-size:18px;font-weight:800;color:#fff">Finding Driver...</div>
    <button class="btn btn-red btn-sm" onclick="App.cancelSearch()">Cancel</button>
  </div>
  <div class="searching-animation">
    <div class="search-ring"></div>
    <h3 id="search-status-text" style="font-size:17px;font-weight:800">🔍 Searching for nearby drivers...</h3>
    <p id="search-attempt-text" style="font-size:13px;color:var(--muted);margin-top:6px">This may take up to 60 seconds</p>
  </div>
  <div id="search-booking-summary"></div>
</div>

<!-- ============================== ACTIVE RIDE ============================== -->
<div id="screen-ride-active" class="screen">
  <div class="app-header">
    <div style="display:flex;align-items:center;gap:8px">
      <div>
        <div id="active-ride-status-text" style="font-size:16px;font-weight:800;color:#fff">Driver on the way</div>
        <div class="header-sub">Track your ride live</div>
      </div>
    </div>
    <button class="sos-btn" style="width:40px;height:40px;font-size:14px;border-radius:8px" onclick="App.triggerSOS()">SOS</button>
  </div>
  <div class="map-container" style="height:280px">
    <div id="gmap-active" style="width:100%;height:100%"></div>
  </div>
  <div id="driver-info-section"></div>
  <div class="tracking-steps" id="tracking-steps"></div>
  <div class="card" id="active-ride-actions"></div>
</div>

<!-- ============================== RIDE DONE ============================== -->
<div id="screen-ride-done" class="screen">
  <div class="app-header">
    <button class="btn btn-gray btn-sm" onclick="App.goUserBooking()">← Back</button>
  </div>
  <div style="text-align:center;padding:40px 20px 20px">
    <div style="font-size:60px">🎉</div>
    <h2 style="font-size:24px;font-weight:900;margin:12px 0">Ride Complete!</h2>
    <p style="color:var(--muted);font-size:15px">Thank you for riding with RideGo</p>
  </div>
  <div class="card"><div class="card-title">Ride Summary</div><div id="ride-done-summary"></div></div>
  <div class="card">
    <div class="card-title">⭐ Rate Your Driver</div>
    <div style="display:flex;justify-content:center;gap:8px;margin:12px 0" id="star-rating">
      <span style="font-size:32px;cursor:pointer" onclick="App.setRating(1)">☆</span>
      <span style="font-size:32px;cursor:pointer" onclick="App.setRating(2)">☆</span>
      <span style="font-size:32px;cursor:pointer" onclick="App.setRating(3)">☆</span>
      <span style="font-size:32px;cursor:pointer" onclick="App.setRating(4)">☆</span>
      <span style="font-size:32px;cursor:pointer" onclick="App.setRating(5)">☆</span>
    </div>
    <textarea id="ride-feedback" placeholder="Any feedback? (optional)"></textarea>
    <button class="btn btn-primary" onclick="App.submitRating()">Submit Rating</button>
    <button class="btn btn-gray" onclick="App.goUserBooking()">Skip &amp; Book Another Ride</button>
  </div>
</div>

<!-- ============================== USER RIDES ============================== -->
<div id="screen-user-rides" class="screen page-with-nav">
  <div class="app-header">
    <div style="font-size:18px;font-weight:800;color:#fff">My Rides</div>
  </div>
  <div id="user-rides-list" style="padding-top:8px"></div>
</div>

<!-- ============================== USER PROFILE ============================== -->
<div id="screen-user-profile" class="screen page-with-nav">
  <div class="app-header">
    <div style="font-size:18px;font-weight:800;color:#fff">My Profile</div>
  </div>
  <div class="card" style="margin-top:16px;text-align:center">
    <div style="font-size:56px">👤</div>
    <h3 id="profile-name" style="margin-top:8px;font-size:18px;font-weight:800">Guest User</h3>
    <p style="color:var(--muted);font-size:13px">Total rides: <span id="profile-ride-count">0</span></p>
  </div>
  <div class="card">
    <div class="card-title">Save Your Details</div>
    <label>Name</label><input id="profile-name-input" placeholder="Your name">
    <label>Phone</label><input id="profile-phone-input" type="tel" placeholder="Phone number">
    <button class="btn btn-primary" onclick="App.saveUserProfile()">Save Profile</button>
  </div>
  <div class="card">
    <div class="card-title">📞 Emergency Contact</div>
    <label>Name</label><input id="emer-name" placeholder="Emergency contact name">
    <label>Phone</label><input id="emer-phone" type="tel" placeholder="Emergency phone">
    <button class="btn btn-outline" onclick="App.saveEmergencyContact()">Save Emergency Contact</button>
  </div>
  <div style="padding:10px">
    <button class="btn btn-gray" onclick="App.goUserBooking()">← Back to Booking</button>
  </div>
</div>

<!-- ============================== DRIVER DASHBOARD ============================== -->
<div id="screen-driver" class="screen page-with-nav">
  <div class="app-header">
    <div>
      <div style="font-size:18px;font-weight:800;color:#fff" id="driver-header-name">Driver Dashboard</div>
      <div class="header-sub" id="driver-header-status">Loading...</div>
    </div>
    <button class="btn btn-gray btn-sm" onclick="App.driverLogout()">Logout</button>
  </div>
  <div id="driver-approval-banner" style="display:none"></div>
  <div class="duty-toggle" id="duty-toggle-section" style="display:none">
    <div class="toggle-info">
      <h3 id="duty-label">Go Online</h3>
      <p id="duty-sublabel">Tap to start accepting rides</p>
    </div>
    <label class="toggle-switch">
      <input type="checkbox" id="duty-toggle" onchange="App.toggleDuty(this.checked)">
      <span class="toggle-slider"></span>
    </label>
  </div>
  <div id="driver-main-content"></div>
</div>

<!-- DRIVER BOTTOM NAV -->
<div id="driver-bottom-nav" class="bottom-nav" style="display:none">
  <div class="nav-item active" onclick="App.driverNav('rides')" id="dnav-rides">
    <span class="nav-icon">🏠</span>Rides
  </div>
  <div class="nav-item" onclick="App.driverNav('earnings')" id="dnav-earnings">
    <span class="nav-icon">💰</span>Earn
  </div>
  <div class="nav-item" onclick="App.driverNav('kyc')" id="dnav-kyc">
    <span class="nav-icon">📄</span>Docs
  </div>
  <div class="nav-item" onclick="App.driverNav('withdraw')" id="dnav-withdraw">
    <span class="nav-icon">🏧</span>Withdraw
  </div>
  <div class="nav-item" onclick="App.driverNav('profile')" id="dnav-profile">
    <span class="nav-icon">👤</span>Profile
  </div>
</div>

<!-- ============================== DRIVER ACTIVE RIDE SCREEN ============================== -->
<div id="screen-driver-ride" class="screen">
  <div class="app-header">
    <div>
      <div style="font-size:18px;font-weight:800;color:#fff" id="dride-status-title">Ride in Progress</div>
      <div class="header-sub" id="dride-status-sub">Heading to pickup</div>
    </div>
  </div>
  <div class="map-container" style="height:260px">
    <div id="gmap-driver" style="width:100%;height:100%"></div>
  </div>
  <div id="driver-ride-detail"></div>
  <div class="card" id="driver-ride-actions"></div>
</div>

<!-- ============================== ADMIN DASHBOARD ============================== -->
<div id="screen-admin" class="screen page-with-nav">
  <div class="app-header">
    <div>
      <div style="font-size:18px;font-weight:800;color:#fff">Admin Panel</div>
      <div class="header-sub">RideGo Management</div>
    </div>
    <button class="btn btn-gray btn-sm" onclick="App.adminLogout()">Logout</button>
  </div>
  <div class="admin-stat-grid" id="admin-stats-grid"></div>
  <div class="tabs" id="admin-tabs">
    <button class="tab-btn active" onclick="App.adminTab('rides')">All Rides</button>
    <button class="tab-btn" onclick="App.adminTab('kyc')">🔍 KYC</button>
    <button class="tab-btn" onclick="App.adminTab('drivers')">Drivers</button>
    <button class="tab-btn" onclick="App.adminTab('withdrawals')">🏧 Payouts</button>
    <button class="tab-btn" onclick="App.adminTab('analytics')">📊 Stats</button>
  </div>
  <div id="admin-content" style="padding-bottom:70px"></div>
</div>
<div id="admin-bottom-nav" class="bottom-nav" style="display:none">
  <div class="nav-item active" onclick="App.adminTab('rides')"><span class="nav-icon">🚗</span>Rides</div>
  <div class="nav-item" onclick="App.adminTab('kyc')"><span class="nav-icon">🔍</span>KYC</div>
  <div class="nav-item" onclick="App.adminTab('withdrawals')"><span class="nav-icon">🏧</span>Payouts</div>
  <div class="nav-item" onclick="App.adminTab('analytics')"><span class="nav-icon">📊</span>Stats</div>
</div>

<!-- ============================== CHAT OVERLAY ============================== -->
<div id="chat-overlay" style="display:none" class="chat-overlay" onclick="App.closeChatIfBg(event)">
  <div class="chat-sheet">
    <div class="modal-handle"></div>
    <div class="chat-header">
      <div>
        <div style="font-size:16px;font-weight:800" id="chat-title">Chat</div>
        <div style="font-size:12px;color:var(--muted)" id="chat-subtitle">End-to-end encrypted</div>
      </div>
      <button onclick="App.closeChat()" style="background:none;border:none;font-size:22px;cursor:pointer">✕</button>
    </div>
    <div class="quick-msgs" id="quick-msgs">
      <button class="quick-chip" onclick="App.sendQuick('I am 2 minutes away')">⏱ 2 mins away</button>
      <button class="quick-chip" onclick="App.sendQuick('I have arrived, please come out')">📍 Arrived</button>
      <button class="quick-chip" onclick="App.sendQuick('Please share exact location')">🗺 Share location</button>
      <button class="quick-chip" onclick="App.sendQuick('On the way!')">🚀 On the way</button>
      <button class="quick-chip" onclick="App.sendQuick('Traffic ahead, may be delayed')">🚦 Traffic delay</button>
    </div>
    <div class="chat-messages" id="chat-messages">
      <div style="text-align:center;padding:20px;color:var(--muted);font-size:13px">💬 Start chatting with your driver/passenger</div>
    </div>
    <div class="chat-input-row">
      <input id="chat-input" placeholder="Type a message..." onkeypress="if(event.key==='Enter')App.sendChat()">
      <button class="chat-send-btn" onclick="App.sendChat()">➤</button>
    </div>
  </div>
</div>

<!-- ============================== FIREBASE + APP LOGIC ============================== -->
<script type="module">
import { initializeApp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js";
import {
  getFirestore, collection, addDoc, onSnapshot, updateDoc,
  doc, query, where, runTransaction, setDoc, getDoc, getDocs,
  orderBy, limit, serverTimestamp, deleteDoc, increment
} from "https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore.js";
import {
  getAuth, createUserWithEmailAndPassword,
  signInWithEmailAndPassword, signOut, sendPasswordResetEmail
} from "https://www.gstatic.com/firebasejs/10.7.1/firebase-auth.js";

// ─── FIREBASE CONFIG ─────────────────────────────────────────────────────────
const firebaseConfig = {
  apiKey: "AIzaSyBIebPXVjzVBZl380ZCELg2LUSCLr-6QrE",
  authDomain: "neurocoin-ntc.firebaseapp.com",
  databaseURL: "https://neurocoin-ntc-default-rtdb.firebaseio.com",
  projectId: "neurocoin-ntc",
  storageBucket: "neurocoin-ntc.firebasestorage.app",
  messagingSenderId: "1040732555800",
  appId: "1:1040732555800:web:55e63b5ff81781c6b1916d"
};
const fbApp = initializeApp(firebaseConfig);
const db    = getFirestore(fbApp);
const auth  = getAuth(fbApp);
const ADMIN_EMAIL = "bonagirispinoj@gmail.com";

// ─── VEHICLE RATES ────────────────────────────────────────────────────────────
const VEHICLE_RATES = {
  "Bike": { base: 20, perKm: 14, icon: "🏍️", commission: 0.20 },
  "Auto": { base: 25, perKm: 22, icon: "🛺",  commission: 0.20 },
  "Car":  { base: 40, perKm: 30, icon: "🚗",  commission: 0.20 }
};

// ─── STATE ────────────────────────────────────────────────────────────────────
let state = {
  currentUser: null, role: null,
  userLocation: { lat: null, lng: null, address: "" },
  dropLocation: null,
  selectedVehicle: { name: "Bike", ...VEHICLE_RATES["Bike"] },
  selectedPayment: "cash",
  currentRideId: null,
  activeRideSub: null,
  driverRidesSub: null,
  driverDuty: false,
  uploadedDocs: {},
  pendingRating: null,
  userProfile: JSON.parse(localStorage.getItem("ridego_profile") || "{}"),
  currentRating: 0,
  estimatedFare: 0,
  estimatedKm: 0,
  pickupMode: "live",
  chatRideId: null,
  chatRole: null,  // 'user' or 'driver'
  chatSub: null,
  driverActiveRide: null
};

let locationWatcher = null;
let unsubAdmin = null;

// ─── GOOGLE MAPS INSTANCES ────────────────────────────────────────────────────
let bookingMap = null, bookingPickupMarker = null, bookingDropMarker = null, bookingDirectionsRenderer = null;
let activeMap = null, activeDriverMarker = null, activePickupMarker = null;
let driverMap = null, driverPickupMarker = null, driverDropMarker = null;
const directionsService = () => new google.maps.DirectionsService();

// Wait for Google Maps to load
window._gmapsReady = false;
window.initGoogleMaps = function() {
  window._gmapsReady = true;
};

function waitForGmaps(cb) {
  if (window._gmapsReady && typeof google !== 'undefined') { cb(); return; }
  setTimeout(() => waitForGmaps(cb), 200);
}

// ─── INIT BOOKING MAP ─────────────────────────────────────────────────────────
function initBookingMap(lat, lng) {
  waitForGmaps(() => {
    const center = { lat: lat || 17.9689, lng: lng || 79.5941 };
    if (!bookingMap) {
      bookingMap = new google.maps.Map(document.getElementById("gmap-booking"), {
        center, zoom: 14,
        styles: mapStyles(),
        disableDefaultUI: true,
        zoomControl: true,
        gestureHandling: 'cooperative'
      });
      bookingDirectionsRenderer = new google.maps.DirectionsRenderer({
        suppressMarkers: false,
        polylineOptions: { strokeColor: "#FF6B35", strokeWeight: 5 }
      });
      bookingDirectionsRenderer.setMap(bookingMap);
    } else {
      bookingMap.setCenter(center);
    }
    updateBookingPickupMarker(center.lat, center.lng);
  });
}

function updateBookingPickupMarker(lat, lng) {
  if (!bookingMap) return;
  if (bookingPickupMarker) bookingPickupMarker.setMap(null);
  bookingPickupMarker = new google.maps.Marker({
    position: { lat, lng }, map: bookingMap,
    title: "Pickup",
    icon: { url: "https://maps.google.com/mapfiles/ms/icons/red-dot.png" }
  });
}

// ─── CALC ROUTE & FARE (Google Maps Directions) ───────────────────────────────
function calcRouteAndFare() {
  if (!state.userLocation.lat || !state.dropLocation?.lat) return;

  const v = state.selectedVehicle;
  waitForGmaps(() => {
    const origin = new google.maps.LatLng(state.userLocation.lat, state.userLocation.lng);
    const destination = new google.maps.LatLng(state.dropLocation.lat, state.dropLocation.lng);

    directionsService().route({
      origin, destination,
      travelMode: google.maps.TravelMode.DRIVING
    }, (result, status) => {
      if (status === "OK" && result.routes.length > 0) {
        const leg = result.routes[0].legs[0];
        const km = leg.distance.value / 1000;
        const mins = Math.ceil(leg.duration.value / 60);

        if (bookingDirectionsRenderer) bookingDirectionsRenderer.setDirections(result);

        const fare = Math.round(v.base + km * v.perKm);
        state.estimatedFare = fare;
        state.estimatedKm = km;

        document.getElementById("fare-display").textContent = `₹${fare}`;
        document.getElementById("dist-display").textContent = `${leg.distance.text} · ${leg.duration.text} · Base ₹${v.base} + ₹${Math.round(km * v.perKm)}`;

        const bar = document.getElementById("route-info-bar");
        bar.style.display = "flex";
        document.getElementById("ri-km").textContent = leg.distance.text;
        document.getElementById("ri-time").textContent = leg.duration.text;
        document.getElementById("ri-fare").textContent = `₹${fare}`;
      } else {
        // Fallback haversine
        const km = haversineKm(state.userLocation.lat, state.userLocation.lng, state.dropLocation.lat, state.dropLocation.lng) * 1.25;
        const fare = Math.round(v.base + km * v.perKm);
        state.estimatedFare = fare;
        state.estimatedKm = km;
        document.getElementById("fare-display").textContent = `₹${fare}`;
        document.getElementById("dist-display").textContent = `~${km.toFixed(1)} km · ~${Math.ceil(km*3)} mins (approx)`;
        const bar = document.getElementById("route-info-bar");
        bar.style.display = "flex";
        document.getElementById("ri-km").textContent = `~${km.toFixed(1)} km`;
        document.getElementById("ri-time").textContent = `~${Math.ceil(km*3)} min`;
        document.getElementById("ri-fare").textContent = `₹${fare}`;
      }
    });
  });
}

function haversineKm(lat1, lng1, lat2, lng2) {
  const R = 6371;
  const dLat = (lat2-lat1)*Math.PI/180;
  const dLng = (lng2-lng1)*Math.PI/180;
  const a = Math.sin(dLat/2)**2 + Math.cos(lat1*Math.PI/180)*Math.cos(lat2*Math.PI/180)*Math.sin(dLng/2)**2;
  return R*2*Math.atan2(Math.sqrt(a),Math.sqrt(1-a));
}

// ─── ACTIVE RIDE MAP ──────────────────────────────────────────────────────────
function initActiveMap(lat, lng) {
  waitForGmaps(() => {
    if (!activeMap) {
      activeMap = new google.maps.Map(document.getElementById("gmap-active"), {
        center: { lat, lng }, zoom: 15,
        styles: mapStyles(),
        disableDefaultUI: true,
        zoomControl: false
      });
    } else {
      activeMap.setCenter({ lat, lng });
    }
  });
}

function updateActiveMapDriver(dLat, dLng, pLat, pLng) {
  if (!activeMap) { initActiveMap(dLat, dLng); }
  waitForGmaps(() => {
    if (activeDriverMarker) activeDriverMarker.setMap(null);
    activeDriverMarker = new google.maps.Marker({
      position: { lat: dLat, lng: dLng },
      map: activeMap,
      title: "Driver",
      icon: { url: "https://maps.google.com/mapfiles/ms/icons/yellow-dot.png" }
    });
    if (pLat && !activePickupMarker) {
      activePickupMarker = new google.maps.Marker({
        position: { lat: pLat, lng: pLng },
        map: activeMap,
        title: "Pickup",
        icon: { url: "https://maps.google.com/mapfiles/ms/icons/blue-dot.png" }
      });
    }
    activeMap.panTo({ lat: dLat, lng: dLng });
  });
}

// ─── DRIVER MAP ───────────────────────────────────────────────────────────────
function initDriverMap(lat, lng) {
  waitForGmaps(() => {
    if (!driverMap) {
      driverMap = new google.maps.Map(document.getElementById("gmap-driver"), {
        center: { lat, lng }, zoom: 14,
        styles: mapStyles(),
        disableDefaultUI: true,
        zoomControl: true
      });
    }
  });
}

function showDriverRouteOnMap(pLat, pLng, dLat, dLng) {
  waitForGmaps(() => {
    if (!driverMap) { initDriverMap(pLat, pLng); }
    const r = new google.maps.DirectionsRenderer({
      suppressMarkers: false,
      polylineOptions: { strokeColor: "#00C853", strokeWeight: 5 }
    });
    r.setMap(driverMap);
    directionsService().route({
      origin: new google.maps.LatLng(dLat, dLng),
      destination: new google.maps.LatLng(pLat, pLng),
      travelMode: google.maps.TravelMode.DRIVING
    }, (result, status) => {
      if (status === "OK") r.setDirections(result);
    });
  });
}

// ─── MAP STYLES ───────────────────────────────────────────────────────────────
function mapStyles() {
  return [
    { featureType: "poi", elementType: "labels", stylers: [{ visibility: "off" }] },
    { featureType: "transit", elementType: "labels", stylers: [{ visibility: "off" }] }
  ];
}

// ─── GPS ──────────────────────────────────────────────────────────────────────
function startGPS() {
  if (locationWatcher !== null) return;
  if (!navigator.geolocation) { toast("⚠️ GPS not supported"); return; }
  locationWatcher = navigator.geolocation.watchPosition(
    pos => {
      state.userLocation.lat = pos.coords.latitude;
      state.userLocation.lng = pos.coords.longitude;
      if (state.pickupMode === "live") {
        reverseGeocode(pos.coords.latitude, pos.coords.longitude);
        if (bookingMap) bookingMap.panTo({ lat: pos.coords.latitude, lng: pos.coords.longitude });
        updateBookingPickupMarker(pos.coords.latitude, pos.coords.longitude);
      }
      if (state.role === "driver" && state.driverDuty && state.currentUser) {
        updateDoc(doc(db, "users", state.currentUser.uid), {
          lat: pos.coords.latitude, lng: pos.coords.longitude
        }).catch(() => {});
      }
    },
    err => {
      const el = document.getElementById("user-location-sub");
      if (el) el.textContent = "Enable GPS for accurate location";
    },
    { enableHighAccuracy: true, maximumAge: 8000, timeout: 15000 }
  );
}
startGPS();

async function reverseGeocode(lat, lng) {
  try {
    const r = await fetch(`https://nominatim.openstreetmap.org/reverse?lat=${lat}&lon=${lng}&format=json`);
    const d = await r.json();
    state.userLocation.address = d.display_name || `${lat.toFixed(4)}, ${lng.toFixed(4)}`;
    const el = document.getElementById("pickup-input");
    if (el && state.pickupMode === "live") el.value = state.userLocation.address;
    const sub = document.getElementById("user-location-sub");
    if (sub) sub.textContent = "📡 " + (d.address?.city || d.address?.town || d.address?.village || "Location found");
  } catch (e) {}
}

// ─── TOAST ────────────────────────────────────────────────────────────────────
function toast(msg, duration=3000) {
  const t = document.getElementById("toast");
  t.textContent = msg;
  t.classList.add("show");
  clearTimeout(toast._t);
  toast._t = setTimeout(() => t.classList.remove("show"), duration);
}

// ─── SCREEN MANAGEMENT ────────────────────────────────────────────────────────
function showScreen(id) {
  document.querySelectorAll(".screen").forEach(s => s.classList.remove("active"));
  const el = document.getElementById(id);
  if (el) { el.classList.add("active"); window.scrollTo(0,0); }
  ["user-bottom-nav","driver-bottom-nav","admin-bottom-nav"].forEach(n => {
    const nav = document.getElementById(n);
    if (nav) nav.style.display = "none";
  });
  if (["screen-user-booking","screen-user-rides","screen-user-profile"].includes(id)) {
    document.getElementById("user-bottom-nav").style.display = "flex";
  } else if (id === "screen-driver") {
    document.getElementById("driver-bottom-nav").style.display = "flex";
  } else if (id === "screen-admin") {
    document.getElementById("admin-bottom-nav").style.display = "flex";
  }
}

function requestNotifPermission() {
  if (!("Notification" in window)) return;
  Notification.requestPermission().then(p => {
    const b = document.getElementById("notif-banner");
    if (b) b.style.display = "none";
    if (p === "granted") toast("🔔 Notifications enabled!");
  });
}

function sendLocalNotif(title, body) {
  if (Notification.permission === "granted") new Notification(title, { body, icon: "🚀" });
}

// ─── PLACE AUTOCOMPLETE (Nominatim) ──────────────────────────────────────────
async function fetchPlaceSuggestions(val) {
  try {
    const r = await fetch(`https://nominatim.openstreetmap.org/search?q=${encodeURIComponent(val)}&format=json&limit=5&countrycodes=in`);
    return await r.json();
  } catch(e) { return []; }
}

// ─── APP OBJECT ───────────────────────────────────────────────────────────────
window.App = {
  showScreen,
  requestNotifPermission,

  goUserBooking: function() {
    showScreen("screen-user-booking");
    setTimeout(() => {
      const lat = state.userLocation.lat || 17.9689;
      const lng = state.userLocation.lng || 79.5941;
      initBookingMap(lat, lng);
      if (state.userLocation.lat) updateBookingPickupMarker(state.userLocation.lat, state.userLocation.lng);
    }, 200);
    if (state.userProfile.name) document.getElementById("user-name-input").value = state.userProfile.name;
    if (state.userProfile.phone) document.getElementById("user-phone-input").value = state.userProfile.phone;
    if ("Notification" in window && Notification.permission === "default") {
      document.getElementById("notif-banner").style.display = "flex";
    }
  },

  showAdminLogin: function() { showScreen("screen-admin-login"); },

  setPickupMode: function(mode) {
    state.pickupMode = mode;
    document.getElementById("pm-live").classList.toggle("active", mode==="live");
    document.getElementById("pm-manual").classList.toggle("active", mode==="manual");
    const input = document.getElementById("pickup-input");
    if (mode === "live") {
      input.readOnly = false;
      if (state.userLocation.address) input.value = state.userLocation.address;
    } else {
      input.readOnly = false;
      input.value = "";
      input.placeholder = "Type your pickup address";
      input.focus();
    }
  },

  pickupInputChanged: async function() {
    if (state.pickupMode !== "manual") return;
    const val = document.getElementById("pickup-input").value.trim();
    const sugEl = document.getElementById("pickup-suggestions");
    if (val.length < 3) { sugEl.style.display = "none"; return; }
    const results = await fetchPlaceSuggestions(val);
    if (!results.length) { sugEl.style.display = "none"; return; }
    sugEl.style.display = "block";
    sugEl.innerHTML = results.map(p =>
      `<div class="suggestion-item" onclick="App.selectPickupSugg('${encodeURIComponent(p.display_name)}','${p.lat}','${p.lon}')">📍 ${p.display_name}</div>`
    ).join("");
  },

  selectPickupSugg: function(name, lat, lng) {
    const decoded = decodeURIComponent(name);
    document.getElementById("pickup-input").value = decoded;
    document.getElementById("pickup-suggestions").style.display = "none";
    state.userLocation.lat = parseFloat(lat);
    state.userLocation.lng = parseFloat(lng);
    state.userLocation.address = decoded;
    if (bookingMap) bookingMap.panTo({ lat: parseFloat(lat), lng: parseFloat(lng) });
    updateBookingPickupMarker(parseFloat(lat), parseFloat(lng));
    calcRouteAndFare();
  },

  recenterMap: function() {
    if (state.userLocation.lat) {
      reverseGeocode(state.userLocation.lat, state.userLocation.lng);
      if (bookingMap) bookingMap.panTo({ lat: state.userLocation.lat, lng: state.userLocation.lng });
      updateBookingPickupMarker(state.userLocation.lat, state.userLocation.lng);
    } else {
      toast("📍 Getting your GPS location...");
      startGPS();
    }
  },

  dropInputChanged: async function() {
    const val = document.getElementById("drop-input").value.trim();
    const sug = document.getElementById("drop-suggestions");
    if (val.length < 3) { sug.style.display = "none"; return; }
    const results = await fetchPlaceSuggestions(val);
    if (!results.length) { sug.style.display = "none"; return; }
    sug.style.display = "block";
    sug.innerHTML = results.map(p =>
      `<div class="suggestion-item" onclick="App.selectDropSugg('${encodeURIComponent(p.display_name)}','${p.lat}','${p.lon}')">📍 ${p.display_name}</div>`
    ).join("");
  },

  selectDropSugg: function(name, lat, lng) {
    const decoded = decodeURIComponent(name);
    document.getElementById("drop-input").value = decoded;
    document.getElementById("drop-suggestions").style.display = "none";
    state.dropLocation = { address: decoded, lat: parseFloat(lat), lng: parseFloat(lng) };
    if (bookingMap && bookingDropMarker) bookingDropMarker.setMap(null);
    waitForGmaps(() => {
      bookingDropMarker = new google.maps.Marker({
        position: { lat: parseFloat(lat), lng: parseFloat(lng) },
        map: bookingMap,
        title: "Drop",
        icon: { url: "https://maps.google.com/mapfiles/ms/icons/green-dot.png" }
      });
    });
    calcRouteAndFare();
  },

  searchDropOnMaps: function() {
    const drop = document.getElementById("drop-input").value;
    if (drop) window.open(`https://maps.google.com?q=${encodeURIComponent(drop)}`, "_blank");
  },

  selectVehicle: function(el, name) {
    document.querySelectorAll(".vehicle-option").forEach(v => v.classList.remove("selected"));
    el.classList.add("selected");
    state.selectedVehicle = { name, ...VEHICLE_RATES[name] };
    calcRouteAndFare();
  },

  selectPayment: function(method) {
    state.selectedPayment = method;
    document.querySelectorAll(".payment-opt").forEach(p => p.classList.remove("selected"));
    document.getElementById(`pay-${method}`).classList.add("selected");
  },

  // ─── BOOK RIDE ─────────────────────────────────────────────────────────────
  bookRide: async function() {
    const pickup = document.getElementById("pickup-input").value.trim();
    const drop   = document.getElementById("drop-input").value.trim();
    const name   = document.getElementById("user-name-input").value.trim();
    const phone  = document.getElementById("user-phone-input").value.trim();
    const note   = document.getElementById("user-note").value.trim();

    if (!pickup) return toast("📍 Enter your pickup location");
    if (!drop)   return toast("🏁 Please enter drop location");
    if (!name)   return toast("👤 Please enter your name");
    if (!phone || phone.length < 10) return toast("📞 Please enter a valid phone number");

    // Ensure fare calculated
    if (!state.estimatedFare && state.userLocation.lat && state.dropLocation?.lat) {
      calcRouteAndFare();
      await new Promise(r => setTimeout(r, 1500));
    }

    const v    = state.selectedVehicle;
    const fare = state.estimatedFare || (v.base + 5 * v.perKm);
    const km   = state.estimatedKm   || 5;

    const btn = document.getElementById("book-btn");
    btn.innerHTML = '<span class="spinner"></span> Booking...';
    btn.disabled  = true;

    try {
      const rideRef = await addDoc(collection(db, "rides"), {
        userName: name, userPhone: phone, userNote: note,
        vehicle: v.name, vehicleIcon: v.icon,
        pickup, drop,
        pickupLat: state.userLocation.lat || null,
        pickupLng: state.userLocation.lng || null,
        dropLat: state.dropLocation?.lat || null,
        dropLng: state.dropLocation?.lng || null,
        fare, estimatedKm: km,
        paymentMethod: state.selectedPayment,
        status: "pending",
        createdAt: new Date().toISOString(),
        driverEmail: null, driverUid: null,
        driverName: null, driverPhone: null,
        rating: null, feedback: null,
        messages: []
      });

      state.currentRideId = rideRef.id;
      state.userProfile = { name, phone };
      localStorage.setItem("ridego_profile", JSON.stringify(state.userProfile));

      showScreen("screen-searching");
      document.getElementById("search-booking-summary").innerHTML = `
        <div class="card">
          <p><b>${v.icon} ${v.name}</b></p>
          <p style="font-size:22px;font-weight:900;color:var(--primary)">₹${fare}</p>
          <p style="font-size:13px;color:var(--muted)">📍 ${pickup}</p>
          <p style="font-size:13px;color:var(--muted)">🏁 ${drop}</p>
          <p style="font-size:13px;color:var(--muted)">~${km.toFixed(1)} km · 💳 ${state.selectedPayment.toUpperCase()}</p>
        </div>`;
      App.watchRideStatus(rideRef.id, fare, { pickup, drop });
      toast("✅ Ride booked! Finding driver...");
    } catch (err) {
      toast("❌ Booking failed: " + (err.message || "Check Firestore rules"));
    }
    btn.innerHTML = "🚀 Book Now";
    btn.disabled  = false;
  },

  watchRideStatus: function(rideId, fare, rideInfo) {
    if (state.activeRideSub) state.activeRideSub();
    let attempt = 1; const MAX_TRIES = 3;
    let retryTimer = null;
    const scheduleRetry = () => {
      retryTimer = setTimeout(async () => {
        if (attempt >= MAX_TRIES) {
          toast("❌ No drivers available. Try again.");
          if (state.activeRideSub) state.activeRideSub();
          try { await updateDoc(doc(db,"rides",rideId),{status:"cancelled"}); } catch(_) {}
          showScreen("screen-user-booking"); return;
        }
        attempt++;
        const se = document.getElementById("search-status-text");
        if (se) se.textContent = `🔄 Retry ${attempt}/${MAX_TRIES}...`;
        scheduleRetry();
      }, 60000);
    };
    scheduleRetry();

    state.activeRideSub = onSnapshot(doc(db,"rides",rideId), snap => {
      if (!snap.exists()) return;
      const data = snap.data();

      if (data.driverLat && data.driverLng &&
          document.getElementById("screen-ride-active")?.classList.contains("active")) {
        updateActiveMapDriver(data.driverLat, data.driverLng, data.pickupLat, data.pickupLng);
      }

      if (data.status === "accepted" || data.status === "arriving") {
        clearTimeout(retryTimer);
        sendLocalNotif("🏍️ Driver Found!", `${data.driverName} is on the way`);
        showScreen("screen-ride-active");
        setTimeout(() => initActiveMap(data.driverLat || data.pickupLat || 17.9689, data.driverLng || data.pickupLng || 79.5941), 200);
        App.renderActiveRide(rideId, data, rideInfo);
      } else if (data.status === "completed") {
        clearTimeout(retryTimer);
        sendLocalNotif("✅ Ride Complete", `₹${data.fare}`);
        showScreen("screen-ride-done");
        state.pendingRating = { rideId, driverUid: data.driverUid, driverName: data.driverName };
        document.getElementById("ride-done-summary").innerHTML = `
          <div class="fare-box">
            <div class="fare-label">Total Fare</div>
            <div class="fare-amount">₹${data.fare}</div>
            <div style="font-size:13px;color:var(--muted);margin-top:4px">${(data.paymentMethod||"CASH").toUpperCase()}</div>
          </div>
          <p style="font-size:13px;margin-top:10px">🚗 Driver: <b>${data.driverName||"—"}</b></p>
          <p style="font-size:13px">📞 ${data.driverPhone||""}</p>
          <p style="font-size:13px">📍 ${data.pickup} → ${data.drop}</p>
          <p style="font-size:13px">📏 ~${data.estimatedKm?.toFixed(1)||"—"} km</p>
        `;
      } else if (data.status === "cancelled") {
        clearTimeout(retryTimer);
        toast("❌ Ride was cancelled.");
        showScreen("screen-user-booking");
      }
    });
  },

  cancelSearch: async function() {
    if (state.currentRideId) await updateDoc(doc(db,"rides",state.currentRideId), { status:"cancelled" });
    if (state.activeRideSub) state.activeRideSub();
    showScreen("screen-user-booking");
  },

  renderActiveRide: function(rideId, data, rideInfo) {
    if (data.driverLat) updateActiveMapDriver(data.driverLat, data.driverLng, data.pickupLat, data.pickupLng);

    document.getElementById("driver-info-section").innerHTML = `
      <div class="driver-info-card">
        <div style="display:flex;align-items:center;gap:14px">
          <div class="driver-avatar">${data.vehicleIcon||"🚗"}</div>
          <div style="flex:1">
            <div class="d-name">${data.driverName||"Your Driver"}</div>
            <div class="d-detail">${data.vehicle} · ${data.driverVehicleNum||""}</div>
            <div class="d-rating">★ ${data.driverRating||"New"} · <span class="live-dot"></span>Live</div>
            <div style="margin-top:6px">
              <span style="font-size:13px;background:#e8f5e9;color:var(--green-dark);border-radius:8px;padding:3px 8px;font-weight:700">📞 ${data.driverPhone||"—"}</span>
            </div>
          </div>
          <div style="text-align:right">
            <div style="font-size:22px;font-weight:900;color:var(--primary)">₹${data.fare}</div>
            <div style="font-size:11px;opacity:0.6">${(data.paymentMethod||"cash").toUpperCase()}</div>
          </div>
        </div>
        <div class="contact-row">
          <button class="contact-btn" onclick="window.open('tel:${data.driverPhone}')">📞 Call</button>
          <button class="contact-btn" onclick="App.openChat('${rideId}','user','${data.driverName||"Driver"}','${data.driverPhone}')">💬 Chat</button>
          <button class="contact-btn" onclick="window.open('https://wa.me/91${data.driverPhone}?text=Hi, I booked a ride from RideGo. Ride ID: ${rideId}')">📱 WhatsApp</button>
          <button class="contact-btn" onclick="App.shareRide('${rideId}')">🔗 Share</button>
        </div>
      </div>`;

    document.getElementById("tracking-steps").innerHTML = `
      <div class="tracking-step">
        <div class="step-dot done">✓</div>
        <div class="step-info"><h4>Ride Booked</h4><p>Your booking was confirmed</p></div>
      </div>
      <div class="tracking-step">
        <div class="step-dot done">✓</div>
        <div class="step-info"><h4>Driver Assigned</h4><p>${data.driverName||"Driver"} accepted your ride</p></div>
      </div>
      <div class="tracking-step">
        <div class="step-dot active">🏍</div>
        <div class="step-info"><h4>On the Way</h4><p>Driver heading to pickup</p></div>
      </div>
      <div class="tracking-step">
        <div class="step-dot pending">🏁</div>
        <div class="step-info"><h4>Ride in Progress</h4><p>On the way to destination</p></div>
      </div>`;

    document.getElementById("active-ride-actions").innerHTML = `
      <div style="display:flex;gap:8px">
        <button class="btn btn-gray" style="flex:1" onclick="App.cancelActiveRide('${rideId}')">Cancel Ride</button>
        <button class="btn btn-red" style="flex:1;width:auto" onclick="App.triggerSOS()">🆘 SOS</button>
      </div>
      <p style="font-size:12px;color:var(--muted);text-align:center;margin-top:8px">Location updates every 10 seconds</p>`;
  },

  cancelActiveRide: async function(rideId) {
    if (!confirm("Cancel this ride?")) return;
    await updateDoc(doc(db,"rides",rideId), { status:"cancelled" });
    if (state.activeRideSub) state.activeRideSub();
    showScreen("screen-user-booking");
    toast("Ride cancelled.");
  },

  triggerSOS: function() {
    const emer = JSON.parse(localStorage.getItem("ridego_emergency")||"{}");
    if (emer.phone) window.open(`tel:${emer.phone}`);
    else window.open("tel:112");
    toast("🆘 SOS activated! Calling emergency contact...", 5000);
  },

  shareRide: function(rideId) {
    const text = `Track my RideGo ride: ID ${rideId}`;
    if (navigator.share) navigator.share({ title:"RideGo Ride", text });
    else { navigator.clipboard?.writeText(text); toast("📋 Ride info copied!"); }
  },

  setRating: function(n) {
    state.currentRating = n;
    document.querySelectorAll("#star-rating span").forEach((s, i) => {
      s.textContent = i < n ? "★" : "☆";
      s.style.color  = i < n ? "#FFD600" : "#ccc";
    });
  },

  submitRating: async function() {
    if (!state.pendingRating || !state.currentRating) return toast("Please select a rating");
    try {
      await updateDoc(doc(db,"rides",state.pendingRating.rideId), {
        rating: state.currentRating,
        feedback: document.getElementById("ride-feedback").value
      });
      if (state.pendingRating.driverUid && state.pendingRating.driverUid !== "admin") {
        const dRef = doc(db,"users",state.pendingRating.driverUid);
        await runTransaction(db, async t => {
          const d = await t.get(dRef);
          if (!d.exists()) return;
          const sum = (d.data().sumRatings||0) + state.currentRating;
          const cnt = (d.data().totalRatings||0) + 1;
          t.update(dRef, { sumRatings: sum, totalRatings: cnt, avgRating: (sum/cnt).toFixed(1) });
        });
      }
      toast("⭐ Thanks for rating!");
      state.pendingRating = null;
      App.goUserBooking();
    } catch(e) { toast("❌ Failed to submit: " + e.message); }
  },

  showUserRides: async function() {
    showScreen("screen-user-rides");
    const phone = state.userProfile?.phone || document.getElementById("user-phone-input")?.value;
    if (!phone) { document.getElementById("user-rides-list").innerHTML = "<div class='card'>Save your phone number to see ride history</div>"; return; }
    try {
      const q = query(collection(db,"rides"), where("userPhone","==",phone), orderBy("createdAt","desc"), limit(20));
      const snap = await getDocs(q);
      if (snap.empty) { document.getElementById("user-rides-list").innerHTML = "<div class='card' style='text-align:center;padding:30px'><div style='font-size:48px'>🚗</div><h3 style='margin:12px 0'>No Rides Yet</h3><p style='color:var(--muted)'>Book your first ride!</p></div>"; return; }
      let html = "";
      snap.forEach(d => {
        const r = d.data();
        html += `<div class="ride-card ${r.status}">
          <div style="display:flex;justify-content:space-between;align-items:center">
            <h4>${r.vehicleIcon||"🚗"} ${r.vehicle}</h4>
            <span class="badge badge-${r.status}">${r.status}</span>
          </div>
          <div class="route-line">
            <div class="pickup">📍 ${r.pickup}</div>
            <div class="drop">🏁 ${r.drop}</div>
          </div>
          <div style="display:flex;justify-content:space-between;align-items:center">
            <span class="fare">₹${r.fare}</span>
            <span style="font-size:12px;color:var(--muted)">${new Date(r.createdAt).toLocaleDateString("en-IN",{day:"2-digit",month:"short",year:"numeric"})}</span>
          </div>
          ${r.driverName ? `<p style="font-size:12px;margin-top:6px">🚗 ${r.driverName} · ★ ${r.rating||"Not rated"}</p>` : ""}
        </div>`;
      });
      document.getElementById("user-rides-list").innerHTML = html;
    } catch(e) { document.getElementById("user-rides-list").innerHTML = "<div class='card'>Could not load rides. Check connection.</div>"; }
  },

  showUserProfile: function() {
    showScreen("screen-user-profile");
    if (state.userProfile.name) {
      document.getElementById("profile-name").textContent = state.userProfile.name;
      document.getElementById("profile-name-input").value = state.userProfile.name;
    }
    if (state.userProfile.phone) document.getElementById("profile-phone-input").value = state.userProfile.phone;
    const emer = JSON.parse(localStorage.getItem("ridego_emergency")||"{}");
    if (emer.name)  document.getElementById("emer-name").value  = emer.name;
    if (emer.phone) document.getElementById("emer-phone").value = emer.phone;
  },

  saveUserProfile: function() {
    const name  = document.getElementById("profile-name-input").value.trim();
    const phone = document.getElementById("profile-phone-input").value.trim();
    if (!name||!phone) return toast("Enter name and phone");
    state.userProfile = { name, phone };
    localStorage.setItem("ridego_profile", JSON.stringify(state.userProfile));
    document.getElementById("profile-name").textContent = name;
    toast("✅ Profile saved!");
  },

  saveEmergencyContact: function() {
    const name  = document.getElementById("emer-name").value.trim();
    const phone = document.getElementById("emer-phone").value.trim();
    if (!name||!phone) return toast("Enter emergency contact details");
    localStorage.setItem("ridego_emergency", JSON.stringify({ name, phone }));
    toast("✅ Emergency contact saved!");
  },

  // ─── DRIVER LOGIN ──────────────────────────────────────────────────────────
  driverLogin: async function() {
    const email = document.getElementById("d-login-email").value.trim();
    const pass  = document.getElementById("d-login-pass").value;
    if (!email||!pass) return toast("Enter email and password");
    const btn = document.querySelector("#screen-driver-login .btn-primary");
    btn.innerHTML = '<span class="spinner"></span>'; btn.disabled = true;
    try {
      const cred = await signInWithEmailAndPassword(auth, email, pass);
      const snap = await getDoc(doc(db,"users",cred.user.uid));
      if (!snap.exists()) { await signOut(auth); btn.innerHTML="Login"; btn.disabled=false; return toast("❌ Account not found"); }
      const ud = snap.data();
      if (ud.role !== "driver") { await signOut(auth); btn.innerHTML="Login"; btn.disabled=false; return toast("❌ Not a driver account"); }
      state.currentUser = { ...ud, uid: cred.user.uid };
      state.role = "driver";
      App.loadDriverDashboard();
    } catch(err) { toast("❌ " + err.message.replace("Firebase:","").trim()); }
    btn.innerHTML = "Login"; btn.disabled = false;
  },

  // ─── DRIVER SIGNUP (No login required — open registration) ────────────────
  driverSignup: async function() {
    const name    = document.getElementById("s-name").value.trim();
    const email   = document.getElementById("s-email").value.trim();
    const phone   = document.getElementById("s-phone").value.trim();
    const pass    = document.getElementById("s-pass").value;
    const vehicle = document.getElementById("s-vehicle").value;
    const vnum    = document.getElementById("s-vnum").value.trim();
    const upi     = document.getElementById("s-upi").value.trim();
    const bankAcc = document.getElementById("s-bank-acc").value.trim();
    const ifsc    = document.getElementById("s-ifsc").value.trim();

    if (!name||!email||!phone||!pass) return toast("Fill all personal details");
    if (pass.length < 8) return toast("Password must be at least 8 characters");
    if (!vehicle)        return toast("Select vehicle type");
    if (!vnum)           return toast("Enter vehicle number");

    const btn = document.getElementById("signup-btn");
    btn.innerHTML = '<span class="spinner"></span> Creating account...';
    btn.disabled  = true;

    try {
      const cred = await createUserWithEmailAndPassword(auth, email, pass);
      const trialUntil = new Date(Date.now() + 7*24*60*60*1000).toISOString(); // 7 day trial

      const docStatus = {
        licence:   state.uploadedDocs.licence   ? "pending_review" : "missing",
        rc:        state.uploadedDocs.rc        ? "pending_review" : "missing",
        aadhaar:   state.uploadedDocs.aadhaar   ? "pending_review" : "missing",
        bank:      state.uploadedDocs.bank      ? "pending_review" : "missing",
        insurance: state.uploadedDocs.insurance ? "pending_review" : "missing",
        selfie:    state.uploadedDocs.selfie    ? "pending_review" : "missing"
      };

      await setDoc(doc(db,"users",cred.user.uid), {
        uid: cred.user.uid, name, email, phone, role: "driver",
        vehicle, vehicleNum: vnum,
        upiId: upi||"", bankAccount: bankAcc||"", ifsc: ifsc||"",
        documents: state.uploadedDocs||{},
        docStatus,
        approvalStatus: "trial",
        trialUntil,
        kycDeadline: trialUntil,
        isOnDuty: false, earnings: 0, withdrawn: 0,
        totalRides: 0, totalRatings: 0, sumRatings: 0, avgRating: "New",
        joinedAt: new Date().toISOString()
      });

      state.currentUser = {
        uid: cred.user.uid, name, email, phone, role: "driver",
        vehicle, vehicleNum: vnum, upiId: upi||"",
        approvalStatus: "trial", trialUntil, isOnDuty: false,
        avgRating: "New", totalRides: 0, docStatus
      };
      state.role = "driver";

      toast("🎉 Account created! Start driving. Upload all docs within 7 days.");
      App.loadDriverDashboard();
    } catch(err) {
      toast("❌ " + err.message.replace("Firebase:","").trim());
    }
    btn.innerHTML = "🚀 Register & Start Driving Now →";
    btn.disabled  = false;
  },

  handleFileUpload: function(docType, input) {
    if (!input.files[0]) return;
    const file = input.files[0];
    if (file.size > 5*1024*1024) { toast("File too large. Max 5MB."); return; }
    const reader = new FileReader();
    reader.onload = e => {
      state.uploadedDocs[docType] = { name: file.name, type: file.type, uploadedAt: new Date().toISOString() };
      const statusEl  = document.getElementById(`status-${docType}`);
      const sectionEl = document.getElementById(`kyc-${docType}`);
      if (statusEl)  statusEl.textContent = `✅ ${file.name} — Uploaded`;
      if (sectionEl) sectionEl.classList.add("uploaded");
    };
    reader.readAsDataURL(file);
  },

  // Upload docs for existing driver
  uploadDocForDriver: async function(docType, input) {
    if (!input.files[0] || !state.currentUser) return;
    const file = input.files[0];
    if (file.size > 5*1024*1024) { toast("File too large. Max 5MB."); return; }
    const reader = new FileReader();
    reader.onload = async e => {
      const docData = { name: file.name, type: file.type, uploadedAt: new Date().toISOString() };
      const updateObj = {};
      updateObj[`documents.${docType}`] = docData;
      updateObj[`docStatus.${docType}`] = "pending_review";
      await updateDoc(doc(db,"users",state.currentUser.uid), updateObj);
      toast(`✅ ${docType} uploaded! Admin will verify within 24 hours.`);
      App.driverNav("kyc");
    };
    reader.readAsDataURL(file);
  },

  // ─── DRIVER DASHBOARD ──────────────────────────────────────────────────────
  loadDriverDashboard: function() {
    showScreen("screen-driver");
    const driver = state.currentUser;
    document.getElementById("driver-header-name").textContent = driver.name || driver.email;
    document.getElementById("driver-header-status").textContent = `${driver.vehicle} · ${driver.vehicleNum||""}`;

    const now = new Date();
    const isTrial = driver.approvalStatus === "trial";
    const isApproved = driver.approvalStatus === "approved";
    const isRejected = driver.approvalStatus === "rejected";
    const trialUntil = driver.trialUntil ? new Date(driver.trialUntil) : null;
    const trialExpired = isTrial && trialUntil && now > trialUntil;
    const daysLeft = trialUntil ? Math.max(0, Math.ceil((trialUntil-now)/(24*3600*1000))) : 0;
    const canDrive = isApproved || (isTrial && !trialExpired);

    const bannerEl = document.getElementById("driver-approval-banner");
    bannerEl.style.display = "block";

    if (isTrial && !trialExpired) {
      bannerEl.innerHTML = `
        <div class="trial-banner" style="margin:10px">
          <h4>⚡ 7-Day Trial Active — ${daysLeft} day${daysLeft!==1?"s":""} remaining</h4>
          <p>You're on trial. Upload <b>Licence + RC + Aadhaar + Bank details</b> within ${daysLeft} days to avoid account suspension.</p>
          <button class="btn btn-blue btn-sm" style="margin-top:8px" onclick="App.driverNav('kyc')">📄 Upload Documents Now</button>
        </div>`;
    } else if (trialExpired) {
      bannerEl.innerHTML = `
        <div class="approval-banner">
          <h4>⏱ Trial Period Expired</h4>
          <p>Your 7-day trial ended. Upload all required documents. Admin will approve within 24 hours.</p>
          <button class="btn btn-outline btn-sm" style="margin-top:8px" onclick="App.driverNav('kyc')">📄 Upload Documents Now</button>
        </div>`;
    } else if (isRejected) {
      bannerEl.innerHTML = `
        <div class="approval-banner">
          <h4>❌ Documents Rejected</h4>
          <p>Some documents were rejected. Please re-upload clear images. Contact admin for details.</p>
          <button class="btn btn-outline btn-sm" style="margin-top:8px" onclick="App.driverNav('kyc')">Re-upload Documents</button>
        </div>`;
    } else if (isApproved) {
      bannerEl.innerHTML = `
        <div class="free-badge" style="margin:10px">
          <div class="fb-icon">✅</div>
          <div><h4>Account Verified</h4><p>All documents approved. You can accept rides.</p></div>
        </div>`;
    }

    const dutySection = document.getElementById("duty-toggle-section");
    dutySection.style.display = canDrive ? "flex" : "none";
    if (!canDrive && !isApproved) {
      document.getElementById("driver-main-content").innerHTML = `
        <div class="card" style="text-align:center;padding:30px">
          <div style="font-size:48px">🚫</div>
          <h3 style="margin:12px 0">Account Suspended</h3>
          <p style="color:var(--muted)">Please upload required documents to continue driving.</p>
          <button class="btn btn-primary" style="margin-top:12px" onclick="App.driverNav('kyc')">Upload Documents</button>
        </div>`;
    } else {
      App.driverNav("rides");
    }
  },

  driverNav: async function(tab) {
    ["rides","earnings","kyc","withdraw","profile"].forEach(t => {
      const el = document.getElementById(`dnav-${t}`);
      if (el) el.classList.remove("active");
    });
    const active = document.getElementById(`dnav-${tab}`);
    if (active) active.classList.add("active");

    const content = document.getElementById("driver-main-content");

    if (tab === "rides") {
      App.driverLoadRides(content);
    } else if (tab === "earnings") {
      App.driverLoadEarnings(content);
    } else if (tab === "kyc") {
      App.driverLoadKYC(content);
    } else if (tab === "withdraw") {
      App.driverLoadWithdraw(content);
    } else if (tab === "profile") {
      App.driverLoadProfile(content);
    }
  },

  driverLoadRides: function(content) {
    if (!state.currentUser) return;
    const isOnDuty = state.driverDuty;
    if (!isOnDuty) {
      content.innerHTML = `
        <div class="card" style="text-align:center;padding:30px">
          <div style="font-size:48px">😴</div>
          <h3 style="margin:12px 0">You're Offline</h3>
          <p style="color:var(--muted)">Toggle the switch above to go online and start accepting rides</p>
        </div>
        <div class="section-label">Recent Rides</div>
        <div id="driver-ride-history"></div>`;
      App.loadDriverRideHistory();
      return;
    }

    content.innerHTML = `
      <div class="card" style="text-align:center;background:linear-gradient(135deg,#e8f5e9,#c8e6c9);margin:10px">
        <div style="font-size:32px">🟢</div>
        <h3 style="color:var(--green-dark)">You're Online!</h3>
        <p style="font-size:13px;color:var(--green-dark)">Searching for ride requests near you...</p>
        <div style="margin-top:8px"><div class="search-ring" style="width:40px;height:40px;border-width:3px;border-top-color:var(--green)"></div></div>
      </div>
      <div id="driver-available-rides"></div>
      <div class="section-label">Recent Rides</div>
      <div id="driver-ride-history"></div>`;
    App.listenForAvailableRides();
    App.loadDriverRideHistory();
  },

  listenForAvailableRides: function() {
    if (state.driverRidesSub) state.driverRidesSub();
    const vehicle = state.currentUser.vehicle;
    const q = query(collection(db,"rides"), where("status","==","pending"), where("vehicle","==",vehicle));
    state.driverRidesSub = onSnapshot(q, snap => {
      const container = document.getElementById("driver-available-rides");
      if (!container) return;
      const rides = snap.docs.map(d => ({ id: d.id, ...d.data() }));
      if (!rides.length) {
        container.innerHTML = `<div style="text-align:center;padding:20px;color:var(--muted);font-size:14px">🔍 No pending rides for ${vehicle} right now. Waiting...</div>`;
        return;
      }
      let html = `<div class="section-label">New Ride Requests (${rides.length})</div>`;
      rides.forEach(ride => {
        const minutesAgo = Math.floor((Date.now()-new Date(ride.createdAt))/60000);
        html += `<div class="incoming-alert" id="alert-${ride.id}">
          <div style="display:flex;justify-content:space-between;align-items:center">
            <h3>${ride.vehicleIcon} New Ride! ${minutesAgo>0?`(${minutesAgo}m ago)`:""}</h3>
            <span style="font-size:22px;font-weight:900">₹${ride.fare}</span>
          </div>
          <div class="route-line" style="background:rgba(255,255,255,0.2);margin:10px 0">
            <div class="pickup" style="color:#fff">📍 ${ride.pickup}</div>
            <div class="drop" style="color:#fff">🏁 ${ride.drop}</div>
          </div>
          <p style="font-size:13px;opacity:0.9">👤 ${ride.userName} · 📞 ${ride.userPhone} · 📏 ~${ride.estimatedKm?.toFixed(1)||"?"}km</p>
          <div class="timer-bar"><div class="timer-fill" id="timer-${ride.id}" style="width:100%"></div></div>
          <div style="display:flex;gap:8px;margin-top:10px">
            <button class="btn btn-sm" style="flex:1;background:#fff;color:var(--green-dark);font-weight:900" onclick="App.acceptRide('${ride.id}')">✅ Accept</button>
            <button class="btn btn-sm" style="flex:1;background:rgba(255,255,255,0.2);color:#fff" onclick="App.declineRide('${ride.id}')">❌ Decline</button>
            <button class="btn btn-sm" style="background:rgba(255,255,255,0.2);color:#fff" onclick="window.open('tel:${ride.userPhone}')">📞</button>
          </div>
        </div>`;
      });
      container.innerHTML = html;
      rides.forEach(ride => App.startRideTimer(ride.id));
    });
  },

  startRideTimer: function(rideId) {
    let w = 100;
    const t = setInterval(() => {
      w -= 2;
      const el = document.getElementById(`timer-${rideId}`);
      if (el) el.style.width = w + "%";
      if (w <= 0) clearInterval(t);
    }, 600); // 30s countdown
  },

  acceptRide: async function(rideId) {
    if (!state.currentUser) return toast("❌ Not logged in");
    const driver = state.currentUser;
    try {
      await runTransaction(db, async t => {
        const rideRef = doc(db,"rides",rideId);
        const ride = await t.get(rideRef);
        if (!ride.exists()||ride.data().status!=="pending") throw new Error("Ride no longer available");
        t.update(rideRef, {
          status: "accepted",
          driverUid: driver.uid,
          driverEmail: driver.email,
          driverName: driver.name,
          driverPhone: driver.phone,
          driverVehicleNum: driver.vehicleNum||"",
          driverRating: driver.avgRating||"New",
          driverLat: state.userLocation.lat||null,
          driverLng: state.userLocation.lng||null,
          acceptedAt: new Date().toISOString()
        });
      });
      state.driverActiveRide = rideId;
      if (state.driverRidesSub) state.driverRidesSub();
      toast("✅ Ride accepted! Head to pickup location.");
      sendLocalNotif("✅ Ride Accepted!", "You accepted a ride. Head to pickup.");
      App.showDriverActiveRide(rideId);
    } catch(err) {
      toast("❌ " + err.message);
    }
  },

  declineRide: function(rideId) {
    const el = document.getElementById(`alert-${rideId}`);
    if (el) el.style.display = "none";
    toast("Ride declined.");
  },

  showDriverActiveRide: function(rideId) {
    showScreen("screen-driver-ride");
    setTimeout(() => initDriverMap(state.userLocation.lat||17.9689, state.userLocation.lng||79.5941), 200);

    onSnapshot(doc(db,"rides",rideId), snap => {
      if (!snap.exists()) return;
      const ride = snap.data();
      if (ride.pickupLat) showDriverRouteOnMap(ride.pickupLat, ride.pickupLng, state.userLocation.lat||17.9689, state.userLocation.lng||79.5941);

      document.getElementById("dride-status-title").textContent =
        ride.status==="accepted" ? "Heading to Pickup" :
        ride.status==="pickedup" ? "Ride in Progress" : "Ride Done";

      document.getElementById("driver-ride-detail").innerHTML = `
        <div class="driver-info-card" style="margin:10px">
          <div style="display:flex;align-items:center;gap:12px">
            <div class="driver-avatar">👤</div>
            <div style="flex:1">
              <div class="d-name">${ride.userName}</div>
              <div class="d-detail">📞 ${ride.userPhone}</div>
              <div style="margin-top:4px">
                <span style="font-size:13px;background:#e8f5e9;color:var(--green-dark);border-radius:8px;padding:3px 8px;font-weight:700">📞 ${ride.userPhone}</span>
              </div>
            </div>
            <div style="text-align:right">
              <div style="font-size:22px;font-weight:900;color:var(--primary)">₹${ride.fare}</div>
              <div style="font-size:11px;opacity:0.7">${(ride.paymentMethod||"cash").toUpperCase()}</div>
            </div>
          </div>
          <div class="route-line" style="margin-top:12px">
            <div class="pickup">📍 ${ride.pickup}</div>
            <div class="drop">🏁 ${ride.drop}</div>
          </div>
          <div class="contact-row">
            <button class="contact-btn" onclick="window.open('tel:${ride.userPhone}')">📞 Call User</button>
            <button class="contact-btn" onclick="App.openChat('${rideId}','driver','${ride.userName}','${ride.userPhone}')">💬 Chat</button>
            <button class="contact-btn" onclick="window.open('https://wa.me/91${ride.userPhone}?text=Hi, I am your RideGo driver ${ride.driverName}. On my way!')">📱 WhatsApp</button>
          </div>
        </div>`;

      const actions = document.getElementById("driver-ride-actions");
      if (ride.status === "accepted") {
        actions.innerHTML = `
          <p style="font-size:14px;color:var(--muted);margin-bottom:10px">Have you reached the pickup location?</p>
          <button class="btn btn-green" onclick="App.driverMarkArrived('${rideId}')">📍 I've Arrived at Pickup</button>
          <button class="btn btn-gray" onclick="App.cancelDriverRide('${rideId}')">Cancel Ride</button>`;
      } else if (ride.status === "pickedup" || ride.status === "arriving") {
        actions.innerHTML = `
          <p style="font-size:14px;color:var(--muted);margin-bottom:10px">Ride in progress. Mark complete when you reach destination.</p>
          <button class="btn btn-primary" onclick="App.driverCompleteRide('${rideId}','${ride.fare}','${ride.driverUid}')">✅ Complete Ride — ₹${ride.fare}</button>
          <button class="btn btn-red" onclick="App.triggerSOS()">🆘 SOS Emergency</button>`;
      } else if (ride.status === "completed") {
        actions.innerHTML = `
          <div class="free-badge">
            <div class="fb-icon">💰</div>
            <div><h4>Ride Completed!</h4><p>₹${ride.fare} earned. Great job!</p></div>
          </div>
          <button class="btn btn-primary" onclick="App.loadDriverDashboard()">← Back to Dashboard</button>`;
      }
    });
  },

  driverMarkArrived: async function(rideId) {
    await updateDoc(doc(db,"rides",rideId), { status:"arriving", arrivedAt: new Date().toISOString() });
    toast("✅ Marked as arrived! User notified.");
  },

  driverCompleteRide: async function(rideId, fare, driverUid) {
    if (!confirm(`Complete this ride and collect ₹${fare}?`)) return;
    await updateDoc(doc(db,"rides",rideId), { status:"completed", completedAt: new Date().toISOString() });
    // Update earnings
    if (driverUid && driverUid !== "admin") {
      const earnings = Math.round(parseInt(fare) * 0.80); // 80% to driver
      await updateDoc(doc(db,"users",driverUid), {
        earnings: increment(earnings), totalRides: increment(1)
      });
    }
    toast("🎉 Ride completed! ₹" + Math.round(parseInt(fare)*0.80) + " added to earnings.");
    if (state.driverRidesSub) state.driverRidesSub();
    App.loadDriverDashboard();
  },

  cancelDriverRide: async function(rideId) {
    if (!confirm("Cancel this ride?")) return;
    await updateDoc(doc(db,"rides",rideId), { status:"cancelled", cancelledBy:"driver" });
    toast("Ride cancelled.");
    App.loadDriverDashboard();
  },

  loadDriverRideHistory: async function() {
    if (!state.currentUser) return;
    try {
      const q = query(collection(db,"rides"), where("driverUid","==",state.currentUser.uid), orderBy("createdAt","desc"), limit(10));
      const snap = await getDocs(q);
      const el = document.getElementById("driver-ride-history");
      if (!el) return;
      if (snap.empty) { el.innerHTML = `<div style="text-align:center;padding:20px;color:var(--muted)">No ride history yet</div>`; return; }
      let html = "";
      snap.forEach(d => {
        const r = d.data();
        html += `<div class="ride-card ${r.status}">
          <div style="display:flex;justify-content:space-between">
            <h4>${r.vehicleIcon||"🚗"} ${r.userName}</h4>
            <span class="fare">₹${Math.round(r.fare*0.80)}</span>
          </div>
          <div class="route-line"><div class="pickup">📍 ${r.pickup}</div><div class="drop">🏁 ${r.drop}</div></div>
          <p style="font-size:12px;color:var(--muted)">${new Date(r.createdAt).toLocaleString("en-IN")}</p>
        </div>`;
      });
      el.innerHTML = html;
    } catch(e) {}
  },

  driverLoadEarnings: async function(content) {
    if (!state.currentUser) return;
    const snap = await getDoc(doc(db,"users",state.currentUser.uid));
    const d = snap.data()||{};
    const total = d.earnings||0;
    const withdrawn = d.withdrawn||0;
    const balance = total - withdrawn;
    const today = new Date().toISOString().slice(0,10);

    content.innerHTML = `
      <div class="earnings-grid">
        <div class="earnings-item"><div class="amount">₹${total}</div><div class="label">Total Earned</div></div>
        <div class="earnings-item"><div class="amount">₹${balance}</div><div class="label">Available Balance</div></div>
        <div class="earnings-item"><div class="amount">₹${withdrawn}</div><div class="label">Withdrawn</div></div>
        <div class="earnings-item"><div class="amount">${d.totalRides||0}</div><div class="label">Total Rides</div></div>
      </div>
      <div class="card">
        <div class="card-title">💡 Earnings Info</div>
        <p style="font-size:13px;color:var(--muted)">You earn <b>80%</b> of each fare. 20% is RideGo's platform fee. Withdrawals processed within 24 hours.</p>
        <div style="background:#e8f5e9;border-radius:10px;padding:12px;margin-top:10px">
          <p style="font-size:14px;font-weight:700;color:var(--green-dark)">⭐ Rating: ${d.avgRating||"New"} (${d.totalRatings||0} ratings)</p>
        </div>
      </div>`;
  },

  driverLoadKYC: async function(content) {
    if (!state.currentUser) return;
    const snap = await getDoc(doc(db,"users",state.currentUser.uid));
    const d = snap.data()||{};
    const docStatus = d.docStatus||{};
    const trialUntil = d.trialUntil ? new Date(d.trialUntil) : null;
    const daysLeft = trialUntil ? Math.max(0, Math.ceil((trialUntil-new Date())/(24*3600*1000))) : 0;
    const isUrgent = daysLeft <= 2;

    const docs = [
      { key:"licence",   label:"Driving Licence",     icon:"🪪", required:true  },
      { key:"rc",        label:"Vehicle RC",           icon:"📋", required:true  },
      { key:"aadhaar",   label:"Aadhaar Card",         icon:"🪪", required:true  },
      { key:"bank",      label:"Bank/Passbook Cheque", icon:"🏦", required:true  },
      { key:"insurance", label:"Vehicle Insurance",    icon:"🛡️", required:false },
      { key:"selfie",    label:"Profile Selfie",       icon:"🤳", required:false }
    ];

    const statusIcon = { "missing":"📤 Upload", "pending_review":"⏳ Pending Review", "verified":"✅ Verified", "rejected":"❌ Rejected" };
    const statusColor = { "missing":"var(--muted)", "pending_review":"var(--blue)", "verified":"var(--green-dark)", "rejected":"var(--red)" };

    let html = `
      <div class="${isUrgent?'doc-deadline urgent':'doc-deadline'}" style="margin:10px">
        ⏰ Document deadline: <b>${daysLeft} day${daysLeft!==1?"s":""} remaining</b>${daysLeft===0?" — EXPIRED!":""}<br>
        Required: Licence · RC · Aadhaar · Bank Details
      </div>
      <div class="card">
        <div class="card-title">📄 KYC Documents Status</div>`;
    docs.forEach(doc_ => {
      const st = docStatus[doc_.key] || "missing";
      html += `<div class="doc-verify-row">
        <div class="doc-icon">${doc_.icon}</div>
        <div class="doc-info">
          <h5>${doc_.label} ${doc_.required?'<span style="color:var(--red);font-size:10px">*Required</span>':''}</h5>
          <p style="color:${statusColor[st]};font-weight:700">${statusIcon[st]||st}</p>
        </div>
        ${st==="missing"||st==="rejected"?`<label style="background:var(--primary);color:#fff;padding:6px 12px;border-radius:8px;font-size:12px;font-weight:700;cursor:pointer;text-transform:none">
          Upload<input type="file" accept="image/*,application/pdf" style="display:none" onchange="App.uploadDocForDriver('${doc_.key}',this)">
        </label>`:''}
      </div>`;
    });
    html += "</div>";
    content.innerHTML = html;
  },

  driverLoadWithdraw: async function(content) {
    if (!state.currentUser) return;
    const snap = await getDoc(doc(db,"users",state.currentUser.uid));
    const d = snap.data()||{};
    const balance = (d.earnings||0)-(d.withdrawn||0);

    content.innerHTML = `
      <div class="earnings-grid">
        <div class="earnings-item" style="grid-column:1/-1"><div class="amount" style="font-size:32px">₹${balance}</div><div class="label">Available to Withdraw</div></div>
      </div>
      <div class="card">
        <div class="card-title">🏧 Request Withdrawal</div>
        ${balance < 100 ? `<div style="background:#fff3e0;border-radius:10px;padding:12px;color:#e65100;font-weight:700;margin-bottom:12px">Minimum withdrawal: ₹100</div>`:""}
        <label>Amount (Min ₹100)</label>
        <input id="withdraw-amount" type="number" placeholder="Enter amount" min="100" max="${balance}">
        <label>UPI ID / Bank Details</label>
        <input id="withdraw-upi" placeholder="UPI ID or account number" value="${d.upiId||""}">
        <label>Note (Optional)</label>
        <input id="withdraw-note" placeholder="Any note for admin">
        <button class="btn btn-green" onclick="App.requestWithdraw(${balance})" ${balance<100?"disabled":""}>Request Withdrawal →</button>
        <p style="font-size:12px;color:var(--muted);margin-top:8px;text-align:center">Processed within 24 hours on weekdays</p>
      </div>
      <div class="section-label">Recent Withdrawals</div>
      <div id="withdraw-history"></div>`;
    App.loadWithdrawHistory();
  },

  requestWithdraw: async function(balance) {
    const amount = parseInt(document.getElementById("withdraw-amount").value)||0;
    const upi    = document.getElementById("withdraw-upi").value.trim();
    const note   = document.getElementById("withdraw-note").value.trim();
    if (amount < 100) return toast("Minimum withdrawal ₹100");
    if (amount > balance) return toast("Insufficient balance");
    if (!upi) return toast("Enter UPI ID or bank details");
    try {
      await addDoc(collection(db,"withdrawals"), {
        driverUid: state.currentUser.uid, driverName: state.currentUser.name,
        driverPhone: state.currentUser.phone, amount, upiId: upi, note,
        status: "pending", requestedAt: new Date().toISOString()
      });
      toast("✅ Withdrawal request submitted! Admin will process within 24 hours.");
      App.driverNav("withdraw");
    } catch(e) { toast("❌ " + e.message); }
  },

  loadWithdrawHistory: async function() {
    if (!state.currentUser) return;
    try {
      const q = query(collection(db,"withdrawals"), where("driverUid","==",state.currentUser.uid), orderBy("requestedAt","desc"), limit(10));
      const snap = await getDocs(q);
      const el = document.getElementById("withdraw-history");
      if (!el) return;
      if (snap.empty) { el.innerHTML = "<div style='text-align:center;padding:16px;color:var(--muted)'>No withdrawal history</div>"; return; }
      let html = "";
      snap.forEach(d => {
        const w = d.data();
        html += `<div class="ride-card ${w.status}" style="margin:8px 10px">
          <div style="display:flex;justify-content:space-between"><h4>₹${w.amount}</h4><span class="badge badge-${w.status}">${w.status}</span></div>
          <p>UPI: ${w.upiId}</p>
          <p style="font-size:12px">${new Date(w.requestedAt).toLocaleString("en-IN")}</p>
        </div>`;
      });
      el.innerHTML = html;
    } catch(e) {}
  },

  driverLoadProfile: async function(content) {
    if (!state.currentUser) return;
    const d = state.currentUser;
    content.innerHTML = `
      <div class="card" style="text-align:center">
        <div style="font-size:56px">🏍️</div>
        <h3 style="margin-top:8px;font-size:18px;font-weight:800">${d.name}</h3>
        <p style="color:var(--muted)">${d.vehicle} · ${d.vehicleNum||""}</p>
        <p style="margin-top:4px">⭐ ${d.avgRating||"New"} · ${d.totalRides||0} rides</p>
      </div>
      <div class="card">
        <div class="card-title">Profile Details</div>
        <p><b>Email:</b> ${d.email}</p>
        <p style="margin-top:6px"><b>Phone:</b> ${d.phone}</p>
        <p style="margin-top:6px"><b>UPI:</b> ${d.upiId||"Not set"}</p>
        <p style="margin-top:6px"><b>Status:</b> <span class="badge badge-${d.approvalStatus}">${d.approvalStatus}</span></p>
        <button class="btn btn-outline" style="margin-top:12px" onclick="App.driverLogout()">Logout</button>
      </div>`;
  },

  toggleDuty: async function(checked) {
    state.driverDuty = checked;
    const label  = document.getElementById("duty-label");
    const sublbl = document.getElementById("duty-sublabel");
    if (checked) {
      label.textContent  = "You're Online 🟢";
      sublbl.textContent = "Accepting rides now";
      toast("🟢 You're now online! Searching for rides...");
    } else {
      label.textContent  = "Go Online";
      sublbl.textContent = "Tap to start accepting rides";
      toast("⚫ You're offline.");
      if (state.driverRidesSub) { state.driverRidesSub(); state.driverRidesSub = null; }
    }
    if (state.currentUser) {
      await updateDoc(doc(db,"users",state.currentUser.uid), { isOnDuty: checked });
    }
    App.driverNav("rides");
  },

  driverLogout: async function() {
    if (state.driverRidesSub) state.driverRidesSub();
    if (state.currentUser) await updateDoc(doc(db,"users",state.currentUser.uid), { isOnDuty: false }).catch(()=>{});
    await signOut(auth);
    state.currentUser = null; state.role = null; state.driverDuty = false;
    showScreen("screen-landing");
    toast("👋 Logged out");
  },

  // ─── CHAT SYSTEM ───────────────────────────────────────────────────────────
  openChat: function(rideId, role, otherName, otherPhone) {
    state.chatRideId = rideId;
    state.chatRole   = role;
    document.getElementById("chat-title").textContent = `Chat with ${otherName}`;
    document.getElementById("chat-subtitle").textContent = `📞 ${otherPhone} · End-to-end`;
    document.getElementById("chat-messages").innerHTML = `<div style="text-align:center;padding:20px;color:var(--muted);font-size:13px">💬 Chat loaded</div>`;
    document.getElementById("chat-overlay").style.display = "flex";
    App.listenChat(rideId, role);
    document.getElementById("chat-input").focus();
  },

  listenChat: function(rideId, role) {
    if (state.chatSub) state.chatSub();
    state.chatSub = onSnapshot(doc(db,"rides",rideId), snap => {
      if (!snap.exists()) return;
      const messages = snap.data().messages || [];
      const container = document.getElementById("chat-messages");
      if (!container) return;
      if (!messages.length) {
        container.innerHTML = `<div style="text-align:center;padding:20px;color:var(--muted);font-size:13px">💬 No messages yet. Say hi!</div>`;
        return;
      }
      container.innerHTML = messages.map(m => {
        const isMine = m.sender === role;
        const time = new Date(m.timestamp).toLocaleTimeString("en-IN",{hour:"2-digit",minute:"2-digit"});
        return `<div class="chat-msg ${isMine?"sent":"received"}">
          ${m.text}
          <div class="msg-time">${time}</div>
        </div>`;
      }).join("");
      container.scrollTop = container.scrollHeight;
    });
  },

  sendChat: async function() {
    const input = document.getElementById("chat-input");
    const text  = input.value.trim();
    if (!text || !state.chatRideId) return;
    input.value = "";
    const message = {
      sender: state.chatRole,
      text, timestamp: new Date().toISOString()
    };
    try {
      const rideRef = doc(db,"rides",state.chatRideId);
      const snap = await getDoc(rideRef);
      const msgs = snap.data()?.messages || [];
      msgs.push(message);
      await updateDoc(rideRef, { messages: msgs });
    } catch(e) { toast("❌ Message failed to send"); }
  },

  sendQuick: function(text) {
    document.getElementById("chat-input").value = text;
    App.sendChat();
  },

  closeChat: function() {
    document.getElementById("chat-overlay").style.display = "none";
    if (state.chatSub) { state.chatSub(); state.chatSub = null; }
  },

  closeChatIfBg: function(e) {
    if (e.target.id === "chat-overlay") App.closeChat();
  },

  // ─── ADMIN LOGIN ───────────────────────────────────────────────────────────
  adminLogin: async function() {
    const email = document.getElementById("a-login-email").value.trim();
    const pass  = document.getElementById("a-login-pass").value;
    if (!email||!pass) return toast("Enter email and password");
    const btn = document.getElementById("admin-login-btn");
    btn.innerHTML = '<span class="spinner"></span>'; btn.disabled = true;
    try {
      const cred = await signInWithEmailAndPassword(auth, email, pass);
      if (cred.user.email?.toLowerCase() !== ADMIN_EMAIL.toLowerCase()) {
        await signOut(auth); btn.innerHTML="Login as Admin"; btn.disabled=false;
        return toast("❌ Not an admin account");
      }
      const userRef = doc(db,"users",cred.user.uid);
      const snap = await getDoc(userRef);
      if (!snap.exists()) {
        await setDoc(userRef, { uid:cred.user.uid, email:cred.user.email, name:"Admin", role:"admin", createdAt: new Date().toISOString() });
      }
      state.currentUser = { uid:cred.user.uid, email:cred.user.email, name:"Admin", role:"admin" };
      state.role = "admin";
      App.loadAdminDashboard();
    } catch(err) { toast("❌ " + err.message.replace("Firebase:","").trim()); }
    btn.innerHTML = "Login as Admin"; btn.disabled = false;
  },

  loadAdminDashboard: async function() {
    showScreen("screen-admin");
    // Stats
    try {
      const [ridesSnap, usersSnap, withdrawSnap] = await Promise.all([
        getDocs(collection(db,"rides")),
        getDocs(query(collection(db,"users"),where("role","==","driver"))),
        getDocs(query(collection(db,"withdrawals"),where("status","==","pending")))
      ]);
      const rides = ridesSnap.docs.map(d=>d.data());
      const totalFare = rides.filter(r=>r.status==="completed").reduce((a,r)=>a+(r.fare||0),0);
      document.getElementById("admin-stats-grid").innerHTML = `
        <div class="stat-card"><div class="stat-num">${ridesSnap.size}</div><div class="stat-label">Total Rides</div></div>
        <div class="stat-card"><div class="stat-num">${usersSnap.size}</div><div class="stat-label">Drivers</div></div>
        <div class="stat-card"><div class="stat-num">₹${totalFare}</div><div class="stat-label">Total Revenue</div></div>
        <div class="stat-card"><div class="stat-num">${withdrawSnap.size}</div><div class="stat-label">Pending Payouts</div></div>`;
    } catch(e) {
      document.getElementById("admin-stats-grid").innerHTML = "<div class='card' style='grid-column:1/-1;text-align:center'>Loading stats...</div>";
    }
    App.adminTab("rides");
  },

  adminTab: async function(tab) {
    document.querySelectorAll(".tab-btn").forEach(b=>b.classList.remove("active"));
    const allBtns = document.querySelectorAll(`#admin-tabs .tab-btn`);
    const tabMap = ["rides","kyc","drivers","withdrawals","analytics"];
    const idx = tabMap.indexOf(tab);
    if (idx>=0 && allBtns[idx]) allBtns[idx].classList.add("active");

    const content = document.getElementById("admin-content");
    if (tab==="rides") App.adminLoadRides(content);
    else if (tab==="kyc") App.adminLoadKYC(content);
    else if (tab==="drivers") App.adminLoadDrivers(content);
    else if (tab==="withdrawals") App.adminLoadWithdrawals(content);
    else if (tab==="analytics") App.adminLoadAnalytics(content);
  },

  adminLoadRides: function(content) {
    if (unsubAdmin) unsubAdmin();
    content.innerHTML = `<div class="tabs" style="padding:6px 10px">
      <button class="tab-btn active" onclick="App.adminFilterRides(this,'all')">All</button>
      <button class="tab-btn" onclick="App.adminFilterRides(this,'pending')">Pending</button>
      <button class="tab-btn" onclick="App.adminFilterRides(this,'accepted')">Active</button>
      <button class="tab-btn" onclick="App.adminFilterRides(this,'completed')">Done</button>
    </div>
    <div id="admin-rides-list"></div>`;

    const q = query(collection(db,"rides"), orderBy("createdAt","desc"), limit(50));
    unsubAdmin = onSnapshot(q, snap => {
      let rides = snap.docs.map(d=>({id:d.id,...d.data()}));
      const activeFilter = document.querySelector("#admin-rides-list")?.dataset?.filter || "all";
      if (activeFilter!=="all") rides = rides.filter(r=>r.status===activeFilter);
      const listEl = document.getElementById("admin-rides-list");
      if (!listEl) return;
      if (!rides.length) { listEl.innerHTML = "<div style='text-align:center;padding:30px;color:var(--muted)'>No rides found</div>"; return; }
      listEl.innerHTML = rides.map(r=>`<div class="ride-card ${r.status}">
        <div style="display:flex;justify-content:space-between;align-items:center">
          <h4>${r.vehicleIcon||"🚗"} ${r.vehicle} — ${r.userName}</h4>
          <span class="badge badge-${r.status}">${r.status}</span>
        </div>
        <div class="route-line"><div class="pickup">📍 ${r.pickup}</div><div class="drop">🏁 ${r.drop}</div></div>
        <p>📞 ${r.userPhone} · 💰 ₹${r.fare} · 📏 ~${r.estimatedKm?.toFixed(1)||"?"}km</p>
        ${r.driverName?`<p>🚗 Driver: <b>${r.driverName}</b> (${r.driverPhone})</p>`:""}
        <div style="display:flex;gap:6px;margin-top:8px;flex-wrap:wrap">
          ${r.status==="pending"?`<button class="btn btn-green btn-sm" onclick="App.adminAcceptRide('${r.id}')">✅ Accept as Admin</button>`:""}
          ${r.status==="accepted"||r.status==="arriving"?`<button class="btn btn-primary btn-sm" onclick="App.adminCompleteRide('${r.id}','${r.fare}','${r.driverUid||""}')">✅ Complete</button>`:""}
          ${r.status==="pending"||r.status==="accepted"?`<button class="btn btn-red btn-sm" onclick="App.adminCancelRide('${r.id}')">❌ Cancel</button>`:""}
          <button class="btn btn-sm btn-gray" onclick="window.open('tel:${r.userPhone}')">📞 User</button>
          ${r.driverPhone?`<button class="btn btn-sm btn-gray" onclick="window.open('tel:${r.driverPhone}')">📞 Driver</button>`:""}
        </div>
      </div>`).join("");
    });
  },

  adminFilterRides: function(btn, filter) {
    document.querySelectorAll("#admin-content .tabs .tab-btn").forEach(b=>b.classList.remove("active"));
    btn.classList.add("active");
    const el = document.getElementById("admin-rides-list");
    if (el) el.dataset.filter = filter;
    App.adminLoadRides(document.getElementById("admin-content"));
  },

  adminAcceptRide: async function(rideId) {
    try {
      await runTransaction(db, async t => {
        const rideRef = doc(db,"rides",rideId);
        const ride = await t.get(rideRef);
        if (!ride.exists()||ride.data().status!=="pending") throw new Error("Ride not available");
        t.update(rideRef, {
          status: "accepted", driverEmail: ADMIN_EMAIL, driverUid: "admin",
          driverName: "Admin Driver", driverPhone: "9110563922",
          driverVehicleNum: "ADMIN00", driverRating: "5.0",
          acceptedAt: new Date().toISOString()
        });
      });
      toast("✅ Ride accepted as Admin Driver");
    } catch(err) { toast("❌ " + err.message); }
  },

  adminCompleteRide: async function(rideId, fare, driverUid) {
    await updateDoc(doc(db,"rides",rideId), { status:"completed", completedAt: new Date().toISOString() });
    if (driverUid && driverUid!=="admin") {
      await updateDoc(doc(db,"users",driverUid), { earnings: increment(Math.round(fare*0.80)), totalRides: increment(1) });
    }
    toast("✅ Ride marked complete");
  },

  adminCancelRide: async function(rideId) {
    if (!confirm("Cancel this ride?")) return;
    await updateDoc(doc(db,"rides",rideId), { status:"cancelled", cancelledBy:"admin" });
    toast("Ride cancelled by admin");
  },

  adminLoadKYC: async function(content) {
    const q = query(collection(db,"users"), where("role","==","driver"));
    const snap = await getDocs(q);
    let html = "<div class='section-label'>Driver KYC Verification</div>";
    snap.forEach(d => {
      const u = d.data();
      const docStatus = u.docStatus||{};
      const trialUntil = u.trialUntil ? new Date(u.trialUntil) : null;
      const daysLeft = trialUntil ? Math.max(0,Math.ceil((trialUntil-new Date())/(24*3600*1000))) : 0;
      const allDocs = ["licence","rc","aadhaar","bank"];
      const pendingCount = allDocs.filter(k=>docStatus[k]==="pending_review").length;
      const missingCount = allDocs.filter(k=>!docStatus[k]||docStatus[k]==="missing").length;

      if (pendingCount===0&&missingCount===0&&u.approvalStatus==="approved") return;

      html += `<div class="card" style="border-left:4px solid ${pendingCount>0?"var(--primary)":missingCount>0?"var(--yellow)":"var(--green)"}">
        <div style="display:flex;justify-content:space-between;align-items:center">
          <div>
            <h4 style="font-size:15px;font-weight:800">${u.name}</h4>
            <p style="font-size:13px;color:var(--muted)">${u.vehicle} · ${u.vehicleNum||""} · ${u.phone}</p>
          </div>
          <span class="badge badge-${u.approvalStatus||"trial"}">${u.approvalStatus||"trial"}</span>
        </div>
        <p style="font-size:12px;margin:6px 0;color:${daysLeft<=2?"var(--red)":"var(--muted)"}">⏰ ${daysLeft} days left · ${pendingCount} pending · ${missingCount} missing</p>
        <div style="display:flex;gap:4px;flex-wrap:wrap;margin:8px 0">
          ${["licence","rc","aadhaar","bank","insurance"].map(k=>`<span style="background:${docStatus[k]==="verified"?"#e8f5e9":docStatus[k]==="pending_review"?"#e3f2fd":docStatus[k]==="rejected"?"#ffebee":"#f5f5f5"};color:${docStatus[k]==="verified"?"var(--green-dark)":docStatus[k]==="pending_review"?"var(--blue)":docStatus[k]==="rejected"?"var(--red)":"var(--muted)"};border-radius:6px;padding:3px 8px;font-size:11px;font-weight:700">${k}: ${docStatus[k]||"missing"}</span>`).join("")}
        </div>
        <div style="display:flex;gap:6px;flex-wrap:wrap">
          <button class="btn btn-green btn-sm" onclick="App.adminApproveDriver('${d.id}')">✅ Approve</button>
          <button class="btn btn-red btn-sm" onclick="App.adminRejectDriver('${d.id}')">❌ Reject</button>
          ${["licence","rc","aadhaar"].map(k=>docStatus[k]==="pending_review"?`<button class="btn btn-blue btn-sm" onclick="App.adminVerifyDoc('${d.id}','${k}')">✓ ${k}</button>`:"").join("")}
          <button class="btn btn-sm btn-gray" onclick="window.open('tel:${u.phone}')">📞 Call</button>
        </div>
      </div>`;
    });
    content.innerHTML = html || "<div style='text-align:center;padding:30px;color:var(--muted)'>No pending KYC</div>";
  },

  adminApproveDriver: async function(uid) {
    const docStatus = { licence:"verified",rc:"verified",aadhaar:"verified",bank:"verified",insurance:"verified",selfie:"verified" };
    await updateDoc(doc(db,"users",uid), { approvalStatus:"approved", docStatus, approvedAt: new Date().toISOString() });
    toast("✅ Driver approved!");
    App.adminTab("kyc");
  },

  adminRejectDriver: async function(uid) {
    const reason = prompt("Rejection reason:");
    if (!reason) return;
    await updateDoc(doc(db,"users",uid), { approvalStatus:"rejected", rejectionReason: reason, rejectedAt: new Date().toISOString() });
    toast("Driver rejected.");
    App.adminTab("kyc");
  },

  adminVerifyDoc: async function(uid, docKey) {
    const updateObj = {};
    updateObj[`docStatus.${docKey}`] = "verified";
    await updateDoc(doc(db,"users",uid), updateObj);
    toast(`✅ ${docKey} verified!`);
    App.adminTab("kyc");
  },

  adminLoadDrivers: async function(content) {
    const q = query(collection(db,"users"), where("role","==","driver"));
    const snap = await getDocs(q);
    let html = "<div class='section-label'>All Drivers</div>";
    snap.forEach(d => {
      const u = d.data();
      html += `<div class="card">
        <div style="display:flex;justify-content:space-between;align-items:center">
          <div>
            <h4 style="font-size:15px;font-weight:800">${u.name}</h4>
            <p style="font-size:13px;color:var(--muted)">${u.vehicle} · ${u.vehicleNum||""}</p>
          </div>
          <span class="badge badge-${u.isOnDuty?"online":"trial"}">${u.isOnDuty?"🟢 Online":"⚫ Offline"}</span>
        </div>
        <div style="display:flex;gap:6px;flex-wrap:wrap;margin-top:8px">
          <span style="font-size:12px;color:var(--muted)">📞 ${u.phone}</span>
          <span style="font-size:12px;color:var(--muted)">⭐ ${u.avgRating||"New"}</span>
          <span style="font-size:12px;color:var(--muted)">🚗 ${u.totalRides||0} rides</span>
          <span style="font-size:12px;color:var(--muted)">💰 ₹${u.earnings||0} earned</span>
          <span class="badge badge-${u.approvalStatus||"trial"}" style="font-size:11px">${u.approvalStatus||"trial"}</span>
        </div>
        <div style="display:flex;gap:6px;margin-top:8px">
          <button class="btn btn-sm btn-gray" onclick="window.open('tel:${u.phone}')">📞 Call</button>
          <button class="btn btn-sm btn-red" onclick="App.adminSuspendDriver('${d.id}')">🚫 Suspend</button>
          ${u.approvalStatus!=="approved"?`<button class="btn btn-sm btn-green" onclick="App.adminApproveDriver('${d.id}')">✅ Approve</button>`:""}
        </div>
      </div>`;
    });
    content.innerHTML = html || "<div style='text-align:center;padding:30px;color:var(--muted)'>No drivers yet</div>";
  },

  adminSuspendDriver: async function(uid) {
    if (!confirm("Suspend this driver?")) return;
    await updateDoc(doc(db,"users",uid), { approvalStatus:"rejected", isOnDuty: false });
    toast("Driver suspended.");
    App.adminTab("drivers");
  },

  adminLoadWithdrawals: async function(content) {
    const q = query(collection(db,"withdrawals"), orderBy("requestedAt","desc"), limit(50));
    const snap = await getDocs(q);
    let html = "<div class='section-label'>Withdrawal Requests</div>";
    snap.forEach(d => {
      const w = d.data();
      html += `<div class="card" style="border-left:4px solid ${w.status==="pending"?"var(--yellow)":w.status==="paid"?"var(--green)":"var(--red)"}">
        <div style="display:flex;justify-content:space-between">
          <h4 style="font-size:16px;font-weight:800">₹${w.amount}</h4>
          <span class="badge badge-${w.status==="paid"?"approved":w.status==="rejected"?"rejected":"pending"}">${w.status}</span>
        </div>
        <p style="margin:4px 0"><b>${w.driverName}</b> · ${w.driverPhone}</p>
        <p style="font-size:13px;color:var(--muted)">UPI: ${w.upiId}</p>
        ${w.note?`<p style="font-size:12px;color:var(--muted)">Note: ${w.note}</p>`:""}
        <p style="font-size:12px;color:var(--muted)">${new Date(w.requestedAt).toLocaleString("en-IN")}</p>
        ${w.status==="pending"?`<div style="display:flex;gap:8px;margin-top:10px">
          <button class="btn btn-green btn-sm" onclick="App.adminPayWithdraw('${d.id}','${w.driverUid}','${w.amount}')">💸 Mark Paid</button>
          <button class="btn btn-red btn-sm" onclick="App.adminRejectWithdraw('${d.id}')">❌ Reject</button>
        </div>`:""}
      </div>`;
    });
    content.innerHTML = snap.empty ? "<div style='text-align:center;padding:30px;color:var(--muted)'>No withdrawal requests</div>" : html;
  },

  adminPayWithdraw: async function(wId, driverUid, amount) {
    if (!confirm(`Mark ₹${amount} as paid?`)) return;
    await updateDoc(doc(db,"withdrawals",wId), { status:"paid", paidAt: new Date().toISOString() });
    if (driverUid) await updateDoc(doc(db,"users",driverUid), { withdrawn: increment(parseInt(amount)) });
    toast("✅ Marked as paid!");
    App.adminTab("withdrawals");
  },

  adminRejectWithdraw: async function(wId) {
    const reason = prompt("Rejection reason:");
    if (!reason) return;
    await updateDoc(doc(db,"withdrawals",wId), { status:"rejected", rejectionReason: reason });
    toast("Withdrawal rejected.");
    App.adminTab("withdrawals");
  },

  adminLoadAnalytics: async function(content) {
    const [ridesSnap, driversSnap] = await Promise.all([
      getDocs(collection(db,"rides")),
      getDocs(query(collection(db,"users"),where("role","==","driver")))
    ]);
    const rides = ridesSnap.docs.map(d=>d.data());
    const completed = rides.filter(r=>r.status==="completed");
    const cancelled = rides.filter(r=>r.status==="cancelled");
    const pending   = rides.filter(r=>r.status==="pending");
    const totalRevenue = completed.reduce((a,r)=>a+(r.fare||0),0);
    const platformCut = Math.round(totalRevenue*0.20);
    const bikeRides  = completed.filter(r=>r.vehicle==="Bike").length;
    const autoRides  = completed.filter(r=>r.vehicle==="Auto").length;
    const carRides   = completed.filter(r=>r.vehicle==="Car").length;
    const approved   = driversSnap.docs.filter(d=>d.data().approvalStatus==="approved").length;
    const trial      = driversSnap.docs.filter(d=>d.data().approvalStatus==="trial").length;
    const online     = driversSnap.docs.filter(d=>d.data().isOnDuty).length;

    content.innerHTML = `
      <div class="admin-stat-grid">
        <div class="stat-card"><div class="stat-num">${completed.length}</div><div class="stat-label">Completed Rides</div></div>
        <div class="stat-card"><div class="stat-num">${cancelled.length}</div><div class="stat-label">Cancelled Rides</div></div>
        <div class="stat-card"><div class="stat-num">₹${totalRevenue}</div><div class="stat-label">Total Revenue</div></div>
        <div class="stat-card"><div class="stat-num">₹${platformCut}</div><div class="stat-label">Platform Earnings</div></div>
        <div class="stat-card"><div class="stat-num">${driversSnap.size}</div><div class="stat-label">Total Drivers</div></div>
        <div class="stat-card"><div class="stat-num">${online}</div><div class="stat-label">Online Drivers</div></div>
        <div class="stat-card"><div class="stat-num">${approved}</div><div class="stat-label">Verified Drivers</div></div>
        <div class="stat-card"><div class="stat-num">${trial}</div><div class="stat-label">Trial Drivers</div></div>
        <div class="stat-card"><div class="stat-num">${pending.length}</div><div class="stat-label">Pending Rides</div></div>
        <div class="stat-card"><div class="stat-num">${bikeRides}</div><div class="stat-label">Bike Rides</div></div>
        <div class="stat-card"><div class="stat-num">${autoRides}</div><div class="stat-label">Auto Rides</div></div>
        <div class="stat-card"><div class="stat-num">${carRides}</div><div class="stat-label">Car Rides</div></div>
      </div>`;
  },

  adminLogout: async function() {
    if (unsubAdmin) { unsubAdmin(); unsubAdmin = null; }
    await signOut(auth);
    state.currentUser = null; state.role = null;
    showScreen("screen-landing");
    toast("👋 Logged out");
  },

  forgotPassword: async function(role) {
    const emailEl = document.getElementById(role==="driver" ? "d-login-email" : "a-login-email");
    const email = emailEl?.value.trim();
    if (!email) return toast("Enter your email first");
    try {
      await sendPasswordResetEmail(auth, email);
      toast("📧 Password reset email sent!");
    } catch(err) { toast("❌ " + err.message.replace("Firebase:","").trim()); }
  }
};

// ─── AUTO-LOGIN ────────────────────────────────────────────────────────────────
auth.onAuthStateChanged(async user => {
  if (!user) return;
  try {
    if (user.email?.toLowerCase() === ADMIN_EMAIL.toLowerCase()) {
      const userRef = doc(db,"users",user.uid);
      const snap = await getDoc(userRef);
      if (!snap.exists()) {
        await setDoc(userRef, { uid:user.uid, email:user.email, name:"Admin", role:"admin", createdAt: new Date().toISOString() });
      }
      state.currentUser = { uid:user.uid, email:user.email, name:"Admin", role:"admin" };
      state.role = "admin";
      App.loadAdminDashboard();
      return;
    }
    const snap = await getDoc(doc(db,"users",user.uid));
    if (!snap.exists()) return;
    const ud = { ...snap.data(), uid:user.uid };
    state.currentUser = ud;
    state.role = ud.role;
    if (ud.role==="driver") App.loadDriverDashboard();
    else if (ud.role==="admin") App.loadAdminDashboard();
  } catch(e) { console.warn("Auto-login:", e); }
});

showScreen("screen-landing");
</script>

<!-- Google Maps API — Replace YOUR_API_KEY with your actual Google Maps API Key -->
<!-- Get one free at: https://console.cloud.google.com/google/maps-apis -->
<script async defer src="https://maps.googleapis.com/maps/api/js?key=YOUR_GOOGLE_MAPS_API_KEY&libraries=places&callback=initGoogleMaps"></script>

</body>
</html>

<!--
════════════════════════════════════════════════════
  SETUP GUIDE — READ BEFORE DEPLOYING
════════════════════════════════════════════════════

1. GOOGLE MAPS API KEY (REQUIRED for live maps, routes, distances)
   ─────────────────────────────────────────────────
   • Go to: https://console.cloud.google.com/
   • Create project → Enable these APIs:
     - Maps JavaScript API
     - Directions API
     - Places API
     - Geocoding API
   • Create API Key → Restrict to your domain
   • Replace "YOUR_GOOGLE_MAPS_API_KEY" in the script tag at bottom
   • Without this key, maps won't show but app still works with Nominatim geocoding

2. FIREBASE SETUP
   ─────────────────────────────────────────────────
   • Authentication → Enable Email/Password
   • Create admin account: bonagirispinoj@gmail.com
   • Paste Firestore rules below and Publish
   • Create Firestore indexes if prompted:
     rides: createdAt DESC
     rides: vehicle ASC, createdAt DESC

3. FIRESTORE RULES
   ─────────────────────────────────────────────────

rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    function isAdmin() {
      return request.auth != null && request.auth.token.email == "bonagirispinoj@gmail.com";
    }
    match /users/{uid} {
      allow read: if request.auth != null && (request.auth.uid == uid || isAdmin());
      allow create: if request.auth != null && request.auth.uid == uid;
      allow update: if request.auth != null && (request.auth.uid == uid || isAdmin());
      allow delete: if isAdmin();
    }
    match /rides/{rideId} {
      allow create: if true;
      allow read: if true;
      allow update: if true;
      allow delete: if isAdmin();
    }
    match /withdrawals/{wId} {
      allow create: if request.auth != null;
      allow read: if request.auth != null;
      allow update: if isAdmin();
    }
  }
}

════════════════════════════════════════════════════
  NEW FEATURES IN THIS VERSION
════════════════════════════════════════════════════

✅ DRIVER PANEL WITHOUT LOGIN — Open registration, anyone can become a driver
✅ 7-DAY DOCUMENT VERIFICATION WINDOW (was 3 days)
✅ DOCUMENTS REQUIRED: Licence, RC, Aadhaar, Bank Account, Insurance (auto/car)
✅ GOOGLE MAPS INTEGRATION — Real routes, distances, ETAs, turn-by-turn directions
✅ DRIVER ACCEPT → Shows user phone to driver + driver phone to user
✅ IN-APP CHAT — Real-time messaging between user and driver with WhatsApp backup
✅ QUICK MESSAGE CHIPS — "2 mins away", "I've arrived", etc.
✅ DRIVER ACTIVE RIDE SCREEN — Map showing route to pickup
✅ ADMIN KYC PANEL — Verify individual documents, approve/reject drivers
✅ OTP-STYLE RIDE COMPLETION — Driver confirms arrival, then marks complete
✅ ANALYTICS DASHBOARD — Revenue, rides by vehicle type, driver stats
✅ SOS EMERGENCY — Calls emergency contact or 112
════════════════════════════════════════════════════
-->
