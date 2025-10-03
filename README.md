# tova-ai
<!DOCTYPE html>
<html lang="sv">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Investera med Tova AI</title>
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; }
        body { font-family: 'Helvetica Neue', Helvetica, Arial, sans-serif; background: #ffffff; color: #1a1a1a; display: flex; height: 100vh; overflow: hidden; }
        .left-sidebar { width: 300px; background: #ffffff; border-right: 1px solid #EDEDED; display: flex; flex-direction: column; padding: 32px 24px; transition: all 0.3s ease; position: relative; z-index: 30; }
        .left-sidebar.collapsed { width: 80px; padding: 32px 16px; }
        .sidebar-logo { display: flex; align-items: center; gap: 8px; margin-bottom: 40px; position: relative; justify-content: flex-start; }
        .left-sidebar.collapsed .sidebar-logo { justify-content: center; }
        .sidebar-logo-text { font-size: 28px; font-weight: 600; color: #1a1a1a; transition: opacity 0.3s ease, width 0.3s ease; }
        .left-sidebar.collapsed .sidebar-logo-text { opacity: 0; width: 0; overflow: hidden; }
        .sidebar-badge { background: #1a1a1a; color: #ffffff; padding: 6px 10px; border-radius: 8px; font-size: 13px; font-weight: 400; transition: opacity 0.3s ease, width 0.3s ease; margin-top: 4px; }
        .left-sidebar.collapsed .sidebar-badge { opacity: 0; width: 0; overflow: hidden; }
        .sidebar-logo-collapsed { display: none; font-size: 28px; font-weight: 600; color: #1a1a1a; transition: opacity 0.3s ease; position: absolute; left: 50%; transform: translateX(-50%); }
        .left-sidebar.collapsed .sidebar-logo-collapsed { display: block; opacity: 1; }
        .sidebar-nav { display: flex; flex-direction: column; gap: 4px; flex: 1; }
        .nav-item-sidebar { display: flex; align-items: center; gap: 12px; padding: 12px 16px; border-radius: 12px; cursor: pointer; transition: all 0.2s; color: #7E7E7E; font-size: 15px; white-space: nowrap; position: relative; }
        .left-sidebar.collapsed .nav-item-sidebar { justify-content: center; padding: 12px; gap: 0; }
        .nav-item-sidebar:hover { background: #F5F4F2; }
        .nav-item-sidebar.active { background: #F5F4F2; color: #000000; font-weight: 500; }
        .nav-item-sidebar svg { flex-shrink: 0; width: 20px; height: 20px; stroke: #7E7E7E; }
        .nav-item-sidebar.active svg { stroke: #000000; }
        .nav-item-sidebar span { transition: opacity 0.3s ease; }
        .left-sidebar.collapsed .nav-item-sidebar span:not(.nav-tooltip) { opacity: 0; width: 0; overflow: hidden; }
        .nav-tooltip { position: absolute; left: calc(100% + 12px); background: #1a1a1a; color: #ffffff; padding: 8px 12px; border-radius: 8px; font-size: 13px; white-space: nowrap; pointer-events: none; opacity: 0; transition: opacity 0.2s ease; z-index: 200; box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15); width: auto; overflow: visible; }
        .nav-tooltip::before { content: ''; position: absolute; left: -4px; top: 50%; transform: translateY(-50%); width: 0; height: 0; border-top: 4px solid transparent; border-bottom: 4px solid transparent; border-right: 4px solid #1a1a1a; }
        .left-sidebar.collapsed .nav-item-sidebar:hover .nav-tooltip { opacity: 1; }
        .sidebar-divider-line { height: 1px; background: #EDEDED; margin: 16px 0; }
        .sidebar-footer { margin-top: auto; }
        .sidebar-user { display: flex; align-items: center; gap: 12px; padding: 12px 16px; border-radius: 12px; cursor: pointer; transition: background 0.2s; }
        .left-sidebar.collapsed .sidebar-user { justify-content: center; padding: 12px; gap: 0; }
        .sidebar-user:hover { background: #F5F4F2; }
        .sidebar-user-avatar { width: 40px; height: 40px; border-radius: 50%; background: #F5F4F2; color: #000000; display: flex; align-items: center; justify-content: center; font-weight: 500; font-size: 14px; flex-shrink: 0; }
        .sidebar-user-info { flex: 1; transition: opacity 0.3s ease; }
        .left-sidebar.collapsed .sidebar-user-info { opacity: 0; width: 0; overflow: hidden; }
        .sidebar-user-name { font-size: 14px; font-weight: 500; color: #1a1a1a; white-space: nowrap; }
        .sidebar-user-plan { font-size: 12px; color: #999; white-space: nowrap; }
        .sidebar-user-dropdown { width: 20px; height: 20px; color: #999; transition: opacity 0.3s ease; }
        .left-sidebar.collapsed .sidebar-user-dropdown { opacity: 0; width: 0; overflow: hidden; }
        .minimize-icon-collapsed { display: none; }
        .left-sidebar.collapsed .minimize-icon-expanded { display: none; }
        .left-sidebar.collapsed .minimize-icon-collapsed { display: block; }
        .transfer-overlay { position: fixed; top: 0; left: 0; right: 0; bottom: 0; background: rgba(0, 0, 0, 0.2); z-index: 200; display: none; align-items: center; justify-content: center; opacity: 0; transition: opacity 0.3s ease; }
        .transfer-overlay.active { display: flex; opacity: 1; }
        .transfer-popup { background: #ffffff; border-radius: 24px; padding: 40px; width: 90%; max-width: 500px; box-shadow: 0 20px 60px rgba(0, 0, 0, 0.15); position: relative; z-index: 201; transform: scale(0.9); opacity: 0; transition: transform 0.3s ease, opacity 0.3s ease; }
        .transfer-overlay.active .transfer-popup { transform: scale(1); opacity: 1; }
        .transfer-popup-header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 32px; }
        .transfer-popup-title { font-size: 24px; font-weight: 600; color: #1a1a1a; }
        .transfer-popup-close { width: 32px; height: 32px; border: none; background: #F5F4F2; border-radius: 8px; cursor: pointer; display: flex; align-items: center; justify-content: center; color: #1a1a1a; transition: background 0.2s; }
        .transfer-popup-close:hover { background: #E9E7E2; }
        .transfer-tabs { display: flex; gap: 8px; margin-bottom: 32px; background: #F5F4F2; padding: 4px; border-radius: 12px; transition: opacity 0.3s ease; }
        .transfer-tabs.hidden { opacity: 0; pointer-events: none; height: 0; margin-bottom: 0; overflow: hidden; }
        .transfer-tab { flex: 1; padding: 12px; border: none; background: transparent; border-radius: 8px; font-size: 14px; font-weight: 500; cursor: pointer; transition: all 0.2s; color: #7E7E7E; font-family: 'Helvetica Neue', Helvetica, Arial, sans-serif; }
        .transfer-tab.active { background: #ffffff; color: #000000; box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1); }
        .transfer-content { display: none; }
        .transfer-content.active { display: block; }
        .transfer-field { margin-bottom: 24px; }
        .transfer-label { display: block; font-size: 13px; font-weight: 500; color: #666; margin-bottom: 8px; }
        .transfer-input { width: 100%; padding: 14px 16px; border: 1px solid #EDEDED; border-radius: 12px; font-size: 15px; font-family: 'Helvetica Neue', Helvetica, Arial, sans-serif; outline: none; transition: border 0.2s; }
        .transfer-input:focus { border-color: #999; }
        .transfer-select { width: 100%; padding: 14px 16px; padding-right: 40px; border: 1px solid #EDEDED; border-radius: 12px; font-size: 15px; font-family: 'Helvetica Neue', Helvetica, Arial, sans-serif; outline: none; transition: border 0.2s; background: #ffffff; cursor: pointer; appearance: none; background-image: url("data:image/svg+xml,%3Csvg width='16' height='16' viewBox='0 0 24 24' fill='none' xmlns='http://www.w3.org/2000/svg'%3E%3Cpath d='M6 9l6 6 6-6' stroke='%237E7E7E' stroke-width='2' stroke-linecap='round' stroke-linejoin='round'/%3E%3C/svg%3E"); background-repeat: no-repeat; background-position: right 16px center; }
        .transfer-select:focus { border-color: #999; }
        .transfer-info { background: #F5F4F2; padding: 16px; border-radius: 12px; margin-bottom: 24px; font-size: 13px; color: #666; line-height: 1.5; }
        .transfer-button { width: 100%; padding: 16px; background: #1a1a1a; color: #ffffff; border: none; border-radius: 12px; font-size: 15px; font-weight: 500; cursor: pointer; transition: background 0.2s; font-family: 'Helvetica Neue', Helvetica, Arial, sans-serif; }
        .transfer-button:hover { background: #333; }
        .transfer-button:disabled { opacity: 0.4; cursor: not-allowed; }
        .transfer-button:disabled:hover { background: #1a1a1a; }
        .transfer-button.loading { position: relative; color: transparent; pointer-events: none; }
        .transfer-button.loading::after { content: ''; position: absolute; top: calc(50% - 10px); left: calc(50% - 10px); width: 20px; height: 20px; border: 3px solid rgba(255,255,255,0.3); border-top-color: #ffffff; border-radius: 50%; animation: spin 0.8s linear infinite; }
        .transfer-success-view { display: none; text-align: center; padding: 0; }
        .transfer-success-view.active { display: block; animation: fadeIn 0.4s ease; }
        .transfer-success-icon { width: 80px; height: 80px; margin: 0 auto 24px; background: #00A86B; border-radius: 50%; display: flex; align-items: center; justify-content: center; animation: successPop 0.5s ease; }
        .transfer-success-icon svg { width: 40px; height: 40px; stroke: #ffffff; stroke-width: 3; }
        .transfer-success-title { font-size: 24px; font-weight: 600; color: #1a1a1a; margin-bottom: 12px; }
        .transfer-success-text { font-size: 15px; color: #666; line-height: 1.6; margin-bottom: 12px; }
        .transfer-success-amount { font-size: 32px; font-weight: 400; color: #1a1a1a; margin-bottom: 24px; }
        .transfer-success-info { font-size: 13px; color: #666; line-height: 1.5; margin-bottom: 32px; background: #F5F4F2; padding: 16px; border-radius: 12px; }
        .transfer-success-button { width: 100%; padding: 16px; background: #1a1a1a; color: #ffffff; border: none; border-radius: 12px; font-size: 15px; font-weight: 500; cursor: pointer; transition: background 0.2s; font-family: 'Helvetica Neue', Helvetica, Arial, sans-serif; }
        .transfer-success-button:hover { background: #333; }
        .bank-overlay { position: fixed; top: 0; left: 0; right: 0; bottom: 0; background: rgba(0, 0, 0, 0.2); z-index: 200; display: none; align-items: center; justify-content: center; opacity: 0; transition: opacity 0.3s ease; }
        .bank-overlay.active { display: flex; opacity: 1; }
        .bank-popup { background: #ffffff; border-radius: 24px; padding: 32px; width: 600px; max-width: 600px; box-shadow: 0 20px 60px rgba(0, 0, 0, 0.15); position: relative; z-index: 201; transform: scale(0.9); opacity: 0; transition: transform 0.3s ease, opacity 0.3s ease; }
        .bank-overlay.active .bank-popup { transform: scale(1); opacity: 1; }
        .bank-popup-header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 24px; }
        .bank-popup-title { font-size: 24px; font-weight: 500; color: #1a1a1a; }
        .bank-popup-subtitle { font-size: 14px; color: #666; margin-bottom: 32px; line-height: 1.5; }
        .bank-popup-close { width: 32px; height: 32px; border: none; background: #F5F4F2; border-radius: 8px; cursor: pointer; display: flex; align-items: center; justify-content: center; color: #1a1a1a; transition: background 0.2s; }
        .bank-popup-close:hover { background: #E9E7E2; }
        .bank-list { display: grid; grid-template-columns: repeat(3, 1fr); gap: 12px; }
        .bank-item { padding: 20px; border: 1px solid #EDEDED; border-radius: 12px; cursor: pointer; transition: all 0.2s; display: flex; flex-direction: column; align-items: center; gap: 12px; text-align: center; }
        .bank-item:hover { border-color: #000000; background: #FBFBFA; }
        .bank-logo { width: 48px; height: 48px; border-radius: 50%; display: flex; align-items: center; justify-content: center; font-weight: 400; font-size: 12px; color: #ffffff; }
        .bank-logo.swedbank { background: #fb4f00; }
        .bank-logo.nordea { background: #00005e; }
        .bank-logo.seb { background: #003824; }
        .bank-logo.handelsbanken { background: #005fa5; }
        .bank-logo.skandiabanken { background: #038069; }
        .bank-logo.avanza { background: #07383e; }
        .bank-logo.lansforsakringar { background: #005aa0; }
        .bank-logo.ica { background: #e13205; }
        .bank-logo.other { background: #F5F4F2; color: #1a1a1a; }
        .bank-name { font-size: 14px; font-weight: 500; color: #1a1a1a; }
        .bank-step { display: none; }
        .bank-step.active { display: block; }
        .bank-back { display: flex; align-items: center; gap: 8px; color: #7E7E7E; font-size: 14px; cursor: pointer; margin-bottom: 24px; transition: color 0.2s; }
        .bank-back:hover { color: #1a1a1a; }
        .account-list { display: flex; flex-direction: column; gap: 12px; }
        .account-item { padding: 16px 20px; border: 1px solid #EDEDED; border-radius: 12px; cursor: pointer; transition: all 0.2s; display: flex; justify-content: space-between; align-items: center; gap: 12px; }
        .account-item:hover { border-color: #999; background: #FBFBFA; }
        .account-info { display: flex; flex-direction: column; gap: 4px; flex: 1; }
        .account-name { font-size: 15px; font-weight: 500; color: #1a1a1a; }
        .account-number { font-size: 13px; color: #666; }
        .account-check { width: 20px; height: 20px; border-radius: 50%; border: 2px solid #EDEDED; flex-shrink: 0; }
        .account-bank-logo { width: 40px; height: 40px; border-radius: 50%; display: flex; align-items: center; justify-content: center; font-weight: 400; font-size: 12px; color: #ffffff; flex-shrink: 0; background: #E0E0E0; }
        .account-bank-logo.swedbank { background: #fb4f00; }
        .account-bank-logo.nordea { background: #00005e; }
        .account-bank-logo.seb { background: #003824; }
        .account-bank-logo.handelsbanken { background: #005fa5; }
        .account-bank-logo.skandiabanken { background: #038069; }
        .account-bank-logo.avanza { background: #07383e; }
        .account-bank-logo.lansforsakringar { background: #005aa0; }
        .account-bank-logo.ica { background: #e13205; }
        .account-bank-logo.other { background: #F5F4F2; color: #1a1a1a; }
        .account-item.selected .account-check { background: #1a1a1a; border-color: #1a1a1a; position: relative; }
        .account-item.selected .account-check::after { content: ''; position: absolute; top: 50%; left: 50%; transform: translate(-50%, -50%); width: 6px; height: 6px; background: #ffffff; border-radius: 50%; }
        .bank-connect-button { width: 100%; padding: 16px; background: #1a1a1a; color: #ffffff; border: none; border-radius: 12px; font-size: 15px; font-weight: 500; cursor: pointer; transition: background 0.2s; font-family: 'Helvetica Neue', Helvetica, Arial, sans-serif; margin-top: 24px; }
        .bank-connect-button:hover { background: #333; }
        .bank-connect-button:disabled { opacity: 0.4; cursor: not-allowed; }
        .bank-connect-button:disabled:hover { background: #1a1a1a; }
        .bank-connect-button.loading { position: relative; color: transparent; pointer-events: none; }
        .bank-connect-button.loading::after { content: ''; position: absolute; top: 50%; left: 50%; width: 20px; height: 20px; margin: -10px 0 0 -10px; border: 3px solid rgba(255,255,255,0.3); border-top-color: #ffffff; border-radius: 50%; animation: spin 0.8s linear infinite; }
        @keyframes spin { to { transform: rotate(360deg); } }
        .bank-success-view { display: none; text-align: center; padding: 0; }
        .bank-success-view.active { display: block; animation: fadeIn 0.4s ease; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }
        .bank-success-icon { width: 80px; height: 80px; margin: 0 auto 24px; background: #00A86B; border-radius: 50%; display: flex; align-items: center; justify-content: center; animation: successPop 0.5s ease; }
        @keyframes successPop { 0% { transform: scale(0); } 50% { transform: scale(1.1); } 100% { transform: scale(1); } }
        .bank-success-icon svg { width: 40px; height: 40px; stroke: #ffffff; stroke-width: 3; }
        .bank-success-title { font-size: 24px; font-weight: 600; color: #1a1a1a; margin-bottom: 12px; }
        .bank-success-text { font-size: 15px; color: #666; line-height: 1.6; margin-bottom: 32px; }
        .bank-success-button { width: 100%; padding: 16px; background: #1a1a1a; color: #ffffff; border: none; border-radius: 12px; font-size: 15px; font-weight: 500; cursor: pointer; transition: background 0.2s; font-family: 'Helvetica Neue', Helvetica, Arial, sans-serif; }
        .bank-success-button:hover { background: #333; }
        .bank-bankid-view { display: none; padding: 0; }
        .bank-bankid-view.active { display: block; }
        .bank-bankid-tabs { display: flex; gap: 8px; margin-bottom: 24px; background: #F5F4F2; padding: 4px; border-radius: 12px; }
        .bank-bankid-tab { flex: 1; padding: 12px; border: none; background: transparent; border-radius: 8px; font-size: 14px; font-weight: 500; cursor: pointer; transition: all 0.2s; color: #7E7E7E; font-family: 'Helvetica Neue', Helvetica, Arial, sans-serif; }
        .bank-bankid-tab.active { background: #ffffff; color: #000000; box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1); }
        .bank-bankid-instruction { font-size: 14px; color: #666; line-height: 1.6; margin-bottom: 24px; text-align: left; }
        .bank-bankid-qr { width: 200px; height: 200px; margin: 0 auto 32px; background: #ffffff; border: 1px solid #EDEDED; border-radius: 12px; display: flex; align-items: center; justify-content: center; }
        .bank-bankid-qr svg { width: 180px; height: 180px; }
        .bank-bankid-spinner { width: 40px; height: 40px; border: 4px solid #F5F4F2; border-top-color: #1a1a1a; border-radius: 50%; animation: spin 0.8s linear infinite; }
        .bank-bankid-divider { display: flex; align-items: center; gap: 16px; margin: 32px 0 24px; }
        .bank-bankid-divider-line { flex: 1; height: 1px; background: #EDEDED; }
        .bank-bankid-divider-text { font-size: 14px; color: #666; }
        .bank-bankid-alternate { font-size: 14px; color: #1a1a1a; text-align: center; margin-bottom: 24px; }
        .bank-bankid-card-content { display: none; text-align: left; }
        .bank-bankid-card-content.active { display: block; }
        .bank-bankid-card-content ol { margin: 0; padding-left: 20px; }
        .bank-bankid-card-content li { font-size: 14px; color: #666; line-height: 1.8; margin-bottom: 12px; }
        .package-sidebar-overlay { position: fixed; top: 0; left: 0; right: 0; bottom: 0; background: rgba(0, 0, 0, 0.2); z-index: 300; display: none; opacity: 0; transition: opacity 0.4s ease; }
        .package-sidebar-overlay.active { display: block; opacity: 1; }
        .package-sidebar { position: fixed; top: 0; right: -500px; width: 500px; height: 100%; background: #ffffff; z-index: 301; overflow-y: auto; transition: right 0.4s ease; box-shadow: -4px 0 20px rgba(0, 0, 0, 0.1); display: flex; flex-direction: column; }
        .package-sidebar.active { right: 0; }
        .package-sidebar-header { padding: 32px; border-bottom: 1px solid #EDEDED; display: flex; justify-content: space-between; align-items: flex-start; flex-shrink: 0; }
        .package-sidebar-close { width: 32px; height: 32px; border: none; background: #F5F4F2; border-radius: 8px; cursor: pointer; display: flex; align-items: center; justify-content: center; color: #1a1a1a; transition: background 0.2s; flex-shrink: 0; }
        .package-sidebar-close:hover { background: #E9E7E2; }
        .package-sidebar-content { padding: 32px; flex: 1; overflow-y: auto; }
        .package-sidebar-title { font-size: 24px; font-weight: 600; color: #1a1a1a; margin-bottom: 8px; }
        .package-sidebar-subtitle { font-size: 14px; color: #666; line-height: 1.5; }
        .package-sidebar-section { margin-bottom: 32px; }
        .package-sidebar-section-title { font-size: 16px; font-weight: 600; color: #1a1a1a; margin-bottom: 16px; }
        .package-sidebar-allocation { display: flex; align-items: center; gap: 16px; padding: 20px; background: #FBFBFA; border-radius: 12px; margin-bottom: 16px; }
        .package-sidebar-pie { width: 60px; height: 60px; border-radius: 50%; flex-shrink: 0; }
        .package-sidebar-breakdown { flex: 1; display: grid; grid-template-columns: repeat(3, 1fr); gap: 12px; }
        .package-sidebar-breakdown-item { display: flex; flex-direction: column; gap: 4px; margin-bottom: 0; font-size: 14px; }
        .package-sidebar-breakdown-item:last-child { margin-bottom: 0; }
        .package-sidebar-breakdown-label { color: #666; font-size: 11px; text-transform: uppercase; letter-spacing: 0.5px; }
        .package-sidebar-breakdown-value { font-weight: 600; color: #1a1a1a; font-size: 16px; }
        .package-sidebar-details { display: grid; grid-template-columns: repeat(3, 1fr); gap: 12px; }
        .package-sidebar-detail { display: flex; justify-content: space-between; align-items: center; padding: 16px; background: #FBFBFA; border-radius: 12px; }
        .package-sidebar-detail-label { font-size: 13px; color: #666; text-transform: uppercase; letter-spacing: 0.5px; }
        .package-sidebar-detail-value { font-size: 18px; font-weight: 600; color: #1a1a1a; }
        .package-sidebar-button { padding: 20px 32px; background: #ffffff; border-top: 1px solid #EDEDED; flex-shrink: 0; }
        .package-sidebar-button button { width: 100%; padding: 16px; background: #1a1a1a; color: #ffffff; border: none; border-radius: 12px; font-size: 15px; font-weight: 500; cursor: pointer; transition: background 0.2s; font-family: 'Helvetica Neue', Helvetica, Arial, sans-serif; }
        .package-sidebar-button button:hover { background: #333; }
        .package-sidebar-holdings { display: flex; flex-direction: column; gap: 8px; }
        .package-sidebar-holding { display: flex; align-items: center; gap: 12px; padding: 12px; background: #FBFBFA; border-radius: 8px; }
        .package-sidebar-holding-flag { width: 32px; height: 32px; border-radius: 50%; overflow: hidden; flex-shrink: 0; display: flex; align-items: center; justify-content: center; }
        .package-sidebar-holding-info { flex: 1; min-width: 0; }
        .package-sidebar-holding-name { font-size: 14px; font-weight: 500; color: #1a1a1a; margin-bottom: 2px; }
        .package-sidebar-holding-detail { font-size: 12px; color: #666; }
        .package-sidebar-holding-value { font-size: 14px; font-weight: 600; color: #1a1a1a; text-align: right; flex-shrink: 0; }
        .left-panel { width: 300px; background: #ffffff; border-right: 1px solid #EDEDED; display: flex; flex-direction: column; overflow: visible; position: relative; z-index: 20; }
        .panel-header { padding: 38px 32px; border-bottom: 1px solid #EDEDED; overflow: visible; position: relative; }
        .panel-title { font-size: 18px; font-weight: 600; color: #1a1a1a; }
        .nav-section { padding: 16px; border-bottom: 1px solid #EDEDED; }
        .nav-item { display: flex; align-items: center; gap: 12px; padding: 8px 16px; border-radius: 8px; cursor: pointer; transition: background 0.2s; margin-bottom: 4px; }
        .nav-item:hover { background: #F5F4F2; }
        .nav-icon { width: 40px; height: 40px; border-radius: 50%; display: flex; align-items: center; justify-content: center; font-weight: 600; font-size: 14px; }
        .nav-icon.blue { background: #e3f2fd; color: #1976d2; }
        .nav-icon.purple { background: #f3e5f5; color: #7b1fa2; }
        .nav-icon.orange { background: #fff3e0; color: #f57c00; }
        .nav-text { font-size: 14px; color: #1a1a1a; }
        .section-header { padding: 12px 16px; margin: 16px 16px 0 16px; display: flex; align-items: center; justify-content: space-between; cursor: pointer; transition: background 0.2s; border-radius: 6px; }
        .section-header:hover { background: #F5F4F2; }
        .section-title { font-size: 13px; color: #666; font-weight: 500; }
        .section-arrow { transition: transform 0.3s ease; color: #999; display: flex; align-items: center; }
        .section-arrow.collapsed { transform: rotate(-180deg); }
        .section-list { padding: 0 16px; max-height: 500px; overflow: hidden; transition: max-height 0.3s ease, opacity 0.3s ease; opacity: 1; }
        .section-list.collapsed { max-height: 0; opacity: 0; }
        .section-list-item { padding: 8px 16px; font-size: 13px; color: #1a1a1a; cursor: pointer; border-radius: 6px; margin-bottom: 2px; white-space: nowrap; overflow: hidden; text-overflow: ellipsis; }
        .section-list-item:hover { background: #F5F4F2; }
        .ai-fund-box { margin: 16px; padding: 20px; background: #ffffff; border-radius: 12px; border: 1px solid #EDEDED; margin-top: auto; }
        .ai-fund-title { font-size: 14px; font-weight: 600; margin-bottom: 8px; }
        .ai-fund-description { font-size: 13px; color: #666; line-height: 1.5; margin-bottom: 16px; }
        .ai-fund-button { width: 100%; padding: 0; height: 44px; background: #F5F4F2; border: none; border-radius: 8px; font-size: 13px; font-weight: 500; cursor: pointer; transition: all 0.2s; }
        .ai-fund-button:hover { background: #E9E7E2; }
        .main-content { flex: 1; display: flex; flex-direction: column; overflow: hidden; }
        .top-bar { padding: 24px; border-bottom: 1px solid #EDEDED; display: flex; align-items: center; justify-content: space-between; }
        .search-bar { flex: 1; max-width: 100%; position: relative; z-index: 100; width: 100%; }
        .search-input { width: 100%; padding: 0 120px 0 40px; border: 1px solid #EDEDED; border-radius: 12px; font-size: 14px; outline: none; transition: border 0.2s; height: 50px; font-family: 'Helvetica Neue', Helvetica, Arial, sans-serif; }
        .search-input::placeholder { color: #999999; }
        .search-input:focus { border-color: #999; }
        .search-icon { position: absolute; left: 12px; top: 50%; transform: translateY(-50%); color: #999; }
        .top-bar-actions { display: flex; gap: 8px; }
        .top-bar-button { padding: 0 16px; border: none; background: #F5F4F2; border-radius: 12px; font-size: 14px; cursor: pointer; display: flex; align-items: center; gap: 8px; transition: all 0.2s; height: 50px; font-family: 'Helvetica Neue', Helvetica, Arial, sans-serif; }
        .top-bar-button:hover { background: #E9E7E2; }
        .chat-area { flex: 1; display: flex; flex-direction: column; align-items: center; justify-content: center; padding: 32px; overflow-y: auto; background: #FBFBFA; margin: 0; border-radius: 0; position: relative; }
        .welcome-message { font-size: 48px; font-weight: 400; margin-bottom: 48px; margin-top: 40px; text-align: center; }
        .suggestions-container { display: flex; gap: 12px; max-width: 768px; width: 768px; margin-bottom: 24px; }
        .suggestion-card { flex: 1; padding: 20px; background: #F5F4F2; border: none; border-radius: 12px; cursor: pointer; font-size: 14px; color: #1a1a1a; min-height: 100px; display: flex; flex-direction: column; justify-content: space-between; position: relative; }
        .suggestion-card:hover { background: #E9E7E2; }
        .suggestion-icon { width: 24px; height: 24px; margin-top: 24px; color: #7E7E7E; }
        .help-text { text-align: left; color: #000000; font-size: 20px; margin-bottom: 16px; width: calc(100% - 0px); max-width: 768px; align-self: flex-start; }
        .input-container { position: relative; padding: 0; background: transparent; border-top: none; max-width: 768px; margin: 0 auto 32px; width: 768px; }
        .chat-input-wrapper { max-width: 768px; margin: 0 auto; position: relative; width: 100%; }
        .chat-input { width: 100%; padding: 16px 16px 60px 16px; border: 1px solid #EDEDED; border-radius: 16px; font-size: 16px; outline: none; transition: all 0.2s; resize: none; line-height: 1.5; font-family: 'Helvetica Neue', Helvetica, Arial, sans-serif; background: #FFFFFF; box-shadow: 0 0 20px 0 rgba(0, 0, 0, 0.05); min-height: calc(1.5em * 2 + 28px + 60px); max-height: 150px; }
        .chat-input::placeholder { color: #999999; }
        .chat-input:focus { border-color: #999; box-shadow: 0 0 0 3px rgba(0,0,0,0.05); }
        .input-actions { position: absolute; left: 16px; right: 16px; bottom: 20px; display: flex; gap: 8px; justify-content: space-between; }
        .input-action-btn { width: 50px; height: 50px; border: none; background: #1a1a1a; color: white; border-radius: 12px; cursor: pointer; display: flex; align-items: center; justify-content: center; }
        .input-action-btn:hover { background: #333; }
        .input-action-btn:disabled { opacity: 0.4; cursor: not-allowed; }
        .input-action-btn:disabled:hover { background: #1a1a1a; }
        .input-action-btn.voice { background: #ffffff; color: #1a1a1a; border: 1px solid #F1F0F0; }
        .input-action-btn.voice:hover { background: #E9E7E3; }
        .input-action-btn.attach { background: #ffffff; color: #1a1a1a; border: 1px solid #F1F0F0; width: auto; padding: 0 16px; gap: 8px; font-size: 14px; font-weight: 400; font-family: 'Helvetica Neue', Helvetica, Arial, sans-serif; }
        .input-action-btn.attach:hover { background: #E9E7E3; }
        .welcome-content { display: flex; flex-direction: column; align-items: center; }
        .welcome-content.hidden { display: none; }
        .home-view { display: none; width: 100%; padding: 48px; overflow-y: auto; }
        .home-view.active { display: block; }
        .chat-messages { width: calc(100% - 48px); max-width: 768px; display: none; flex-direction: column; gap: 24px; margin-bottom: 32px; }
        .chat-messages.active { display: flex; }
        .chat-message { display: flex; gap: 12px; align-items: flex-start; opacity: 0; animation: messageAppear 0.4s ease forwards; }
        .chat-message.user { justify-content: flex-end; }
        @keyframes messageAppear { 
            from { 
                opacity: 0; 
                transform: translateY(10px); 
            } 
            to { 
                opacity: 1; 
                transform: translateY(0); 
            } 
        }
        .message-content { max-width: 70%; padding: 16px 20px; border-radius: 16px; background: transparent; font-size: 15px; line-height: 1.6; color: #000000; }
        .chat-message.user .message-content { background: #ffffff; color: #000000; border-radius: 16px 16px 0 16px; }
        .typing-indicator { display: flex; gap: 4px; padding: 16px 20px; }
        .typing-star { width: 20px; height: 20px; animation: sparkle 2s ease-in-out infinite; }
        @keyframes sparkle {
            0% { opacity: 0.4; transform: scale(0.9) rotate(0deg); }
            25% { opacity: 1; transform: scale(1.1) rotate(90deg); }
            50% { opacity: 0.5; transform: scale(0.95) rotate(180deg); }
            75% { opacity: 1; transform: scale(1.15) rotate(270deg); }
            100% { opacity: 0.4; transform: scale(0.9) rotate(360deg); }
        }
        .chat-area.chat-active { justify-content: flex-start; padding-bottom: 200px; position: relative; }
        .chat-area.chat-active .input-container.sticky { position: fixed; left: 50%; margin-left: calc((80px + 300px) / 2); transform: translateX(-50%); bottom: 32px; transition: margin-left 0.3s ease; }
        .left-sidebar:not(.collapsed) ~ .left-panel ~ .main-content .chat-area.chat-active .input-container.sticky { margin-left: calc((300px + 300px) / 2); }
        .chat-area.chat-active::after { content: ''; position: fixed; bottom: 0; left: 0; right: 0; height: 200px; background: linear-gradient(to bottom, rgba(251, 251, 250, 0) 0%, rgba(251, 251, 250, 1) 32px, rgba(251, 251, 250, 1) 100%); pointer-events: none; z-index: 5; }
        .input-container.sticky { position: absolute; bottom: 24px; left: 50%; transform: translateX(-50%); width: 768px; max-width: 768px; margin: 0; z-index: 10; background: transparent; padding-top: 40px; padding-bottom: 0; margin-bottom: 0; }
        .search-overlay { position: fixed; top: 0; left: 0; right: 0; bottom: 0; background: rgba(0, 0, 0, 0.2); z-index: 90; display: none; }
        .search-overlay.active { display: block; }
        .search-dropdown { position: absolute; top: calc(100% + 8px); left: 0; right: 0; background: #FFFFFF; border: none; border-radius: 12px; box-shadow: 0 4px 24px rgba(0, 0, 0, 0.1); padding: 16px 0; display: none; max-height: 400px; overflow-y: auto; width: 600px; z-index: 102; }
        .search-dropdown.active { display: block; }
        .dropdown-header { display: grid; grid-template-columns: 2fr 1fr 1fr; padding: 8px 24px; font-size: 13px; color: #999; font-weight: 500; border-bottom: 1px solid #EDEDED; margin-bottom: 8px; }
        .dropdown-item { display: grid; grid-template-columns: 2fr 1fr 1fr; padding: 12px 24px; cursor: pointer; transition: background 0.2s; align-items: center; }
        .dropdown-item:hover { background: #F5F4F2; }
        .dropdown-name { display: flex; align-items: center; gap: 12px; font-size: 16px; font-weight: 500; color: #1a1a1a; }
        .flag-icon { width: 24px; height: 24px; border-radius: 50%; overflow: hidden; flex-shrink: 0; }
        .dropdown-fee { font-size: 16px; color: #999; }
        .dropdown-return { font-size: 16px; font-weight: 500; text-align: right; }
        .dropdown-return.positive { color: #00A86B; }
        .dropdown-return.negative { color: #E74C3C; }
        .ai-suggestions { display: grid; grid-template-columns: repeat(2, 1fr); gap: 12px; margin-top: 20px; width: 100%; }
        .ai-suggestion-card { padding: 16px; background: #F5F4F2; border: none; border-radius: 12px; cursor: pointer; transition: all 0.2s; font-size: 13px; color: #1a1a1a; text-align: left; }
        .ai-suggestion-card:hover { background: #E9E7E2; transform: translateY(-1px); }
        .savings-packages { display: flex; flex-direction: column; gap: 16px; margin-top: 24px; }
        .funds-list { display: flex; flex-direction: column; gap: 0; margin-top: 24px; border: 1px solid #EDEDED; border-radius: 12px; overflow: hidden; background: #FFFFFF; }
        .fund-item { display: grid; grid-template-columns: 40px 2fr 1fr 1fr; padding: 16px 20px; align-items: center; gap: 16px; border-bottom: 1px solid #EDEDED; transition: background 0.2s; }
        .fund-item:last-child { border-bottom: none; }
        .fund-item:hover { background: #FBFBFA; }
        .fund-rank { font-size: 14px; font-weight: 600; color: #999; }
        .fund-name { display: flex; align-items: center; gap: 12px; }
        .fund-flag { width: 24px; height: 24px; border-radius: 50%; overflow: hidden; flex-shrink: 0; }
        .fund-title { font-size: 15px; font-weight: 500; color: #1a1a1a; }
        .fund-fee { font-size: 14px; color: #666; }
        .fund-return { font-size: 15px; font-weight: 600; text-align: right; }
        .fund-return.positive { color: #00A86B; }
        .fund-return.negative { color: #E74C3C; }
        .package-card { flex: 1; background: #FFFFFF; border: 1px solid #EDEDED; border-radius: 16px; padding: 20px; display: flex; flex-direction: column; gap: 16px; transition: all 0.2s; }
        .package-card:hover { border-color: #EDEDED; }
        .package-header { display: flex; flex-direction: column; gap: 6px; }
        .package-title { font-size: 18px; font-weight: 600; color: #1a1a1a; }
        .package-subtitle { font-size: 13px; color: #666; line-height: 1.4; }
        .package-allocation { display: flex; align-items: center; gap: 12px; padding: 12px 0; border-top: 1px solid #EDEDED; border-bottom: 1px solid #EDEDED; }
        .package-pie { width: 50px; height: 50px; border-radius: 50%; flex-shrink: 0; }
        .allocation-breakdown { display: grid; grid-template-columns: 1fr 1fr 1fr; gap: 12px; flex: 1; }
        .allocation-item { display: flex; flex-direction: column; gap: 4px; font-size: 13px; }
        .allocation-label { color: #666; font-size: 11px; text-transform: uppercase; letter-spacing: 0.5px; }
        .allocation-value { font-weight: 600; color: #1a1a1a; font-size: 16px; }
        .package-details { display: grid; grid-template-columns: 1fr 1fr 1fr; gap: 16px; }
        .detail-row { display: flex; flex-direction: column; gap: 3px; }
        .detail-row.full-width { grid-column: 1 / -1; }
        .detail-label { font-size: 11px; color: #666; text-transform: uppercase; letter-spacing: 0.5px; }
        .detail-value { font-size: 18px; font-weight: 600; color: #1a1a1a; }
        .detail-subvalue { font-size: 12px; color: #666; margin-top: 8px; }
        .toggle-expanded { display: none; }
        .ai-carousel-overlay { position: fixed; top: 0; left: 0; right: 0; bottom: 0; background: rgba(0, 0, 0, 0.2); z-index: 400; display: none; align-items: center; justify-content: center; opacity: 0; transition: opacity 0.3s ease; }
        .ai-carousel-overlay.active { display: flex; opacity: 1; }
        .ai-carousel-popup { background: #ffffff; border-radius: 24px; width: 90%; max-width: 600px; box-shadow: 0 20px 60px rgba(0, 0, 0, 0.15); position: relative; z-index: 401; overflow: hidden; transform: scale(0.9); opacity: 0; transition: transform 0.3s ease, opacity 0.3s ease; }
        .ai-carousel-overlay.active .ai-carousel-popup { transform: scale(1); opacity: 1; }
        .ai-carousel-close { position: absolute; top: 16px; right: 16px; width: 32px; height: 32px; border: none; background: rgba(255, 255, 255, 0.9); border-radius: 50%; cursor: pointer; display: flex; align-items: center; justify-content: center; color: #1a1a1a; transition: background 0.2s; z-index: 10; }
        .ai-carousel-close:hover { background: #ffffff; }
        .ai-carousel-container { position: relative; width: 100%; }
        .ai-carousel-slides { display: flex; transition: transform 0.4s ease; }
        .ai-carousel-slide { min-width: 100%; display: flex; flex-direction: column; }
        .ai-carousel-image { width: 100%; height: 300px; object-fit: cover; background: #F5F4F2; display: flex; align-items: center; justify-content: center; font-size: 48px; }
        .ai-carousel-image.slide1 { background: linear-gradient(135deg, #667eea 0%, #764ba2 100%); }
        .ai-carousel-image.slide2 { background: linear-gradient(135deg, #f093fb 0%, #f5576c 100%); }
        .ai-carousel-image.slide3 { background: linear-gradient(135deg, #4facfe 0%, #00f2fe 100%); }
        .ai-carousel-content { padding: 40px; }
        .ai-carousel-title { font-size: 24px; font-weight: 600; color: #1a1a1a; margin-bottom: 16px; }
        .ai-carousel-text { font-size: 15px; color: #666; line-height: 1.6; margin-bottom: 32px; }
        .ai-carousel-nav { display: flex; gap: 12px; justify-content: center; margin-bottom: 24px; }
        .ai-carousel-nav-btn { padding: 12px 24px; border: none; background: #F5F4F2; color: #1a1a1a; border-radius: 12px; font-size: 14px; font-weight: 500; cursor: pointer; transition: background 0.2s; font-family: 'Helvetica Neue', Helvetica, Arial, sans-serif; }
        .ai-carousel-nav-btn:hover { background: #E9E7E2; }
        .ai-carousel-nav-btn.primary { background: #1a1a1a; color: #ffffff; }
        .ai-carousel-nav-btn.primary:hover { background: #333; }
        .ai-carousel-nav-btn:disabled { opacity: 0.4; cursor: not-allowed; }
        .ai-carousel-nav-btn:disabled:hover { background: #F5F4F2; }
        .ai-carousel-bullets { display: flex; gap: 8px; justify-content: center; padding: 0 40px 32px; }
        .ai-carousel-bullet { width: 8px; height: 8px; border-radius: 50%; background: #EDEDED; cursor: pointer; transition: all 0.3s; }
        .ai-carousel-bullet.active { background: #1a1a1a; width: 24px; border-radius: 4px; }
    </style>
</head>
<body>
    <div class="left-sidebar collapsed">
        <div class="sidebar-logo">
            <span class="sidebar-logo-text">tova</span>
            <span class="sidebar-badge">Market</span>
            <span class="sidebar-logo-collapsed">t</span>
        </div>
        
        <div class="sidebar-nav">
            <div class="nav-item-sidebar active">
                <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                    <path d="M12 2v20M2 12h20M5.64 5.64l12.72 12.72M5.64 18.36l12.72-12.72"></path>
                    <circle cx="12" cy="12" r="3"></circle>
                </svg>
                <span>Investera med Tova AI</span>
                <span class="nav-tooltip">Investera med Tova AI</span>
            </div>
            
            <div class="sidebar-divider-line"></div>
            
            <div class="nav-item-sidebar">
                <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                    <rect x="2" y="4" width="20" height="16" rx="2"></rect>
                    <path d="M7 15h0M2 9.5h20"></path>
                </svg>
                <span>Min ekonomi</span>
                <span class="nav-tooltip">Min ekonomi</span>
            </div>
            
            <div class="nav-item-sidebar">
                <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                    <circle cx="11" cy="11" r="8"></circle>
                    <path d="m21 21-4.35-4.35"></path>
                </svg>
                <span>Upptäck</span>
                <span class="nav-tooltip">Upptäck</span>
            </div>
            
            <div class="nav-item-sidebar">
                <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                    <circle cx="12" cy="8" r="7"></circle>
                    <polyline points="8.21 13.89 7 23 12 20 17 23 15.79 13.88"></polyline>
                </svg>
                <span>Tjäna</span>
                <span class="nav-tooltip">Tjäna</span>
            </div>
            
            <div class="nav-item-sidebar">
                <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                    <path d="M3 3v18h18"></path>
                    <path d="M7 16v-6M12 16V8M17 16v-4"></path>
                    <path d="M14 6l4 4"></path>
                    <path d="M14 10l4-4"></path>
                </svg>
                <span>Marknad</span>
                <span class="nav-tooltip">Marknad</span>
            </div>
            
            <div class="nav-item-sidebar">
                <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                    <circle cx="12" cy="12" r="10"></circle>
                    <path d="M12 6v6l4 2"></path>
                    <path d="M16.24 7.76A6 6 0 0 1 19 13.28"></path>
                </svg>
                <span>Valutakurser</span>
                <span class="nav-tooltip">Valutakurser</span>
            </div>
            
            <div class="nav-item-sidebar">
                <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                    <path d="M4 22h16a2 2 0 0 0 2-2V4a2 2 0 0 0-2-2H8a2 2 0 0 0-2 2v16a2 2 0 0 1-2 2Zm0 0a2 2 0 0 1-2-2v-9c0-1.1.9-2 2-2h2"></path>
                    <path d="M18 14h-8"></path>
                    <path d="M15 18h-5"></path>
                    <path d="M10 6h8v4h-8V6Z"></path>
                </svg>
                <span>Marknadsnytt</span>
                <span class="nav-tooltip">Marknadsnytt</span>
            </div>
            
            <div class="sidebar-divider-line"></div>
            
            <div class="nav-item-sidebar">
                <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                    <line x1="12" y1="5" x2="12" y2="19"></line>
                    <line x1="5" y1="12" x2="19" y2="12"></line>
                </svg>
                <span>Öppna ISK</span>
                <span class="nav-tooltip">Öppna ISK</span>
            </div>
        </div>
        
        <div class="sidebar-footer">
            <div class="nav-item-sidebar" id="minimizeBtn">
                <svg class="minimize-icon-expanded" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                    <path d="M15 3h4a2 2 0 0 1 2 2v14a2 2 0 0 1-2 2h-4"></path>
                    <polyline points="8 7 3 12 8 17"></polyline>
                    <line x1="3" y1="12" x2="15" y2="12"></line>
                </svg>
                <svg class="minimize-icon-collapsed" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                    <rect x="3" y="3" width="18" height="18" rx="2" ry="2"></rect>
                    <line x1="9" y1="3" x2="9" y2="21"></line>
                </svg>
                <span>Minimera meny</span>
                <span class="nav-tooltip">Minimera meny</span>
            </div>
            
            <div class="sidebar-divider-line"></div>
            
            <div class="sidebar-user">
                <div class="sidebar-user-avatar">AV</div>
                <div class="sidebar-user-info">
                    <div class="sidebar-user-name">Anna Vallin</div>
                    <div class="sidebar-user-plan">Tova Premium</div>
                </div>
                <svg class="sidebar-user-dropdown" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                    <polyline points="6 9 12 15 18 9"></polyline>
                </svg>
            </div>
        </div>
    </div>

    <div class="left-panel">
        <div class="panel-header">
            <h1 class="panel-title">Investera med Tova AI</h1>
        </div>
        <div class="nav-section">
            <div class="nav-item"><div class="nav-icon blue">F</div><div class="nav-text">Investera i fonder</div></div>
            <div class="nav-item"><div class="nav-icon purple">P</div><div class="nav-text">Tova-podden</div></div>
            <div class="nav-item"><div class="nav-icon orange">A</div><div class="nav-text">Köp aktier</div></div>
        </div>
        <div class="section-header" data-section="today">
            <span class="section-title">Idag</span>
            <span class="section-arrow"><svg width="16" height="16" viewBox="0 0 16 16" fill="none" stroke="currentColor" stroke-width="2"><polyline points="4 6 8 10 12 6"></polyline></svg></span>
        </div>
        <div class="section-list" data-section-content="today"></div>
        <div class="section-header" data-section="yesterday">
            <span class="section-title">Igår</span>
            <span class="section-arrow"><svg width="16" height="16" viewBox="0 0 16 16" fill="none" stroke="currentColor" stroke-width="2"><polyline points="4 6 8 10 12 6"></polyline></svg></span>
        </div>
        <div class="section-list" data-section-content="yesterday">
            <div class="section-list-item">Jag vill starta ett månadssparande</div>
            <div class="section-list-item">Vilka är Tovas topp 10 köpta aktier...</div>
            <div class="section-list-item">Hur fungerar Tova AI</div>
            <div class="section-list-item">Vilka fonder ska jag välja för lång spar...</div>
        </div>
        <div class="ai-fund-box">
            <div class="ai-fund-title">Vår AI-drivna fondutforskare</div>
            <div class="ai-fund-description">Hitta fonder som passar dina sparmål med hjälp av vår nya AI-drivna fondutforskare.</div>
            <button class="ai-fund-button">Så här funkar det</button>
        </div>
    </div>

    <div class="main-content">
        <div class="top-bar">
            <div class="search-bar" style="width: 600px; max-width: 600px;">
                <span class="search-icon"><svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><circle cx="11" cy="11" r="8"></circle><path d="m21 21-4.35-4.35"></path></svg></span>
                <input type="text" class="search-input" id="searchInput" placeholder="Sök på värdepapper, marknader och mer">
                <span style="position: absolute; right: 12px; top: 50%; transform: translateY(-50%); font-size: 13px; color: #000000; background: #F5F4F2; padding: 6px 8px; border-radius: 8px;" id="cmdKBadge">⌘ + K</span>
                <button style="position: absolute; right: 12px; top: 50%; transform: translateY(-50%); width: 32px; height: 32px; border: none; background: #F5F4F2; border-radius: 8px; cursor: pointer; display: none; align-items: center; justify-content: center; color: #000000; transition: background-color 0.2s;" id="clearSearchBtn" onmouseover="this.style.backgroundColor='#E9E7E2'" onmouseout="this.style.backgroundColor='#F5F4F2'">
                    <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><line x1="18" y1="6" x2="6" y2="18"></line><line x1="6" y1="6" x2="18" y2="18"></line></svg>
                </button>
                <div class="search-dropdown" id="searchDropdown"></div>
            </div>
            <div class="top-bar-actions">
                <button class="top-bar-button">
                    <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                        <line x1="12" y1="5" x2="12" y2="19"></line>
                        <line x1="5" y1="12" x2="19" y2="12"></line>
                    </svg>
                    Anslut konto
                </button>
                <button class="top-bar-button"><span style="display: inline-block; transform: rotate(90deg);">⇄</span> Överföring</button>
            </div>
        </div>

        <div class="search-overlay" id="searchOverlay"></div>

        <div class="chat-area">
            <div class="home-view" id="homeView">
                <div style="text-align: center; padding: 100px 0;">
                    <h2 style="font-size: 24px; color: #666;">Home View</h2>
                    <p style="color: #999; margin-top: 16px;">Click the home icon in the sidebar to see portfolio content</p>
                </div>
            </div>

            <div class="welcome-content" id="welcomeContent">
                <h2 class="welcome-message">Hur är din dag, Anna?</h2>
                <div class="input-container">
                    <div class="chat-input-wrapper">
                        <textarea class="chat-input" id="chatInput" placeholder="Berätta hur jag kan hjälpa dig" rows="3"></textarea>
                        <div class="input-actions">
                            <button class="input-action-btn attach">
                                <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><line x1="12" y1="5" x2="12" y2="19"></line><line x1="5" y1="12" x2="19" y2="12"></line></svg>
                                <span>Bifoga</span>
                            </button>
                            <div style="display: flex; gap: 8px;">
                                <button class="input-action-btn voice"><svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M12 1a3 3 0 0 0-3 3v8a3 3 0 0 0 6 0V4a3 3 0 0 0-3-3z"></path><path d="M19 10v2a7 7 0 0 1-14 0v-2"></path><line x1="12" y1="19" x2="12" y2="23"></line><line x1="8" y1="23" x2="16" y2="23"></line></svg></button>
                                <button class="input-action-btn" id="sendButton" disabled><svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><line x1="12" y1="19" x2="12" y2="5"></line><polyline points="5 12 12 5 19 12"></polyline></svg></button>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="help-text">Om du behöver lite hjälp på traven</div>
                <div class="suggestions-container">
                    <div class="suggestion-card" data-message="Jag vill starta ett månadssparande">
                        <span>Jag vill starta ett månadssparande</span>
                        <svg class="suggestion-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M19 5c-1.5 0-2.8 1.4-3 2-3.5-1.5-11-.3-11 5 0 1.8 0 3 2 4.5V20h4v-2h3v2h4v-4c1-.5 1.7-1 2-2h2v-4h-2c0-1-.5-1.5-1-2h0V5z"></path><path d="M2 9v1c0 1.1.9 2 2 2h1"></path><path d="M16 11h0"></path></svg>
                    </div>
                    <div class="suggestion-card" data-message="Visa mig Tovas 10 populäraste fonder 2025">
                        <span>Visa mig Tovas 10 populäraste fonder 2025</span>
                        <svg class="suggestion-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><circle cx="12" cy="8" r="7"></circle><polyline points="8.21 13.89 7 23 12 20 17 23 15.79 13.88"></polyline></svg>
                    </div>
                    <div class="suggestion-card" data-message="Hur mycket ska man spara per månad?">
                        <span>Hur mycket ska man spara per månad?</span>
                        <svg class="suggestion-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><circle cx="12" cy="12" r="10"></circle><line x1="12" y1="16" x2="12" y2="12"></line><line x1="12" y1="8" x2="12.01" y2="8"></line></svg>
                    </div>
                    <div class="suggestion-card" data-message="Visa mig fonder med bäst avkastning på 3 år">
                        <span>Visa mig fonder med bäst avkastning på 3 år</span>
                        <svg class="suggestion-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><polyline points="23 6 13.5 15.5 8.5 10.5 1 18"></polyline><polyline points="17 6 23 6 23 12"></polyline></svg>
                    </div>
                </div>
            </div>

            <div class="chat-messages" id="chatMessages"></div>

            <div class="input-container sticky" id="chatInputContainer" style="display: none; width: 768px; max-width: 768px;">
                <div class="chat-input-wrapper" style="width: 768px; max-width: 768px;">
                    <textarea class="chat-input" id="chatInputActive" placeholder="Berätta hur jag kan hjälpa dig" rows="3"></textarea>
                    <div class="input-actions">
                        <button class="input-action-btn attach">
                            <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><line x1="12" y1="5" x2="12" y2="19"></line><line x1="5" y1="12" x2="19" y2="12"></line></svg>
                            <span>Bifoga</span>
                        </button>
                        <div style="display: flex; gap: 8px;">
                            <button class="input-action-btn voice"><svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M12 1a3 3 0 0 0-3 3v8a3 3 0 0 0 6 0V4a3 3 0 0 0-3-3z"></path><path d="M19 10v2a7 7 0 0 1-14 0v-2"></path><line x1="12" y1="19" x2="12" y2="23"></line><line x1="8" y1="23" x2="16" y2="23"></line></svg></button>
                            <button class="input-action-btn" id="sendButtonActive" disabled><svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><line x1="12" y1="19" x2="12" y2="5"></line><polyline points="5 12 12 5 19 12"></polyline></svg></button>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <div class="transfer-overlay" id="transferOverlay">
        <div class="transfer-popup">
            <div class="transfer-popup-header">
                <h2 class="transfer-popup-title">Överföring</h2>
                <button class="transfer-popup-close" id="closeTransferPopup">
                    <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                        <line x1="18" y1="6" x2="6" y2="18"></line>
                        <line x1="6" y1="6" x2="18" y2="18"></line>
                    </svg>
                </button>
            </div>
            
            <div class="transfer-tabs">
                <button class="transfer-tab active" data-tab="topup">Sätt in</button>
                <button class="transfer-tab" data-tab="payout">Ta ut</button>
            </div>
            
            <div class="transfer-content active" id="topupContent">
                <div class="transfer-field">
                    <label class="transfer-label">Välj bankkonto</label>
                    <select class="transfer-select">
                        <option>Swedbank **** 1234</option>
                        <option>Nordea **** 5678</option>
                        <option>SEB **** 9012</option>
                    </select>
                </div>
                
                <div class="transfer-field">
                    <label class="transfer-label">Belopp</label>
                    <input type="number" class="transfer-input" id="topupAmount" placeholder="0 SEK">
                </div>
                
                <button class="transfer-button" id="topupButton" disabled>Sätt in pengar</button>
            </div>
            
            <div class="transfer-content" id="payoutContent">
                <div class="transfer-field">
                    <label class="transfer-label">Välj bankkonto</label>
                    <select class="transfer-select">
                        <option>Swedbank **** 1234</option>
                        <option>Nordea **** 5678</option>
                        <option>SEB **** 9012</option>
                    </select>
                </div>
                
                <div class="transfer-field">
                    <label class="transfer-label">Belopp</label>
                    <input type="number" class="transfer-input" id="payoutAmount" placeholder="0 SEK">
                </div>
                
                <div class="transfer-info">
                    Tillgängligt saldo: 3 751,00 SEK<br>
                    Pengar överförs till ditt bankkonto inom 1-2 bankdagar.
                </div>
                
                <button class="transfer-button" id="payoutButton" disabled>Ta ut pengar</button>
            </div>
            
            <div class="transfer-content transfer-success-view" id="transferSuccessView">
                <div class="transfer-success-icon">
                    <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-linecap="round" stroke-linejoin="round">
                        <polyline points="20 6 9 17 4 12"></polyline>
                    </svg>
                </div>
                <h3 class="transfer-success-title" id="transferSuccessTitle">Insättning genomförd!</h3>
                <p class="transfer-success-amount" id="transferSuccessAmount">5 000 SEK</p>
                <p class="transfer-success-text" id="transferSuccessText">Pengarna har satts in på ditt Tova-konto och är tillgängliga direkt.</p>
                <p class="transfer-success-info" id="transferSuccessInfo"></p>
                <button class="transfer-success-button" id="transferSuccessDone">Klar</button>
            </div>
        </div>
    </div>

    <div class="bank-overlay" id="bankOverlay">
        <div class="bank-popup">
            <div class="bank-popup-header">
                <div>
                    <h2 class="bank-popup-title" id="bankPopupTitle">Anslut konto</h2>
                </div>
                <button class="bank-popup-close" id="closeBankPopup">
                    <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                        <line x1="18" y1="6" x2="6" y2="18"></line>
                        <line x1="6" y1="6" x2="18" y2="18"></line>
                    </svg>
                </button>
            </div>
            
            <div class="bank-steps-container">
                <div class="bank-step active" id="bankSelectStep">
                <p class="bank-popup-subtitle">Anslut ditt bankkonto för att enkelt sätta in och ta ut pengar från ditt Tova-konto.</p>
                
                <div class="bank-list">
                    <div class="bank-item" data-bank="swedbank" onclick="window.updateBankLogos('SWED', 'swedbank', 'Swedbank')">
                        <div class="bank-logo swedbank">SWED</div>
                        <div class="bank-name">Swedbank</div>
                    </div>
                    <div class="bank-item" data-bank="nordea" onclick="window.updateBankLogos('NDA', 'nordea', 'Nordea')">
                        <div class="bank-logo nordea">NDA</div>
                        <div class="bank-name">Nordea</div>
                    </div>
                    <div class="bank-item" data-bank="seb" onclick="window.updateBankLogos('SEB', 'seb', 'SEB')">
                        <div class="bank-logo seb">SEB</div>
                        <div class="bank-name">SEB</div>
                    </div>
                    <div class="bank-item" data-bank="handelsbanken" onclick="window.updateBankLogos('HB', 'handelsbanken', 'Handelsbanken')">
                        <div class="bank-logo handelsbanken">HB</div>
                        <div class="bank-name">Handelsbanken</div>
                    </div>
                    <div class="bank-item" data-bank="skandiabanken" onclick="window.updateBankLogos('SB', 'skandiabanken', 'Skandiabanken')">
                        <div class="bank-logo skandiabanken">SB</div>
                        <div class="bank-name">Skandiabanken</div>
                    </div>
                    <div class="bank-item" data-bank="avanza" onclick="window.updateBankLogos('AV', 'avanza', 'Avanza')">
                        <div class="bank-logo avanza">AV</div>
                        <div class="bank-name">Avanza</div>
                    </div>
                    <div class="bank-item" data-bank="lansforsakringar" onclick="window.updateBankLogos('LF', 'lansforsakringar', 'Länsförsäkringar')">
                        <div class="bank-logo lansforsakringar">LF</div>
                        <div class="bank-name">Länsförsäkringar</div>
                    </div>
                    <div class="bank-item" data-bank="ica" onclick="window.updateBankLogos('ICA', 'ica', 'ICA Banken')">
                        <div class="bank-logo ica">ICA</div>
                        <div class="bank-name">ICA Banken</div>
                    </div>
                    <div class="bank-item" data-bank="other" onclick="window.updateBankLogos('+', 'other', 'Annan bank')">
                        <div class="bank-logo other">+</div>
                        <div class="bank-name">Annan bank</div>
                    </div>
                </div>
            </div>
            
            <div class="bank-step" id="accountSelectStep">
                <div class="bank-back" id="backToBanks">
                    <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                        <line x1="19" y1="12" x2="5" y2="12"></line>
                        <polyline points="12 19 5 12 12 5"></polyline>
                    </svg>
                    <span>Tillbaka</span>
                </div>
                
                <p class="bank-popup-subtitle">Välj vilket konto du vill ansluta till ditt Tova-konto.</p>
                
                <div class="account-list" id="accountList">
                    <div class="account-item" data-account="1">
                        <div class="account-bank-logo" id="accountLogo1">?</div>
                        <div class="account-info">
                            <div class="account-name">Privatkonto</div>
                            <div class="account-number">**** **** **** 1234</div>
                        </div>
                        <div class="account-check"></div>
                    </div>
                    <div class="account-item" data-account="2">
                        <div class="account-bank-logo" id="accountLogo2">?</div>
                        <div class="account-info">
                            <div class="account-name">Sparkonto</div>
                            <div class="account-number">**** **** **** 5678</div>
                        </div>
                        <div class="account-check"></div>
                    </div>
                    <div class="account-item" data-account="3">
                        <div class="account-bank-logo" id="accountLogo3">?</div>
                        <div class="account-info">
                            <div class="account-name">Lönekonto</div>
                            <div class="account-number">**** **** **** 9012</div>
                        </div>
                        <div class="account-check"></div>
                    </div>
                </div>
                
                <button class="bank-connect-button" id="connectBankButton" disabled>Anslut konto</button>
            </div>
            
            <div class="bank-step bank-bankid-view" id="bankBankIdStep">
                <div class="bank-bankid-tabs">
                    <button class="bank-bankid-tab active" id="bankIdMobileTab">Mobilt BankID</button>
                    <button class="bank-bankid-tab" id="bankIdCardTab">BankID på kort</button>
                </div>
                
                <div id="bankIdMobileContent">
                    <p class="bank-bankid-instruction">Starta BankID-appen i din mobil eller surfplatta och tryck på Skanna QR-kod.</p>
                    <div class="bank-bankid-qr" id="bankIdQrContainer">
                        <svg viewBox="0 0 200 200" xmlns="http://www.w3.org/2000/svg">
                            <rect width="200" height="200" fill="white"/>
                            <g fill="black">
                                <rect x="20" y="20" width="20" height="20"/>
                                <rect x="60" y="20" width="20" height="20"/>
                                <rect x="80" y="20" width="20" height="20"/>
                                <rect x="100" y="20" width="20" height="20"/>
                                <rect x="140" y="20" width="20" height="20"/>
                                <rect x="160" y="20" width="20" height="20"/>
                                <rect x="20" y="40" width="20" height="20"/>
                                <rect x="100" y="40" width="20" height="20"/>
                                <rect x="160" y="40" width="20" height="20"/>
                                <rect x="20" y="60" width="20" height="20"/>
                                <rect x="60" y="60" width="20" height="20"/>
                                <rect x="100" y="60" width="20" height="20"/>
                                <rect x="120" y="60" width="20" height="20"/>
                                <rect x="160" y="60" width="20" height="20"/>
                                <rect x="20" y="80" width="20" height="20"/>
                                <rect x="60" y="80" width="20" height="20"/>
                                <rect x="100" y="80" width="20" height="20"/>
                                <rect x="140" y="80" width="20" height="20"/>
                                <rect x="160" y="80" width="20" height="20"/>
                                <rect x="20" y="100" width="20" height="20"/>
                                <rect x="100" y="100" width="20" height="20"/>
                                <rect x="120" y="100" width="20" height="20"/>
                                <rect x="140" y="100" width="20" height="20"/>
                                <rect x="160" y="100" width="20" height="20"/>
                                <rect x="60" y="120" width="20" height="20"/>
                                <rect x="80" y="120" width="20" height="20"/>
                                <rect x="140" y="120" width="20" height="20"/>
                                <rect x="20" y="140" width="20" height="20"/>
                                <rect x="40" y="140" width="20" height="20"/>
                                <rect x="60" y="140" width="20" height="20"/>
                                <rect x="80" y="140" width="20" height="20"/>
                                <rect x="100" y="140" width="20" height="20"/>
                                <rect x="120" y="140" width="20" height="20"/>
                                <rect x="140" y="140" width="20" height="20"/>
                                <rect x="160" y="140" width="20" height="20"/>
                                <rect x="40" y="160" width="20" height="20"/>
                                <rect x="80" y="160" width="20" height="20"/>
                                <rect x="120" y="160" width="20" height="20"/>
                                <rect x="140" y="160" width="20" height="20"/>
                            </g>
                        </svg>
                    </div>
                    
                    <div class="bank-bankid-divider">
                        <div class="bank-bankid-divider-line"></div>
                        <span class="bank-bankid-divider-text">eller</span>
                        <div class="bank-bankid-divider-line"></div>
                    </div>
                    
                    <p class="bank-bankid-alternate">Välj annat sätt att identifiera dig</p>
                </div>
                
                <div class="bank-bankid-card-content" id="bankIdCardContent">
                    <ol>
                        <li>Anslut kortläsaren till datorn och klicka på Logga in.</li>
                        <li>Klicka på Jag identifierar mig i BankID på din dator.</li>
                        <li>Skriv in din PIN i kortläsaren och tryck på Ok.</li>
                    </ol>
                </div>
            </div>
            
            <div class="bank-step bank-success-view" id="bankSuccessStep">
                <div class="bank-success-icon">
                    <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-linecap="round" stroke-linejoin="round">
                        <polyline points="20 6 9 17 4 12"></polyline>
                    </svg>
                </div>
                <h3 class="bank-success-title">Konto anslutet!</h3>
                <p class="bank-success-text" id="bankSuccessText">Du har nu anslutit ditt Privatkonto till Tova. Du kan börja överföra pengar direkt.</p>
                <button class="bank-success-button" id="bankSuccessDone">Klar</button>
            </div>
            </div>
        </div>
    </div>

    <div class="ai-carousel-overlay" id="aiCarouselOverlay">
        <div class="ai-carousel-popup">
            <button class="ai-carousel-close" id="closeAiCarousel">
                <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                    <line x1="18" y1="6" x2="6" y2="18"></line>
                    <line x1="6" y1="6" x2="18" y2="18"></line>
                </svg>
            </button>
            
            <div class="ai-carousel-container">
                <div class="ai-carousel-slides" id="aiCarouselSlides">
                    <div class="ai-carousel-slide">
                        <div class="ai-carousel-image slide1"></div>
                        <div class="ai-carousel-content">
                            <h3 class="ai-carousel-title">Smart AI-analys</h3>
                            <p class="ai-carousel-text">Vår AI analyserar tusentals fonder i realtid och matchar dem mot dina sparmål, risktolerans och tidshorisont. Vi använder avancerad maskininlärning för att hitta de bästa fonderna för just dig.</p>
                            <div class="ai-carousel-nav">
                                <button class="ai-carousel-nav-btn" id="aiCarouselPrev" disabled>Föregående</button>
                                <button class="ai-carousel-nav-btn primary" id="aiCarouselNext">Nästa</button>
                            </div>
                        </div>
                    </div>
                    
                    <div class="ai-carousel-slide">
                        <div class="ai-carousel-image slide2"></div>
                        <div class="ai-carousel-content">
                            <h3 class="ai-carousel-title">Personliga rekommendationer</h3>
                            <p class="ai-carousel-text">Baserat på din ekonomiska situation och dina mål får du skräddarsydda fondförslag. AI:n lär sig av dina val och blir bättre på att rekommendera fonder ju mer du använder tjänsten.</p>
                            <div class="ai-carousel-nav">
                                <button class="ai-carousel-nav-btn" id="aiCarouselPrev2">Föregående</button>
                                <button class="ai-carousel-nav-btn primary" id="aiCarouselNext2">Nästa</button>
                            </div>
                        </div>
                    </div>
                    
                    <div class="ai-carousel-slide">
                        <div class="ai-carousel-image slide3"></div>
                        <div class="ai-carousel-content">
                            <h3 class="ai-carousel-title">Kontinuerlig övervakning</h3>
                            <p class="ai-carousel-text">AI:n arbetar dygnet runt och övervakar dina investeringar. Om marknadsförhållandena ändras eller det finns bättre alternativ får du automatiska notifikationer och förslag på justeringar.</p>
                            <div class="ai-carousel-nav">
                                <button class="ai-carousel-nav-btn" id="aiCarouselPrev3">Föregående</button>
                                <button class="ai-carousel-nav-btn primary" id="aiCarouselDone">Kom igång</button>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
            
            <div class="ai-carousel-bullets">
                <div class="ai-carousel-bullet active" data-slide="0"></div>
                <div class="ai-carousel-bullet" data-slide="1"></div>
                <div class="ai-carousel-bullet" data-slide="2"></div>
            </div>
        </div>
    </div>

    <div class="package-sidebar-overlay" id="packageSidebarOverlay"></div>
    <div class="package-sidebar" id="packageSidebar">
        <div class="package-sidebar-header">
            <div>
                <h2 class="package-sidebar-title" id="packageSidebarTitle">Tova liten</h2>
                <p class="package-sidebar-subtitle" id="packageSidebarSubtitle">Ett sparande som fokuserar på trygghet och stabilitet framför högre risk och potentiell avkastning.</p>
            </div>
            <button class="package-sidebar-close" id="closePackageSidebar">
                <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                    <line x1="18" y1="6" x2="6" y2="18"></line>
                    <line x1="6" y1="6" x2="18" y2="18"></line>
                </svg>
            </button>
        </div>
        
        <div class="package-sidebar-content">
            <div class="package-sidebar-section">
                <h3 class="package-sidebar-section-title">Fördelning</h3>
                <div class="package-sidebar-allocation">
                    <div class="package-sidebar-pie" id="packageSidebarPie" style="background: conic-gradient(#F5F4F2 0% 10%, #181512 10% 100%);"></div>
                    <div class="package-sidebar-breakdown">
                        <div class="package-sidebar-breakdown-item">
                            <span class="package-sidebar-breakdown-label">Aktier</span>
                            <span class="package-sidebar-breakdown-value" id="packageSidebarStocks">10%</span>
                        </div>
                        <div class="package-sidebar-breakdown-item">
                            <span class="package-sidebar-breakdown-label">Ränta</span>
                            <span class="package-sidebar-breakdown-value" id="packageSidebarBonds">90%</span>
                        </div>
                        <div class="package-sidebar-breakdown-item">
                            <span class="package-sidebar-breakdown-label">Övrigt</span>
                            <span class="package-sidebar-breakdown-value" id="packageSidebarOther">0%</span>
                        </div>
                    </div>
                </div>
            </div>
            
            <div class="package-sidebar-section">
                <h3 class="package-sidebar-section-title">Detaljer</h3>
                <div class="package-sidebar-details">
                    <div class="package-sidebar-detail">
                        <div>
                            <div class="package-sidebar-detail-label">Månadssparande</div>
                            <div class="package-sidebar-detail-value" id="packageSidebarMonthly">500 kr</div>
                        </div>
                    </div>
                    <div class="package-sidebar-detail">
                        <div>
                            <div class="package-sidebar-detail-label">Om 5 år</div>
                            <div class="package-sidebar-detail-value" id="packageSidebarFiveYear">32 000 kr</div>
                        </div>
                    </div>
                    <div class="package-sidebar-detail">
                        <div>
                            <div class="package-sidebar-detail-label">Avkastning</div>
                            <div class="package-sidebar-detail-value" id="packageSidebarReturn">+2 000 kr</div>
                        </div>
                    </div>
                </div>
                <p style="font-size: 12px; color: #666; margin-top: 12px;">Om börsen går som förväntat.</p>
            </div>
            
            <div class="package-sidebar-section">
                <h3 class="package-sidebar-section-title">Aktier (3)</h3>
                <div class="package-sidebar-holdings">
                    <div class="package-sidebar-holding">
                        <div class="package-sidebar-holding-flag">
                            <svg width="32" height="32" viewBox="0 0 32 32" fill="none">
                                <circle cx="16" cy="16" r="16" fill="#B22234"/>
                                <rect x="0" y="2.46" width="32" height="2.05" fill="#FFFFFF"/>
                                <rect x="0" y="6.56" width="32" height="2.05" fill="#FFFFFF"/>
                                <rect x="0" y="10.67" width="32" height="2.05" fill="#FFFFFF"/>
                                <rect x="0" y="14.77" width="32" height="2.05" fill="#FFFFFF"/>
                                <rect x="0" y="18.87" width="32" height="2.05" fill="#FFFFFF"/>
                                <rect x="0" y="22.97" width="32" height="2.05" fill="#FFFFFF"/>
                                <rect x="0" y="27.08" width="32" height="2.05" fill="#FFFFFF"/>
                                <rect x="0" y="0" width="12.8" height="14.77" fill="#3C3B6E"/>
                            </svg>
                        </div>
                        <div class="package-sidebar-holding-info">
                            <div class="package-sidebar-holding-name">Apple Inc.</div>
                            <div class="package-sidebar-holding-detail">AAPL · USA</div>
                        </div>
                        <div class="package-sidebar-holding-value">2.5%</div>
                    </div>
                    <div class="package-sidebar-holding">
                        <div class="package-sidebar-holding-flag">
                            <svg width="32" height="32" viewBox="0 0 32 32" fill="none">
                                <circle cx="16" cy="16" r="16" fill="#B22234"/>
                                <rect x="0" y="2.46" width="32" height="2.05" fill="#FFFFFF"/>
                                <rect x="0" y="6.56" width="32" height="2.05" fill="#FFFFFF"/>
                                <rect x="0" y="10.67" width="32" height="2.05" fill="#FFFFFF"/>
                                <rect x="0" y="14.77" width="32" height="2.05" fill="#FFFFFF"/>
                                <rect x="0" y="18.87" width="32" height="2.05" fill="#FFFFFF"/>
                                <rect x="0" y="22.97" width="32" height="2.05" fill="#FFFFFF"/>
                                <rect x="0" y="27.08" width="32" height="2.05" fill="#FFFFFF"/>
                                <rect x="0" y="0" width="12.8" height="14.77" fill="#3C3B6E"/>
                            </svg>
                        </div>
                        <div class="package-sidebar-holding-info">
                            <div class="package-sidebar-holding-name">Microsoft Corp.</div>
                            <div class="package-sidebar-holding-detail">MSFT · USA</div>
                        </div>
                        <div class="package-sidebar-holding-value">2.0%</div>
                    </div>
                    <div class="package-sidebar-holding">
                        <div class="package-sidebar-holding-flag">
                            <svg width="32" height="32" viewBox="0 0 32 32" fill="none">
                                <circle cx="16" cy="16" r="16" fill="#006AA7"/>
                                <rect x="13.33" y="0" width="5.33" height="32" fill="#FECC00"/>
                                <rect x="0" y="13.33" width="32" height="5.33" fill="#FECC00"/>
                            </svg>
                        </div>
                        <div class="package-sidebar-holding-info">
                            <div class="package-sidebar-holding-name">Volvo Group</div>
                            <div class="package-sidebar-holding-detail">VOLV B · Sverige</div>
                        </div>
                        <div class="package-sidebar-holding-value">1.5%</div>
                    </div>
                </div>
            </div>
            
            <div class="package-sidebar-section">
                <h3 class="package-sidebar-section-title">Räntefonder (5)</h3>
                <div class="package-sidebar-holdings">
                    <div class="package-sidebar-holding">
                        <div class="package-sidebar-holding-flag">
                            <svg width="32" height="32" viewBox="0 0 32 32" fill="none">
                                <circle cx="16" cy="16" r="16" fill="#006AA7"/>
                                <rect x="13.33" y="0" width="5.33" height="32" fill="#FECC00"/>
                                <rect x="0" y="13.33" width="32" height="5.33" fill="#FECC00"/>
                            </svg>
                        </div>
                        <div class="package-sidebar-holding-info">
                            <div class="package-sidebar-holding-name">Swedbank Robur Räntefond Kort</div>
                            <div class="package-sidebar-holding-detail">Avgift 0.25%</div>
                        </div>
                        <div class="package-sidebar-holding-value">18%</div>
                    </div>
                    <div class="package-sidebar-holding">
                        <div class="package-sidebar-holding-flag">
                            <svg width="32" height="32" viewBox="0 0 32 32" fill="none">
                                <circle cx="16" cy="16" r="16" fill="#006AA7"/>
                                <rect x="13.33" y="0" width="5.33" height="32" fill="#FECC00"/>
                                <rect x="0" y="13.33" width="32" height="5.33" fill="#FECC00"/>
                            </svg>
                        </div>
                        <div class="package-sidebar-holding-info">
                            <div class="package-sidebar-holding-name">Nordea Obligationsfond</div>
                            <div class="package-sidebar-holding-detail">Avgift 0.30%</div>
                        </div>
                        <div class="package-sidebar-holding-value">18%</div>
                    </div>
                    <div class="package-sidebar-holding">
                        <div class="package-sidebar-holding-flag">
                            <svg width="32" height="32" viewBox="0 0 32 32" fill="none">
                                <circle cx="16" cy="16" r="16" fill="#006AA7"/>
                                <rect x="13.33" y="0" width="5.33" height="32" fill="#FECC00"/>
                                <rect x="0" y="13.33" width="32" height="5.33" fill="#FECC00"/>
                            </svg>
                        </div>
                        <div class="package-sidebar-holding-info">
                            <div class="package-sidebar-holding-name">SEB Obligationsfond</div>
                            <div class="package-sidebar-holding-detail">Avgift 0.28%</div>
                        </div>
                        <div class="package-sidebar-holding-value">18%</div>
                    </div>
                    <div class="package-sidebar-holding">
                        <div class="package-sidebar-holding-flag">
                            <svg width="32" height="32" viewBox="0 0 32 32" fill="none">
                                <circle cx="16" cy="16" r="16" fill="#006AA7"/>
                                <rect x="13.33" y="0" width="5.33" height="32" fill="#FECC00"/>
                                <rect x="0" y="13.33" width="32" height="5.33" fill="#FECC00"/>
                            </svg>
                        </div>
                        <div class="package-sidebar-holding-info">
                            <div class="package-sidebar-holding-name">Handelsbanken Räntefond</div>
                            <div class="package-sidebar-holding-detail">Avgift 0.22%</div>
                        </div>
                        <div class="package-sidebar-holding-value">18%</div>
                    </div>
                    <div class="package-sidebar-holding">
                        <div class="package-sidebar-holding-flag">
                            <svg width="32" height="32" viewBox="0 0 32 32" fill="none">
                                <circle cx="16" cy="16" r="16" fill="#00247D"/>
                                <path d="M0 0 L32 21.33" stroke="#FFFFFF" stroke-width="3.2"/>
                                <path d="M32 0 L0 21.33" stroke="#FFFFFF" stroke-width="3.2"/>
                                <path d="M0 0 L32 21.33" stroke="#CF142B" stroke-width="1.6"/>
                                <path d="M32 0 L0 21.33" stroke="#CF142B" stroke-width="1.6"/>
                                <rect x="0" y="12.8" width="32" height="6.4" fill="#FFFFFF"/>
                                <rect x="12.8" y="0" width="6.4" height="32" fill="#FFFFFF"/>
                                <rect x="0" y="14.4" width="32" height="3.2" fill="#CF142B"/>
                                <rect x="14.4" y="0" width="3.2" height="32" fill="#CF142B"/>
                            </svg>
                        </div>
                        <div class="package-sidebar-holding-info">
                            <div class="package-sidebar-holding-name">Vanguard Global Bond Index</div>
                            <div class="package-sidebar-holding-detail">Avgift 0.15%</div>
                        </div>
                        <div class="package-sidebar-holding-value">18%</div>
                    </div>
                </div>
            </div>
        </div>
        
        <div class="package-sidebar-button">
            <button>Välj detta paket</button>
        </div>
    </div>

    <script>
        document.addEventListener('DOMContentLoaded', function() {
            const leftSidebar = document.querySelector('.left-sidebar');
            const minimizeBtn = document.getElementById('minimizeBtn');
            const homeView = document.getElementById('homeView');
            const chatMessages = document.getElementById('chatMessages');
            const welcomeContent = document.getElementById('welcomeContent');
            const chatInputContainer = document.getElementById('chatInputContainer');
            
            // Randomize welcome message
            const greetings = [
                { text: 'Hur är din dag', isQuestion: true },
                { text: 'Hur mår du', isQuestion: true },
                { text: 'Vad kan jag hjälpa dig med', isQuestion: true },
                { text: 'Hej', isQuestion: false },
                { text: 'God morgon', isQuestion: false },
                { text: 'God eftermiddag', isQuestion: false },
                { text: 'Trevlig dag', isQuestion: false }
            ];
            
            const randomGreeting = greetings[Math.floor(Math.random() * greetings.length)];
            const punctuation = randomGreeting.isQuestion ? '?' : '!';
            const welcomeMessage = document.querySelector('.welcome-message');
            
            if (welcomeMessage) {
                welcomeMessage.textContent = `${randomGreeting.text}, Anna${punctuation}`;
            }
            
            // Get the active nav item (star icon)
            const activeNavItem = document.querySelector('.nav-item-sidebar.active');
            
            // Reset to default state when clicking active nav item
            if (activeNavItem) {
                activeNavItem.addEventListener('click', function() {
                    // Reset chat to default state
                    chatMessages.classList.remove('active');
                    chatMessages.innerHTML = '';
                    welcomeContent.classList.remove('hidden');
                    chatInputContainer.style.display = 'none';
                    document.querySelector('.chat-area').classList.remove('chat-active');
                    
                    // Reset input fields
                    if (textarea) {
                        textarea.value = '';
                        textarea.style.height = 'auto';
                    }
                    if (textareaActive) {
                        textareaActive.value = '';
                        textareaActive.style.height = 'auto';
                    }
                    if (sendButton) sendButton.disabled = true;
                    if (sendButtonActive) sendButtonActive.disabled = true;
                    
                    // Reset first message flag
                    if (typeof isFirstMessage !== 'undefined') {
                        isFirstMessage = true;
                    }
                });
            }

            // Toggle sidebar collapse
            if (minimizeBtn) {
                minimizeBtn.addEventListener('click', function() {
                    leftSidebar.classList.toggle('collapsed');
                    
                    // Update tooltip text based on collapsed state
                    const tooltip = minimizeBtn.querySelector('.nav-tooltip');
                    if (leftSidebar.classList.contains('collapsed')) {
                        tooltip.textContent = 'Maximera meny';
                    } else {
                        tooltip.textContent = 'Minimera meny';
                    }
                });
            }
            
            // Transfer popup functionality
            const transferOverlay = document.getElementById('transferOverlay');
            const closeTransferPopup = document.getElementById('closeTransferPopup');
            const transferButton = document.querySelector('.top-bar-button:last-child');
            const transferTabElements = document.querySelectorAll('.transfer-tab');
            
            // Bank popup functionality
            const bankOverlay = document.getElementById('bankOverlay');
            const closeBankPopup = document.getElementById('closeBankPopup');
            const bankButton = document.querySelector('.top-bar-button:first-child');
            const bankSelectStep = document.getElementById('bankSelectStep');
            const accountSelectStep = document.getElementById('accountSelectStep');
            const backToBanks = document.getElementById('backToBanks');
            const bankItems = document.querySelectorAll('.bank-item');
            const accountItems = document.querySelectorAll('.account-item');
            const connectBankButton = document.getElementById('connectBankButton');
            
            const bankSuccessStep = document.getElementById('bankSuccessStep');
            const bankSuccessDone = document.getElementById('bankSuccessDone');
            
            let selectedAccountName = '';
            let selectedBankName = '';
            
            // Open bank popup
            if (bankButton) {
                bankButton.addEventListener('click', function() {
                    bankOverlay.style.display = 'flex';
                    // Reset to first step
                    bankSelectStep.classList.add('active');
                    accountSelectStep.classList.remove('active');
                    bankSuccessStep.classList.remove('active');
                    // Reset transforms
                    bankSelectStep.style.transform = '';
                    bankSelectStep.style.opacity = '';
                    accountSelectStep.style.transform = '';
                    accountSelectStep.style.opacity = '';
                    // Reset title
                    document.getElementById('bankPopupTitle').textContent = 'Anslut konto';
                    
                    requestAnimationFrame(() => {
                        requestAnimationFrame(() => {
                            bankOverlay.classList.add('active');
                        });
                    });
                });
            }
            
            // Close bank popup
            if (closeBankPopup) {
                closeBankPopup.addEventListener('click', function() {
                    bankOverlay.classList.remove('active');
                    setTimeout(() => {
                        if (!bankOverlay.classList.contains('active')) {
                            bankOverlay.style.display = 'none';
                            // Reset steps
                            bankSelectStep.classList.add('active');
                            accountSelectStep.classList.remove('active');
                            // Reset transforms
                            bankSelectStep.style.transform = '';
                            bankSelectStep.style.opacity = '';
                            accountSelectStep.style.transform = '';
                            accountSelectStep.style.opacity = '';
                            // Reset title
                            document.getElementById('bankPopupTitle').textContent = 'Anslut konto';
                            // Reset button
                            connectBankButton.textContent = 'Anslut konto';
                            connectBankButton.disabled = true;
                            accountItems.forEach(acc => acc.classList.remove('selected'));
                        }
                    }, 300);
                });
            }
            
            // Bank logo update function (global)
            window.updateBankLogos = function(text, cssClass, bankName) {
                selectedBankName = bankName;
                
                const logo1 = document.getElementById('accountLogo1');
                const logo2 = document.getElementById('accountLogo2');
                const logo3 = document.getElementById('accountLogo3');
                
                if (logo1) {
                    logo1.textContent = text;
                    logo1.className = 'account-bank-logo ' + cssClass;
                }
                if (logo2) {
                    logo2.textContent = text;
                    logo2.className = 'account-bank-logo ' + cssClass;
                }
                if (logo3) {
                    logo3.textContent = text;
                    logo3.className = 'account-bank-logo ' + cssClass;
                }
            };
            
            // Bank selection
            bankItems.forEach(item => {
                item.addEventListener('click', function() {
                    selectedBankName = this.querySelector('.bank-name').textContent;
                    
                    // Animate current step out to left
                    bankSelectStep.style.transform = 'translateX(-100%)';
                    bankSelectStep.style.opacity = '0';
                    
                    // Prepare next step (start from right)
                    accountSelectStep.style.transform = 'translateX(100%)';
                    accountSelectStep.style.opacity = '0';
                    
                    // After a brief moment, slide in the account step
                    setTimeout(() => {
                        bankSelectStep.classList.remove('active');
                        accountSelectStep.classList.add('active');
                        
                        // Update title with bank name
                        document.getElementById('bankPopupTitle').textContent = 'Anslut konto i ' + selectedBankName;
                        
                        requestAnimationFrame(() => {
                            accountSelectStep.style.transform = 'translateX(0)';
                            accountSelectStep.style.opacity = '1';
                        });
                    }, 10);
                });
            });
            
            // Back to banks
            if (backToBanks) {
                backToBanks.addEventListener('click', function() {
                    // Animate current step out to right
                    accountSelectStep.style.transform = 'translateX(100%)';
                    accountSelectStep.style.opacity = '0';
                    
                    // Prepare previous step (start from left)
                    bankSelectStep.style.transform = 'translateX(-100%)';
                    bankSelectStep.style.opacity = '0';
                    
                    // After a brief moment, slide in the bank step
                    setTimeout(() => {
                        accountSelectStep.classList.remove('active');
                        bankSelectStep.classList.add('active');
                        
                        // Reset title
                        document.getElementById('bankPopupTitle').textContent = 'Anslut konto';
                        
                        requestAnimationFrame(() => {
                            bankSelectStep.style.transform = 'translateX(0)';
                            bankSelectStep.style.opacity = '1';
                        });
                    }, 10);
                    
                    // Deselect accounts
                    accountItems.forEach(acc => acc.classList.remove('selected'));
                    connectBankButton.disabled = true;
                });
            }
            
            // Account selection
            accountItems.forEach(item => {
                item.addEventListener('click', function() {
                    // Remove selected from all
                    accountItems.forEach(acc => acc.classList.remove('selected'));
                    // Add selected to clicked
                    this.classList.add('selected');
                    // Enable connect button
                    connectBankButton.disabled = false;
                    
                    // Update button text with selected account name
                    selectedAccountName = this.querySelector('.account-name').textContent;
                    connectBankButton.textContent = 'Anslut ' + selectedAccountName;
                });
            });
            
            // Connect bank button
            if (connectBankButton) {
                connectBankButton.addEventListener('click', function() {
                    // Show loading state
                    connectBankButton.classList.add('loading');
                    connectBankButton.disabled = true;
                    
                    // Simulate connection process - go to BankID step
                    setTimeout(() => {
                        // Hide account selection step
                        accountSelectStep.style.transform = 'translateX(-100%)';
                        accountSelectStep.style.opacity = '0';
                        
                        setTimeout(() => {
                            accountSelectStep.classList.remove('active');
                            bankBankIdStep.classList.add('active');
                            
                            // Update title for BankID step
                            document.getElementById('bankPopupTitle').textContent = 'Identifiera dig med Mobilt BankID';
                            
                            // Remove loading state
                            connectBankButton.classList.remove('loading');
                            
                            // BankID tab switching
                            const bankIdMobileTab = document.getElementById('bankIdMobileTab');
                            const bankIdCardTab = document.getElementById('bankIdCardTab');
                            const bankIdMobileContent = document.getElementById('bankIdMobileContent');
                            const bankIdCardContent = document.getElementById('bankIdCardContent');
                            
                            bankIdMobileTab.addEventListener('click', function() {
                                bankIdMobileTab.classList.add('active');
                                bankIdCardTab.classList.remove('active');
                                bankIdMobileContent.style.display = 'block';
                                bankIdCardContent.classList.remove('active');
                            });
                            
                            bankIdCardTab.addEventListener('click', function() {
                                bankIdCardTab.classList.add('active');
                                bankIdMobileTab.classList.remove('active');
                                bankIdMobileContent.style.display = 'none';
                                bankIdCardContent.classList.add('active');
                            });
                            
                            // After 2 seconds, show spinner
                            setTimeout(() => {
                                bankIdQrContainer.innerHTML = '<div class="bank-bankid-spinner"></div>';
                                
                                // After another 2 seconds, show success
                                setTimeout(() => {
                                    bankBankIdStep.classList.remove('active');
                                    bankSuccessStep.classList.add('active');
                                    
                                    // Hide title for success step
                                    document.getElementById('bankPopupTitle').style.display = 'none';
                                    
                                    // Update success message
                                    document.getElementById('bankSuccessText').textContent = 
                                        `Du har nu anslutit ditt ${selectedAccountName} från ${selectedBankName} till Tova. Du kan börja överföra pengar direkt.`;
                                    
                                    // Reset QR code for next time
                                    bankIdQrContainer.innerHTML = `<svg viewBox="0 0 200 200" xmlns="http://www.w3.org/2000/svg">
                                        <rect width="200" height="200" fill="white"/>
                                        <g fill="black">
                                            <rect x="20" y="20" width="20" height="20"/>
                                            <rect x="60" y="20" width="20" height="20"/>
                                            <rect x="80" y="20" width="20" height="20"/>
                                            <rect x="100" y="20" width="20" height="20"/>
                                            <rect x="140" y="20" width="20" height="20"/>
                                            <rect x="160" y="20" width="20" height="20"/>
                                            <rect x="20" y="40" width="20" height="20"/>
                                            <rect x="100" y="40" width="20" height="20"/>
                                            <rect x="160" y="40" width="20" height="20"/>
                                            <rect x="20" y="60" width="20" height="20"/>
                                            <rect x="60" y="60" width="20" height="20"/>
                                            <rect x="100" y="60" width="20" height="20"/>
                                            <rect x="120" y="60" width="20" height="20"/>
                                            <rect x="160" y="60" width="20" height="20"/>
                                            <rect x="20" y="80" width="20" height="20"/>
                                            <rect x="60" y="80" width="20" height="20"/>
                                            <rect x="100" y="80" width="20" height="20"/>
                                            <rect x="140" y="80" width="20" height="20"/>
                                            <rect x="160" y="80" width="20" height="20"/>
                                            <rect x="20" y="100" width="20" height="20"/>
                                            <rect x="100" y="100" width="20" height="20"/>
                                            <rect x="120" y="100" width="20" height="20"/>
                                            <rect x="140" y="100" width="20" height="20"/>
                                            <rect x="160" y="100" width="20" height="20"/>
                                            <rect x="60" y="120" width="20" height="20"/>
                                            <rect x="80" y="120" width="20" height="20"/>
                                            <rect x="140" y="120" width="20" height="20"/>
                                            <rect x="20" y="140" width="20" height="20"/>
                                            <rect x="40" y="140" width="20" height="20"/>
                                            <rect x="60" y="140" width="20" height="20"/>
                                            <rect x="80" y="140" width="20" height="20"/>
                                            <rect x="100" y="140" width="20" height="20"/>
                                            <rect x="120" y="140" width="20" height="20"/>
                                            <rect x="140" y="140" width="20" height="20"/>
                                            <rect x="160" y="140" width="20" height="20"/>
                                            <rect x="40" y="160" width="20" height="20"/>
                                            <rect x="80" y="160" width="20" height="20"/>
                                            <rect x="120" y="160" width="20" height="20"/>
                                            <rect x="140" y="160" width="20" height="20"/>
                                        </g>
                                    </svg>`;
                                }, 2000);
                            }, 2000);
                        }, 300);
                    }, 500);
                });
            }
            
            // Success done button
            if (bankSuccessDone) {
                bankSuccessDone.addEventListener('click', function() {
                    bankOverlay.classList.remove('active');
                    setTimeout(() => {
                        if (!bankOverlay.classList.contains('active')) {
                            bankOverlay.style.display = 'none';
                            // Reset all steps
                            bankSelectStep.classList.add('active');
                            accountSelectStep.classList.remove('active');
                            bankSuccessStep.classList.remove('active');
                            // Reset transforms
                            bankSelectStep.style.transform = '';
                            bankSelectStep.style.opacity = '';
                            accountSelectStep.style.transform = '';
                            accountSelectStep.style.opacity = '';
                            accountItems.forEach(acc => acc.classList.remove('selected'));
                            connectBankButton.disabled = true;
                            connectBankButton.textContent = 'Anslut konto';
                            // Reset title
                            document.getElementById('bankPopupTitle').textContent = 'Anslut konto';
                        }
                    }, 300);
                });
            }
            
            // Close on overlay click
            bankOverlay.addEventListener('click', function(e) {
                if (e.target === bankOverlay) {
                    bankOverlay.classList.remove('active');
                    setTimeout(() => {
                        if (!bankOverlay.classList.contains('active')) {
                            bankOverlay.style.display = 'none';
                            // Reset steps
                            bankSelectStep.classList.add('active');
                            accountSelectStep.classList.remove('active');
                            // Reset transforms
                            bankSelectStep.style.transform = '';
                            bankSelectStep.style.opacity = '';
                            accountSelectStep.style.transform = '';
                            accountSelectStep.style.opacity = '';
                            accountItems.forEach(acc => acc.classList.remove('selected'));
                            connectBankButton.disabled = true;
                            connectBankButton.textContent = 'Anslut konto';
                            // Reset title
                            document.getElementById('bankPopupTitle').textContent = 'Anslut konto';
                        }
                    }, 300);
                }
            });
            
            // Close on escape key (moved outside individual handlers for efficiency)
            document.addEventListener('keydown', function(e) {
                if (e.key === 'Escape') {
                    if (bankOverlay.classList.contains('active')) {
                        bankOverlay.classList.remove('active');
                    }
                    if (transferOverlay.classList.contains('active')) {
                        transferOverlay.classList.remove('active');
                    }
                    if (packageSidebar.classList.contains('active')) {
                        closePackageSidebar();
                    }
                    if (aiCarouselOverlay.classList.contains('active')) {
                        closeAiCarousel();
                    }
                }
            });
            
            // AI Carousel functionality
            const aiCarouselOverlay = document.getElementById('aiCarouselOverlay');
            const closeAiCarouselBtn = document.getElementById('closeAiCarousel');
            const aiCarouselSlides = document.getElementById('aiCarouselSlides');
            const aiCarouselBullets = document.querySelectorAll('.ai-carousel-bullet');
            const aiFundButton = document.querySelector('.ai-fund-button');
            
            let currentSlide = 0;
            
            function updateCarousel() {
                aiCarouselSlides.style.transform = `translateX(-${currentSlide * 100}%)`;
                
                // Update bullets
                aiCarouselBullets.forEach((bullet, index) => {
                    if (index === currentSlide) {
                        bullet.classList.add('active');
                    } else {
                        bullet.classList.remove('active');
                    }
                });
                
                // Update button states
                document.getElementById('aiCarouselPrev').disabled = currentSlide === 0;
                document.getElementById('aiCarouselPrev2').disabled = currentSlide === 0;
                document.getElementById('aiCarouselPrev3').disabled = currentSlide === 0;
            }
            
            function openAiCarousel() {
                aiCarouselOverlay.style.display = 'flex';
                currentSlide = 0;
                updateCarousel();
                
                requestAnimationFrame(() => {
                    requestAnimationFrame(() => {
                        aiCarouselOverlay.classList.add('active');
                    });
                });
            }
            
            function closeAiCarousel() {
                aiCarouselOverlay.classList.remove('active');
                setTimeout(() => {
                    if (!aiCarouselOverlay.classList.contains('active')) {
                        aiCarouselOverlay.style.display = 'none';
                        currentSlide = 0;
                        updateCarousel();
                    }
                }, 300);
            }
            
            function nextSlide() {
                if (currentSlide < 2) {
                    currentSlide++;
                    updateCarousel();
                }
            }
            
            function prevSlide() {
                if (currentSlide > 0) {
                    currentSlide--;
                    updateCarousel();
                }
            }
            
            // Open carousel when clicking AI fund button
            if (aiFundButton) {
                aiFundButton.addEventListener('click', openAiCarousel);
            }
            
            // Close carousel
            if (closeAiCarouselBtn) {
                closeAiCarouselBtn.addEventListener('click', closeAiCarousel);
            }
            
            // Close on overlay click
            aiCarouselOverlay.addEventListener('click', function(e) {
                if (e.target === aiCarouselOverlay) {
                    closeAiCarousel();
                }
            });
            
            // Navigation buttons
            document.getElementById('aiCarouselNext').addEventListener('click', nextSlide);
            document.getElementById('aiCarouselNext2').addEventListener('click', nextSlide);
            document.getElementById('aiCarouselPrev2').addEventListener('click', prevSlide);
            document.getElementById('aiCarouselPrev3').addEventListener('click', prevSlide);
            
            // Done button closes carousel
            document.getElementById('aiCarouselDone').addEventListener('click', closeAiCarousel);
            
            // Bullet navigation
            aiCarouselBullets.forEach((bullet, index) => {
                bullet.addEventListener('click', function() {
                    currentSlide = index;
                    updateCarousel();
                });
            });
            
            // Package sidebar functionality
            const packageSidebar = document.getElementById('packageSidebar');
            const packageSidebarOverlay = document.getElementById('packageSidebarOverlay');
            const closePackageSidebarBtn = document.getElementById('closePackageSidebar');
            
            const packageData = {
                'Tova liten': {
                    title: 'Tova liten',
                    subtitle: 'Ett sparande som fokuserar på trygghet och stabilitet framför högre risk och potentiell avkastning.',
                    pie: 'conic-gradient(#F5F4F2 0% 10%, #181512 10% 100%)',
                    stocks: '10%',
                    bonds: '90%',
                    other: '0%',
                    monthly: '500 kr',
                    fiveYear: '32 000 kr',
                    return: '+2 000 kr'
                },
                'Tova mellan': {
                    title: 'Tova mellan',
                    subtitle: 'En balanserad mix av trygghet och tillväxt för stabil långsiktig avkastning.',
                    pie: 'conic-gradient(#F5F4F2 0% 40%, #181512 40% 100%)',
                    stocks: '40%',
                    bonds: '60%',
                    other: '0%',
                    monthly: '1 500 kr',
                    fiveYear: '98 000 kr',
                    return: '+8 000 kr'
                },
                'Tova stor': {
                    title: 'Tova stor',
                    subtitle: 'Maximera tillväxtpotentialen med en aktietung portfölj för långsiktiga sparare.',
                    pie: 'conic-gradient(#F5F4F2 0% 70%, #181512 70% 100%)',
                    stocks: '70%',
                    bonds: '30%',
                    other: '0%',
                    monthly: '2 000 kr',
                    fiveYear: '129 000 kr',
                    return: '+20 000 kr'
                }
            };
            
            function openPackageSidebar(packageName) {
                const data = packageData[packageName];
                if (!data) return;
                
                // Update sidebar content
                document.getElementById('packageSidebarTitle').textContent = data.title;
                document.getElementById('packageSidebarSubtitle').textContent = data.subtitle;
                document.getElementById('packageSidebarPie').style.background = data.pie;
                document.getElementById('packageSidebarStocks').textContent = data.stocks;
                document.getElementById('packageSidebarBonds').textContent = data.bonds;
                document.getElementById('packageSidebarOther').textContent = data.other;
                document.getElementById('packageSidebarMonthly').textContent = data.monthly;
                document.getElementById('packageSidebarFiveYear').textContent = data.fiveYear;
                document.getElementById('packageSidebarReturn').textContent = data.return;
                
                // Show overlay and sidebar with animation
                packageSidebarOverlay.style.display = 'block';
                requestAnimationFrame(() => {
                    packageSidebarOverlay.classList.add('active');
                    packageSidebar.classList.add('active');
                });
            }
            
            function closePackageSidebar() {
                packageSidebarOverlay.classList.remove('active');
                packageSidebar.classList.remove('active');
                // Hide overlay after transition
                setTimeout(() => {
                    if (!packageSidebarOverlay.classList.contains('active')) {
                        packageSidebarOverlay.style.display = 'none';
                    }
                }, 400);
            }
            
            // Close button
            if (closePackageSidebarBtn) {
                closePackageSidebarBtn.addEventListener('click', closePackageSidebar);
            }
            
            // Close on overlay click
            packageSidebarOverlay.addEventListener('click', closePackageSidebar);
            
            // Event delegation for package content buttons (since they're dynamically created)
            document.addEventListener('click', function(e) {
                if (e.target.classList.contains('package-content-btn')) {
                    const packageName = e.target.getAttribute('data-package');
                    openPackageSidebar(packageName);
                }
            });
            
            // Transfer functionality
            const topupAmountInput = document.getElementById('topupAmount');
            const payoutAmountInput = document.getElementById('payoutAmount');
            const topupButton = document.getElementById('topupButton');
            const payoutButton = document.getElementById('payoutButton');
            const transferSuccessView = document.getElementById('transferSuccessView');
            const transferSuccessDone = document.getElementById('transferSuccessDone');
            
            const transferTabs = document.querySelector('.transfer-tabs');
            
            // Enable/disable buttons based on input
            topupAmountInput.addEventListener('input', function() {
                topupButton.disabled = this.value.trim() === '' || parseFloat(this.value) <= 0;
            });
            
            payoutAmountInput.addEventListener('input', function() {
                payoutButton.disabled = this.value.trim() === '' || parseFloat(this.value) <= 0;
            });
            
            // Top-up button click
            topupButton.addEventListener('click', function() {
                const amount = parseFloat(topupAmountInput.value);
                topupButton.classList.add('loading');
                topupButton.disabled = true;
                
                setTimeout(() => {
                    // Hide topup content and tabs
                    document.getElementById('topupContent').classList.remove('active');
                    transferTabs.classList.add('hidden');
                    transferSuccessView.classList.add('active');
                    
                    // Update success message
                    document.getElementById('transferSuccessTitle').textContent = 'Insättning genomförd!';
                    document.getElementById('transferSuccessAmount').textContent = amount.toLocaleString('sv-SE') + ' SEK';
                    document.getElementById('transferSuccessText').textContent = 'Pengarna har satts in på ditt Tova-konto och är tillgängliga direkt.';
                    document.getElementById('transferSuccessInfo').textContent = '';
                    document.getElementById('transferSuccessInfo').style.display = 'none';
                    
                    // Remove loading state
                    topupButton.classList.remove('loading');
                }, 2000);
            });
            
            // Payout button click
            payoutButton.addEventListener('click', function() {
                const amount = parseFloat(payoutAmountInput.value);
                payoutButton.classList.add('loading');
                payoutButton.disabled = true;
                
                setTimeout(() => {
                    // Hide payout content and tabs
                    document.getElementById('payoutContent').classList.remove('active');
                    transferTabs.classList.add('hidden');
                    transferSuccessView.classList.add('active');
                    
                    // Update success message
                    document.getElementById('transferSuccessTitle').textContent = 'Uttag genomfört!';
                    document.getElementById('transferSuccessAmount').textContent = amount.toLocaleString('sv-SE') + ' SEK';
                    document.getElementById('transferSuccessText').textContent = 'Pengarna har tagits ut från ditt Tova-konto och är på väg till ditt bankkonto.';
                    document.getElementById('transferSuccessInfo').textContent = 'Det kan ta 2-3 bankdagar innan pengarna syns på ditt konto.';
                    document.getElementById('transferSuccessInfo').style.display = 'block';
                    
                    // Remove loading state
                    payoutButton.classList.remove('loading');
                }, 2000);
            });
            
            // Success done button
            transferSuccessDone.addEventListener('click', function() {
                transferOverlay.classList.remove('active');
                setTimeout(() => {
                    if (!transferOverlay.classList.contains('active')) {
                        transferOverlay.style.display = 'none';
                        // Reset to default tab
                        transferSuccessView.classList.remove('active');
                        transferTabs.classList.remove('hidden');
                        document.getElementById('topupContent').classList.add('active');
                        // Reset inputs
                        topupAmountInput.value = '';
                        payoutAmountInput.value = '';
                        topupButton.disabled = true;
                        payoutButton.disabled = true;
                    }
                }, 300);
            });
            
            // Open transfer popup
            if (transferButton) {
                transferButton.addEventListener('click', function() {
                    transferOverlay.style.display = 'flex';
                    // Reset to topup tab
                    transferSuccessView.classList.remove('active');
                    transferTabs.classList.remove('hidden');
                    document.getElementById('topupContent').classList.add('active');
                    document.getElementById('payoutContent').classList.remove('active');
                    // Reset active tab button
                    document.querySelectorAll('.transfer-tab').forEach(t => t.classList.remove('active'));
                    document.querySelector('.transfer-tab[data-tab="topup"]').classList.add('active');
                    
                    requestAnimationFrame(() => {
                        requestAnimationFrame(() => {
                            transferOverlay.classList.add('active');
                        });
                    });
                });
            }
            
            // Close transfer popup
            if (closeTransferPopup) {
                closeTransferPopup.addEventListener('click', function() {
                    transferOverlay.classList.remove('active');
                    setTimeout(() => {
                        if (!transferOverlay.classList.contains('active')) {
                            transferOverlay.style.display = 'none';
                            // Reset
                            transferSuccessView.classList.remove('active');
                            transferTabs.classList.remove('hidden');
                            document.getElementById('topupContent').classList.add('active');
                            topupAmountInput.value = '';
                            payoutAmountInput.value = '';
                            topupButton.disabled = true;
                            payoutButton.disabled = true;
                        }
                    }, 300);
                });
            }
            
            // Close on overlay click
            transferOverlay.addEventListener('click', function(e) {
                if (e.target === transferOverlay) {
                    transferOverlay.classList.remove('active');
                    setTimeout(() => {
                        if (!transferOverlay.classList.contains('active')) {
                            transferOverlay.style.display = 'none';
                            // Reset
                            transferSuccessView.classList.remove('active');
                            transferTabs.classList.remove('hidden');
                            document.getElementById('topupContent').classList.add('active');
                            topupAmountInput.value = '';
                            payoutAmountInput.value = '';
                            topupButton.disabled = true;
                            payoutButton.disabled = true;
                        }
                    }, 300);
                }
            });
            
            // Tab switching
            transferTabElements.forEach(tab => {
                tab.addEventListener('click', function() {
                    // Remove active from all tabs
                    transferTabElements.forEach(t => t.classList.remove('active'));
                    // Add active to clicked tab
                    this.classList.add('active');
                    
                    // Show corresponding content
                    const tabName = this.getAttribute('data-tab');
                    document.querySelectorAll('.transfer-content').forEach(content => {
                        content.classList.remove('active');
                    });
                    document.getElementById(tabName + 'Content').classList.add('active');
                });
            });

            document.querySelectorAll('.section-header').forEach(header => {
                header.addEventListener('click', function() {
                    const sectionId = this.getAttribute('data-section');
                    const content = document.querySelector(`[data-section-content="${sectionId}"]`);
                    const arrow = this.querySelector('.section-arrow');
                    content.classList.toggle('collapsed');
                    arrow.classList.toggle('collapsed');
                });
            });

            const searchInput = document.getElementById('searchInput');
            const searchDropdown = document.getElementById('searchDropdown');
            const searchOverlay = document.getElementById('searchOverlay');
            const cmdKBadge = document.getElementById('cmdKBadge');
            const clearSearchBtn = document.getElementById('clearSearchBtn');
            const textarea = document.getElementById('chatInput');
            const textareaActive = document.getElementById('chatInputActive');
            const sendButton = document.getElementById('sendButton');
            const sendButtonActive = document.getElementById('sendButtonActive');
            
            // Function to update button visibility
            function updateSearchButtons() {
                if (searchInput.value.length > 0) {
                    cmdKBadge.style.display = 'none';
                    clearSearchBtn.style.display = 'flex';
                } else {
                    cmdKBadge.style.display = 'block';
                    clearSearchBtn.style.display = 'none';
                }
            }
            
            // Update buttons on input
            searchInput.addEventListener('input', updateSearchButtons);
            
            // Update buttons on focus/blur
            searchInput.addEventListener('focus', updateSearchButtons);
            searchInput.addEventListener('blur', () => {
                if (searchInput.value.length === 0) {
                    cmdKBadge.style.display = 'block';
                    clearSearchBtn.style.display = 'none';
                }
            });
            
            // Clear search input when X button is clicked
            clearSearchBtn.addEventListener('click', (e) => {
                e.preventDefault();
                searchInput.value = '';
                searchInput.focus();
                renderDropdown(allFunds.slice(0,5));
                searchDropdown.classList.add('active');
                searchOverlay.classList.add('active');
                updateSearchButtons();
            });
            
            const allFunds = [
                { name: 'Tova Aktiefond', fee: '0,92%', return: '+5,24%', positive: true, country: 'se' },
                { name: 'Global Auto 6', fee: '2,45%', return: '+9,80%', positive: true, country: 'se' },
                { name: 'MS INFV Handel', fee: '3,60%', return: '-0,97%', positive: false, country: 'se' },
                { name: 'BG Aktiefond', fee: '1,18%', return: '+4,22%', positive: true, country: 'se' },
                { name: 'Nuro Bank', fee: '1,38%', return: '+1,50%', positive: true, country: 'se' },
                { name: 'Swedbank Robur Ny Teknik', fee: '1,65%', return: '+12,50%', positive: true, country: 'se' },
                { name: 'Nordea Emerging Stars', fee: '2,10%', return: '+7,30%', positive: true, country: 'se' },
                { name: 'Apple AAPL', fee: '-', return: '+28,50%', positive: true, country: 'us' },
                { name: 'Microsoft MSFT', fee: '-', return: '+31,20%', positive: true, country: 'us' },
                { name: 'Tesla TSLA', fee: '-', return: '+45,80%', positive: true, country: 'us' }
            ];

            function getFlagIcon(c) {
                return c === 'us' ? '<svg width="24" height="24" viewBox="0 0 24 24" fill="none"><circle cx="12" cy="12" r="12" fill="#B22234"/><rect x="0" y="1.85" width="24" height="1.54" fill="#FFFFFF"/><rect x="0" y="4.92" width="24" height="1.54" fill="#FFFFFF"/><rect x="0" y="8" width="24" height="1.54" fill="#FFFFFF"/><rect x="0" y="11.08" width="24" height="1.54" fill="#FFFFFF"/><rect x="0" y="14.15" width="24" height="1.54" fill="#FFFFFF"/><rect x="0" y="17.23" width="24" height="1.54" fill="#FFFFFF"/><rect x="0" y="20.31" width="24" height="1.54" fill="#FFFFFF"/><rect x="0" y="0" width="9.6" height="11.08" fill="#3C3B6E"/></svg>' : '<svg width="24" height="24" viewBox="0 0 24 24" fill="none"><circle cx="12" cy="12" r="12" fill="#006AA7"/><rect x="10" y="0" width="4" height="24" fill="#FECC00"/><rect x="0" y="10" width="24" height="4" fill="#FECC00"/></svg>';
            }

            function renderDropdown(funds) {
                let html = '<div class="dropdown-header"><div>Namn</div><div>Total avgift</div><div style="text-align: right;">1 m.</div></div>';
                funds.forEach(f => {
                    html += `<div class="dropdown-item"><div class="dropdown-name"><div class="flag-icon">${getFlagIcon(f.country)}</div><span>${f.name}</span></div><div class="dropdown-fee">${f.fee}</div><div class="dropdown-return ${f.positive?'positive':'negative'}">${f.return}</div></div>`;
                });
                searchDropdown.innerHTML = html;
            }

            searchInput.addEventListener('focus', () => { renderDropdown(allFunds.slice(0,5)); searchDropdown.classList.add('active'); searchOverlay.classList.add('active'); });
            searchInput.addEventListener('input', function() {
                const q = this.value.toLowerCase().trim();
                renderDropdown(q.length > 0 ? allFunds.filter(f => f.name.toLowerCase().includes(q)).slice(0,8) : allFunds.slice(0,5));
                searchDropdown.classList.add('active'); searchOverlay.classList.add('active');
            });
            searchOverlay.addEventListener('click', () => { searchDropdown.classList.remove('active'); searchOverlay.classList.remove('active'); searchInput.blur(); });
            window.addEventListener('keydown', e => {
                if ((e.metaKey || e.ctrlKey) && (e.key === 'k' || e.key === 'K')) { e.preventDefault(); searchInput.focus(); searchDropdown.classList.add('active'); searchOverlay.classList.add('active'); }
                if (e.key === 'Escape') { searchDropdown.classList.remove('active'); searchOverlay.classList.remove('active'); searchInput.blur(); }
            });

            sendButton.disabled = true;
            sendButtonActive.disabled = true;

            textarea.addEventListener('input', function() { sendButton.disabled = this.value.trim() === ''; });
            textareaActive.addEventListener('input', function() { sendButtonActive.disabled = this.value.trim() === ''; });

            function addMessage(text, isUser) {
                const div = document.createElement('div');
                div.className = `chat-message ${isUser?'user':'ai'}`;
                const content = document.createElement('div');
                content.className = 'message-content';
                content.textContent = text;
                div.appendChild(content);
                chatMessages.appendChild(div);
                const chatArea = document.querySelector('.chat-area');
                requestAnimationFrame(() => {
                    if (!isFirstMessage) {
                        chatArea.scrollTop = chatArea.scrollHeight;
                    }
                });
                return div;
            }

            function addTypingIndicator() {
                const div = document.createElement('div');
                div.className = 'chat-message ai typing-message';
                const content = document.createElement('div');
                content.className = 'message-content';
                const typing = document.createElement('div');
                typing.className = 'typing-indicator';
                typing.innerHTML = '<svg class="typing-star" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M12 2v20M2 12h20M5.64 5.64l12.72 12.72M5.64 18.36l12.72-12.72"></path><circle cx="12" cy="12" r="3"></circle></svg>';
                content.appendChild(typing);
                div.appendChild(content);
                chatMessages.appendChild(div);
                const chatArea = document.querySelector('.chat-area');
                requestAnimationFrame(() => {
                    if (!isFirstMessage) {
                        chatArea.scrollTop = chatArea.scrollHeight;
                    }
                });
                return div;
            }

            function typeMessage(el, text) {
                return new Promise(resolve => {
                    const words = text.split(' ');
                    let i = 0;
                    el.innerHTML = '';
                    const chatArea = document.querySelector('.chat-area');
                    function type() {
                        if (i < words.length) {
                            el.innerHTML += (i > 0 ? ' ' : '') + words[i];
                            i++;
                            if (!isFirstMessage) {
                                requestAnimationFrame(() => {
                                    chatArea.scrollTop = chatArea.scrollHeight;
                                });
                            }
                            const progress = i / words.length;
                            setTimeout(type, 80 - (60 * progress));
                        } else resolve();
                    }
                    type();
                });
            }

            function simulateAIResponse(msg) {
                const typing = addTypingIndicator();
                setTimeout(() => {
                    typing.remove();
                    const lm = msg.toLowerCase();
                    let response = '', showPackages = false, showFundsList = false;
                    
                    if (lm.includes('månadssparande')) {
                        response = 'Här har du tre paket du kan börja månadsspara i – Tova <b>liten</b>, <b>mellan</b> och <b>stor</b>. Paketen är utformade efter din ekonomi och tidigare aktiviteter.';
                        showPackages = true;
                    } else if (lm.includes('varför') && (lm.includes('tova') || lm.includes('paket'))) {
                        response = 'Tovas paket tar bort komplexiteten i sparande. Istället för att välja bland tusentals fonder får du färdiga, balanserade portföljer som vår AI optimerat baserat på din risknivå. Du slipper göra egna analyser, ombalansering sker automatiskt, och avgifterna är transparenta. Plus – paketen anpassas efter din ekonomiska situation, så du får ett sparande som faktiskt passar just dig. Det är som att ha en personlig rådgivare, fast enklare och billigare.';
                    } else if (lm.includes('populäraste') || lm.includes('tovas 10')) {
                        response = 'Här är Tovas 10 mest populära fonder och aktier för 2025:';
                        showFundsList = true;
                    } else if (lm.includes('skillnaden mellan aktier och fonder')) {
                        response = 'Aktier är andelar i enskilda företag - när du köper en Apple-aktie äger du en liten del av Apple. Fonder är istället "korgar" med många olika aktier eller obligationer. När du köper en fond sprider du automatiskt din risk över många företag. Fonder är ofta ett bra val för dig som vill ha enklare och mer diversifierad investering, medan aktier ger dig mer kontroll men kräver mer arbete och kunskap.';
                    } else if (lm.includes('fonder')) {
                        response = 'Jag har hittat flera intressanta fonder åt dig. Bland de mest populära just nu är Tova Aktiefond med 0,92% avgift och +5,24% avkastning, samt Swedbank Robur Ny Teknik med 1,65% avgift och +12,50% avkastning. Vill du veta mer om någon specifik fond?';
                    } else if (lm.includes('spara')) {
                        response = 'En bra tumregel är att spara minst 10-15% av din månadsinkomst. Om du tjänar 30 000 kr per månad bör du sträva efter att spara 3 000-4 500 kr. Det viktiga är att börja någonstans och öka successivt!';
                    } else {
                        response = 'Tack för din fråga! Jag hjälper dig gärna med investeringar och sparande. Vill du veta mer om fonder, aktier eller månadssparande?';
                    }
                    
                    const div = document.createElement('div');
                    div.className = 'chat-message ai';
                    const content = document.createElement('div');
                    content.className = 'message-content';
                    div.appendChild(content);
                    chatMessages.appendChild(div);
                    
                    typeMessage(content, response).then(() => {
                        if (showFundsList) {
                            const fundsList = document.createElement('div');
                            fundsList.className = 'funds-list';
                            fundsList.innerHTML = `
                                <div class="fund-item">
                                    <div class="fund-rank">1</div>
                                    <div class="fund-name">
                                        <div class="fund-flag"><svg width="24" height="24" viewBox="0 0 24 24" fill="none"><circle cx="12" cy="12" r="12" fill="#006AA7"/><rect x="10" y="0" width="4" height="24" fill="#FECC00"/><rect x="0" y="10" width="24" height="4" fill="#FECC00"/></svg></div>
                                        <span class="fund-title">Tova Aktiefond</span>
                                    </div>
                                    <div class="fund-fee">0,92%</div>
                                    <div class="fund-return positive">+5,24%</div>
                                </div>
                                <div class="fund-item">
                                    <div class="fund-rank">2</div>
                                    <div class="fund-name">
                                        <div class="fund-flag"><svg width="24" height="24" viewBox="0 0 24 24" fill="none"><circle cx="12" cy="12" r="12" fill="#006AA7"/><rect x="10" y="0" width="4" height="24" fill="#FECC00"/><rect x="0" y="10" width="24" height="4" fill="#FECC00"/></svg></div>
                                        <span class="fund-title">Global Auto 6</span>
                                    </div>
                                    <div class="fund-fee">2,45%</div>
                                    <div class="fund-return positive">+9,80%</div>
                                </div>
                                <div class="fund-item">
                                    <div class="fund-rank">3</div>
                                    <div class="fund-name">
                                        <div class="fund-flag"><svg width="24" height="24" viewBox="0 0 24 24" fill="none"><circle cx="12" cy="12" r="12" fill="#006AA7"/><rect x="10" y="0" width="4" height="24" fill="#FECC00"/><rect x="0" y="10" width="24" height="4" fill="#FECC00"/></svg></div>
                                        <span class="fund-title">MS INFV Handel</span>
                                    </div>
                                    <div class="fund-fee">3,60%</div>
                                    <div class="fund-return negative">-0,97%</div>
                                </div>
                                <div class="fund-item">
                                    <div class="fund-rank">4</div>
                                    <div class="fund-name">
                                        <div class="fund-flag"><svg width="24" height="24" viewBox="0 0 24 24" fill="none"><circle cx="12" cy="12" r="12" fill="#006AA7"/><rect x="10" y="0" width="4" height="24" fill="#FECC00"/><rect x="0" y="10" width="24" height="4" fill="#FECC00"/></svg></div>
                                        <span class="fund-title">BG Aktiefond</span>
                                    </div>
                                    <div class="fund-fee">1,18%</div>
                                    <div class="fund-return positive">+4,22%</div>
                                </div>
                                <div class="fund-item">
                                    <div class="fund-rank">5</div>
                                    <div class="fund-name">
                                        <div class="fund-flag"><svg width="24" height="24" viewBox="0 0 24 24" fill="none"><circle cx="12" cy="12" r="12" fill="#006AA7"/><rect x="10" y="0" width="4" height="24" fill="#FECC00"/><rect x="0" y="10" width="24" height="4" fill="#FECC00"/></svg></div>
                                        <span class="fund-title">Nuro Bank</span>
                                    </div>
                                    <div class="fund-fee">1,38%</div>
                                    <div class="fund-return positive">+1,50%</div>
                                </div>
                                <div class="fund-item">
                                    <div class="fund-rank">6</div>
                                    <div class="fund-name">
                                        <div class="fund-flag"><svg width="24" height="24" viewBox="0 0 24 24" fill="none"><circle cx="12" cy="12" r="12" fill="#006AA7"/><rect x="10" y="0" width="4" height="24" fill="#FECC00"/><rect x="0" y="10" width="24" height="4" fill="#FECC00"/></svg></div>
                                        <span class="fund-title">Swedbank Robur Ny Teknik</span>
                                    </div>
                                    <div class="fund-fee">1,65%</div>
                                    <div class="fund-return positive">+12,50%</div>
                                </div>
                                <div class="fund-item">
                                    <div class="fund-rank">7</div>
                                    <div class="fund-name">
                                        <div class="fund-flag"><svg width="24" height="24" viewBox="0 0 24 24" fill="none"><circle cx="12" cy="12" r="12" fill="#006AA7"/><rect x="10" y="0" width="4" height="24" fill="#FECC00"/><rect x="0" y="10" width="24" height="4" fill="#FECC00"/></svg></div>
                                        <span class="fund-title">Nordea Emerging Stars</span>
                                    </div>
                                    <div class="fund-fee">2,10%</div>
                                    <div class="fund-return positive">+7,30%</div>
                                </div>
                                <div class="fund-item">
                                    <div class="fund-rank">8</div>
                                    <div class="fund-name">
                                        <div class="fund-flag"><svg width="24" height="24" viewBox="0 0 24 24" fill="none"><circle cx="12" cy="12" r="12" fill="#B22234"/><rect x="0" y="1.85" width="24" height="1.54" fill="#FFFFFF"/><rect x="0" y="4.92" width="24" height="1.54" fill="#FFFFFF"/><rect x="0" y="8" width="24" height="1.54" fill="#FFFFFF"/><rect x="0" y="11.08" width="24" height="1.54" fill="#FFFFFF"/><rect x="0" y="14.15" width="24" height="1.54" fill="#FFFFFF"/><rect x="0" y="17.23" width="24" height="1.54" fill="#FFFFFF"/><rect x="0" y="20.31" width="24" height="1.54" fill="#FFFFFF"/><rect x="0" y="0" width="9.6" height="11.08" fill="#3C3B6E"/></svg></div>
                                        <span class="fund-title">Apple AAPL</span>
                                    </div>
                                    <div class="fund-fee">-</div>
                                    <div class="fund-return positive">+28,50%</div>
                                </div>
                                <div class="fund-item">
                                    <div class="fund-rank">9</div>
                                    <div class="fund-name">
                                        <div class="fund-flag"><svg width="24" height="24" viewBox="0 0 24 24" fill="none"><circle cx="12" cy="12" r="12" fill="#B22234"/><rect x="0" y="1.85" width="24" height="1.54" fill="#FFFFFF"/><rect x="0" y="4.92" width="24" height="1.54" fill="#FFFFFF"/><rect x="0" y="8" width="24" height="1.54" fill="#FFFFFF"/><rect x="0" y="11.08" width="24" height="1.54" fill="#FFFFFF"/><rect x="0" y="14.15" width="24" height="1.54" fill="#FFFFFF"/><rect x="0" y="17.23" width="24" height="1.54" fill="#FFFFFF"/><rect x="0" y="20.31" width="24" height="1.54" fill="#FFFFFF"/><rect x="0" y="0" width="9.6" height="11.08" fill="#3C3B6E"/></svg></div>
                                        <span class="fund-title">Microsoft MSFT</span>
                                    </div>
                                    <div class="fund-fee">-</div>
                                    <div class="fund-return positive">+31,20%</div>
                                </div>
                                <div class="fund-item">
                                    <div class="fund-rank">10</div>
                                    <div class="fund-name">
                                        <div class="fund-flag"><svg width="24" height="24" viewBox="0 0 24 24" fill="none"><circle cx="12" cy="12" r="12" fill="#B22234"/><rect x="0" y="1.85" width="24" height="1.54" fill="#FFFFFF"/><rect x="0" y="4.92" width="24" height="1.54" fill="#FFFFFF"/><rect x="0" y="8" width="24" height="1.54" fill="#FFFFFF"/><rect x="0" y="11.08" width="24" height="1.54" fill="#FFFFFF"/><rect x="0" y="14.15" width="24" height="1.54" fill="#FFFFFF"/><rect x="0" y="17.23" width="24" height="1.54" fill="#FFFFFF"/><rect x="0" y="20.31" width="24" height="1.54" fill="#FFFFFF"/><rect x="0" y="0" width="9.6" height="11.08" fill="#3C3B6E"/></svg></div>
                                        <span class="fund-title">Tesla TSLA</span>
                                    </div>
                                    <div class="fund-fee">-</div>
                                    <div class="fund-return positive">+45,80%</div>
                                </div>
                            `;
                            content.appendChild(fundsList);
                            const chatArea = document.querySelector('.chat-area');
                            requestAnimationFrame(() => {
                                if (!isFirstMessage) {
                                    chatArea.scrollTop = chatArea.scrollHeight;
                                }
                            });
                            
                            // Add suggestion cards after funds list
                            const suggestionsWrapper = document.createElement('div');
                            suggestionsWrapper.style.marginTop = '24px';
                            const heading = document.createElement('div');
                            heading.style.fontSize = '14px';
                            heading.style.fontWeight = '600';
                            heading.style.color = '#1a1a1a';
                            heading.style.marginBottom = '12px';
                            heading.textContent = 'Vad vill du veta mer om?';
                            const suggestions = document.createElement('div');
                            suggestions.className = 'ai-suggestions';
                            suggestions.innerHTML = `
                                <button class="ai-suggestion-card" data-suggestion="Jag vill starta ett månadssparande">Jag vill starta ett månadssparande</button>
                                <button class="ai-suggestion-card" data-suggestion="Hjälp mig välja en riskprofil">Hjälp mig välja en riskprofil</button>
                                <button class="ai-suggestion-card" data-suggestion="Vad är skillnaden mellan aktier och fonder?">Vad är skillnaden mellan aktier och fonder?</button>
                                <button class="ai-suggestion-card" data-suggestion="Skapa en investeringsplan åt mig">Skapa en investeringsplan åt mig</button>
                            `;
                            suggestionsWrapper.appendChild(heading);
                            suggestionsWrapper.appendChild(suggestions);
                            content.appendChild(suggestionsWrapper);
                            requestAnimationFrame(() => {
                                if (!isFirstMessage) {
                                    chatArea.scrollTop = chatArea.scrollHeight;
                                }
                            });
                            
                            // Add click handlers for suggestion cards
                            suggestions.querySelectorAll('.ai-suggestion-card').forEach(card => {
                                card.addEventListener('click', function() {
                                    const text = this.getAttribute('data-suggestion');
                                    addMessage(text, true);
                                    addConversationToList(text);
                                    // Ensure subsequent messages scroll properly
                                    if (isFirstMessage) {
                                        isFirstMessage = false;
                                    }
                                    simulateAIResponse(text);
                                });
                            });
                        }
                        
                        if (showPackages) {
                            const packages = document.createElement('div');
                            packages.className = 'savings-packages';
                            packages.innerHTML = `
                                <div class="package-card">
                                    <div class="package-header">
                                        <div class="package-title">Tova liten</div>
                                        <div class="package-subtitle">Ett sparande som fokuserar på trygghet och stabilitet framför högre risk och potentiell avkastning.</div>
                                    </div>
                                    <div class="package-allocation">
                                        <div class="package-pie conservative" style="background: conic-gradient(#F5F4F2 0% 10%, #181512 10% 100%);"></div>
                                        <div class="allocation-breakdown">
                                            <div class="allocation-item"><span class="allocation-label">Aktier</span><span class="allocation-value">10%</span></div>
                                            <div class="allocation-item"><span class="allocation-label">Ränta</span><span class="allocation-value">90%</span></div>
                                            <div class="allocation-item"><span class="allocation-label">Övrigt</span><span class="allocation-value">0%</span></div>
                                        </div>
                                    </div>
                                    <div class="package-details">
                                        <div class="detail-row"><div class="detail-label">Månadssparande</div><div class="detail-value">500 kr</div></div>
                                        <div class="detail-row"><div class="detail-label">Om 5 år</div><div class="detail-value">32 000 kr</div></div>
                                        <div class="detail-row"><div class="detail-label">Avkastning</div><div class="detail-value">+2 000 kr</div></div>
                                        <div class="detail-subvalue">Om börsen går som förväntat.</div>
                                    </div>
                                    <div style="display: flex; gap: 8px; width: 100%;">
                                        <button style="flex: 1; height: 50px !important; padding: 0; background: #F5F4F2; color: #1a1a1a; border: none; border-radius: 12px; font-size: 14px; font-weight: 500; cursor: pointer; font-family: 'Helvetica Neue', Helvetica, Arial, sans-serif; display: flex; align-items: center; justify-content: center; transition: background 0.2s;" class="package-content-btn" data-package="Tova liten" onmouseover="this.style.backgroundColor='#E9E7E2'" onmouseout="this.style.backgroundColor='#F5F4F2'">Visa paketinnehåll</button>
                                        <button style="flex: 1; height: 50px !important; padding: 0; background: #1a1a1a; color: #FFFFFF; border: none; border-radius: 12px; font-size: 14px; font-weight: 500; cursor: pointer; font-family: 'Helvetica Neue', Helvetica, Arial, sans-serif; display: flex; align-items: center; justify-content: center; transition: background 0.2s;" onmouseover="this.style.backgroundColor='#333333'" onmouseout="this.style.backgroundColor='#1a1a1a'">Välj Tova liten</button>
                                    </div>
                                </div>
                                <div class="package-card">
                                    <div class="package-header">
                                        <div class="package-title">Tova mellan</div>
                                        <div class="package-subtitle">En balanserad mix av trygghet och tillväxt för stabil långsiktig avkastning.</div>
                                    </div>
                                    <div class="package-allocation">
                                        <div class="package-pie balanced" style="background: conic-gradient(#F5F4F2 0% 40%, #181512 40% 100%);"></div>
                                        <div class="allocation-breakdown">
                                            <div class="allocation-item"><span class="allocation-label">Aktier</span><span class="allocation-value">40%</span></div>
                                            <div class="allocation-item"><span class="allocation-label">Ränta</span><span class="allocation-value">60%</span></div>
                                            <div class="allocation-item"><span class="allocation-label">Övrigt</span><span class="allocation-value">0%</span></div>
                                        </div>
                                    </div>
                                    <div class="package-details">
                                        <div class="detail-row"><div class="detail-label">Månadssparande</div><div class="detail-value">1 500 kr</div></div>
                                        <div class="detail-row"><div class="detail-label">Om 5 år</div><div class="detail-value">98 000 kr</div></div>
                                        <div class="detail-row"><div class="detail-label">Avkastning</div><div class="detail-value">+8 000 kr</div></div>
                                        <div class="detail-subvalue">Om börsen går som förväntat.</div>
                                    </div>
                                    <div style="display: flex; gap: 8px; width: 100%;">
                                        <button style="flex: 1; height: 50px !important; padding: 0; background: #F5F4F2; color: #1a1a1a; border: none; border-radius: 12px; font-size: 14px; font-weight: 500; cursor: pointer; font-family: 'Helvetica Neue', Helvetica, Arial, sans-serif; display: flex; align-items: center; justify-content: center; transition: background 0.2s;" class="package-content-btn" data-package="Tova mellan" onmouseover="this.style.backgroundColor='#E9E7E2'" onmouseout="this.style.backgroundColor='#F5F4F2'">Visa paketinnehåll</button>
                                        <button style="flex: 1; height: 50px !important; padding: 0; background: #1a1a1a; color: #FFFFFF; border: none; border-radius: 12px; font-size: 14px; font-weight: 500; cursor: pointer; font-family: 'Helvetica Neue', Helvetica, Arial, sans-serif; display: flex; align-items: center; justify-content: center; transition: background 0.2s;" onmouseover="this.style.backgroundColor='#333333'" onmouseout="this.style.backgroundColor='#1a1a1a'">Välj Tova mellan</button>
                                    </div>
                                </div>
                                <div class="package-card">
                                    <div class="package-header">
                                        <div class="package-title">Tova stor</div>
                                        <div class="package-subtitle">Maximera tillväxtpotentialen med en aktietung portfölj för långsiktiga sparare.</div>
                                    </div>
                                    <div class="package-allocation">
                                        <div class="package-pie growth" style="background: conic-gradient(#F5F4F2 0% 70%, #181512 70% 100%);"></div>
                                        <div class="allocation-breakdown">
                                            <div class="allocation-item"><span class="allocation-label">Aktier</span><span class="allocation-value">70%</span></div>
                                            <div class="allocation-item"><span class="allocation-label">Ränta</span><span class="allocation-value">30%</span></div>
                                            <div class="allocation-item"><span class="allocation-label">Övrigt</span><span class="allocation-value">0%</span></div>
                                        </div>
                                    </div>
                                    <div class="package-details">
                                        <div class="detail-row"><div class="detail-label">Månadssparande</div><div class="detail-value">2 000 kr</div></div>
                                        <div class="detail-row"><div class="detail-label">Om 5 år</div><div class="detail-value">129 000 kr</div></div>
                                        <div class="detail-row"><div class="detail-label">Avkastning</div><div class="detail-value">+20 000 kr</div></div>
                                        <div class="detail-subvalue">Om börsen går som förväntat.</div>
                                    </div>
                                    <div style="display: flex; gap: 8px; width: 100%;">
                                        <button style="flex: 1; height: 50px !important; padding: 0; background: #F5F4F2; color: #1a1a1a; border: none; border-radius: 12px; font-size: 14px; font-weight: 500; cursor: pointer; font-family: 'Helvetica Neue', Helvetica, Arial, sans-serif; display: flex; align-items: center; justify-content: center; transition: background 0.2s;" class="package-content-btn" data-package="Tova stor" onmouseover="this.style.backgroundColor='#E9E7E2'" onmouseout="this.style.backgroundColor='#F5F4F2'">Visa paketinnehåll</button>
                                        <button style="flex: 1; height: 50px !important; padding: 0; background: #1a1a1a; color: #FFFFFF; border: none; border-radius: 12px; font-size: 14px; font-weight: 500; cursor: pointer; font-family: 'Helvetica Neue', Helvetica, Arial, sans-serif; display: flex; align-items: center; justify-content: center; transition: background 0.2s;" onmouseover="this.style.backgroundColor='#333333'" onmouseout="this.style.backgroundColor='#1a1a1a'">Välj Tova stor</button>
                                    </div>
                                </div>
                            `;
                            content.appendChild(packages);
                            const chatArea = document.querySelector('.chat-area');
                            requestAnimationFrame(() => {
                                if (!isFirstMessage) {
                                    chatArea.scrollTop = chatArea.scrollHeight;
                                }
                            });
                            
                            // Add suggestion cards after packages
                            const suggestionsWrapper = document.createElement('div');
                            suggestionsWrapper.style.marginTop = '24px';
                            const heading = document.createElement('div');
                            heading.style.fontSize = '14px';
                            heading.style.fontWeight = '600';
                            heading.style.color = '#1a1a1a';
                            heading.style.marginBottom = '12px';
                            heading.textContent = 'Vad vill du veta mer om?';
                            const suggestions = document.createElement('div');
                            suggestions.className = 'ai-suggestions';
                            suggestions.innerHTML = `
                                <button class="ai-suggestion-card" data-suggestion="Vilka är de bästa fonderna just nu?">Vilka är de bästa fonderna just nu?</button>
                                <button class="ai-suggestion-card" data-suggestion="Varför ska jag välja Tovas paket?">Varför ska jag välja Tovas paket?</button>
                                <button class="ai-suggestion-card" data-suggestion="Vad är skillnaden mellan aktier och fonder?">Vad är skillnaden mellan aktier och fonder?</button>
                                <button class="ai-suggestion-card" data-suggestion="Hur mycket ska man spara per månad?">Hur mycket ska man spara per månad?</button>
                            `;
                            suggestionsWrapper.appendChild(heading);
                            suggestionsWrapper.appendChild(suggestions);
                            content.appendChild(suggestionsWrapper);
                            requestAnimationFrame(() => {
                                if (!isFirstMessage) {
                                    chatArea.scrollTop = chatArea.scrollHeight;
                                }
                            });
                            
                            // Add click handlers for suggestion cards
                            suggestions.querySelectorAll('.ai-suggestion-card').forEach(card => {
                                card.addEventListener('click', function() {
                                    const text = this.getAttribute('data-suggestion');
                                    addMessage(text, true);
                                    addConversationToList(text);
                                    // Ensure subsequent messages scroll properly
                                    if (isFirstMessage) {
                                        isFirstMessage = false;
                                    }
                                    simulateAIResponse(text);
                                });
                            });
                        }
                    });
                }, 2000);
            }

            let isFirstMessage = true;

            function handleSend(input) {
                const msg = input.value.trim();
                if (msg) {
                    welcomeContent.classList.add('hidden');
                    chatMessages.classList.add('active');
                    chatInputContainer.style.display = 'block';
                    const chatArea = document.querySelector('.chat-area');
                    chatArea.classList.add('chat-active');
                    addMessage(msg, true);
                    input.value = '';
                    input.style.height = 'auto';
                    sendButton.disabled = true;
                    sendButtonActive.disabled = true;
                    
                    addConversationToList(msg);
                    
                    // Keep scroll at top after first message only
                    if (isFirstMessage) {
                        chatArea.scrollTop = 0;
                        isFirstMessage = false;
                    }
                    
                    simulateAIResponse(msg);
                }
            }

            function addConversationToList(message) {
                const todayList = document.querySelector('[data-section-content="today"]');
                const truncated = message.length > 40 ? message.substring(0, 37) + '...' : message;
                
                const newItem = document.createElement('div');
                newItem.className = 'section-list-item';
                newItem.textContent = truncated;
                
                todayList.insertBefore(newItem, todayList.firstChild);
                
                while (todayList.children.length > 5) {
                    todayList.removeChild(todayList.lastChild);
                }
            }

            sendButton.addEventListener('click', () => handleSend(textarea));
            sendButtonActive.addEventListener('click', () => handleSend(textareaActive));
            textarea.addEventListener('keydown', e => { if (e.key === 'Enter' && !e.shiftKey) { e.preventDefault(); handleSend(textarea); } });
            textareaActive.addEventListener('keydown', e => { if (e.key === 'Enter' && !e.shiftKey) { e.preventDefault(); handleSend(textareaActive); } });

            document.querySelectorAll('.suggestion-card').forEach(card => {
                card.addEventListener('click', function() {
                    const text = this.getAttribute('data-message');
                    welcomeContent.classList.add('hidden');
                    chatMessages.classList.add('active');
                    chatInputContainer.style.display = 'block';
                    document.querySelector('.chat-area').classList.add('chat-active');
                    addMessage(text, true);
                    addConversationToList(text);
                    simulateAIResponse(text);
                });
            });
        });
    </script>

</body>
</html>
                
