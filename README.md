<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AI Chinese Pronunciation Coach</title>
    <style>
        :root {
            --primary: #6366f1;
            --primary-hover: #4f46e5;
            --success: #10b981;
            --warning: #f59e0b;
            --danger: #ef4444;
            --background: #f8fafc;
            --card-bg: #ffffff;
            --text-main: #0f172a;
            --text-muted: #64748b;
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: 'Segoe UI', system-ui, -apple-system, sans-serif;
        }

        body {
            background: linear-gradient(135deg, #e0e7ff 0%, #f1f5f9 100%);
            color: var(--text-main);
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 20px;
        }

        .app-card {
            background: var(--card-bg);
            border-radius: 24px;
            box-shadow: 0 20px 25px -5px rgba(0, 0, 0, 0.1), 0 10px 10px -5px rgba(0, 0, 0, 0.04);
            width: 100%;
            max-width: 550px;
            padding: 40px;
            transition: all 0.3s ease;
        }

        header {
            text-align: center;
            margin-bottom: 30px;
        }

        header h1 {
            font-size: 26px;
            font-weight: 800;
            color: var(--primary);
            letter-spacing: -0.5px;
        }

        header p {
            color: var(--text-muted);
            font-size: 14px;
            margin-top: 5px;
        }

        .display-card {
            background: #f1f5f9;
            border-radius: 16px;
            padding: 30px 20px;
            text-align: center;
            margin-bottom: 25px;
            position: relative;
            border: 1px solid #e2e8f0;
        }

        .zh-text {
            font-size: 42px;
            font-weight: 700;
            color: #000;
            margin-bottom: 8px;
            letter-spacing: 2px;
        }

        .py-text {
            font-size: 18px;
            color: var(--primary);
            font-weight: 500;
        }

        .action-zone {
            display: flex;
            gap: 15px;
            margin-bottom: 25px;
        }

        .btn {
            flex: 1;
            padding: 14px 20px;
            border: none;
            border-radius: 12px;
            font-size: 16px;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.2s ease;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 8px;
        }

        .btn-listen {
            background-color: #e0e7ff;
            color: var(--primary);
        }

        .btn-listen:hover { background-color: #c7d2fe; }

        .btn-speak {
            background-color: var(--primary);
            color: white;
            flex: 2;
            box-shadow: 0 4px 12px rgba(99, 102, 241, 0.3);
        }

        .btn-speak:hover { background-color: var(--primary-hover); }

        .btn-speak.recording {
            background-color: var(--danger);
            animation: pulse 1.5s infinite;
            box-shadow: 0 4px 12px rgba(239, 68, 68, 0.3);
        }

        @keyframes pulse {
            0 { transform: scale(1); }
            50% { transform: scale(1.02); }
            100% { transform: scale(1); }
        }

        .result-card {
            background: #fafafa;
            border-radius: 16px;
            padding: 25px;
            border: 1px solid #e2e8f0;
            display: none;
            animation: fadeIn 0.4s ease forwards;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }

        .result-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 15px;
        }

        .score-badge {
            font-size: 36px;
            font-weight: 800;
        }

        .user-say {
            font-size: 15px;
            color: var(--text-muted);
            margin-bottom: 15px;
            line-height: 1.5;
        }

        .user-say span {
            color: var(--text-main);
            font-weight: 600;
            background: #f1f5f9;
            padding: 2px 8px;
            border-radius: 6px;
        }

        .feedback-text {
            font-size: 15px;
            font-weight: 600;
            margin-bottom: 20px;
        }

        .btn-next {
            background-color: var(--success);
            color: white;
            width: 100%;
        }
        .btn-next:hover { opacity: 0.9; }

        .disabled {
            opacity: 0.6;
            cursor: not-allowed !important;
            pointer-events: none;
        }
    </style>
</head>
<body>

<div class="app-card">
    <header>
        <h1>AI 汉语发音 Coach</h1>
        <p>Luyện phát âm tiếng Trung chuẩn xác với AI</p>
    </header>

    <div class="display-card">
        <div class="zh-text" id="zhText">你好</div>
        <div class="py-text" id="pyText">Nǐ hǎo</div>
    </div>

    <div class="action-zone">
        <button class="btn btn-listen" id="listenBtn">🔊 Nghe mẫu</button>
        <button class="btn btn-speak" id="speakBtn">🎙️ Bắt đầu nói</button>
    </div>

    <div class="result-card" id="resultCard">
        <div class="result-header">
            <span style="font-weight: 600
