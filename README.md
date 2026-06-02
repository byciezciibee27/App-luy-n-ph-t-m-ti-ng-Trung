<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AI Chinese Pronunciation Master</title>
    <style>
        :root {
            --bg-gradient: linear-gradient(135deg, #f5f7fa 0%, #c3cfe2 100%);
            --accent: #4f46e5;
            --accent-light: #eef2ff;
            --text-dark: #1e293b;
            --text-light: #64748b;
            --c-success: #10b981;
            --c-warning: #f59e0b;
            --c-danger: #ef4444;
        }

        * { box-sizing: border-box; margin: 0; padding: 0; font-family: system-ui, -apple-system, sans-serif; }

        body {
            background: var(--bg-gradient);
            color: var(--text-dark);
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 20px;
        }

        .wrapper {
            background: #ffffff;
            width: 100%;
            max-width: 500px;
            border-radius: 24px;
            padding: 35px;
            box-shadow: 0 20px 40px rgba(0,0,0,0.05);
        }

        .title-area { text-align: center; margin-bottom: 30px; }
        .title-area h1 { font-size: 24px; font-weight: 800; color: var(--accent); }
        .title-area p { font-size: 13px; color: var(--text-light); margin-top: 4px; }

        /* Tối ưu hiển thị chữ Hán kèm Pinyin phía trên bằng thẻ ruby */
        .card-display {
            background: var(--accent-light);
            border: 1px solid rgba(79, 70, 229, 0.1);
            border-radius: 20px;
            padding: 40px 20px;
            text-align: center;
            margin-bottom: 25px;
        }

        ruby {
            font-size: 42px;
            font-weight: 700;
            ruby-position: over;
        }

        rt {
            font-size: 16px;
            font-weight: 500;
            color: var(--accent);
            padding-bottom: 12px;
            letter-spacing: 1px;
        }

        .controls { display: flex; gap: 12px; margin-bottom: 25px; }

        .btn {
            border: none;
            outline: none;
            padding: 14px 24px;
            border-radius: 14px;
            font-size: 15px;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.2s ease;
            display: inline-flex;
            align-items: center;
            justify-content: center;
            gap: 8px;
        }

        .btn-audio { background: #e2e8f0; color: var(--text-dark); width: 35%; }
        .btn-audio:hover { background: #cbd5e1; }

        .btn-mic { background: var(--accent); color: white; width: 65%; box-shadow: 0 4px 12px rgba(79, 70, 229, 0.2); }
        .btn-mic:hover { background: #4338ca; }
        
        /* Hiệu ứng xung động sóng âm khi nói */
        .btn-mic.recording {
            background: var(--c-danger);
            box-shadow: 0 0 0 0 rgba(239, 68, 68, 0.7);
            animation: pulse 1.6s infinite cubic-bezier(0.66, 0, 0, 1);
        }

        @keyframes pulse {
            to { box-shadow: 0 0 0 18px rgba(239, 68, 68, 0); }
        }

        .panel-result {
            background: #f8fafc;
            border: 1px solid #e2e8f0;
            border-radius: 16px;
            padding: 20px;
            display: none;
        }

        .panel-result.show { display: block; }

        .rank-box { display: flex; justify-content: space-between; align-items: baseline; margin-bottom: 15px; }
        .rank-title { font-size: 13px; font-weight: 600; color: var(--text-light); text-transform: uppercase; }
        .rank-num { font-size: 32px; font-weight: 800; }

        .output-text { font-size: 14px; color: var(--text-light); line-height: 1.6; margin-bottom: 15px; }
        .output-text strong { color: var(--text-dark); font-weight: 600; background: #e2e8f0; padding: 2px 6px; border-radius: 4px; }

        .btn-next { background: var(--c-success); color: white; width: 100
