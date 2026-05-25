[smart_scheduling_system_FIXED (2).html](https://github.com/user-attachments/files/28224855/smart_scheduling_system_FIXED.2.html)
# 104<!DOCTYPE html>
<html lang="zh-TW">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>智慧排班系統</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <script src="https://cdnjs.cloudflare.com/ajax/libs/html2pdf.js/0.10.1/html2pdf.bundle.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.18.5/xlsx.min.js"></script>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        :root {
            --primary-color: #C4956C;
            --primary-dark: #9E7B5B;
            --primary-light: #D4A97A;
            --accent-color: #8B6F47;
            --light-bg: #F5F1ED;
        }

        body {
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
            background: linear-gradient(135deg, var(--primary-color) 0%, var(--primary-dark) 100%);
            min-height: 100vh;
            padding: 20px;
        }

        .container {
            max-width: 1400px;
            margin: 0 auto;
            background: white;
            border-radius: 12px;
            box-shadow: 0 20px 60px rgba(0,0,0,0.3);
            overflow: hidden;
        }

        .header {
            background: linear-gradient(135deg, var(--primary-color) 0%, var(--primary-dark) 100%);
            color: white;
            padding: 30px;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .header h1 {
            font-size: 28px;
            font-weight: 600;
        }

        .header-info {
            display: flex;
            gap: 20px;
            align-items: center;
        }

        .user-badge {
            background: rgba(255,255,255,0.2);
            padding: 10px 20px;
            border-radius: 20px;
            font-size: 14px;
        }

        .current-date {
            background: rgba(255,255,255,0.2);
            padding: 10px 20px;
            border-radius: 20px;
            font-size: 14px;
            font-weight: 500;
        }

        .role-select {
            padding: 8px 16px;
            border: none;
            border-radius: 6px;
            background: rgba(255,255,255,0.3);
            color: white;
            cursor: pointer;
            font-weight: 500;
        }

        .role-select option {
            background: white;
            color: #333;
        }

        .main-content {
            display: flex;
            height: calc(100vh - 200px);
        }

        .sidebar {
            width: 110px;
            background: var(--light-bg);
            padding: 10px;
            border-right: 1px solid #e0e0e0;
            overflow-y: auto;
        }

        .sidebar-section {
            margin-bottom: 15px;
        }

        .sidebar-title {
            font-size: 11px;
            font-weight: 600;
            color: #999;
            text-transform: uppercase;
            margin-bottom: 8px;
            letter-spacing: 0.3px;
        }

        .sidebar-item {
            padding: 8px 10px;
            margin-bottom: 5px;
            border-radius: 6px;
            cursor: pointer;
            transition: all 0.3s;
            font-size: 12px;
            color: #666;
        }

        .sidebar-item:hover {
            background: #e8e1d6;
            color: var(--primary-color);
        }

        .sidebar-item.active {
            background: var(--primary-color);
            color: white;
            font-weight: 600;
        }

        .sidebar-item i {
            margin-right: 6px;
            width: 14px;
        }

        .content {
            flex: 1;
            display: flex;
            flex-direction: column;
            background: #ffffff;
        }

        .tab-navigation {
            display: flex;
            border-bottom: 2px solid #e0e0e0;
            background: var(--light-bg);
        }

        .tab-button {
            padding: 16px 24px;
            border: none;
            background: transparent;
            cursor: pointer;
            font-size: 14px;
            font-weight: 500;
            color: #666;
            transition: all 0.3s;
            border-bottom: 3px solid transparent;
            margin-bottom: -2px;
        }

        .tab-button.active {
            color: var(--primary-color);
            border-bottom-color: var(--primary-color);
            background: white;
        }

        .tab-content {
            display: none;
            flex: 1;
            overflow-y: auto;
            overflow-x: hidden;
            padding: 20px;
            min-height: 0;
        }

        .tab-content.active {
            display: block;
        }

        .scheduling-grid {
            display: grid;
            grid-template-columns: 150px 1fr;
            gap: 15px;
            margin-bottom: 30px;
            min-height: 0;
        }

        .employee-list {
            background: var(--light-bg);
            border-radius: 8px;
            padding: 15px;
            max-height: 500px;
            overflow-y: auto;
        }

        .employee-item {
            padding: 10px;
            margin-bottom: 8px;
            background: white;
            border-radius: 6px;
            font-size: 13px;
            cursor: pointer;
            transition: all 0.3s;
            border: 1px solid #e0e0e0;
        }

        .employee-item:hover {
            background: var(--primary-color);
            color: white;
            border-color: var(--primary-color);
        }

        .employee-avatar {
            width: 24px;
            height: 24px;
            border-radius: 50%;
            display: inline-flex;
            align-items: center;
            justify-content: center;
            font-size: 10px;
            font-weight: bold;
            color: white;
            margin-right: 8px;
            background: linear-gradient(135deg, var(--primary-color) 0%, var(--primary-dark) 100%);
            overflow: hidden;
            flex-shrink: 0;
        }

        .employee-avatar img {
            width: 100%;
            height: 100%;
            object-fit: cover;
        }

        .calendar-grid {
            display: grid;
            grid-template-columns: repeat(7, 1fr);
            gap: 10px;
            flex: 1;
            min-height: 0;
            overflow: hidden;
        }

        .day-column {
            background: var(--light-bg);
            border-radius: 8px;
            padding: 10px;
            display: flex;
            flex-direction: column;
            min-width: 0;
        }

        .day-header {
            font-weight: 600;
            color: var(--primary-color);
            margin-bottom: 2px;
            text-align: center;
            font-size: 12px;
            white-space: nowrap;
            overflow: hidden;
            text-overflow: ellipsis;
        }

        .day-date {
            font-size: 10px;
            color: #999;
            text-align: center;
            margin-bottom: 3px;
            display: none;
        }

        .day-slots {
            flex: 1;
            display: flex;
            flex-direction: column;
            gap: 5px;
            min-height: 250px;
            overflow-y: auto;
        }

        .shift-slot {
            padding: 8px;
            background: white;
            border: 2px dashed #ddd;
            border-radius: 6px;
            text-align: center;
            cursor: pointer;
            transition: all 0.3s;
            min-height: 40px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 12px;
            color: #999;
        }

        .shift-slot:hover {
            border-color: var(--primary-color);
            background: #faf8f3;
        }

        .shift-slot.filled {
            background: var(--primary-color);
            color: white;
            border: 2px solid var(--primary-color);
            cursor: pointer;
            padding: 8px;
            min-height: auto;
        }

        .shift-employee {
            display: flex;
            align-items: center;
            gap: 4px;
            font-weight: 500;
            width: 100%;
            font-size: 10px;
            white-space: nowrap;
            overflow: hidden;
            text-overflow: ellipsis;
        }

        .shift-time {
            font-size: 10px;
            color: #fff;
            opacity: 0.9;
        }

        .leave-request-section {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 30px;
        }

        .employee-form, .availability-view {
            background: var(--light-bg);
            border-radius: 8px;
            padding: 25px;
        }

        .form-group {
            margin-bottom: 20px;
        }

        .form-label {
            font-weight: 600;
            color: #333;
            margin-bottom: 8px;
            display: block;
            font-size: 14px;
        }

        .form-input, .form-select {
            width: 100%;
            padding: 12px;
            border: 1px solid #ddd;
            border-radius: 6px;
            font-size: 14px;
            transition: all 0.3s;
        }

        .form-input:focus, .form-select:focus {
            outline: none;
            border-color: var(--primary-color);
            box-shadow: 0 0 0 3px rgba(196, 149, 108, 0.1);
        }

        .btn {
            padding: 12px 24px;
            border: none;
            border-radius: 6px;
            cursor: pointer;
            font-weight: 600;
            font-size: 14px;
            transition: all 0.3s;
            display: inline-flex;
            align-items: center;
            gap: 8px;
        }

        .btn-primary {
            background: var(--primary-color);
            color: white;
        }

        .btn-primary:hover {
            background: var(--primary-dark);
            transform: translateY(-2px);
            box-shadow: 0 10px 20px rgba(196, 149, 108, 0.2);
        }

        .btn-secondary {
            background: #e0e0e0;
            color: #333;
        }

        .btn-secondary:hover {
            background: #d0d0d0;
        }

        .availability-grid {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 15px;
            margin-top: 15px;
        }

        .availability-stat {
            background: white;
            padding: 15px;
            border-radius: 8px;
            text-align: center;
            border: 1px solid #e0e0e0;
        }

        .stat-number {
            font-size: 24px;
            font-weight: bold;
            color: var(--primary-color);
            margin-bottom: 5px;
        }

        .stat-label {
            font-size: 12px;
            color: #999;
        }

        .gantt-container {
            background: white;
            border-radius: 8px;
            padding: 10px;
            overflow-x: auto;
            overflow-y: auto;
            max-height: calc(100vh - 250px);
        }

        .gantt-table {
            width: auto;
            border-collapse: collapse;
            min-width: auto;
        }

        .gantt-header {
            font-weight: 600;
            background: #faf8f3;
            padding: 3px 1px;
            border: 1px solid #ddd;
            text-align: center;
            font-size: 9px;
            min-width: 24px;
            width: 24px;
            white-space: nowrap;
            overflow: visible;
        }

        .gantt-cell {
            padding: 1px;
            border: 1px solid #ddd;
            height: 35px;
            position: relative;
            min-width: 24px;
            width: 24px;
        }

        .gantt-bar {
            height: 100%;
            background: linear-gradient(135deg, var(--primary-color) 0%, var(--primary-dark) 100%);
            color: white;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 11px;
            border-radius: 4px;
            overflow: hidden;
            text-overflow: ellipsis;
            white-space: nowrap;
        }

        .gantt-bar.afternoon {
            background: linear-gradient(135deg, #E67E22 0%, #D35400 100%);
        }

        .gantt-employee-name {
            font-weight: 600;
            background: var(--light-bg);
            padding: 4px 6px;
            min-width: 50px;
            border-right: 2px solid var(--primary-color);
            white-space: nowrap;
            overflow: hidden;
            text-overflow: ellipsis;
            max-width: 70px;
            font-size: 10px;
        }

        .export-buttons {
            display: flex;
            gap: 10px;
            margin-bottom: 10px;
        }

        .modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0,0,0,0.5);
            z-index: 1000;
            align-items: center;
            justify-content: center;
        }

        .modal.active {
            display: flex;
        }

        .modal-content {
            background: white;
            border-radius: 12px;
            padding: 30px;
            max-width: 500px;
            width: 90%;
            box-shadow: 0 20px 60px rgba(0,0,0,0.3);
            position: relative;
        }

        .modal-header {
            font-size: 20px;
            font-weight: 600;
            margin-bottom: 20px;
            color: #333;
        }

        .modal-close {
            position: absolute;
            top: 20px;
            right: 20px;
            background: none;
            border: none;
            font-size: 24px;
            cursor: pointer;
            color: #999;
        }

        .shift-selector {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 10px;
            margin-bottom: 20px;
        }

        .shift-option {
            padding: 12px;
            border: 2px solid #ddd;
            border-radius: 6px;
            text-align: center;
            cursor: pointer;
            transition: all 0.3s;
            font-size: 12px;
        }

        .shift-option:hover {
            border-color: var(--primary-color);
            background: #faf8f3;
        }

        .shift-option.selected {
            background: var(--primary-color);
            color: white;
            border-color: var(--primary-color);
        }

        .availability-calendar {
            display: grid;
            grid-template-columns: repeat(7, 1fr);
            gap: 8px;
            margin-top: 15px;
        }

        .calendar-date {
            aspect-ratio: 1;
            display: flex;
            align-items: center;
            justify-content: center;
            border-radius: 6px;
            cursor: pointer;
            font-size: 13px;
            font-weight: 600;
            border: 2px solid #ddd;
            transition: all 0.3s;
            background: white;
            flex-direction: column;
        }

        .calendar-date:hover {
            border-color: var(--primary-color);
        }

        .calendar-date.unavailable {
            background: var(--primary-color);
            color: white;
            border-color: var(--primary-color);
        }

        .calendar-date.full {
            background: #e74c3c;
            color: white;
            border-color: #e74c3c;
            cursor: not-allowed;
        }

        .time-slot-selector {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 10px;
            margin-bottom: 20px;
        }

        .time-slot-btn {
            padding: 10px;
            border: 2px solid #ddd;
            background: white;
            border-radius: 6px;
            cursor: pointer;
            transition: all 0.3s;
            font-size: 12px;
            font-weight: 500;
        }

        .time-slot-btn.selected {
            background: var(--primary-color);
            color: white;
            border-color: var(--primary-color);
        }

        .settings-card {
            background: var(--light-bg);
            border-radius: 8px;
            padding: 25px;
            margin-bottom: 20px;
        }

        .settings-item {
            background: white;
            padding: 15px;
            border-radius: 6px;
            margin-bottom: 12px;
            border: 1px solid #e0e0e0;
        }

        .delete-btn {
            background: #f8f0ea;
            border: 1px solid #f5e6d3;
            color: #c95141;
            padding: 6px 12px;
            border-radius: 4px;
            cursor: pointer;
            font-size: 12px;
            font-weight: 600;
        }

        .photo-preview {
            width: 40px;
            height: 40px;
            border-radius: 50%;
            object-fit: cover;
            margin-right: 10px;
        }

        
        .calendar-date.pending {
            background: #f39c12;
            color: white;
            border-color: #f39c12;
        }

        .leave-preview-table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 15px;
            background: white;
            border-radius: 8px;
            overflow: hidden;
        }

        .leave-preview-table th,
        .leave-preview-table td {
            border: 1px solid #ddd;
            padding: 10px;
            text-align: center;
            font-size: 13px;
        }

        .leave-preview-table th {
            background: var(--primary-color);
            color: white;
        }


        .stats-grid {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 20px;
            margin-bottom: 30px;
        }

        .stat-card {
            background: linear-gradient(135deg, var(--primary-color) 0%, var(--primary-dark) 100%);
            color: white;
            padding: 20px;
            border-radius: 8px;
            text-align: center;
        }

        .stat-card-value {
            font-size: 32px;
            font-weight: bold;
            margin-bottom: 5px;
        }

        .stat-card-label {
            font-size: 13px;
            opacity: 0.9;
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="header">
            <div>
                <h1><i class="fas fa-calendar-alt"></i> 智慧排班系統</h1>
            </div>
            <div class="header-info">
                <div class="current-date">
                    <i class="fas fa-clock"></i> <span id="currentDateTime"></span>
                </div>
                <select class="role-select" id="roleSelect">
                    <option value="employee">員工</option>
                    <option value="manager" selected>主管</option>
                </select>
                <div class="user-badge">
                    <i class="fas fa-user-circle"></i> 登入用戶
                </div>
            </div>
        </div>

        <div class="main-content">
            <div class="sidebar">
                <div class="sidebar-section">
                    <div class="sidebar-title">主要功能</div>
                    <div class="sidebar-item active" onclick="switchTab('dashboard')">
                        <i class="fas fa-chart-bar"></i> 儀表板
                    </div>
                    <div class="sidebar-item" onclick="switchTab('scheduling')">
                        <i class="fas fa-calendar-check"></i> 排班管理
                    </div>
                    <div class="sidebar-item" onclick="switchTab('requests')">
                        <i class="fas fa-hand-paper"></i> 休假申請
                    </div>
                    <div class="sidebar-item" onclick="switchTab('gantt')">
                        <i class="fas fa-chart-gantt"></i> 班表查看
                    </div>
                </div>

                <div class="sidebar-section">
                    <div class="sidebar-title">其他</div>
                    <div class="sidebar-item" onclick="switchTab('settings')">
                        <i class="fas fa-cog"></i> 系統設定
                    </div>
                </div>
            </div>

            <div class="content">
                <div class="tab-navigation">
                    <button class="tab-button active" onclick="switchTab('dashboard')">
                        <i class="fas fa-chart-bar"></i> 儀表板
                    </button>
                    <button class="tab-button" onclick="switchTab('scheduling')">
                        <i class="fas fa-calendar-check"></i> 排班管理
                    </button>
                    <button class="tab-button" onclick="switchTab('requests')">
                        <i class="fas fa-hand-paper"></i> 休假申請
                    </button>
                    <button class="tab-button" onclick="switchTab('gantt')">
                        <i class="fas fa-chart-gantt"></i> 班表查看
                    </button>
                    <button class="tab-button" onclick="switchTab('settings')">
                        <i class="fas fa-cog"></i> 系統設定
                    </button>
                </div>

                <!-- 儀表板 -->
                <div class="tab-content active" id="dashboard">
                    <h2 style="margin-bottom: 20px;">本週排班概覽</h2>
                    <div class="stats-grid">
                        <div class="stat-card">
                            <div class="stat-card-value" id="totalEmployees">8</div>
                            <div class="stat-card-label">在職員工</div>
                        </div>
                        <div class="stat-card">
                            <div class="stat-card-value" id="assignedEmployees">0</div>
                            <div class="stat-card-label">已排班</div>
                        </div>
                        <div class="stat-card">
                            <div class="stat-card-value" id="shortStaff">8</div>
                            <div class="stat-card-label">缺工警示</div>
                        </div>
                        <div class="stat-card">
                            <div class="stat-card-value" id="totalHours">0</div>
                            <div class="stat-card-label">本週工時</div>
                        </div>
                    </div>
                </div>

                <!-- 排班管理 -->
                <div class="tab-content" id="scheduling">
                    <h2 style="margin-bottom: 10px;" id="schedulingTitle">週排班</h2>
                    
                    <div style="margin-bottom: 20px; display: flex; gap: 10px;">
                        <button class="btn btn-secondary" onclick="previousWeek()">
                            <i class="fas fa-chevron-left"></i> 上一週
                        </button>
                        <button class="btn btn-secondary" onclick="nextWeek()">
                            下一週 <i class="fas fa-chevron-right"></i>
                        </button>
                        <button class="btn btn-primary" onclick="publishSchedule()">
                            <i class="fas fa-paper-plane"></i> 發布班表
                        </button>
                    </div>

                    <div class="scheduling-grid">
                        <div class="employee-list" id="employeeList">
                            <div style="text-align: center; color: #999; font-size: 12px; margin-bottom: 10px;">
                                員工列表
                            </div>
                        </div>

                        <div class="calendar-grid" id="calendarGrid">
                        </div>
                    </div>
                </div>

                <!-- 休假申請 -->
                <div class="tab-content" id="requests">
                    <h2 style="margin-bottom: 20px;">員工休假申請與可用日期</h2>
                    
                    <div class="leave-request-section">
                        <div class="employee-form">
                            <h3 style="margin-bottom: 15px; font-size: 16px;">員工無法排班日期</h3>
                            <p style="font-size: 13px; color: #666; margin-bottom: 15px;">
                                員工可選擇無法上班的日期，同一天最多 2 位員工申報
                            </p>

                            <div class="form-group">
                                <label class="form-label">選擇員工</label>
                                <select class="form-select" id="employeeSelect" onchange="updateEmployeeCalendar()">
                                    <option value="">-- 請選擇員工 --</option>
                                </select>
                            </div>

                            <div class="form-group">
                                <label class="form-label">選擇無法排班的日期</label>
                                <div class="availability-calendar" id="availabilityCalendar"></div>
                            </div>

                            <div class="form-group">
                                <label class="form-label">選擇班別</label>
                                <div class="time-slot-selector" id="timeSlotSelector">
                                </div>
                            </div>

                            <button class="btn btn-primary" onclick="submitUnavailability()" style="margin-right: 10px;">
                                <i class="fas fa-check"></i> 確認無法排班
                            </button>
                            <button class="btn btn-danger" onclick="clearPendingUnavailability()">
                                <i class="fas fa-times"></i> 清空預選
                            </button>
                        </div>

                        <div class="availability-view">
                            <h3 style="margin-bottom: 15px; font-size: 16px;">本週無法排班統計</h3>
                            
                            <div id="unavailableDatesList" style="background: white; border-radius: 8px; max-height: 300px; overflow-y: auto;">
                            </div>

                            
                            <div style="margin-top:30px;">
                                <h3 style="margin-bottom:15px;font-size:16px;">
                                    員工排休一覽表
                                </h3>

                                <div style="overflow:auto;">
                                    <table class="leave-preview-table">
                                        <thead>
                                            <tr>
                                                <th>日期</th>
                                                <th>員工</th>
                                                <th>班別</th>
                                            </tr>
                                        </thead>
                                        <tbody id="leavePreviewTableBody">
                                        </tbody>
                                    </table>
                                </div>
                                
                                <div id="leaveTablePagination" style="display: flex; justify-content: center; align-items: center; gap: 10px; margin-top: 15px;">
                                </div>
                            </div>


                            <div class="availability-grid">
                                <div class="availability-stat">
                                    <div class="stat-number" id="totalUnavailable">0</div>
                                    <div class="stat-label">申報人次</div>
                                </div>
                                <div class="availability-stat">
                                    <div class="stat-number" id="fullCapacity">0</div>
                                    <div class="stat-label">已滿額日期</div>
                                </div>
                                <div class="availability-stat">
                                    <div class="stat-number" id="availableSlots">7</div>
                                    <div class="stat-label">可排班日期</div>
                                </div>
                                <div class="availability-stat">
                                    <div class="stat-number" id="avgUnavailable">0%</div>
                                    <div class="stat-label">平均無法率</div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>

                <!-- 甘特圖 -->
                <div class="tab-content" id="gantt">
                    <h2 style="margin-bottom: 10px;" id="ganttTitle">班表甘特圖</h2>
                    
                    <div style="margin-bottom: 15px; display: flex; gap: 10px; align-items: center;">
                        <button class="btn btn-secondary" onclick="previousGanttDay()">
                            <i class="fas fa-chevron-left"></i> 前一天
                        </button>
                        <input type="date" id="ganttDatePicker" onchange="updateGanttDate()" min="1900-01-01" max="2100-12-31" style="padding: 8px 12px; border: 1px solid #ddd; border-radius: 6px; font-size: 14px;">
                        <button class="btn btn-secondary" onclick="nextGanttDay()">
                            下一天 <i class="fas fa-chevron-right"></i>
                        </button>
                    </div>
                    
                    <div class="export-buttons">
                        <button class="btn btn-primary" onclick="showAllDaySchedule(getGanttDateStr())">
                            <i class="fas fa-calendar-check"></i> 當天排班詳情
                        </button>
                        <button class="btn btn-primary" onclick="showDayWageCalculation(getGanttDateStr())">
                            <i class="fas fa-calculator"></i> 當天薪資計算
                        </button>
                        <button class="btn btn-primary" onclick="showMonthlyHours()">
                            <i class="fas fa-chart-bar"></i> 月度統計
                        </button>
                        <button class="btn btn-primary" onclick="exportGanttPDF()">
                            <i class="fas fa-download"></i> 匯出 PDF
                        </button>
                        <button class="btn btn-primary" onclick="exportGanttExcel()">
                            <i class="fas fa-file-excel"></i> 匯出 Excel
                        </button>
                        <button class="btn btn-secondary" onclick="printGantt()">
                            <i class="fas fa-print"></i> 列印
                        </button>
                    </div>

                    <div class="gantt-container" id="ganttContainer">
                    </div>
                </div>

                <!-- 系統設定 -->
                <div class="tab-content" id="settings">
                    <h2 style="margin-bottom: 20px;">系統設定</h2>
                    
                    <div class="settings-card">
                        <h3><i class="fas fa-users"></i> 員工管理</h3>
                        <div id="employeesList"></div>
                        <button class="btn btn-primary" onclick="openAddEmployeeModal()" style="margin-top: 15px; width: 100%;">
                            <i class="fas fa-plus"></i> 新增員工
                        </button>
                    </div>

                    <div class="settings-card">
                        <h3><i class="fas fa-clock"></i> 班別設定</h3>
                        <div id="shiftsList"></div>
                        <button class="btn btn-primary" onclick="openAddShiftModal()" style="margin-top: 15px; width: 100%;">
                            <i class="fas fa-plus"></i> 新增班別
                        </button>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <!-- 排班模態框 -->
    <div class="modal" id="scheduleModal">
        <div class="modal-content">
            <button class="modal-close" onclick="closeModal('scheduleModal')">&times;</button>
            <div class="modal-header">
                <i class="fas fa-plus"></i> 新增排班
            </div>

            <div class="form-group">
                <label class="form-label">選擇員工</label>
                <select class="form-select" id="modalEmployeeSelect">
                    <option value="">-- 請選擇 --</option>
                </select>
            </div>

            <div class="form-group">
                <label class="form-label">選擇班別</label>
                <div class="shift-selector" id="shiftSelector">
                </div>
                <div style="margin-top: 10px; text-align: center;">
                    <button class="btn btn-secondary" onclick="toggleCustomShift()" style="width: 100%;">
                        <i class="fas fa-clock"></i> 自訂班別時間
                    </button>
                </div>
            </div>

            <div class="form-group" id="customShiftDiv" style="display: none;">
                <label class="form-label">自訂班別</label>
                <div style="display: flex; gap: 10px; margin-bottom: 10px;">
                    <div style="flex: 1;">
                        <label style="font-size: 12px; color: #666; display: block; margin-bottom: 5px;">開始時間</label>
                        <input type="time" class="form-input" id="customShiftStart">
                    </div>
                    <div style="flex: 1;">
                        <label style="font-size: 12px; color: #666; display: block; margin-bottom: 5px;">結束時間</label>
                        <input type="time" class="form-input" id="customShiftEnd">
                    </div>
                </div>
                <div>
                    <label style="font-size: 12px; color: #666; display: block; margin-bottom: 5px;">班別名稱（選填）</label>
                    <input type="text" class="form-input" id="customShiftName" placeholder="例：加班、特殊班">
                </div>
            </div>

            <button class="btn btn-primary" onclick="confirmSchedule()" style="width: 100%; margin-bottom: 10px;">
                <i class="fas fa-check"></i> 確認排班
            </button>
            <button class="btn btn-danger" onclick="openDeleteScheduleModal()" style="width: 100%;">
                <i class="fas fa-trash"></i> 刪除排班
            </button>
        </div>
    </div>

    <!-- 刪除排班模態框 -->
    <div class="modal" id="deleteScheduleModal">
        <div class="modal-content">
            <button class="modal-close" onclick="closeModal('deleteScheduleModal')">&times;</button>
            <div class="modal-header">
                <i class="fas fa-trash"></i> 刪除排班
            </div>

            <div class="form-group">
                <label class="form-label">選擇員工</label>
                <select class="form-select" id="deleteEmployeeSelect">
                    <option value="">-- 請選擇 --</option>
                </select>
            </div>

            <div class="form-group">
                <label class="form-label">選擇班別</label>
                <div id="deleteShiftSelector" style="display: flex; flex-direction: column; gap: 8px;">
                </div>
            </div>

            <button class="btn btn-danger" onclick="confirmDeleteSchedule()" style="width: 100%;">
                <i class="fas fa-check"></i> 確認刪除
            </button>
        </div>
    </div>

    <!-- 員工當天班別詳情模態框 -->
    <div class="modal" id="employeeDayScheduleModal">
        <div class="modal-content">
            <button class="modal-close" onclick="closeModal('employeeDayScheduleModal')">&times;</button>
            <div class="modal-header">
                <i class="fas fa-calendar-check"></i> 當天排班詳情
            </div>

            <div id="employeeDayScheduleContent" style="padding: 15px; background: white; border-radius: 6px; margin-bottom: 20px;">
            </div>

            <div style="display: flex; gap: 10px; margin-bottom: 15px;">
                <button class="btn btn-primary" onclick="exportDaySchedulePDF()" style="flex: 1;">
                    <i class="fas fa-download"></i> 匯出 PDF
                </button>
                <button class="btn btn-primary" onclick="exportDayScheduleExcel()" style="flex: 1;">
                    <i class="fas fa-file-excel"></i> 匯出 Excel
                </button>
                <button class="btn btn-secondary" onclick="printDaySchedule()" style="flex: 1;">
                    <i class="fas fa-print"></i> 列印
                </button>
            </div>

            <button class="btn btn-secondary" onclick="closeModal('employeeDayScheduleModal')" style="width: 100%;">
                <i class="fas fa-times"></i> 關閉
            </button>
        </div>
    </div>

    <!-- 月度排班統計模態框 -->
    <div class="modal" id="monthlyHoursModal">
        <div class="modal-content">
            <button class="modal-close" onclick="closeModal('monthlyHoursModal')">&times;</button>
            <div class="modal-header">
                <i class="fas fa-chart-bar"></i> 月度排班統計
            </div>

            <!-- 月份選擇器 -->
            <div style="padding: 15px; background: #f9f9f9; border-radius: 6px; margin-bottom: 15px; display: flex; align-items: center; gap: 10px; justify-content: center;">
                <button class="btn btn-primary" onclick="changeMonthlyStatsMonth(-1)" style="padding: 8px 12px; font-size: 12px;">
                    <i class="fas fa-chevron-left"></i> 上月
                </button>
                <input type="month" id="monthlyStatsMonthPicker" style="padding: 8px 12px; border: 1px solid #ddd; border-radius: 4px; font-size: 14px;" onchange="updateMonthlyStatsDisplay()">
                <button class="btn btn-primary" onclick="changeMonthlyStatsMonth(1)" style="padding: 8px 12px; font-size: 12px;">
                    下月 <i class="fas fa-chevron-right"></i>
                </button>
            </div>

            <div id="monthlyHoursContent" style="padding: 15px; background: white; border-radius: 6px; margin-bottom: 20px;">
            </div>

            <button class="btn btn-secondary" onclick="closeModal('monthlyHoursModal')" style="width: 100%;">
                <i class="fas fa-times"></i> 關閉
            </button>
        </div>
    </div>

    <!-- 當天薪資計算模態框 -->
    <div class="modal" id="dayWageModal">
        <div class="modal-content">
            <button class="modal-close" onclick="closeModal('dayWageModal')">&times;</button>
            <div class="modal-header">
                <i class="fas fa-calculator"></i> 當天薪資計算
            </div>

            <div id="dayWageContent" style="padding: 15px; background: white; border-radius: 6px; margin-bottom: 20px;">
            </div>

            <button class="btn btn-secondary" onclick="closeModal('dayWageModal')" style="width: 100%;">
                <i class="fas fa-times"></i> 關閉
            </button>
        </div>
    </div>

    <!-- 新增員工模態框 -->
    <div class="modal" id="addEmployeeModal">
        <div class="modal-content">
            <button class="modal-close" onclick="closeModal('addEmployeeModal')">&times;</button>
            <div class="modal-header">
                <i class="fas fa-user-plus"></i> 新增員工
            </div>

            <div class="form-group">
                <label class="form-label">員工姓名</label>
                <input type="text" class="form-input" id="newEmployeeName" placeholder="請輸入員工姓名">
            </div>

            <div class="form-group">
                <label class="form-label">職位</label>
                <input type="text" class="form-input" id="newEmployeeRole" placeholder="例：經理、正職員工">
            </div>

            <div class="form-group">
                <label class="form-label">員工照片</label>
                <input type="file" class="form-input" id="newEmployeePhoto" accept="image/*">
                <div id="photoPreview" style="margin-top: 10px; text-align: center;"></div>
            </div>

            <button class="btn btn-primary" onclick="confirmAddEmployee()" style="width: 100%;">
                <i class="fas fa-check"></i> 新增員工
            </button>
        </div>
    </div>

    <!-- 新增班別模態框 -->
    <div class="modal" id="addShiftModal">
        <div class="modal-content">
            <button class="modal-close" onclick="closeModal('addShiftModal')">&times;</button>
            <div class="modal-header">
                <i class="fas fa-plus"></i> 新增班別
            </div>

            <div class="form-group">
                <label class="form-label">班別名稱</label>
                <input type="text" class="form-input" id="newShiftName" placeholder="例：早班">
            </div>

            <div class="form-group">
                <label class="form-label">開始時間</label>
                <input type="time" class="form-input" id="newShiftStart">
            </div>

            <div class="form-group">
                <label class="form-label">結束時間</label>
                <input type="time" class="form-input" id="newShiftEnd">
            </div>

            <button class="btn btn-primary" onclick="confirmAddShift()" style="width: 100%;">
                <i class="fas fa-check"></i> 新增班別
            </button>
        </div>
    </div>

    <script>
        // ===== 數據初始化 =====
        const defaultShifts = {
            morning: { name: '早班', start: '08:00', end: '16:00', id: 'morning' },
            afternoon: { name: '中班', start: '12:00', end: '20:00', id: 'afternoon' },
            evening: { name: '晚班', start: '16:00', end: '24:00', id: 'evening' }
        };

        const defaultEmployees = [
            { id: 1, name: '王小明', avatar: 'WX', role: '經理', photo: null, hourlyRate: 200 },
            { id: 2, name: '林怡芬', avatar: 'LY', role: '正職員工', photo: null, hourlyRate: 180 },
            { id: 3, name: '陳建誠', avatar: 'CC', role: '正職員工', photo: null, hourlyRate: 180 },
            { id: 4, name: '張家豪', avatar: 'ZJ', role: '正職員工', photo: null, hourlyRate: 180 },
            { id: 5, name: '李雪晴', avatar: 'LX', role: '正職員工', photo: null, hourlyRate: 180 },
            { id: 6, name: '黃琬婷', avatar: 'HW', role: '計時員工', photo: null, hourlyRate: 150 },
            { id: 7, name: '吳柏亨', avatar: 'WB', role: '計時員工', photo: null, hourlyRate: 150 },
            { id: 8, name: '何宜諺', avatar: 'HY', role: '計時員工', photo: null, hourlyRate: 150 }
        ];

        // ===== IndexedDB初始化（用於存儲員工照片） =====
        let photoDB = null;
        
        function initializePhotoDatabase() {
            return new Promise((resolve, reject) => {
                const request = indexedDB.open('EmployeePhotoDB', 1);
                
                request.onerror = function() {
                    console.error('IndexedDB初始化失敗');
                    reject();
                };
                
                request.onsuccess = function(event) {
                    photoDB = event.target.result;
                    console.log('IndexedDB已初始化');
                    resolve();
                };
                
                request.onupgradeneeded = function(event) {
                    const db = event.target.result;
                    if (!db.objectStoreNames.contains('photos')) {
                        db.createObjectStore('photos', { keyPath: 'empId' });
                    }
                };
            });
        }
        
        function savePhotoToIndexedDB(empId, photoData) {
            if (!photoDB) return;
            
            try {
                const transaction = photoDB.transaction(['photos'], 'readwrite');
                const store = transaction.objectStore('photos');
                store.put({ empId: empId, photo: photoData });
            } catch (e) {
                console.error('保存照片到IndexedDB失敗:', e);
            }
        }
        
        function getPhotoFromIndexedDB(empId) {
            return new Promise((resolve) => {
                if (!photoDB) {
                    resolve(null);
                    return;
                }
                
                try {
                    const transaction = photoDB.transaction(['photos'], 'readonly');
                    const store = transaction.objectStore('photos');
                    const request = store.get(empId);
                    
                    request.onsuccess = function(event) {
                        if (event.target.result) {
                            resolve(event.target.result.photo);
                        } else {
                            resolve(null);
                        }
                    };
                    
                    request.onerror = function() {
                        resolve(null);
                    };
                } catch (e) {
                    console.error('讀取照片失敗:', e);
                    resolve(null);
                }
            });
        }
        
        function deletePhotoFromIndexedDB(empId) {
            if (!photoDB) return;
            
            try {
                const transaction = photoDB.transaction(['photos'], 'readwrite');
                const store = transaction.objectStore('photos');
                store.delete(empId);
            } catch (e) {
                console.error('刪除照片失敗:', e);
            }
        }

        let shifts = JSON.parse(localStorage.getItem('shifts')) || defaultShifts;
        let employees = JSON.parse(localStorage.getItem('employees')) || defaultEmployees;
        let currentWeek = 0;
        let scheduleData = JSON.parse(localStorage.getItem('scheduleData')) || {};
        let unavailableData = JSON.parse(localStorage.getItem('unavailableData')) || {};
        
        // 驗證載入的數據完整性
        if (!shifts || Object.keys(shifts).length === 0) {
            shifts = defaultShifts;
        }
        if (!employees || employees.length === 0) {
            employees = defaultEmployees;
        }
        if (!scheduleData || typeof scheduleData !== 'object') {
            scheduleData = {};
        }
        if (!unavailableData || typeof unavailableData !== 'object') {
            unavailableData = {};
        }
        let selectedShift = null;
        let selectedEmployee = null;
        let selectedShiftType = null;
        let selectedTimeSlots = [];
        let pendingUnavailableSelections = [];
        let currentUser = 'manager';

        // ===== 初始化 =====
        document.addEventListener('DOMContentLoaded', function() {
            // 初始化IndexedDB用於存儲照片
            initializePhotoDatabase().then(() => {
                // 從IndexedDB載入所有員工照片
                employees.forEach(emp => {
                    getPhotoFromIndexedDB(emp.id).then(photoData => {
                        if (photoData) {
                            emp.photo = photoData;
                            // 重新渲染以顯示載入的照片
                            renderEmployeeList();
                            renderSettingsPage();
                        }
                    });
                });
            }).catch(() => {
                console.warn('IndexedDB初始化失敗，照片將無法保留');
            });
            
            // 確保從localStorage載入最新數據
            try {
                const savedShifts = localStorage.getItem('shifts');
                const savedEmployees = localStorage.getItem('employees');
                const savedScheduleData = localStorage.getItem('scheduleData');
                const savedUnavailableData = localStorage.getItem('unavailableData');
                
                if (savedShifts) {
                    shifts = JSON.parse(savedShifts);
                    console.log('已載入班別數據');
                }
                if (savedEmployees) {
                    employees = JSON.parse(savedEmployees);
                    console.log('已載入員工數據');
                }
                if (savedScheduleData) {
                    scheduleData = JSON.parse(savedScheduleData);
                    console.log('已載入排班數據');
                }
                if (savedUnavailableData) {
                    unavailableData = JSON.parse(savedUnavailableData);
                    console.log('已載入休假數據');
                }
            } catch (e) {
                console.error('載入保存的數據時出錯:', e);
            }
            
            updateDateTime();
            setInterval(updateDateTime, 60000);
            cleanupDeletedEmployeesFromSchedule();  // 清理已刪除員工的排班
            initializeScheduleData();
            renderEmployeeList();
            renderCalendarGrid();
            renderEmployeeSelect();
            renderGanttChart();
            renderSettingsPage();
            renderTimeSlotButtons();
            renderAvailabilityCalendar();
            renderLeavePreviewTable();
            updateStatistics();
            setupRoleListener();
            window.isCustomShift = false;  // 初始化自訂班別狀態
            saveData();
        });

        // 確保離開頁面前保存數據
        window.onbeforeunload = function() {
            saveData();
        };
        
        // 定時自動保存（每30秒）
        setInterval(function() {
            saveData();
        }, 30000);
        
        // ===== IndexedDB 初始化用於存儲照片 =====
        let photoDatabase = null;
        
        function initPhotoDatabase() {
            return new Promise((resolve, reject) => {
                const request = indexedDB.open('SchedulingSystem', 1);
                
                request.onerror = function() {
                    console.error('無法打開IndexedDB');
                    reject(request.error);
                };
                
                request.onsuccess = function() {
                    photoDatabase = request.result;
                    console.log('IndexedDB已初始化');
                    resolve(photoDatabase);
                };
                
                request.onupgradeneeded = function(event) {
                    const db = event.target.result;
                    if (!db.objectStoreNames.contains('photos')) {
                        db.createObjectStore('photos', { keyPath: 'empId' });
                        console.log('已創建photos物件存儲');
                    }
                };
            });
        }
        
        function savePhotoToDatabase(empId, photoData) {
            return new Promise((resolve, reject) => {
                if (!photoDatabase) {
                    reject('IndexedDB未初始化');
                    return;
                }
                
                const transaction = photoDatabase.transaction(['photos'], 'readwrite');
                const store = transaction.objectStore('photos');
                const request = store.put({ empId: empId, photo: photoData });
                
                request.onsuccess = function() {
                    console.log('照片已保存到IndexedDB:', empId);
                    resolve();
                };
                
                request.onerror = function() {
                    console.error('保存照片失敗:', request.error);
                    reject(request.error);
                };
            });
        }
        
        function getPhotoFromDatabase(empId) {
            return new Promise((resolve, reject) => {
                if (!photoDatabase) {
                    reject('IndexedDB未初始化');
                    return;
                }
                
                const transaction = photoDatabase.transaction(['photos'], 'readonly');
                const store = transaction.objectStore('photos');
                const request = store.get(empId);
                
                request.onsuccess = function() {
                    if (request.result) {
                        resolve(request.result.photo);
                    } else {
                        resolve(null);
                    }
                };
                
                request.onerror = function() {
                    console.error('讀取照片失敗:', request.error);
                    reject(request.error);
                };
            });
        }
        
        function deletePhotoFromDatabase(empId) {
            return new Promise((resolve, reject) => {
                if (!photoDatabase) {
                    reject('IndexedDB未初始化');
                    return;
                }
                
                const transaction = photoDatabase.transaction(['photos'], 'readwrite');
                const store = transaction.objectStore('photos');
                const request = store.delete(empId);
                
                request.onsuccess = function() {
                    console.log('照片已從IndexedDB刪除:', empId);
                    resolve();
                };
                
                request.onerror = function() {
                    console.error('刪除照片失敗:', request.error);
                    reject(request.error);
                };
            });
        }

        
        function deleteEmployee(empId) {
            const employee = employees.find(e => Number(e.id) === Number(empId));

            if (!employee) {
                alert('找不到員工');
                return;
            }

            const confirmDelete = confirm(`確定要刪除員工「${employee.name}」嗎？`);

            if (!confirmDelete) return;

            // 從 employees 移除
            employees = employees.filter(e => Number(e.id) !== Number(empId));

            // 清除所有排班資料
            Object.keys(scheduleData).forEach(dateKey => {
                Object.keys(scheduleData[dateKey]).forEach(shiftKey => {

                    const shiftData = scheduleData[dateKey][shiftKey];

                    if (Array.isArray(shiftData)) {

                        scheduleData[dateKey][shiftKey] = shiftData.filter(emp => {
                            const currentId =
                                typeof emp === 'object'
                                    ? Number(emp.id)
                                    : Number(emp);

                            return currentId !== Number(empId);
                        });

                        if (scheduleData[dateKey][shiftKey].length === 0) {
                            delete scheduleData[dateKey][shiftKey];
                        }
                    }
                });

                if (Object.keys(scheduleData[dateKey]).length === 0) {
                    delete scheduleData[dateKey];
                }
            });

            // 清除休假資料
            Object.keys(unavailableData).forEach(dateKey => {

                unavailableData[dateKey] = unavailableData[dateKey].filter(item => {

                    const currentId =
                        typeof item === 'object'
                            ? Number(item.empId)
                            : Number(item);

                    return currentId !== Number(empId);
                });

                if (unavailableData[dateKey].length === 0) {
                    delete unavailableData[dateKey];
                }
            });

            // 刪除照片
            try {
                deletePhotoFromIndexedDB(empId);
            } catch(e) {
                console.log('刪除照片失敗', e);
            }

            saveData();

            renderEmployeeList();
            renderCalendarGrid();
            renderEmployeeSelect();
            renderSettingsPage();
            renderGanttChart();
            updateStatistics();

            alert('員工已刪除');
        }


        
        // 🔴 統一日期格式化函數 - 使用本地時區
        function getLocalDateString(date) {
            const year = date.getFullYear();
            const month = String(date.getMonth() + 1).padStart(2, '0');
            const day = String(date.getDate()).padStart(2, '0');
            return `${year}-${month}-${day}`;
        }

        function formatDateDisplay(dateStr) {
            const date = new Date(dateStr + 'T00:00:00');
            const month = String(date.getMonth() + 1).padStart(2, '0');
            const day = String(date.getDate()).padStart(2, '0');
            const week = ['日','一','二','三','四','五','六'][date.getDay()];
            return `${month}/${day} (${week})`;
        }

        function renderLeavePreviewTable() {
            const tbody = document.getElementById('leavePreviewTableBody');

            if (!tbody) return;

            tbody.innerHTML = '';

            // 確保unavailableData已初始化
            if (!unavailableData || typeof unavailableData !== 'object') {
                unavailableData = {};
            }

            const rows = [];

            Object.keys(unavailableData).forEach(dateKey => {
                // 確保該日期的數據是數組
                if (!Array.isArray(unavailableData[dateKey])) {
                    return;
                }

                unavailableData[dateKey].forEach(item => {

                    const empId = typeof item === 'object'
                        ? item.empId
                        : item;

                    const shift = typeof item === 'object'
                        ? item.shift
                        : '全天';

                    const emp = employees.find(e => Number(e.id) === Number(empId));

                    if (emp) {
                        rows.push({
                            date: dateKey,
                            employee: emp.name,
                            shift: shift
                        });
                    }
                });
            });

            rows.sort((a,b)=>a.date.localeCompare(b.date));

            if (rows.length === 0) {
                tbody.innerHTML = `
                    <tr>
                        <td colspan="3" style="color:#999;">
                            尚無排休申請
                        </td>
                    </tr>
                `;
                // 隱藏分頁控制
                const paginationDiv = document.getElementById('leaveTablePagination');
                if (paginationDiv) paginationDiv.style.display = 'none';
                return;
            }

            // 分頁設定
            const pageSize = 10;  // 每頁顯示10筆
            const totalPages = Math.ceil(rows.length / pageSize);
            
            // 初始化或獲取當前頁碼
            if (!window.leaveTableCurrentPage) {
                window.leaveTableCurrentPage = 1;
            }
            
            // 確保頁碼有效
            if (window.leaveTableCurrentPage > totalPages) {
                window.leaveTableCurrentPage = totalPages;
            }
            
            // 計算當前頁的開始和結束索引
            const startIndex = (window.leaveTableCurrentPage - 1) * pageSize;
            const endIndex = startIndex + pageSize;
            const pageRows = rows.slice(startIndex, endIndex);

            // 渲染當前頁的行
            pageRows.forEach(row => {
                tbody.innerHTML += `
                    <tr>
                        <td>${formatDateDisplay(row.date)}</td>
                        <td>${row.employee}</td>
                        <td>${row.shift}</td>
                    </tr>
                `;
            });

            // 如果只有一頁，隱藏分頁
            if (totalPages <= 1) {
                const paginationDiv = document.getElementById('leaveTablePagination');
                if (paginationDiv) paginationDiv.style.display = 'none';
            } else {
                // 顯示分頁控制
                renderLeaveTablePagination(totalPages, window.leaveTableCurrentPage);
            }
        }

        function renderLeaveTablePagination(totalPages, currentPage) {
            const paginationDiv = document.getElementById('leaveTablePagination');
            if (!paginationDiv) return;
            
            paginationDiv.style.display = 'flex';
            paginationDiv.innerHTML = '';
            
            // 上一頁按鈕
            const prevBtn = document.createElement('button');
            prevBtn.textContent = '< 上一頁';
            prevBtn.className = 'btn btn-secondary';
            prevBtn.style.marginRight = '10px';
            prevBtn.disabled = currentPage === 1;
            prevBtn.onclick = () => {
                if (window.leaveTableCurrentPage > 1) {
                    window.leaveTableCurrentPage--;
                    renderLeavePreviewTable();
                }
            };
            paginationDiv.appendChild(prevBtn);
            
            // 頁碼顯示
            const pageInfo = document.createElement('span');
            pageInfo.textContent = `第 ${currentPage} / ${totalPages} 頁`;
            pageInfo.style.display = 'flex';
            pageInfo.style.alignItems = 'center';
            pageInfo.style.marginRight = '10px';
            pageInfo.style.fontSize = '13px';
            pageInfo.style.color = '#666';
            paginationDiv.appendChild(pageInfo);
            
            // 下一頁按鈕
            const nextBtn = document.createElement('button');
            nextBtn.textContent = '下一頁 >';
            nextBtn.className = 'btn btn-secondary';
            nextBtn.disabled = currentPage === totalPages;
            nextBtn.onclick = () => {
                if (window.leaveTableCurrentPage < totalPages) {
                    window.leaveTableCurrentPage++;
                    renderLeavePreviewTable();
                }
            };
            paginationDiv.appendChild(nextBtn);
        }



        function updateDateTime() {
            const now = new Date();
            const year = now.getFullYear();
            const month = String(now.getMonth() + 1).padStart(2, '0');
            const day = String(now.getDate()).padStart(2, '0');
            const hours = String(now.getHours()).padStart(2, '0');
            const minutes = String(now.getMinutes()).padStart(2, '0');
            document.getElementById('currentDateTime').textContent = `${year}年${month}月${day}日 ${hours}:${minutes}`;
        }

        function initializeScheduleData() {
            const today = new Date();
            const weekStart = new Date(today);
            weekStart.setDate(weekStart.getDate() - weekStart.getDay() + 1 + currentWeek * 7);
            
            // 初始化甘特圖日期為週排班的第一天
            if (!window.ganttDate) {
                window.ganttDate = new Date(weekStart);
            }
            
            for (let i = 0; i < 7; i++) {
                const date = new Date(weekStart);
                date.setDate(date.getDate() + i);
                const y = date.getFullYear();
                const m = String(date.getMonth() + 1).padStart(2, '0');
                const d = String(date.getDate()).padStart(2, '0');
                const key = `${y}-${m}-${d}`;
                if (!scheduleData[key]) {
                    scheduleData[key] = {};
                }
            }

            updateSchedulingTitle();
        }

        function updateSchedulingTitle() {
            const today = new Date();
            const weekStart = new Date(today);
            weekStart.setDate(weekStart.getDate() - weekStart.getDay() + 1 + currentWeek * 7);
            const weekEnd = new Date(weekStart);
            weekEnd.setDate(weekEnd.getDate() + 6);

            const format = (date) => {
                const year = date.getFullYear();
                const month = String(date.getMonth() + 1).padStart(2, '0');
                const day = String(date.getDate()).padStart(2, '0');
                return `${year}/${month}/${day}`;
            };

            document.getElementById('schedulingTitle').textContent = `週排班（${format(weekStart)} - ${format(weekEnd)}）`;
            document.getElementById('ganttTitle').textContent = `班表甘特圖 - ${format(weekStart)}`;
        }

        function renderTimeSlotButtons() {
            const container = document.getElementById('timeSlotSelector');
            container.innerHTML = '';
            
            const shiftIds = Object.keys(shifts);
            shiftIds.forEach((shiftId, idx) => {
                const shiftInfo = shifts[shiftId];
                const btn = document.createElement('button');
                btn.className = 'time-slot-btn';
                if (idx === 0) btn.classList.add('selected');
                btn.textContent = shiftInfo.name;
                btn.onclick = () => selectTimeSlot(btn, shiftId);
                container.appendChild(btn);
            });
            
            // 新增「其他」選項
            const otherBtn = document.createElement('button');
            otherBtn.className = 'time-slot-btn';
            otherBtn.textContent = '其他（全天）';
            otherBtn.onclick = () => selectTimeSlot(otherBtn, 'all');
            container.appendChild(otherBtn);
        }

        function selectTimeSlot(element, slot) {
            document.querySelectorAll('#timeSlotSelector .time-slot-btn').forEach(el => el.classList.remove('selected'));
            element.classList.add('selected');
            selectedTimeSlots = [slot];
        }

        
        function renderEmployeeList() {
            const list = document.getElementById('employeeList');
            list.innerHTML = '<div style="text-align: center; color: #999; font-size: 12px; margin-bottom: 10px;">員工列表</div>';
            
            employees.forEach(emp => {
                const div = document.createElement('div');
                div.className = 'employee-item';
                let avatarHtml = `<div class="employee-avatar">`;
                if (emp.photo) {
                    avatarHtml += `<img src="${emp.photo}" alt="${emp.name}">`;
                } else {
                    avatarHtml += emp.avatar;
                }
                avatarHtml += `</div>`;
                
                div.innerHTML = `
                    <div style="display: flex; align-items: center; width: 100%; pointer-events: auto;">
                        ${avatarHtml}
                        <div style="flex: 1; pointer-events: auto;">
                            <div style="font-weight: 600; font-size: 13px;">${emp.name}</div>
                            <div style="font-size: 11px; color: #999;">${emp.role}</div>
                        </div>
                    </div>
                `;
                div.style.pointerEvents = 'auto';
                div.onclick = () => selectEmployee(emp);
                list.appendChild(div);
            });
        }

        function renderCalendarGrid() {
            const grid = document.getElementById('calendarGrid');
            grid.innerHTML = '';
            
            const today = new Date();
            const weekStart = new Date(today);
            weekStart.setDate(weekStart.getDate() - weekStart.getDay() + 1 + currentWeek * 7);
            const daysOfWeek = ['一', '二', '三', '四', '五', '六', '日'];
            
            for (let i = 0; i < 7; i++) {
                const date = new Date(weekStart);
                date.setDate(date.getDate() + i);
                const dateStr = getLocalDateString(date);
                const dayOfWeek = daysOfWeek[i];
                const month = String(date.getMonth() + 1).padStart(2, '0');
                const dateNum = String(date.getDate()).padStart(2, '0');
                
                const col = document.createElement('div');
                col.className = 'day-column';
                
                const dateObj = scheduleData[dateStr] || {};
                
                col.innerHTML = `
                    <div class="day-header">${month}月${dateNum}日(${dayOfWeek})</div>
                    <div class="day-date">${dateStr}</div>
                    <div class="day-slots" id="slots-${dateStr}">
                        ${createShiftSlots(dateStr, dateObj)}
                    </div>
                `;
                
                grid.appendChild(col);
            }
        }

        function createShiftSlots(dateStr, dateObj) {
            let html = '';
            const shiftIds = Object.keys(shifts);
            
            shiftIds.forEach(shiftId => {
                const shiftInfo = shifts[shiftId];
                
                // 如果是自訂班別（custom_開頭），只在建立當天顯示
                if (shiftId.startsWith('custom_')) {
                    // 自訂班別存儲時會有日期信息，檢查是否為當前日期
                    if (shifts[shiftId].dateCreated !== dateStr) {
                        return; // 跳過非當天的自訂班別
                    }
                }
                
                const shiftData = dateObj[shiftId];
                
                if (shiftData && Array.isArray(shiftData) && shiftData.length > 0) {
                    // 顯示已排班的員工，並過濾掉不存在的員工
                    const validEmployees = [];
                    const invalidEmployees = [];
                    
                    shiftData.forEach((emp) => {
                        // 提取員工ID，支持多種格式：完整對象或純ID
                        let empId;
                        let empObject = emp;
                        
                        if (emp && typeof emp === 'object' && emp.id !== undefined) {
                            // 如果是完整的員工對象，使用其ID
                            empId = emp.id;
                            empObject = emp;
                        } else if (typeof emp === 'number' || typeof emp === 'string') {
                            // 如果是純ID，需要查找完整的員工對象
                            empId = emp;
                            const foundEmp = employees.find(e => e.id === Number(empId));
                            if (foundEmp) {
                                empObject = foundEmp;
                            } else {
                                // 如果找不到對應的員工，標記為無效
                                invalidEmployees.push(emp);
                                return;
                            }
                        } else {
                            // 無效的員工格式
                            invalidEmployees.push(emp);
                            return;
                        }
                        
                        // 檢查員工是否存在於系統中
                        const employeeExists = employees.find(e => e.id === Number(empId));
                        
                        if (employeeExists && empObject) {
                            validEmployees.push(empObject);
                        } else {
                            invalidEmployees.push(emp);
                        }
                    });
                    
                    // 更新scheduleData，移除已刪除的員工
                    if (invalidEmployees.length > 0) {
                        scheduleData[dateStr][shiftId] = validEmployees;
                        if (validEmployees.length === 0) {
                            delete scheduleData[dateStr][shiftId];
                        }
                        saveData();  // 立即保存
                    }
                    
                    // 顯示有效的員工
                    validEmployees.forEach((emp) => {
                        html += `
                            <div class="shift-slot filled" onclick="showEmployeeDaySchedule('${dateStr}', ${emp.id})" style="cursor: pointer;">
                                <div class="shift-employee">
                                    <div class="employee-avatar">
                                        ${emp.photo ? `<img src="${emp.photo}" alt="${emp.name}">` : emp.avatar}
                                    </div>
                                    <div>
                                        <div style="font-weight: 600; font-size: 11px;">${emp.name}</div>
                                        <div class="shift-time">${shiftInfo.start}-${shiftInfo.end}</div>
                                    </div>
                                </div>
                            </div>
                        `;
                    });
                    
                    // 添加按鈕允許添加更多員工
                    if (validEmployees.length > 0) {
                        html += `
                            <div class="shift-slot" onclick="openScheduleModal('${dateStr}', '${shiftId}')" style="opacity: 0.7; padding: 6px; min-height: 30px;">
                                <i class="fas fa-plus" style="font-size: 10px;"></i>
                            </div>
                        `;
                    } else {
                        // 如果所有員工都被刪除，顯示添加員工的按鈕
                        html += `
                            <div class="shift-slot" onclick="openScheduleModal('${dateStr}', '${shiftId}')">
                                <i class="fas fa-plus"></i> ${shiftInfo.name}
                            </div>
                        `;
                    }
                } else {
                    // 沒有員工排班
                    html += `
                        <div class="shift-slot" onclick="openScheduleModal('${dateStr}', '${shiftId}')">
                            <i class="fas fa-plus"></i> ${shiftInfo.name}
                        </div>
                    `;
                }
            });
            
            return html;
        }

        function selectEmployee(emp) {
            selectedEmployee = emp;
        }

        function openScheduleModal(dateStr, shiftType) {
            if (currentUser !== 'manager') {
                alert('只有主管可以進行排班');
                return;
            }
            selectedShift = { date: dateStr, type: shiftType };
            selectedShiftType = shiftType;
            document.getElementById('scheduleModal').classList.add('active');
            renderModalEmployeeSelect();
            renderShiftSelector();
        }

        function openDeleteScheduleModal() {
            if (currentUser !== 'manager') {
                alert('只有主管可以刪除排班');
                return;
            }
            if (!selectedShift) {
                alert('請先選擇日期和班別');
                return;
            }
            
            // 渲染刪除排班模態框
            const deleteSelect = document.getElementById('deleteEmployeeSelect');
            deleteSelect.innerHTML = '<option value="">-- 請選擇 --</option>';
            
            // 獲取該日期該班別的所有員工
            const shiftData = scheduleData[selectedShift.date]?.[selectedShift.type];
            if (!shiftData || (Array.isArray(shiftData) && shiftData.length === 0)) {
                alert('此班別目前沒有排班員工');
                return;
            }
            
            // 填充可刪除的員工
            if (Array.isArray(shiftData)) {
                shiftData.forEach(emp => {
                    const option = document.createElement('option');
                    option.value = emp.id;
                    option.textContent = emp.name;
                    deleteSelect.appendChild(option);
                });
            } else if (shiftData && shiftData.id) {
                const option = document.createElement('option');
                option.value = shiftData.id;
                option.textContent = shiftData.name;
                deleteSelect.appendChild(option);
            }
            
            document.getElementById('deleteScheduleModal').classList.add('active');
        }

        function confirmDeleteSchedule() {
            const empId = parseInt(document.getElementById('deleteEmployeeSelect').value);
            
            if (!empId) {
                alert('請選擇要刪除的員工');
                return;
            }
            
            if (!selectedShift) {
                alert('請先選擇日期和班別');
                return;
            }
            
            if (confirm('確定要刪除此排班嗎？')) {
                console.log(`confirmDeleteSchedule: 日期=${selectedShift.date}, 班別=${selectedShift.type}, 員工=${empId}`);
                
                // 確保date和shift都存在
                if (!scheduleData[selectedShift.date] || !scheduleData[selectedShift.date][selectedShift.type]) {
                    alert('找不到該排班記錄');
                    return;
                }
                
                const shiftData = scheduleData[selectedShift.date][selectedShift.type];
                
                if (!Array.isArray(shiftData)) {
                    alert('排班數據格式錯誤');
                    return;
                }
                
                // 找出要刪除的員工索引
                let deleteIndex = -1;
                for (let i = 0; i < shiftData.length; i++) {
                    const emp = shiftData[i];
                    const currentEmpId = emp && typeof emp === 'object' && emp.id !== undefined ? emp.id : emp;
                    
                    if (currentEmpId === empId || Number(currentEmpId) === Number(empId)) {
                        deleteIndex = i;
                        break;
                    }
                }
                
                if (deleteIndex === -1) {
                    alert('找不到該員工的排班記錄');
                    return;
                }
                
                // 刪除該員工
                shiftData.splice(deleteIndex, 1);
                console.log('刪除後長度:', shiftData.length);
                
                // 如果該班別沒有員工了，刪除該班別
                if (shiftData.length === 0) {
                    delete scheduleData[selectedShift.date][selectedShift.type];
                }
                
                // 如果該日期沒有班別了，刪除該日期
                if (Object.keys(scheduleData[selectedShift.date]).length === 0) {
                    delete scheduleData[selectedShift.date];
                }
                
                closeModal('deleteScheduleModal');
                renderCalendarGrid();
                renderGanttChart();
                updateStatistics();
                saveData();
                
                alert('排班已刪除');
            }
        }

        function renderModalEmployeeSelect() {
            const select = document.getElementById('modalEmployeeSelect');
            select.innerHTML = '<option value="">-- 請選擇 --</option>';
            
            employees.forEach(emp => {
                // 檢查該員工是否在選定日期有排休申請
                const isUnavailable = unavailableData[selectedShift.date]?.some(item => 
                    typeof item === 'object' ? item.empId === emp.id : item === emp.id
                );
                
                const option = document.createElement('option');
                option.value = emp.id;
                option.textContent = `${emp.name} (${emp.role})${isUnavailable ? ' [排休]' : ''}`;
                option.disabled = isUnavailable;  // 禁用排休員工
                select.appendChild(option);
            });
        }

        function renderShiftSelector() {
            const selector = document.getElementById('shiftSelector');
            selector.innerHTML = '';
            
            const shiftIds = Object.keys(shifts);
            shiftIds.forEach((shiftId, idx) => {
                const shiftInfo = shifts[shiftId];
                const div = document.createElement('div');
                div.className = 'shift-option';
                if (idx === 0) div.classList.add('selected');
                div.innerHTML = `
                    <div>${shiftInfo.name}</div>
                    <div style="font-size: 11px; color: #999;">${shiftInfo.start}-${shiftInfo.end}</div>
                `;
                div.onclick = () => selectShift(div, shiftId);
                selector.appendChild(div);
            });
        }

        function selectShift(element, shiftId) {
            document.querySelectorAll('.shift-option').forEach(el => el.classList.remove('selected'));
            element.classList.add('selected');
            selectedShiftType = shiftId;
            // 隱藏自訂班別輸入框
            document.getElementById('customShiftDiv').style.display = 'none';
            window.isCustomShift = false;
        }

        function toggleCustomShift() {
            const customDiv = document.getElementById('customShiftDiv');
            if (customDiv.style.display === 'none') {
                // 顯示自訂班別輸入框
                customDiv.style.display = 'block';
                // 取消預設班別選擇
                document.querySelectorAll('.shift-option').forEach(el => el.classList.remove('selected'));
                selectedShiftType = null;
                window.isCustomShift = true;
            } else {
                // 隱藏自訂班別輸入框
                customDiv.style.display = 'none';
                document.getElementById('customShiftStart').value = '';
                document.getElementById('customShiftEnd').value = '';
                document.getElementById('customShiftName').value = '';
                window.isCustomShift = false;
            }
        }

        function confirmSchedule() {
            const empSelect = document.getElementById('modalEmployeeSelect');
            const empId = parseInt(empSelect.value);
            
            if (!empId || !selectedShift) {
                alert('請完整填寫所有欄位');
                return;
            }
            
            // 檢查是使用自訂班別還是預設班別
            if (window.isCustomShift) {
                const startTime = document.getElementById('customShiftStart').value;
                const endTime = document.getElementById('customShiftEnd').value;
                const customName = document.getElementById('customShiftName').value || '自訂班';
                
                if (!startTime || !endTime) {
                    alert('請填寫開始和結束時間');
                    return;
                }
                
                // 檢查時間有效性
                if (startTime >= endTime) {
                    alert('結束時間必須晚於開始時間');
                    return;
                }
                
                selectedShiftType = 'custom_' + Date.now();
                // 臨時創建自訂班別，記錄創建日期
                shifts[selectedShiftType] = {
                    id: selectedShiftType,
                    name: customName,
                    start: startTime,
                    end: endTime,
                    dateCreated: selectedShift.date  // 記錄建立時的日期
                };
            } else if (!selectedShiftType) {
                alert('請選擇班別');
                return;
            }
            
            const emp = employees.find(e => e.id === empId);
            if (!emp) return;
            
            // 🔴 檢查該員工是否在該日期有排休
            const dateStr = selectedShift.date;
            if (unavailableData[dateStr] && unavailableData[dateStr].some(item => {
                const itemEmpId = typeof item === 'object' ? item.empId : item;
                return Number(itemEmpId) === Number(empId);
            })) {
                alert(`${emp.name} 在 ${dateStr} 已申請排休，無法排班`);
                return;
            }
            
            // 檢查是否為編輯模式
            const isEditingShift = window.editingEmployeeShift !== undefined;
            
            if (isEditingShift) {
                // 編輯模式：移除舊班別，添加新班別
                const oldShiftId = window.editingEmployeeShift.shiftId;
                const editDateStr = window.editingEmployeeShift.dateStr;
                const editEmpId = window.editingEmployeeShift.empId;
                
                console.log(`編輯模式: 移除舊班別 日期=${editDateStr}, 班別=${oldShiftId}, 員工=${editEmpId}`);
                
                // 從舊班別中移除該員工
                if (scheduleData[editDateStr] && scheduleData[editDateStr][oldShiftId]) {
                    const oldShiftData = scheduleData[editDateStr][oldShiftId];
                    if (Array.isArray(oldShiftData)) {
                        // 找出要刪除的員工索引
                        let deleteIndex = -1;
                        for (let i = 0; i < oldShiftData.length; i++) {
                            const emp = oldShiftData[i];
                            const currentEmpId = emp && typeof emp === 'object' && emp.id !== undefined ? emp.id : emp;
                            
                            if (currentEmpId === editEmpId || Number(currentEmpId) === Number(editEmpId)) {
                                deleteIndex = i;
                                break;
                            }
                        }
                        
                        if (deleteIndex !== -1) {
                            oldShiftData.splice(deleteIndex, 1);
                            console.log('從舊班別中移除員工成功');
                            
                            if (oldShiftData.length === 0) {
                                delete scheduleData[editDateStr][oldShiftId];
                            }
                        }
                    }
                }
                
                // 更新selectedShift為新的班別
                selectedShift = { date: editDateStr, type: selectedShiftType };
            } else {
                // 新增模式：檢查該員工是否已在該時段排班
                let shiftData = scheduleData[selectedShift.date][selectedShiftType];
                if (shiftData && Array.isArray(shiftData) && shiftData.some(e => e.id === empId)) {
                    alert('此員工已在此時段排班');
                    return;
                }
            }
            
            // 如果該時段沒有排班記錄，創建陣列
            if (!scheduleData[selectedShift.date][selectedShiftType]) {
                scheduleData[selectedShift.date][selectedShiftType] = [];
            }
            
            // 轉換為陣列格式
            if (!Array.isArray(scheduleData[selectedShift.date][selectedShiftType])) {
                const oldEmp = scheduleData[selectedShift.date][selectedShiftType];
                scheduleData[selectedShift.date][selectedShiftType] = [oldEmp];
            }
            
            // 添加員工到該時段
            scheduleData[selectedShift.date][selectedShiftType].push(emp);
            
            // 清除編輯模式
            window.editingEmployeeShift = undefined;
            
            closeModal('scheduleModal');
            renderCalendarGrid();
            renderGanttChart();
            updateStatistics();
            saveData();
        }

        function getGanttDateStr() {
            try {
                let dateToUse = window.ganttDate;
                
                if (!dateToUse) {
                    // 如果沒有甘特圖日期，使用今天
                    dateToUse = new Date();
                }
                
                // 使用與scheduleData相同的日期格式
                const year = dateToUse.getFullYear();
                const month = String(dateToUse.getMonth() + 1).padStart(2, '0');
                const day = String(dateToUse.getDate()).padStart(2, '0');
                const dateStr = `${year}-${month}-${day}`;
                
                console.log('當前甘特圖日期:', dateStr);
                return dateStr;
            } catch (e) {
                console.error('getGanttDateStr錯誤:', e);
                // 回退方案：使用今天
                const today = new Date();
                const year = today.getFullYear();
                const month = String(today.getMonth() + 1).padStart(2, '0');
                const day = String(today.getDate()).padStart(2, '0');
                return `${year}-${month}-${day}`;
            }
        }

        function showAllDaySchedule(dateStr) {
            try {
                console.log('=== 顯示當天全部排班 ===');
                console.log('傳入的dateStr:', dateStr);
                console.log('scheduleData:', scheduleData);
                
                if (!dateStr) {
                    alert('日期參數有誤，請重試');
                    return;
                }
                
                // 嘗試多種日期格式來匹配scheduleData中的數據
                let dayData = null;
                let actualDateStr = dateStr;
                
                // 方法1：直接使用傳入的dateStr
                if (scheduleData[dateStr]) {
                    dayData = scheduleData[dateStr];
                    console.log('方法1成功：直接使用dateStr');
                } else {
                    // 方法2：嘗試從甘特圖日期計算（因為時區問題可能導致差異）
                    // 首先檢查scheduleData中所有的鍵，找到最接近的
                    const dateKeys = Object.keys(scheduleData);
                    console.log('scheduleData中的所有日期:', dateKeys);
                    
                    for (let key of dateKeys) {
                        if (key === dateStr) {
                            dayData = scheduleData[key];
                            actualDateStr = key;
                            break;
                        }
                    }
                    
                    // 方法3：如果還是找不到，嘗試查找包含該月份的日期
                    if (!dayData && dateKeys.length > 0) {
                        // 檢查是否有其他日期有排班
                        const inputDateMonth = dateStr.substring(0, 7);  // 獲取年月部分
                        for (let key of dateKeys) {
                            const keyMonth = key.substring(0, 7);
                            if (keyMonth === inputDateMonth && dayData === null) {
                                console.log(`提示：找到的排班在${key}，不是${dateStr}`);
                                break;
                            }
                        }
                    }
                }
                
                console.log('最終dayData:', dayData);
                console.log('實際使用的日期:', actualDateStr);
                
                // 解析日期
                const date = new Date(actualDateStr + 'T00:00:00');
                const month = String(date.getMonth() + 1).padStart(2, '0');
                const day = String(date.getDate()).padStart(2, '0');
                const dayOfWeek = ['日', '一', '二', '三', '四', '五', '六'][date.getDay()];
                const dateDisplay = `${month}/${day} (${dayOfWeek})`;
                
                let content = `<h3 style="margin: 0 0 20px 0; color: var(--primary-color); text-align: center; font-size: 18px;">${dateDisplay} 排班詳情</h3>`;
                
                // 獲取當天的排班數據
                if (!scheduleData) {
                    console.error('scheduleData未定義');
                    content += '<div style="color: #999; text-align: center; padding: 20px;">排班數據未加載</div>';
                } else if (!dayData || Object.keys(dayData).length === 0) {
                    console.log('當天無排班或dayData為空');
                    content += '<div style="color: #999; font-size: 14px; padding: 30px; text-align: center;">當天無排班</div>';
                } else {
                    // 收集所有當天的排班員工信息
                    const allSchedules = [];
                    
                    Object.keys(dayData).forEach(shiftKey => {
                        console.log('處理班別:', shiftKey);
                        const employees_in_shift = dayData[shiftKey];
                        console.log('該班別的員工:', employees_in_shift);
                        
                        const shiftInfo = shifts[shiftKey];
                        console.log('班別信息:', shiftInfo);
                        
                        const shiftName = shiftInfo ? shiftInfo.name : shiftKey;
                        const shiftTime = shiftInfo ? `${shiftInfo.start} - ${shiftInfo.end}` : '時間未定';
                        
                        if (Array.isArray(employees_in_shift)) {
                            employees_in_shift.forEach(empObj => {
                                console.log('員工對象:', empObj);
                                
                                // 處理不同的員工數據格式
                                let empId = null;
                                let empName = null;
                                
                                if (typeof empObj === 'object' && empObj !== null) {
                                    // 可能是完整的員工對象 {id, name, ...} 或 {empId, ...}
                                    empId = empObj.id || empObj.empId;
                                    empName = empObj.name;
                                } else if (typeof empObj === 'number' || typeof empObj === 'string') {
                                    // 直接是empId
                                    empId = empObj;
                                }
                                
                                console.log('提取的員工ID:', empId, '員工名稱:', empName);
                                
                                // 如果已有員工名稱，直接使用
                                if (empName) {
                                    allSchedules.push({
                                        empName: empName,
                                        shiftName: shiftName,
                                        shiftTime: shiftTime,
                                        shiftKey: shiftKey,
                                        startTime: shiftInfo ? shiftInfo.start : '99:99'
                                    });
                                } else if (empId) {
                                    // 否則從employees中查找
                                    const emp = employees.find(e => Number(e.id) === Number(empId));
                                    console.log('找到的員工:', emp);
                                    
                                    if (emp) {
                                        allSchedules.push({
                                            empName: emp.name,
                                            shiftName: shiftName,
                                            shiftTime: shiftTime,
                                            shiftKey: shiftKey,
                                            startTime: shiftInfo ? shiftInfo.start : '99:99'
                                        });
                                    }
                                }
                            });
                        }
                    });
                    
                    console.log('最終排班列表:', allSchedules);
                    
                    // 按班別開始時間排序（最早上班的人在最前面）
                    allSchedules.sort((a, b) => {
                        // 直接比較已經保存的startTime
                        const aStartTime = a.startTime || '99:99';
                        const bStartTime = b.startTime || '99:99';
                        
                        if (aStartTime !== bStartTime) {
                            return aStartTime.localeCompare(bStartTime);
                        }
                        
                        // 同一班別內，按員工名字排序
                        return a.empName.localeCompare(b.empName);
                    });
                    
                    if (allSchedules.length === 0) {
                        console.log('沒有找到任何排班記錄');
                        content += '<div style="color: #999; font-size: 14px; padding: 30px; text-align: center;">當天無排班</div>';
                    } else {
                        content += '<div style="border-radius: 6px; overflow: hidden;">';
                        
                        allSchedules.forEach((schedule, idx) => {
                            const bgColor = idx % 2 === 0 ? '#f9f9f9' : 'white';
                            
                            content += `<div style="background: ${bgColor}; padding: 12px 15px; border-bottom: 1px solid #e8e8e8; display: flex; align-items: center; gap: 10px;">
                                <span style="font-weight: 600; color: #333; font-size: 14px; min-width: 80px;">${schedule.empName}</span>
                                <span style="color: var(--primary-color); font-weight: 600; font-size: 13px; background: #e8f0ff; padding: 3px 8px; border-radius: 4px;">${schedule.shiftName}</span>
                                <span style="color: #666; font-size: 12px;">${schedule.shiftTime}</span>
                            </div>`;
                        });
                        
                        content += '</div>';
                    }
                }
                
                const contentDiv = document.getElementById('employeeDayScheduleContent');
                if (contentDiv) {
                    contentDiv.innerHTML = content;
                    const modal = document.getElementById('employeeDayScheduleModal');
                    if (modal) {
                        modal.classList.add('active');
                        console.log('=== 當天全部排班已顯示 ===');
                    } else {
                        alert('找不到模態框');
                    }
                } else {
                    alert('找不到內容容器');
                }
            } catch (e) {
                console.error('showAllDaySchedule錯誤:', e);
                console.error('錯誤堆棧:', e.stack);
                alert('顯示排班詳情時出錯: ' + e.message);
            }
        }

        function exportDaySchedulePDF() {
            try {
                const dateStr = getGanttDateStr();
                console.log('匯出PDF - 傳入的dateStr:', dateStr);
                
                // 嘗試多種日期格式來匹配scheduleData中的數據
                let dayData = null;
                let actualDateStr = dateStr;
                
                // 方法1：直接使用傳入的dateStr
                if (scheduleData[dateStr]) {
                    dayData = scheduleData[dateStr];
                    console.log('方法1成功：直接使用dateStr');
                } else {
                    // 方法2：嘗試從scheduleData中的所有鍵查找
                    const dateKeys = Object.keys(scheduleData);
                    console.log('scheduleData中的所有日期:', dateKeys);
                    
                    for (let key of dateKeys) {
                        if (key === dateStr) {
                            dayData = scheduleData[key];
                            actualDateStr = key;
                            console.log('方法2成功');
                            break;
                        }
                    }
                }
                
                const date = new Date(actualDateStr + 'T00:00:00');
                const month = String(date.getMonth() + 1).padStart(2, '0');
                const day = String(date.getDate()).padStart(2, '0');
                const year = date.getFullYear();
                const dayOfWeek = ['日', '一', '二', '三', '四', '五', '六'][date.getDay()];
                const dateDisplay = `${year}年${month}月${day}日(${dayOfWeek})`;
                
                let scheduleList = [];
                
                if (dayData && Object.keys(dayData).length > 0) {
                    Object.keys(dayData).forEach(shiftKey => {
                        const employees_in_shift = dayData[shiftKey];
                        const shiftInfo = shifts[shiftKey];
                        const shiftName = shiftInfo ? shiftInfo.name : shiftKey;
                        const shiftTime = shiftInfo ? `${shiftInfo.start} - ${shiftInfo.end}` : '時間未定';
                        
                        if (Array.isArray(employees_in_shift)) {
                            employees_in_shift.forEach(empObj => {
                                let empId = null;
                                let empName = null;
                                if (typeof empObj === 'object' && empObj !== null) {
                                    empId = empObj.id || empObj.empId;
                                    empName = empObj.name;
                                } else if (typeof empObj === 'number' || typeof empObj === 'string') {
                                    empId = empObj;
                                }
                                const emp = employees.find(e => Number(e.id) === Number(empId));
                                if (emp) {
                                    scheduleList.push({
                                        name: emp.name,
                                        shift: shiftName,
                                        time: shiftTime
                                    });
                                }
                            });
                        }
                    });
                }
                
                scheduleList.sort((a, b) => a.name.localeCompare(b.name));
                
                // 簡單的PDF文本內容（使用HTML打印）
                let content = `
                    <h2>${dateDisplay} 排班詳情</h2>
                    <table border="1" cellpadding="10" cellspacing="0" style="width: 100%; border-collapse: collapse;">
                        <thead>
                            <tr style="background-color: #2c3e50; color: white;">
                                <th style="text-align: left;">員工名字</th>
                                <th style="text-align: left;">班別</th>
                                <th style="text-align: left;">時間</th>
                            </tr>
                        </thead>
                        <tbody>
                `;
                
                if (scheduleList.length === 0) {
                    content += '<tr><td colspan="3" style="text-align: center; padding: 20px;">當天無排班</td></tr>';
                } else {
                    scheduleList.forEach(item => {
                        content += `
                            <tr>
                                <td>${item.name}</td>
                                <td>${item.shift}</td>
                                <td>${item.time}</td>
                            </tr>
                        `;
                    });
                }
                
                content += `
                        </tbody>
                    </table>
                `;
                
                // 打開新窗口進行列印為PDF
                const printWindow = window.open('', '', 'height=600,width=800');
                printWindow.document.write('<html><head><title>' + dateDisplay + ' 排班詳情</title>');
                printWindow.document.write('<style>body{font-family:Arial,sans-serif;margin:20px;} h2{color:#2c3e50;} table{border-collapse:collapse;width:100%;} th,td{border:1px solid #ddd;padding:10px;text-align:left;} th{background-color:#2c3e50;color:white;}</style>');
                printWindow.document.write('</head><body>');
                printWindow.document.write(content);
                printWindow.document.write('</body></html>');
                printWindow.document.close();
                
                // 延遲以確保內容加載
                setTimeout(() => {
                    printWindow.print();
                }, 250);
                
                alert('已打開PDF匯出視窗，請在列印對話框中選擇"另存新檔為PDF"');
            } catch (e) {
                console.error('匯出PDF錯誤:', e);
                alert('匯出PDF時出錯: ' + e.message);
            }
        }

        function exportDayScheduleExcel() {
            try {
                const dateStr = getGanttDateStr();
                console.log('匯出Excel - 傳入的dateStr:', dateStr);
                
                // 嘗試多種日期格式來匹配scheduleData中的數據
                let dayData = null;
                let actualDateStr = dateStr;
                
                // 方法1：直接使用傳入的dateStr
                if (scheduleData[dateStr]) {
                    dayData = scheduleData[dateStr];
                    console.log('方法1成功：直接使用dateStr');
                } else {
                    // 方法2：嘗試從scheduleData中的所有鍵查找
                    const dateKeys = Object.keys(scheduleData);
                    console.log('scheduleData中的所有日期:', dateKeys);
                    
                    for (let key of dateKeys) {
                        if (key === dateStr) {
                            dayData = scheduleData[key];
                            actualDateStr = key;
                            console.log('方法2成功');
                            break;
                        }
                    }
                }
                
                const date = new Date(actualDateStr + 'T00:00:00');
                const month = String(date.getMonth() + 1).padStart(2, '0');
                const day = String(date.getDate()).padStart(2, '0');
                const year = date.getFullYear();
                const dayOfWeek = ['日', '一', '二', '三', '四', '五', '六'][date.getDay()];
                const dateDisplay = `${year}年${month}月${day}日(${dayOfWeek})`;
                
                let scheduleList = [];
                
                if (dayData && Object.keys(dayData).length > 0) {
                    Object.keys(dayData).forEach(shiftKey => {
                        const employees_in_shift = dayData[shiftKey];
                        const shiftInfo = shifts[shiftKey];
                        const shiftName = shiftInfo ? shiftInfo.name : shiftKey;
                        const shiftTime = shiftInfo ? `${shiftInfo.start} - ${shiftInfo.end}` : '時間未定';
                        
                        if (Array.isArray(employees_in_shift)) {
                            employees_in_shift.forEach(empObj => {
                                // 處理不同的員工數據格式
                                let empId = null;
                                let empName = null;
                                
                                if (typeof empObj === 'object' && empObj !== null) {
                                    empId = empObj.id || empObj.empId;
                                    empName = empObj.name;
                                } else if (typeof empObj === 'number' || typeof empObj === 'string') {
                                    empId = empObj;
                                }
                                
                                // 如果已有員工名稱，直接使用
                                if (empName) {
                                    scheduleList.push({
                                        name: empName,
                                        shift: shiftName,
                                        time: shiftTime
                                    });
                                } else if (empId) {
                                    const emp = employees.find(e => Number(e.id) === Number(empId));
                                    if (emp) {
                                        scheduleList.push({
                                            name: emp.name,
                                            shift: shiftName,
                                            time: shiftTime
                                        });
                                    }
                                }
                            });
                        }
                    });
                }
                
                scheduleList.sort((a, b) => a.name.localeCompare(b.name));
                
                // 製作CSV內容
                let csvContent = '員工名字,班別,時間\n';
                
                if (scheduleList.length === 0) {
                    csvContent += '當天無排班';
                } else {
                    scheduleList.forEach(item => {
                        csvContent += `${item.name},${item.shift},${item.time}\n`;
                    });
                }
                
                // 建立下載鏈接
                const blob = new Blob(['\uFEFF' + csvContent], { type: 'text/csv;charset=utf-8;' });
                const link = document.createElement('a');
                const url = URL.createObjectURL(blob);
                link.setAttribute('href', url);
                link.setAttribute('download', `${dateDisplay}_排班詳情.csv`);
                link.style.visibility = 'hidden';
                document.body.appendChild(link);
                link.click();
                document.body.removeChild(link);
                
                alert('排班詳情已匯出為Excel (CSV)格式');
            } catch (e) {
                console.error('匯出Excel錯誤:', e);
                alert('匯出Excel時出錯: ' + e.message);
            }
        }

        function printDaySchedule() {
            try {
                const dateStr = getGanttDateStr();
                console.log('列印 - 傳入的dateStr:', dateStr);
                
                // 嘗試多種日期格式來匹配scheduleData中的數據
                let dayData = null;
                let actualDateStr = dateStr;
                
                // 方法1：直接使用傳入的dateStr
                if (scheduleData[dateStr]) {
                    dayData = scheduleData[dateStr];
                    console.log('方法1成功：直接使用dateStr');
                } else {
                    // 方法2：嘗試從scheduleData中的所有鍵查找
                    const dateKeys = Object.keys(scheduleData);
                    console.log('scheduleData中的所有日期:', dateKeys);
                    
                    for (let key of dateKeys) {
                        if (key === dateStr) {
                            dayData = scheduleData[key];
                            actualDateStr = key;
                            console.log('方法2成功');
                            break;
                        }
                    }
                }
                
                const date = new Date(actualDateStr + 'T00:00:00');
                const month = String(date.getMonth() + 1).padStart(2, '0');
                const day = String(date.getDate()).padStart(2, '0');
                const year = date.getFullYear();
                const dayOfWeek = ['日', '一', '二', '三', '四', '五', '六'][date.getDay()];
                const dateDisplay = `${year}年${month}月${day}日(${dayOfWeek})`;
                
                let scheduleList = [];
                
                if (dayData && Object.keys(dayData).length > 0) {
                    Object.keys(dayData).forEach(shiftKey => {
                        const employees_in_shift = dayData[shiftKey];
                        const shiftInfo = shifts[shiftKey];
                        const shiftName = shiftInfo ? shiftInfo.name : shiftKey;
                        const shiftTime = shiftInfo ? `${shiftInfo.start} - ${shiftInfo.end}` : '時間未定';
                        
                        if (Array.isArray(employees_in_shift)) {
                            employees_in_shift.forEach(empObj => {
                                // 處理不同的員工數據格式
                                let empId = null;
                                let empName = null;
                                
                                if (typeof empObj === 'object' && empObj !== null) {
                                    empId = empObj.id || empObj.empId;
                                    empName = empObj.name;
                                } else if (typeof empObj === 'number' || typeof empObj === 'string') {
                                    empId = empObj;
                                }
                                
                                // 如果已有員工名稱，直接使用
                                if (empName) {
                                    scheduleList.push({
                                        name: empName,
                                        shift: shiftName,
                                        time: shiftTime
                                    });
                                } else if (empId) {
                                    const emp = employees.find(e => Number(e.id) === Number(empId));
                                    if (emp) {
                                        scheduleList.push({
                                            name: emp.name,
                                            shift: shiftName,
                                            time: shiftTime
                                        });
                                    }
                                }
                            });
                        }
                    });
                }
                
                scheduleList.sort((a, b) => a.name.localeCompare(b.name));
                
                // 建立列印內容
                let content = `
                    <h2 style="text-align: center; margin-bottom: 30px;">${dateDisplay} 排班詳情</h2>
                    <table border="1" cellpadding="10" cellspacing="0" style="width: 100%; border-collapse: collapse;">
                        <thead>
                            <tr style="background-color: #2c3e50; color: white;">
                                <th style="text-align: left;">員工名字</th>
                                <th style="text-align: left;">班別</th>
                                <th style="text-align: left;">時間</th>
                            </tr>
                        </thead>
                        <tbody>
                `;
                
                if (scheduleList.length === 0) {
                    content += '<tr><td colspan="3" style="text-align: center; padding: 20px;">當天無排班</td></tr>';
                } else {
                    scheduleList.forEach(item => {
                        content += `
                            <tr>
                                <td>${item.name}</td>
                                <td>${item.shift}</td>
                                <td>${item.time}</td>
                            </tr>
                        `;
                    });
                }
                
                content += `
                        </tbody>
                    </table>
                `;
                
                // 打開新窗口進行列印
                const printWindow = window.open('', '', 'height=600,width=800');
                printWindow.document.write('<html><head><title>' + dateDisplay + ' 排班詳情</title>');
                printWindow.document.write('<style>body{font-family:Arial,sans-serif;margin:20px;} h2{color:#2c3e50;} table{border-collapse:collapse;width:100%;} th,td{border:1px solid #ddd;padding:10px;text-align:left;} th{background-color:#2c3e50;color:white;} @media print { body{margin:0;} }</style>');
                printWindow.document.write('</head><body>');
                printWindow.document.write(content);
                printWindow.document.write('</body></html>');
                printWindow.document.close();
                
                // 延遲以確保內容加載
                setTimeout(() => {
                    printWindow.print();
                }, 250);
            } catch (e) {
                console.error('列印錯誤:', e);
                alert('列印時出錯: ' + e.message);
            }
        }

        function showMonthlyHours() {
            try {
                console.log('=== 顯示月度排班統計 ===');
                
                // 初始化為當前月份
                const today = new Date();
                window.monthlyStatsYear = today.getFullYear();
                window.monthlyStatsMonth = today.getMonth();
                
                // 設定月份選擇器的初始值
                const monthStr = String(window.monthlyStatsMonth + 1).padStart(2, '0');
                document.getElementById('monthlyStatsMonthPicker').value = `${window.monthlyStatsYear}-${monthStr}`;
                
                // 顯示統計數據
                updateMonthlyStatsDisplay();
                
                // 打開模態框
                const modal = document.getElementById('monthlyHoursModal');
                if (modal) {
                    modal.classList.add('active');
                    console.log('=== 月度排班統計已顯示 ===');
                }
            } catch (e) {
                console.error('月度統計錯誤:', e);
                alert('顯示月度統計時出錯: ' + e.message);
            }
        }

        function changeMonthlyStatsMonth(offset) {
            // 改變月份
            window.monthlyStatsMonth += offset;
            
            // 跨年處理
            if (window.monthlyStatsMonth > 11) {
                window.monthlyStatsMonth = 0;
                window.monthlyStatsYear++;
            } else if (window.monthlyStatsMonth < 0) {
                window.monthlyStatsMonth = 11;
                window.monthlyStatsYear--;
            }
            
            // 更新月份選擇器
            const monthStr = String(window.monthlyStatsMonth + 1).padStart(2, '0');
            document.getElementById('monthlyStatsMonthPicker').value = `${window.monthlyStatsYear}-${monthStr}`;
            
            // 更新顯示
            updateMonthlyStatsDisplay();
        }

        function updateMonthlyStatsDisplay() {
            try {
                // 獲取選擇的年月
                const monthPickerValue = document.getElementById('monthlyStatsMonthPicker').value;
                if (monthPickerValue) {
                    const [year, month] = monthPickerValue.split('-');
                    window.monthlyStatsYear = parseInt(year);
                    window.monthlyStatsMonth = parseInt(month) - 1;
                }
                
                const currentYear = window.monthlyStatsYear;
                const currentMonth = window.monthlyStatsMonth;
                const monthStart = new Date(currentYear, currentMonth, 1);
                const monthEnd = new Date(currentYear, currentMonth + 1, 0);
                
                // 生成月份內的所有日期
                const monthDays = [];
                for (let d = 1; d <= monthEnd.getDate(); d++) {
                    const y = currentYear;
                    const m = String(currentMonth + 1).padStart(2, '0');
                    const day = String(d).padStart(2, '0');
                    monthDays.push(`${y}-${m}-${day}`);
                }
                
                // 計算每位員工的總排班分鐘數
                const employeeMinutes = {};
                
                employees.forEach(emp => {
                    employeeMinutes[emp.name] = 0;
                });
                
                // 遍歷整個月份的排班數據
                monthDays.forEach(dateStr => {
                    const dayData = scheduleData[dateStr];
                    if (dayData && Object.keys(dayData).length > 0) {
                        Object.keys(dayData).forEach(shiftKey => {
                            const shiftEmps = dayData[shiftKey];
                            const shiftInfo = shifts[shiftKey];
                            
                            if (shiftInfo && Array.isArray(shiftEmps)) {
                                // 計算該班別的分鐘數
                                const startHour = parseInt(shiftInfo.start.split(':')[0]);
                                const startMin = parseInt(shiftInfo.start.split(':')[1]) || 0;
                                const endHour = parseInt(shiftInfo.end.split(':')[0]);
                                const endMin = parseInt(shiftInfo.end.split(':')[1]) || 0;
                                
                                const startMinutes = startHour * 60 + startMin;
                                const endMinutes = endHour * 60 + endMin;
                                const minutes = endMinutes > startMinutes ? 
                                    (endMinutes - startMinutes) : 
                                    ((24 * 60 - startMinutes) + endMinutes);
                                
                                shiftEmps.forEach(empObj => {
                                    let empName = null;
                                    let empId = null;
                                    
                                    if (typeof empObj === 'object' && empObj !== null) {
                                        empId = empObj.id || empObj.empId;
                                        empName = empObj.name;
                                    } else if (typeof empObj === 'number' || typeof empObj === 'string') {
                                        empId = empObj;
                                    }
                                    
                                    // 如果沒有名字，從employees中查找
                                    if (!empName && empId) {
                                        const emp = employees.find(e => Number(e.id) === Number(empId));
                                        if (emp) {
                                            empName = emp.name;
                                        }
                                    }
                                    
                                    if (empName && employeeMinutes.hasOwnProperty(empName)) {
                                        employeeMinutes[empName] += minutes;
                                    }
                                });
                            }
                        });
                    }
                });
                
                // 生成顯示內容
                const monthDisplay = `${String(currentMonth + 1).padStart(2, '0')}月`;
                let content = `<h3 style="margin: 0 0 20px 0; color: var(--primary-color); text-align: center; font-size: 18px;">${currentYear}年${monthDisplay}排班統計</h3>`;
                
                content += '<div style="border-radius: 6px; overflow: hidden;">';
                
                // 按員工名字排序，計算總分鐘數
                let totalMonthlyMinutes = 0;
                const sortedEmployees = Object.entries(employeeMinutes)
                    .sort((a, b) => a[0].localeCompare(b[0]));
                
                sortedEmployees.forEach((item, idx) => {
                    const empName = item[0];
                    const minutes = item[1];
                    totalMonthlyMinutes += minutes;
                    
                    const bgColor = idx % 2 === 0 ? '#f9f9f9' : 'white';
                    
                    // 轉換為「小時 + 分鐘」的格式
                    const hours = Math.floor(minutes / 60);
                    const mins = minutes % 60;
                    const timeStr = mins > 0 ? `${hours}小時${mins}分` : `${hours}小時`;
                    
                    content += `<div style="background: ${bgColor}; padding: 12px 15px; border-bottom: 1px solid #e8e8e8; display: flex; align-items: center; justify-content: space-between;">
                        <span style="font-weight: 600; color: #333; font-size: 14px;">${empName}</span>
                        <span style="color: var(--primary-color); font-weight: 600; font-size: 14px;">${timeStr}</span>
                    </div>`;
                });
                
                content += '</div>';
                
                // 將總分鐘數轉換為「小時 + 分鐘」的格式
                const totalHours = Math.floor(totalMonthlyMinutes / 60);
                const totalMins = totalMonthlyMinutes % 60;
                const totalTimeStr = totalMins > 0 ? `${totalHours}小時${totalMins}分` : `${totalHours}小時`;
                
                content += `<div style="margin-top: 15px; padding: 15px; background: #f0f0f0; border-radius: 6px; text-align: right;">
                    <div style="color: #666; font-size: 14px;">本月排班總時數：</div>
                    <div style="color: var(--primary-color); font-weight: 600; font-size: 20px;">${totalTimeStr}</div>
                </div>`;
                
                const contentDiv = document.getElementById('monthlyHoursContent');
                if (contentDiv) {
                    contentDiv.innerHTML = content;
                }
            } catch (e) {
                console.error('更新月度統計錯誤:', e);
                alert('更新月度統計時出錯: ' + e.message);
            }
        }

        function showDayWageCalculation(dateStr) {
            try {
                console.log('=== 顯示當天薪資計算 ===');
                console.log('dateStr:', dateStr);
                
                if (!dateStr) {
                    alert('日期參數有誤，請重試');
                    return;
                }
                
                // 嘗試多種日期格式來匹配scheduleData中的數據
                let dayData = null;
                let actualDateStr = dateStr;
                
                if (scheduleData[dateStr]) {
                    dayData = scheduleData[dateStr];
                } else {
                    const dateKeys = Object.keys(scheduleData);
                    for (let key of dateKeys) {
                        if (key === dateStr) {
                            dayData = scheduleData[key];
                            actualDateStr = key;
                            break;
                        }
                    }
                }
                
                // 解析日期
                const date = new Date(actualDateStr + 'T00:00:00');
                const month = String(date.getMonth() + 1).padStart(2, '0');
                const day = String(date.getDate()).padStart(2, '0');
                const dayOfWeek = ['日', '一', '二', '三', '四', '五', '六'][date.getDay()];
                const dateDisplay = `${month}/${day} (${dayOfWeek})`;
                
                let content = `<h3 style="margin: 0 0 20px 0; color: var(--primary-color); text-align: center; font-size: 18px;">${dateDisplay} 薪資計算</h3>`;
                
                // 獲取當天的排班數據
                if (!dayData || Object.keys(dayData).length === 0) {
                    content += '<div style="color: #999; font-size: 14px; padding: 30px; text-align: center;">當天無排班</div>';
                } else {
                    // 收集所有當天的排班員工信息（含時數）
                    const allWages = [];
                    
                    Object.keys(dayData).forEach(shiftKey => {
                        const employees_in_shift = dayData[shiftKey];
                        const shiftInfo = shifts[shiftKey];
                        
                        const shiftName = shiftInfo ? shiftInfo.name : shiftKey;
                        const shiftTime = shiftInfo ? `${shiftInfo.start} - ${shiftInfo.end}` : '時間未定';
                        
                        if (Array.isArray(employees_in_shift)) {
                            employees_in_shift.forEach(empObj => {
                                // 處理不同的員工數據格式
                                let empId = null;
                                let empName = null;
                                
                                if (typeof empObj === 'object' && empObj !== null) {
                                    empId = empObj.id || empObj.empId;
                                    empName = empObj.name;
                                } else if (typeof empObj === 'number' || typeof empObj === 'string') {
                                    empId = empObj;
                                }
                                
                                // 如果已有員工名稱，直接使用
                                if (empName) {
                                    // 計算排班分鐘數
                                    const timeParts = shiftTime.split(' - ');
                                    const startHour = parseInt(timeParts[0].split(':')[0]);
                                    const startMin = parseInt(timeParts[0].split(':')[1]) || 0;
                                    const endHour = parseInt(timeParts[1].split(':')[0]);
                                    const endMin = parseInt(timeParts[1].split(':')[1]) || 0;
                                    
                                    const startMinutes = startHour * 60 + startMin;
                                    const endMinutes = endHour * 60 + endMin;
                                    const minutes = endMinutes > startMinutes ? 
                                        (endMinutes - startMinutes) : 
                                        ((24 * 60 - startMinutes) + endMinutes);
                                    
                                    // 獲取員工的時薪
                                    const emp = employees.find(e => e.name === empName);
                                    const hourlyRate = emp ? (emp.hourlyRate || 0) : 0;
                                    const wage = (minutes / 60) * hourlyRate;
                                    
                                    allWages.push({
                                        empName: empName,
                                        shiftName: shiftName,
                                        shiftTime: shiftTime,
                                        minutes: minutes,
                                        hourlyRate: hourlyRate,
                                        wage: wage
                                    });
                                } else if (empId) {
                                    // 否則從employees中查找
                                    const emp = employees.find(e => Number(e.id) === Number(empId));
                                    
                                    if (emp) {
                                        // 計算排班分鐘數
                                        const timeParts = shiftTime.split(' - ');
                                        const startHour = parseInt(timeParts[0].split(':')[0]);
                                        const startMin = parseInt(timeParts[0].split(':')[1]) || 0;
                                        const endHour = parseInt(timeParts[1].split(':')[0]);
                                        const endMin = parseInt(timeParts[1].split(':')[1]) || 0;
                                        
                                        const startMinutes = startHour * 60 + startMin;
                                        const endMinutes = endHour * 60 + endMin;
                                        const minutes = endMinutes > startMinutes ? 
                                            (endMinutes - startMinutes) : 
                                            ((24 * 60 - startMinutes) + endMinutes);
                                        
                                        const hourlyRate = emp.hourlyRate || 0;
                                        const wage = (minutes / 60) * hourlyRate;
                                        
                                        allWages.push({
                                            empName: emp.name,
                                            shiftName: shiftName,
                                            shiftTime: shiftTime,
                                            minutes: minutes,
                                            hourlyRate: hourlyRate,
                                            wage: wage
                                        });
                                    }
                                }
                            });
                        }
                    });
                    
                    // 按班別開始時間排序（最早上班的人在最前面）
                    allWages.sort((a, b) => {
                        const aStartTime = a.shiftTime.split(' - ')[0];
                        const bStartTime = b.shiftTime.split(' - ')[0];
                        
                        if (aStartTime !== bStartTime) {
                            return aStartTime.localeCompare(bStartTime);
                        }
                        
                        return a.empName.localeCompare(b.empName);
                    });
                    
                    if (allWages.length === 0) {
                        content += '<div style="color: #999; font-size: 14px; padding: 30px; text-align: center;">當天無排班</div>';
                    } else {
                        content += '<div style="border-radius: 6px; overflow: hidden;">';
                        
                        let totalDailyWage = 0;
                        
                        allWages.forEach((item, idx) => {
                            const bgColor = idx % 2 === 0 ? '#f9f9f9' : 'white';
                            totalDailyWage += item.wage;
                            const hours = Math.floor(item.minutes / 60);
                            const mins = item.minutes % 60;
                            const timeStr = mins > 0 ? `${hours}小時${mins}分` : `${hours}小時`;
                            
                            content += `<div style="background: ${bgColor}; padding: 12px 15px; border-bottom: 1px solid #e8e8e8; display: flex; align-items: center; justify-content: space-between;">
                                <div style="display: flex; align-items: center; gap: 10px; flex: 1;">
                                    <span style="font-weight: 600; color: #333; font-size: 14px; min-width: 80px;">${item.empName}</span>
                                    <span style="color: var(--primary-color); font-weight: 600; font-size: 13px; background: #e8f0ff; padding: 3px 8px; border-radius: 4px;">${item.shiftName}</span>
                                    <span style="color: #666; font-size: 12px;">${item.shiftTime}</span>
                                </div>
                                <div style="text-align: right; display: flex; flex-direction: column; align-items: flex-end; gap: 3px;">
                                    <div style="color: #999; font-size: 12px; display: flex; align-items: center; gap: 5px;">
                                        <span>${timeStr}</span>
                                        <span>×</span>
                                        <button onclick="editEmployeeHourlyRate('${item.empName}')" style="background: #e8e8e8; border: none; padding: 2px 6px; border-radius: 3px; cursor: pointer; font-size: 11px;">$${item.hourlyRate}</button>
                                    </div>
                                    <div style="color: #ff6b6b; font-weight: 600; font-size: 14px;">$${item.wage.toLocaleString('en-US', {minimumFractionDigits: 0, maximumFractionDigits: 0})}</div>
                                </div>
                            </div>`;
                        });
                        
                        content += '</div>';
                        content += `<div style="margin-top: 15px; padding: 15px; background: #f0f0f0; border-radius: 6px; text-align: right;">
                            <div style="color: #666; font-size: 14px;">當天薪資總額：</div>
                            <div style="color: #ff6b6b; font-weight: 600; font-size: 20px;">$${totalDailyWage.toLocaleString('en-US', {minimumFractionDigits: 0, maximumFractionDigits: 0})}</div>
                        </div>`;
                    }
                }
                
                const contentDiv = document.getElementById('dayWageContent');
                if (contentDiv) {
                    contentDiv.innerHTML = content;
                    const modal = document.getElementById('dayWageModal');
                    if (modal) {
                        modal.classList.add('active');
                        console.log('=== 當天薪資計算已顯示 ===');
                    }
                }
            } catch (e) {
                console.error('薪資計算錯誤:', e);
                alert('顯示薪資計算時出錯: ' + e.message);
            }
        }

        function editEmployeeHourlyRate(empName) {
            const emp = employees.find(e => e.name === empName);
            if (!emp) {
                alert('找不到員工信息');
                return;
            }
            
            const newRate = prompt(`請設定 ${empName} 的時薪 (目前: $${emp.hourlyRate})：`, emp.hourlyRate);
            if (newRate !== null && newRate !== '') {
                const rate = parseFloat(newRate);
                if (!isNaN(rate) && rate >= 0) {
                    emp.hourlyRate = rate;
                    saveData();
                    // 重新顯示薪資計算
                    const modal = document.getElementById('dayWageModal');
                    if (modal && modal.classList.contains('active')) {
                        // 刷新當前顯示
                        location.reload(); // 簡單做法，重新加載頁面
                    }
                } else {
                    alert('請輸入有效的數字');
                }
            }
        }

        function showEmployeeDaySchedule(dateStr, empId) {
            console.log('=== 點選員工 ===');
            console.log('dateStr:', dateStr);
            console.log('empId:', empId);
            console.log('empId類型:', typeof empId);
            console.log('所有員工:', employees);
            
            // 強制轉換empId為數字進行比較
            const empIdNum = Number(empId);
            console.log('轉換後的empId:', empIdNum);
            
            const employee = employees.find(e => {
                console.log(`比較: e.id=${e.id} (類型:${typeof e.id}) vs empIdNum=${empIdNum}`);
                return Number(e.id) === empIdNum;
            });
            
            console.log('找到的員工:', employee);
            
            if (!employee) {
                console.error('找不到員工信息，empId:', empId);
                alert('找不到員工信息');
                return;
            }
            
            console.log('員工ID:', employee.id);
            console.log('員工名稱:', employee.name);
            
            // 獲取當天該員工的所有班別
            const daySchedules = [];
            const shiftData = scheduleData[dateStr];
            
            console.log('查詢日期:', dateStr);
            console.log('該日期的班別數據:', shiftData);
            
            if (shiftData) {
                for (let shiftId in shiftData) {
                    const empList = shiftData[shiftId];
                    console.log(`班別${shiftId}的員工列表:`, empList);
                    
                    if (Array.isArray(empList)) {
                        const empFound = empList.find(e => {
                            const currentEmpId = e && typeof e === 'object' ? e.id : e;
                            return Number(currentEmpId) === empIdNum;
                        });
                        
                        if (empFound) {
                            const shiftInfo = shifts[shiftId];
                            console.log(`找到班別${shiftId}:`, shiftInfo);
                            daySchedules.push({
                                shiftId: shiftId,
                                shiftName: shiftInfo.name,
                                startTime: shiftInfo.start,
                                endTime: shiftInfo.end
                            });
                        }
                    }
                }
            }
            
            console.log('該員工的所有班別:', daySchedules);
            
            // 格式化日期
            const date = new Date(dateStr);
            const month = String(date.getMonth() + 1).padStart(2, '0');
            const day = String(date.getDate()).padStart(2, '0');
            const dayOfWeek = ['日', '一', '二', '三', '四', '五', '六'][date.getDay()];
            const dateDisplay = `${date.getFullYear()}年${month}月${day}日(${dayOfWeek})`;
            
            // 構建內容
            let content = `
                <div style="margin-bottom: 15px;">
                    <h4 style="margin: 0 0 10px 0; color: var(--primary-color);">${employee.name}</h4>
                    <div style="font-size: 12px; color: #666; margin-bottom: 10px;">日期: ${dateDisplay}</div>
                    <div style="font-size: 12px; color: #666; margin-bottom: 15px;">職位: ${employee.role}</div>
            `;
            
            if (daySchedules.length > 0) {
                content += '<div style="border-top: 1px solid #ddd; padding-top: 10px;">';
                content += '<div style="font-weight: 600; color: var(--primary-color); margin-bottom: 10px;">當天排班:</div>';
                daySchedules.forEach((schedule, idx) => {
                    content += `
                        <div style="background: #f5f5f5; padding: 8px; border-radius: 4px; margin-bottom: 8px; display: flex; justify-content: space-between; align-items: center;">
                            <div>
                                <div style="font-weight: 600; color: #333; font-size: 12px;">${schedule.shiftName}</div>
                                <div style="color: #666; font-size: 11px;">時間: ${schedule.startTime} - ${schedule.endTime}</div>
                            </div>
                            <div style="display: flex; gap: 5px;">
                                <button class="delete-btn" onclick="editEmployeeShift('${dateStr}', ${empIdNum}, '${schedule.shiftId}')" style="background: #e8f0ff; border: 1px solid #d4e0ff; color: var(--primary-color); padding: 4px 8px; font-size: 11px;">編輯</button>
                                <button class="delete-btn" onclick="deleteEmployeeShift('${dateStr}', ${empIdNum}, '${schedule.shiftId}')" style="padding: 4px 8px; font-size: 11px;">刪除</button>
                            </div>
                        </div>
                    `;
                });
                content += '</div>';
            } else {
                content += '<div style="color: #999; font-size: 12px; padding: 15px 0;">當天無排班</div>';
            }
            
            content += '</div>';
            
            console.log('設置模態框內容');
            document.getElementById('employeeDayScheduleContent').innerHTML = content;
            document.getElementById('employeeDayScheduleModal').classList.add('active');
            console.log('=== 員工詳情已顯示 ===');
        }

        function editEmployeeShift(dateStr, empId, shiftId) {
            // 打開排班編輯模態框
            selectedShift = { date: dateStr, type: shiftId };
            selectedShiftType = shiftId;
            
            // 設置編輯模式變量
            window.editingEmployeeShift = { dateStr: dateStr, empId: empId, shiftId: shiftId };
            
            document.getElementById('scheduleModal').classList.add('active');
            renderModalEmployeeSelect();
            renderShiftSelector();
            
            // 在員工選擇框中預選該員工
            setTimeout(() => {
                document.getElementById('modalEmployeeSelect').value = empId;
            }, 100);
        }

        function deleteEmployeeShift(dateStr, empId, shiftId) {
            if (confirm('確定要刪除此排班嗎？')) {
                console.log('=== 開始刪除 ===');
                console.log('dateStr:', dateStr);
                console.log('empId:', empId);
                console.log('shiftId:', shiftId);
                console.log('完整scheduleData:', JSON.stringify(scheduleData));
                
                // 驗證參數
                if (!dateStr || empId === undefined || !shiftId) {
                    alert('刪除參數不完整');
                    return;
                }
                
                // 確保date存在
                if (!scheduleData[dateStr]) {
                    alert('找不到該日期的排班記錄');
                    return;
                }
                
                // 確保班別存在
                if (!scheduleData[dateStr][shiftId]) {
                    alert('找不到該班別的排班記錄');
                    return;
                }
                
                let shiftData = scheduleData[dateStr][shiftId];
                console.log('班別數據:', shiftData);
                console.log('班別數據類型:', typeof shiftData);
                console.log('是否為陣列:', Array.isArray(shiftData));
                
                // 處理不同的數據格式
                if (!Array.isArray(shiftData)) {
                    // 如果不是陣列，直接刪除
                    delete scheduleData[dateStr][shiftId];
                    console.log('刪除非陣列班別');
                } else {
                    // 是陣列，遍歷刪除
                    let found = false;
                    
                    for (let i = shiftData.length - 1; i >= 0; i--) {
                        const emp = shiftData[i];
                        console.log(`[${i}] 員工對象:`, emp);
                        
                        let empIdToMatch;
                        if (emp === null || emp === undefined) {
                            continue;
                        } else if (typeof emp === 'object') {
                            empIdToMatch = emp.id;
                        } else {
                            empIdToMatch = emp;
                        }
                        
                        console.log(`[${i}] 比較: empIdToMatch=${empIdToMatch} vs empId=${empId}`);
                        
                        // 嚴格比較
                        if (Number(empIdToMatch) === Number(empId)) {
                            console.log(`[${i}] 找到匹配，進行刪除`);
                            shiftData.splice(i, 1);
                            found = true;
                            break;
                        }
                    }
                    
                    if (!found) {
                        alert('找不到該員工');
                        console.log('刪除失敗：找不到員工');
                        return;
                    }
                    
                    // 清理空的班別
                    if (shiftData.length === 0) {
                        delete scheduleData[dateStr][shiftId];
                        console.log('班別已為空，已刪除');
                    }
                }
                
                // 清理空的日期
                if (Object.keys(scheduleData[dateStr]).length === 0) {
                    delete scheduleData[dateStr];
                    console.log('日期已為空，已刪除');
                }
                
                console.log('刪除後的scheduleData:', JSON.stringify(scheduleData));
                console.log('=== 刪除完成 ===');
                
                // 刷新UI
                closeModal('employeeDayScheduleModal');
                renderCalendarGrid();
                renderGanttChart();
                updateStatistics();
                saveData();
                
                alert('排班已成功刪除');
            }
        }

        function removeSchedule(dateStr, shiftId, empId) {
            if (currentUser !== 'manager') {
                alert('只有主管可以修改排班');
                return;
            }
            
            if (confirm('確定要移除此排班？')) {
                console.log(`removeSchedule: 日期=${dateStr}, 班別=${shiftId}, 員工=${empId}`);
                
                // 確保date和shift都存在
                if (!scheduleData[dateStr] || !scheduleData[dateStr][shiftId]) {
                    alert('找不到該排班記錄');
                    return;
                }
                
                const shiftData = scheduleData[dateStr][shiftId];
                if (!Array.isArray(shiftData)) {
                    delete scheduleData[dateStr][shiftId];
                    renderCalendarGrid();
                    renderGanttChart();
                    updateStatistics();
                    saveData();
                    return;
                }
                
                // 找出要刪除的員工索引
                let deleteIndex = -1;
                for (let i = 0; i < shiftData.length; i++) {
                    const emp = shiftData[i];
                    const currentEmpId = emp && typeof emp === 'object' && emp.id !== undefined ? emp.id : emp;
                    
                    if (currentEmpId === empId || Number(currentEmpId) === Number(empId)) {
                        deleteIndex = i;
                        break;
                    }
                }
                
                if (deleteIndex !== -1) {
                    // 刪除該員工
                    shiftData.splice(deleteIndex, 1);
                    
                    // 如果該班別沒有員工了，刪除該班別
                    if (shiftData.length === 0) {
                        delete scheduleData[dateStr][shiftId];
                    }
                    
                    // 如果該日期沒有班別了，刪除該日期
                    if (Object.keys(scheduleData[dateStr]).length === 0) {
                        delete scheduleData[dateStr];
                    }
                }
                
                renderCalendarGrid();
                renderGanttChart();
                updateStatistics();
                saveData();
            }
        }

        function closeModal(modalId) {
            document.getElementById(modalId).classList.remove('active');
            selectedShift = null;
            selectedShiftType = null;
            window.isCustomShift = false;
            window.editingEmployeeShift = undefined;
            
            // 清除自訂班別輸入框
            if (modalId === 'scheduleModal') {
                document.getElementById('customShiftDiv').style.display = 'none';
                document.getElementById('customShiftStart').value = '';
                document.getElementById('customShiftEnd').value = '';
                document.getElementById('customShiftName').value = '';
            }
        }

        function switchTab(tabName) {
            document.querySelectorAll('.tab-content').forEach(el => el.classList.remove('active'));
            document.querySelectorAll('.tab-button').forEach(el => el.classList.remove('active'));
            document.querySelectorAll('.sidebar-item').forEach(el => el.classList.remove('active'));
            
            document.getElementById(tabName)?.classList.add('active');
            document.querySelectorAll(`[onclick="switchTab('${tabName}')"]`).forEach(el => el.classList.add('active'));
        }

        function renderEmployeeSelect() {
            const select = document.getElementById('employeeSelect');
            select.innerHTML = '<option value="">-- 請選擇員工 --</option>';
            
            employees.forEach(emp => {
                const option = document.createElement('option');
                option.value = emp.id;
                option.textContent = emp.name;
                select.appendChild(option);
            });
        }

        function updateEmployeeCalendar() {
            const empId = parseInt(document.getElementById('employeeSelect').value);
            if (!empId) {
                document.getElementById('availabilityCalendar').innerHTML = '';
                return;
            }
            
            renderAvailabilityCalendar(empId);
        }

        function renderAvailabilityCalendar(empId) {
            const calendar = document.getElementById('availabilityCalendar');
            calendar.innerHTML = '';
            
            // 獲取當前顯示的月份和年份
            if (window.availabilityMonth === undefined || window.availabilityYear === undefined) {
                const today = new Date();
                window.availabilityMonth = today.getMonth();
                window.availabilityYear = today.getFullYear();
            }
            
            // 添加月份導航
            const navDiv = document.createElement('div');
            navDiv.style.cssText = 'display: flex; justify-content: space-between; align-items: center; margin-bottom: 15px;';
            
            const monthYear = document.createElement('div');
            monthYear.id = 'availabilityMonthYear';
            monthYear.style.cssText = 'font-weight: 600; text-align: center; flex: 1;';
            
            const prevBtn = document.createElement('button');
            prevBtn.className = 'btn btn-secondary';
            prevBtn.style.cssText = 'padding: 6px 12px; font-size: 12px;';
            prevBtn.innerHTML = '<i class="fas fa-chevron-left"></i> 上月';
            prevBtn.onclick = () => changeAvailabilityMonth(-1, empId);
            
            const nextBtn = document.createElement('button');
            nextBtn.className = 'btn btn-secondary';
            nextBtn.style.cssText = 'padding: 6px 12px; font-size: 12px;';
            nextBtn.innerHTML = '下月 <i class="fas fa-chevron-right"></i>';
            nextBtn.onclick = () => changeAvailabilityMonth(1, empId);
            
            navDiv.appendChild(prevBtn);
            navDiv.appendChild(monthYear);
            navDiv.appendChild(nextBtn);
            calendar.appendChild(navDiv);
            
            const year = window.availabilityYear;
            const month = window.availabilityMonth;
            
            // 更新月份顯示
            const monthNames = ['1月', '2月', '3月', '4月', '5月', '6月', '7月', '8月', '9月', '10月', '11月', '12月'];
            monthYear.textContent = `${year}年${monthNames[month]}`;
            
            // 創建日期選擇日曆
            const calendarGrid = document.createElement('div');
            calendarGrid.style.cssText = 'display: grid; grid-template-columns: repeat(7, 1fr); gap: 5px; margin-bottom: 10px;';
            
            // 星期標題
            const weekDays = ['日', '一', '二', '三', '四', '五', '六'];
            weekDays.forEach(day => {
                const dayLabel = document.createElement('div');
                dayLabel.style.cssText = 'text-align: center; font-weight: 600; font-size: 11px; color: #999; padding: 5px 0;';
                dayLabel.textContent = day;
                calendarGrid.appendChild(dayLabel);
            });
            
            // 獲取月份的第一天和最後一天
            const firstDay = new Date(year, month, 1);
            const lastDay = new Date(year, month + 1, 0);
            const startDate = new Date(firstDay);
            startDate.setDate(startDate.getDate() - firstDay.getDay());
            
            // 填充日期按鈕
            for (let i = 0; i < 42; i++) {
                const date = new Date(startDate);
                date.setDate(date.getDate() + i);
                
                // 修復日期格式化，避免時區問題
                const year = date.getFullYear();
                const month_num = date.getMonth();
                const month = String(month_num + 1).padStart(2, '0');
                const day = String(date.getDate()).padStart(2, '0');
                const dateStr = `${year}-${month}-${day}`;
                
                const dayNum = date.getDate();
                const isCurrentMonth = date.getMonth() === month_num;
                
                const btn = document.createElement('button');
                btn.style.cssText = 'padding: 8px; font-size: 12px; border-radius: 4px; cursor: pointer; border: 1px solid #ddd; background: white; transition: all 0.3s;';
                
                if (!isCurrentMonth) {
                    btn.style.opacity = '0.3';
                    btn.disabled = true;
                    btn.textContent = dayNum;
                } else {
                    // 檢查是否已經是正式記錄
                    const isUnavailable = unavailableData[dateStr]?.some(item => 
                        typeof item === 'object' ? item.empId === empId : item === empId
                    );
                    
                    // 檢查是否在預選中
                    const isPending = window.pendingUnavailableData && window.pendingUnavailableData[dateStr] && 
                                     window.pendingUnavailableData[dateStr].includes(empId);
                    
                    // 統計該日期已額滿的人數
                    const countUnavailable = unavailableData[dateStr]?.filter(item => 
                        typeof item === 'object' ? true : true
                    ).length || 0;
                    const isFull = countUnavailable >= 2;
                    
                    btn.textContent = dayNum;
                    btn.onclick = () => toggleUnavailability(empId, dateStr, btn, isFull);
                    
                    // 優先級：正式記錄 > 預選 > 已滿額 > 正常
                    if (isUnavailable) {
                        // 正式記錄 - 紅色（可點擊以修改或刪除）
                        btn.style.cssText = 'padding: 8px; font-size: 12px; border-radius: 4px; cursor: pointer; border: 1px solid #FF6B6B; background: #FF6B6B; color: white; font-weight: 600; position: relative;';
                        btn.disabled = false;
                        // 添加刪除圖示提示
                        btn.title = '點擊刪除或修改排休';
                    } else if (isPending) {
                        // 預選狀態 - 橘色
                        btn.style.cssText = 'padding: 8px; font-size: 12px; border-radius: 4px; cursor: pointer; border: 2px solid #FF9933; background: #FFB366; color: white; font-weight: 600;';
                    } else if (isFull) {
                        // 已滿額
                        btn.disabled = true;
                        btn.style.cssText = 'padding: 8px; font-size: 12px; border-radius: 4px; cursor: not-allowed; border: 1px solid #e74c3c; background: #e74c3c; color: white; opacity: 0.5;';
                    }
                }
                
                calendarGrid.appendChild(btn);
            }
            
            calendar.appendChild(calendarGrid);
            
            updateAvailabilityStats();
            renderAvailableList();
        }

        function clearPendingUnavailability() {
            const empId = parseInt(document.getElementById('employeeSelect').value);
            if (!empId) {
                alert('請先選擇員工');
                return;
            }
            
            window.pendingUnavailableData = {};
            updateEmployeeCalendar();
            alert('已清空預選日期');
        }

        function changeAvailabilityMonth(direction, empId) {
            window.availabilityMonth += direction;
            
            // 處理年份變化
            if (window.availabilityMonth > 11) {
                window.availabilityMonth = 0;
                window.availabilityYear += 1;
            } else if (window.availabilityMonth < 0) {
                window.availabilityMonth = 11;
                window.availabilityYear -= 1;
            }
            
            renderAvailabilityCalendar(empId);
        }

        function toggleUnavailability(empId, dateStr, btn, isFull) {
            // 初始化臨時預選數據
            if (!window.pendingUnavailableData) {
                window.pendingUnavailableData = {};
            }
            if (!window.pendingUnavailableData[dateStr]) {
                window.pendingUnavailableData[dateStr] = [];
            }
            
            // 檢查是否已經是正式記錄
            const hasLeave = unavailableData[dateStr] && unavailableData[dateStr].some(item => 
                typeof item === 'object' ? item.empId === empId : item === empId
            );
            
            // 如果是正式記錄，則允許取消排休
            if (hasLeave) {
                const confirmCancel = confirm('確認要取消該日期的排休申請嗎？');
                if (!confirmCancel) return;
                
                // 從正式記錄中移除該員工
                unavailableData[dateStr] = unavailableData[dateStr].filter(item => 
                    typeof item === 'object' ? item.empId !== empId : item !== empId
                );
                
                // 如果該日期沒有其他人了，刪除該日期的記錄
                if (unavailableData[dateStr].length === 0) {
                    delete unavailableData[dateStr];
                }
                
                // 保存數據
                saveData();
                
                // 更新UI
                updateEmployeeCalendar();
                renderLeavePreviewTable();
                updateAvailabilityStats();
                renderAvailableList();
                
                alert('已取消排休申請');
                return;
            }
            
            // 如果此日期已額滿，且未在預選中
            const countUnavailable = unavailableData[dateStr]?.filter(item => 
                typeof item === 'object' ? true : true
            ).length || 0;
            
            if (countUnavailable >= 2 && !window.pendingUnavailableData[dateStr].includes(empId)) {
                alert('此日期已額滿（已有 2 位員工申報無法排班），無法再選擇');
                return;
            }
            
            // 切換臨時預選狀態
            const idx = window.pendingUnavailableData[dateStr].indexOf(empId);
            if (idx > -1) {
                window.pendingUnavailableData[dateStr].splice(idx, 1);
                // 移除橘色
                btn.style.background = 'white';
                btn.style.color = '#333';
                btn.style.border = '1px solid #ddd';
                btn.style.fontWeight = 'normal';
            } else {
                window.pendingUnavailableData[dateStr].push(empId);
                // 變成橘色預選狀態
                btn.style.background = '#FFB366';
                btn.style.color = 'white';
                btn.style.border = '2px solid #FF9933';
                btn.style.fontWeight = '600';
            }
        }

        
function submitUnavailability() {
            const empId = parseInt(document.getElementById('employeeSelect').value);

            if (!empId) {
                alert('請選擇員工');
                return;
            }

            // 檢查是否有預選日期
            if (!window.pendingUnavailableData || Object.keys(window.pendingUnavailableData).length === 0) {
                // 檢查所有日期中是否有該員工的預選
                let hasPending = false;
                for (let dateStr in window.pendingUnavailableData || {}) {
                    if (window.pendingUnavailableData[dateStr].includes(empId)) {
                        hasPending = true;
                        break;
                    }
                }
                if (!hasPending) {
                    alert('請先點選要排休的日期');
                    return;
                }
            }

            // 將預選日期轉換為正式記錄
            let confirmedCount = 0;
            for (let dateStr in window.pendingUnavailableData) {
                if (window.pendingUnavailableData[dateStr].includes(empId)) {
                    if (!unavailableData[dateStr]) {
                        unavailableData[dateStr] = [];
                    }

                    const alreadyExists = unavailableData[dateStr].some(item => {
                        const currentId = typeof item === 'object'
                            ? item.empId
                            : item;

                        return Number(currentId) === Number(empId);
                    });

                    if (!alreadyExists) {
                        unavailableData[dateStr].push({
                            empId: empId,
                            shift: selectedTimeSlots[0] || '全天'
                        });
                        confirmedCount++;
                    }
                }
            }

            // 清空該員工的預選數據
            window.pendingUnavailableData = {};

            saveData();

            // 重新渲染界面
            updateEmployeeCalendar();
            renderAvailableList();
            renderLeavePreviewTable();
            updateStatistics();

            if (confirmedCount > 0) {
                alert(`已成功送出 ${confirmedCount} 個無法排班申請`);
            } else {
                alert('已確認申請');
            }
        }

        function OLD_submitUnavailability_DISABLED() {

            const empId = parseInt(document.getElementById('employeeSelect').value);
            if (!empId) {
                alert('請先選擇員工');
                return;
            }
            
            if (!selectedTimeSlots || selectedTimeSlots.length === 0) {
                alert('請先選擇班別');
                return;
            }
            
            // 獲取員工已選擇的無法排班日期
            let selectedDates = [];
            const today = new Date();
            const weekStart = new Date(today);
            weekStart.setDate(weekStart.getDate() - weekStart.getDay() + 1 + currentWeek * 7);
            
            for (let i = 0; i < 7; i++) {
                const date = new Date(weekStart);
                date.setDate(date.getDate() + i);
                const y = date.getFullYear();
                const m = String(date.getMonth() + 1).padStart(2, '0');
                const d = String(date.getDate()).padStart(2, '0');
                const dateStr = `${y}-${m}-${d}`;
                const isUnavailable = unavailableData[dateStr]?.some(item => 
                    typeof item === 'object' ? item.empId === empId : item === empId
                );
                if (isUnavailable) {
                    selectedDates.push(dateStr);
                }
            }
            
            // 更新unavailableData以儲存班別信息
            selectedDates.forEach(dateStr => {
                if (!unavailableData[dateStr]) {
                    unavailableData[dateStr] = [];
                }
                
                // 刪除舊的該員工記錄
                unavailableData[dateStr] = unavailableData[dateStr].filter(item => 
                    typeof item === 'object' ? item.empId !== empId : item !== empId
                );
                
                // 新增包含班別信息的記錄
                unavailableData[dateStr].push({
                    empId: empId,
                    shifts: selectedTimeSlots
                });
            });
            
            alert('已儲存員工無法排班信息（班別：' + selectedTimeSlots.join(', ') + '）');
            saveData();
        }

        function updateAvailabilityStats() {
            let totalUnavailable = 0;
            let fullCapacity = 0;
            
            for (let dateStr in unavailableData) {
                const dataArray = unavailableData[dateStr] || [];
                totalUnavailable += dataArray.length;
                if (dataArray.length >= 2) fullCapacity++;
            }
            
            document.getElementById('totalUnavailable').textContent = totalUnavailable;
            document.getElementById('fullCapacity').textContent = fullCapacity;
            document.getElementById('availableSlots').textContent = 7 - fullCapacity;
            const avgRate = employees.length > 0 ? Math.round((totalUnavailable / (employees.length * 7)) * 100) : 0;
            document.getElementById('avgUnavailable').textContent = avgRate + '%';
        }

        function renderAvailableList() {
            const list = document.getElementById('unavailableDatesList');
            list.innerHTML = '';
            
            const today = new Date();
            const weekStart = new Date(today);
            weekStart.setDate(weekStart.getDate() - weekStart.getDay() + 1 + currentWeek * 7);
            const daysOfWeek = ['一', '二', '三', '四', '五', '六', '日'];
            
            for (let i = 0; i < 7; i++) {
                const date = new Date(weekStart);
                date.setDate(date.getDate() + i);
                
                // 使用與renderAvailabilityCalendar相同的日期格式化方式
                const year = date.getFullYear();
                const month_num = date.getMonth();
                const month = String(month_num + 1).padStart(2, '0');
                const day = String(date.getDate()).padStart(2, '0');
                const dateStr = `${year}-${month}-${day}`;
                
                const dayNum = String(date.getDate()).padStart(2, '0');
                const dayOfWeek = daysOfWeek[i];
                const count = unavailableData[dateStr]?.length || 0;
                
                if (count > 0) {
                    const item = document.createElement('div');
                    item.className = 'settings-item';
                    const empNames = (unavailableData[dateStr] || [])
                        .map(entry => {
                            const empId = typeof entry === 'object' ? entry.empId : entry;
                            return employees.find(e => e.id === empId)?.name || '未知';
                        })
                        .join(', ');
                    
                    // 獲取該日期的班別信息
                    let shiftInfo = '';
                    const shiftData = unavailableData[dateStr][0];
                    if (typeof shiftData === 'object' && shiftData.shifts) {
                        const shiftNames = shiftData.shifts.map(s => shifts[s]?.name || s).join('/');
                        shiftInfo = `<span style="font-size: 11px; color: #C4956C; margin-left: 10px;">（${shiftNames}）</span>`;
                    }
                    
                    item.innerHTML = `
                        <div>
                            <strong>${month}月${dayNum}日(${dayOfWeek})</strong>${shiftInfo}<br>
                            <span style="font-size: 12px; color: #666;">${empNames}</span>
                            <span style="font-size: 11px; color: #999; margin-left: 10px;">${count}/2</span>
                        </div>
                    `;
                    list.appendChild(item);
                }
            }
            
            if (list.children.length === 0) {
                list.innerHTML = '<div style="text-align: center; color: #999; padding: 20px; font-size: 12px;">暫無人員申報無法排班</div>';
            }
        }

        function renderGanttChart() {
            const container = document.getElementById('ganttContainer');
            if (!container) {
                console.error('甘特圖容器不存在');
                return;
            }
            
            container.innerHTML = '';
            // 確保容器可見
            container.style.display = 'block';
            
            const table = document.createElement('table');
            table.className = 'gantt-table';
            
            // 獲取當前週的日期範圍
            const today = new Date();
            const weekStart = new Date(today);
            weekStart.setDate(weekStart.getDate() - weekStart.getDay() + 1 + currentWeek * 7);
            
            // 如果甘特圖日期未設定，設定為當前週的第一天
            if (!window.ganttDate) {
                window.ganttDate = new Date(weekStart);
            }
            
            // 確保甘特圖日期在當前週內
            // 使用正確的日期格式化方式（避免時區問題）
            const getDateStr = (date) => {
                const y = date.getFullYear();
                const m = String(date.getMonth() + 1).padStart(2, '0');
                const d = String(date.getDate()).padStart(2, '0');
                return `${y}-${m}-${d}`;
            };
            
            const ganttDateStr = getDateStr(window.ganttDate);
            const weekStartStr = getDateStr(weekStart);
            const weekEndDate = new Date(weekStart);
            weekEndDate.setDate(weekEndDate.getDate() + 6);
            const weekEndStr = getDateStr(weekEndDate);
            
            // 如果甘特圖日期不在當前週內，重置為週開始日期
            if (ganttDateStr < weekStartStr || ganttDateStr > weekEndStr) {
                window.ganttDate = new Date(weekStart);
            }
            
            const selectedDate = getDateStr(window.ganttDate);
            
            // 更新日期輸入框
            const dateInput = document.getElementById('ganttDatePicker');
            if (dateInput) {
                dateInput.value = selectedDate;
            }
            
            // 確保該日期在scheduleData中有記錄
            if (!scheduleData[selectedDate]) {
                scheduleData[selectedDate] = {};
            }
            
            // 計算時間範圍
            const startHour = 8;
            const endHour = 24;
            const totalHours = endHour - startHour;
            
            // 表頭
            let headerHtml = '<tr><td class="gantt-header" style="width: 80px;">人員</td>';
            for (let i = startHour; i < endHour; i++) {
                const displayHour = i >= 24 ? 0 : i;
                const time = String(displayHour).padStart(2, '0');
                headerHtml += `<td class="gantt-header" title="${time}:00">${time}</td>`;
            }
            headerHtml += '</tr>';
            table.innerHTML = headerHtml;
            
            // 檢查是否有員工
            if (!employees || employees.length === 0) {
                const emptyRow = document.createElement('tr');
                const emptyCell = document.createElement('td');
                emptyCell.colSpan = 16 + 1;  // 16小時 + 員工名稱欄
                emptyCell.style.textAlign = 'center';
                emptyCell.style.padding = '20px';
                emptyCell.textContent = '沒有員工';
                emptyRow.appendChild(emptyCell);
                table.appendChild(emptyRow);
                container.appendChild(table);
                return;
            }
            
            // 員工資料 - 只顯示當天有排班的員工
            employees.forEach((emp) => {
                // 計算員工在選定日期的排班
                const empSchedules = [];
                const shiftData = scheduleData[selectedDate];
                if (shiftData && Object.keys(shiftData).length > 0) {
                    for (let shiftId in shiftData) {
                        const shiftEmps = shiftData[shiftId];
                        let found = false;
                        
                        if (Array.isArray(shiftEmps)) {
                            if (shiftEmps.some(e => e && e.id === emp.id)) {
                                found = true;
                            }
                        } else if (shiftEmps && shiftEmps.id === emp.id) {
                            found = true;
                        }
                        
                        if (found) {
                            const shiftInfo = shifts[shiftId];
                            if (shiftInfo) {
                                empSchedules.push({
                                    date: selectedDate,
                                    shiftId: shiftId,
                                    name: shiftInfo.name,
                                    start: parseInt(shiftInfo.start.split(':')[0]),
                                    end: parseInt(shiftInfo.end.split(':')[0])
                                });
                            }
                        }
                    }
                }
                
                // 如果當天沒有排班，跳過此員工
                if (empSchedules.length === 0) {
                    return;
                }
                
                const row = document.createElement('tr');
                
                // 員工名稱和照片
                const nameCell = document.createElement('td');
                nameCell.className = 'gantt-cell gantt-employee-name';
                let avatarHtml = `<div class="employee-avatar">`;
                if (emp.photo) {
                    avatarHtml += `<img src="${emp.photo}" alt="${emp.name}">`;
                } else {
                    avatarHtml += emp.avatar;
                }
                avatarHtml += `</div>`;
                nameCell.innerHTML = avatarHtml + emp.name;
                row.appendChild(nameCell);
                
                // 創建時段格子 - 合併班別為連續長條
                let currentBar = null;
                let barStartHour = null;
                let barSpan = 0;
                
                for (let h = startHour; h < endHour; h++) {
                    // 檢查此小時是否有排班
                    const hasShift = empSchedules.some(sch => h >= sch.start && h < sch.end);
                    const shiftInfo = empSchedules.find(sch => h >= sch.start && h < sch.end);
                    
                    if (hasShift && shiftInfo) {
                        // 比較班別名稱和ID，確保同一班別連續
                        if (!currentBar || currentBar.name !== shiftInfo.name || currentBar.shiftId !== shiftInfo.shiftId) {
                            // 新班別開始
                            if (currentBar) {
                                // 完成上一個長條
                                const cell = document.createElement('td');
                                cell.className = 'gantt-cell';
                                cell.colSpan = barSpan;
                                const bar = document.createElement('div');
                                bar.className = 'gantt-bar';
                                // 如果班別開始時間在下午2點或以後，使用不同顏色
                                if (barStartHour >= 14) {
                                    bar.classList.add('afternoon');
                                }
                                bar.style.width = '100%';
                                bar.textContent = currentBar.name;
                                cell.appendChild(bar);
                                row.appendChild(cell);
                            }
                            currentBar = shiftInfo;
                            barStartHour = h;
                            barSpan = 1;
                        } else {
                            // 繼續當前長條
                            barSpan++;
                        }
                    } else {
                        if (currentBar) {
                            // 完成當前長條
                            const cell = document.createElement('td');
                            cell.className = 'gantt-cell';
                            cell.colSpan = barSpan;
                            const bar = document.createElement('div');
                            bar.className = 'gantt-bar';
                            // 如果班別開始時間在下午2點或以後，使用不同顏色
                            if (barStartHour >= 14) {
                                bar.classList.add('afternoon');
                            }
                            bar.style.width = '100%';
                            bar.textContent = currentBar.name;
                            cell.appendChild(bar);
                            row.appendChild(cell);
                            currentBar = null;
                            barSpan = 0;
                        }
                        // 空白格
                        const cell = document.createElement('td');
                        cell.className = 'gantt-cell';
                        row.appendChild(cell);
                    }
                }
                
                // 處理最後一個長條
                if (currentBar) {
                    const cell = document.createElement('td');
                    cell.className = 'gantt-cell';
                    cell.colSpan = barSpan;
                    const bar = document.createElement('div');
                    bar.className = 'gantt-bar';
                    // 如果班別開始時間在下午2點或以後，使用不同顏色
                    if (barStartHour >= 14) {
                        bar.classList.add('afternoon');
                    }
                    bar.style.width = '100%';
                    bar.textContent = currentBar.name;
                    cell.appendChild(bar);
                    row.appendChild(cell);
                }
                
                table.appendChild(row);
            });
            
            container.appendChild(table);
            
            // 確保容器可見，即使表格為空
            if (container.offsetHeight === 0) {
                container.style.minHeight = '300px';
            }
        }

        function renderSettingsPage() {
            renderEmployeesList();
            renderShiftsList();
        }

        function renderEmployeesList() {
            const container = document.getElementById('employeesList');
            container.innerHTML = '';
            
            employees.forEach((emp) => {
                const div = document.createElement('div');
                div.className = 'settings-item';
                let photoHtml = '';
                if (emp.photo) {
                    photoHtml = `<img src="${emp.photo}" alt="${emp.name}" class="photo-preview">`;
                }
                div.innerHTML = `
                    <div style="display: flex; justify-content: space-between; align-items: center;">
                        <div style="display: flex; align-items: center;">
                            ${photoHtml}
                            <div>
                                <strong>${emp.name}</strong> (${emp.role})
                            </div>
                        </div>
                        <div style="display: flex; gap: 8px;">
                            <button class="delete-btn" onclick="editEmployee(${emp.id})" style="background: #e8f0ff; border: 1px solid #d4e0ff; color: var(--primary-color);">編輯</button>
                            <button class="delete-btn" onclick="deleteEmployee(${emp.id})">刪除</button>
                        </div>
                    </div>
                `;
                container.appendChild(div);
            });
        }

        function renderShiftsList() {
            const container = document.getElementById('shiftsList');
            container.innerHTML = '';
            
            for (let shiftId in shifts) {
                const shift = shifts[shiftId];
                const div = document.createElement('div');
                div.className = 'settings-item';
                div.innerHTML = `
                    <div style="display: flex; justify-content: space-between; align-items: center;">
                        <div>
                            <strong>${shift.name}</strong> ${shift.start} - ${shift.end}
                        </div>
                        <button class="delete-btn" onclick="deleteShift('${shiftId}')">刪除</button>
                    </div>
                `;
                container.appendChild(div);
            }
        }

        function openAddEmployeeModal() {
            // 清除編輯狀態，這是新增員工
            window.editingEmployeeId = undefined;
            
            // 清空輸入框
            document.getElementById('newEmployeeName').value = '';
            document.getElementById('newEmployeeRole').value = '';
            document.getElementById('newEmployeePhoto').value = '';
            document.getElementById('photoPreview').innerHTML = '';
            
            document.getElementById('addEmployeeModal').classList.add('active');
            
            // 添加照片預覽功能
            const photoInput = document.getElementById('newEmployeePhoto');
            photoInput.onchange = function(e) {
                const file = e.target.files[0];
                if (file) {
                    const reader = new FileReader();
                    reader.onload = function(e) {
                        document.getElementById('photoPreview').innerHTML = 
                            `<img src="${e.target.result}" style="width: 100px; height: 100px; border-radius: 8px; object-fit: cover;">`;
                    };
                    reader.readAsDataURL(file);
                }
            };
        }

        function openAddShiftModal() {
            document.getElementById('addShiftModal').classList.add('active');
        }

        function confirmAddEmployee() {
            const name = document.getElementById('newEmployeeName').value.trim();
            const role = document.getElementById('newEmployeeRole').value.trim();
            const photoInput = document.getElementById('newEmployeePhoto');
            
            if (!name || !role) {
                alert('請填寫完整的員工信息');
                return;
            }
            
            // 檢查是否是編輯模式
            const isEditing = window.editingEmployeeId !== undefined;
            const empId = isEditing ? window.editingEmployeeId : Math.max(...employees.map(e => e.id), 0) + 1;
            const avatar = name.substring(0, 2).toUpperCase();
            
            let photo = null;
            if (photoInput.files && photoInput.files[0]) {
                const file = photoInput.files[0];
                const reader = new FileReader();
                reader.onload = function(e) {
                    // 使用圖片壓縮來減少文件大小
                    const img = new Image();
                    img.onload = function() {
                        // 縮小圖片尺寸到150x150像素，降低品質到70%
                        const canvas = document.createElement('canvas');
                        const maxSize = 150;
                        let width = img.width;
                        let height = img.height;
                        
                        if (width > height) {
                            if (width > maxSize) {
                                height *= maxSize / width;
                                width = maxSize;
                            }
                        } else {
                            if (height > maxSize) {
                                width *= maxSize / height;
                                height = maxSize;
                            }
                        }
                        
                        canvas.width = width;
                        canvas.height = height;
                        const ctx = canvas.getContext('2d');
                        ctx.drawImage(img, 0, 0, width, height);
                        
                        // 轉換為JPEG格式品質70%以大幅減少文件大小
                        photo = canvas.toDataURL('image/jpeg', 0.7);
                        
                        if (isEditing) {
                            // 編輯現有員工
                            const emp = employees.find(x => x.id === empId);
                            if (emp) {
                                emp.name = name;
                                emp.role = role;
                                emp.avatar = avatar;
                                emp.photo = photo;
                                // 保存照片到IndexedDB（不保存到localStorage）
                                savePhotoToIndexedDB(empId, photo);
                            }
                        } else {
                            // 新增員工
                            employees.push({
                                id: empId,
                                name: name,
                                role: role,
                                avatar: avatar,
                                photo: photo
                            });
                            // 保存照片到IndexedDB（不保存到localStorage）
                            savePhotoToIndexedDB(empId, photo);
                        }
                        
                        document.getElementById('newEmployeeName').value = '';
                        document.getElementById('newEmployeeRole').value = '';
                        photoInput.value = '';
                        document.getElementById('photoPreview').innerHTML = '';
                        window.editingEmployeeId = undefined;
                        closeModal('addEmployeeModal');
                        
                        renderSettingsPage();
                        renderEmployeeList();
                        renderEmployeeSelect();
                        renderModalEmployeeSelect();
                        renderTimeSlotButtons();
            renderAvailabilityCalendar();
            renderLeavePreviewTable();
                        renderCalendarGrid();
                        renderGanttChart();
                        saveData();  // 只保存employees（不含photo到localStorage）
                    };
                    img.src = e.target.result;
                };
                reader.readAsDataURL(file);
            } else {
                if (isEditing) {
                    // 編輯現有員工（不更換照片）
                    const emp = employees.find(x => x.id === empId);
                    if (emp) {
                        emp.name = name;
                        emp.role = role;
                        emp.avatar = avatar;
                    }
                } else {
                    // 新增員工
                    employees.push({
                        id: empId,
                        name: name,
                        role: role,
                        avatar: avatar,
                        photo: null
                    });
                }
                
                document.getElementById('newEmployeeName').value = '';
                document.getElementById('newEmployeeRole').value = '';
                photoInput.value = '';
                document.getElementById('photoPreview').innerHTML = '';
                window.editingEmployeeId = undefined;
                closeModal('addEmployeeModal');
                
                renderSettingsPage();
                renderEmployeeList();
                renderEmployeeSelect();
                renderModalEmployeeSelect();
                renderTimeSlotButtons();
            renderAvailabilityCalendar();
            renderLeavePreviewTable();
                renderCalendarGrid();
                renderGanttChart();
                saveData();
            }
        }

        function confirmAddShift() {
            const name = document.getElementById('newShiftName').value.trim();
            const start = document.getElementById('newShiftStart').value;
            const end = document.getElementById('newShiftEnd').value;
            
            if (!name || !start || !end) {
                alert('請填寫完整的班別信息');
                return;
            }
            
            const newId = 'shift_' + Date.now();
            shifts[newId] = {
                id: newId,
                name: name,
                start: start,
                end: end
            };
            
            document.getElementById('newShiftName').value = '';
            document.getElementById('newShiftStart').value = '';
            document.getElementById('newShiftEnd').value = '';
            closeModal('addShiftModal');
            
            renderTimeSlotButtons();
            renderAvailabilityCalendar();
            renderLeavePreviewTable();
            renderCalendarGrid();
            renderGanttChart();
            renderShiftsList();
            saveData();
        }

        function editEmployee(empId) {
            const emp = employees.find(e => e.id === empId);
            if (!emp) return;
            
            // 打開編輯模態框，預填現有數據
            const modal = document.getElementById('addEmployeeModal');
            document.getElementById('newEmployeeName').value = emp.name;
            document.getElementById('newEmployeeRole').value = emp.role;
            document.getElementById('newEmployeePhoto').value = '';
            
            if (emp.photo) {
                document.getElementById('photoPreview').innerHTML = 
                    `<img src="${emp.photo}" style="width: 100px; height: 100px; border-radius: 8px; object-fit: cover;">`;
            } else {
                document.getElementById('photoPreview').innerHTML = '';
            }
            
            modal.classList.add('active');
            
            // 保存編輯狀態，下次確認時會更新而不是新增
            window.editingEmployeeId = empId;
        }

        function cleanupDeletedEmployeesFromSchedule() {
            // 獲取當前所有員工的ID集合
            const currentEmployeeIds = new Set(employees.map(e => e.id));
            
            // 清理scheduleData中不存在的員工
            for (let dateStr in scheduleData) {
                for (let shiftId in scheduleData[dateStr]) {
                    const shiftData = scheduleData[dateStr][shiftId];
                    
                    if (Array.isArray(shiftData)) {
                        // 過濾出存在的員工
                        const filteredEmployees = shiftData.filter(emp => {
                            // 處理不同的員工對象結構
                            if (!emp) return false;
                            const empId = typeof emp === 'object' && emp.id ? emp.id : emp;
                            return currentEmployeeIds.has(empId);
                        });
                        
                        // 更新或刪除該班別
                        if (filteredEmployees.length > 0) {
                            scheduleData[dateStr][shiftId] = filteredEmployees;
                        } else {
                            delete scheduleData[dateStr][shiftId];
                        }
                    } else if (shiftData && typeof shiftData === 'object' && shiftData.id) {
                        // 處理非陣列格式的員工對象
                        if (!currentEmployeeIds.has(shiftData.id)) {
                            delete scheduleData[dateStr][shiftId];
                        }
                    }
                }
                
                // 如果該日期沒有班別了，刪除該日期
                if (Object.keys(scheduleData[dateStr]).length === 0) {
                    delete scheduleData[dateStr];
                }
            }
            
            // 清理unavailableData中不存在的員工
            for (let dateStr in unavailableData) {
                unavailableData[dateStr] = unavailableData[dateStr].filter(item => {
                    if (typeof item === 'object' && item.empId) {
                        return currentEmployeeIds.has(item.empId);
                    } else if (typeof item === 'number') {
                        return currentEmployeeIds.has(item);
                    }
                    return true;
                });
                
                // 如果該日期沒有排休記錄了，刪除該日期
                if (unavailableData[dateStr].length === 0) {
                    delete unavailableData[dateStr];
                }
            }
            
            // 保存清理後的數據
            saveData();
        }

        function deleteEmployee(empId) {
            if (confirm('確定要刪除此員工嗎？')) {
                // 從scheduleData中移除該員工的所有排班
                for (let dateStr in scheduleData) {
                    for (let shiftId in scheduleData[dateStr]) {
                        const shiftData = scheduleData[dateStr][shiftId];
                        
                        if (Array.isArray(shiftData)) {
                            // 過濾掉該員工（支持多種對象結構）
                            scheduleData[dateStr][shiftId] = shiftData.filter(emp => {
                                const currentEmpId = emp && emp.id ? emp.id : emp;
                                return currentEmpId !== empId;
                            });
                            
                            // 如果該班別沒有員工了，刪除該班別
                            if (scheduleData[dateStr][shiftId].length === 0) {
                                delete scheduleData[dateStr][shiftId];
                            }
                        } else if (shiftData && shiftData.id === empId) {
                            // 如果是非陣列格式的員工，直接刪除該班別
                            delete scheduleData[dateStr][shiftId];
                        }
                    }
                    
                    // 如果該日期沒有班別了，刪除該日期
                    if (Object.keys(scheduleData[dateStr]).length === 0) {
                        delete scheduleData[dateStr];
                    }
                }
                
                // 從unavailableData中移除該員工的所有排休記錄
                for (let dateStr in unavailableData) {
                    unavailableData[dateStr] = unavailableData[dateStr].filter(item => {
                        if (typeof item === 'object') {
                            return item.empId !== empId;
                        } else {
                            return item !== empId;
                        }
                    });
                    
                    // 如果該日期沒有排休記錄了，刪除該日期
                    if (unavailableData[dateStr].length === 0) {
                        delete unavailableData[dateStr];
                    }
                }
                
                // 從員工列表中刪除
                employees = employees.filter(e => e.id !== empId);
                
                // 同時刪除IndexedDB中該員工的照片
                deletePhotoFromIndexedDB(empId);
                
                // 再次清理確保所有該員工的記錄都被移除
                cleanupDeletedEmployeesFromSchedule();
                
                renderSettingsPage();
                renderEmployeeList();
                renderEmployeeSelect();
                renderModalEmployeeSelect();
                renderTimeSlotButtons();
            renderAvailabilityCalendar();
            renderLeavePreviewTable();
                renderCalendarGrid();
                renderGanttChart();
                saveData();
            }
        }

        function deleteShift(shiftId) {
            if (confirm('確定要刪除此班別嗎？')) {
                delete shifts[shiftId];
                renderTimeSlotButtons();
            renderAvailabilityCalendar();
            renderLeavePreviewTable();
                renderCalendarGrid();
                renderGanttChart();
                renderShiftsList();
                saveData();
            }
        }

        function updateStatistics() {
            let totalAssigned = new Set();
            let totalHours = 0;
            
            for (let date in scheduleData) {
                for (let shift in scheduleData[date]) {
                    const shiftData = scheduleData[date][shift];
                    if (Array.isArray(shiftData)) {
                        shiftData.forEach(emp => totalAssigned.add(emp.id));
                        if (shifts[shift]) {
                            const start = parseInt(shifts[shift].start.split(':')[0]);
                            const end = parseInt(shifts[shift].end.split(':')[0]);
                            totalHours += (end - start) * shiftData.length;
                        }
                    } else if (shiftData) {
                        totalAssigned.add(shiftData.id);
                        if (shifts[shift]) {
                            const start = parseInt(shifts[shift].start.split(':')[0]);
                            const end = parseInt(shifts[shift].end.split(':')[0]);
                            totalHours += (end - start);
                        }
                    }
                }
            }
            
            document.getElementById('totalEmployees').textContent = employees.length;
            document.getElementById('assignedEmployees').textContent = totalAssigned.size;
            document.getElementById('shortStaff').textContent = Math.max(0, employees.length - totalAssigned.size);
            document.getElementById('totalHours').textContent = totalHours;
        }

        
        function publishSchedule() {
            alert('班表已發布，員工將通過 LINE / Email 收到通知');
        }

        function previousWeek() {
            currentWeek--;
            window.ganttDate = undefined;  // 重置甘特圖日期
            initializeScheduleData();
            renderCalendarGrid();
            renderGanttChart();
        }

        function nextWeek() {
            currentWeek++;
            window.ganttDate = undefined;  // 重置甘特圖日期
            initializeScheduleData();
            renderCalendarGrid();
            renderGanttChart();
        }

        function exportGanttPDF() {
            const element = document.getElementById('ganttContainer');
            if (!element) {
                alert('沒有班表數據可匯出');
                return;
            }
            
            try {
                const today = new Date();
                const filename = `班表_${today.getFullYear()}-${String(today.getMonth()+1).padStart(2,'0')}-${String(today.getDate()).padStart(2,'0')}.pdf`;
                
                const opt = {
                    margin: 10,
                    filename: filename,
                    image: { type: 'jpeg', quality: 0.98 },
                    html2canvas: { scale: 2 },
                    jsPDF: { orientation: 'landscape', unit: 'mm', format: 'a4' }
                };
                html2pdf().set(opt).from(element).save();
            } catch (error) {
                alert('匯出PDF失敗: ' + error.message);
            }
        }

        function exportGanttExcel() {
            const table = document.getElementById('ganttContainer').querySelector('.gantt-table');
            if (!table) {
                alert('沒有班表數據可匯出');
                return;
            }
            
            try {
                const ws = XLSX.utils.table_to_sheet(table);
                const wb = XLSX.utils.book_new();
                XLSX.utils.book_append_sheet(wb, ws, "班表");
                
                const today = new Date();
                const filename = `班表_${today.getFullYear()}-${String(today.getMonth()+1).padStart(2,'0')}-${String(today.getDate()).padStart(2,'0')}.xlsx`;
                XLSX.writeFile(wb, filename);
            } catch (error) {
                alert('匯出Excel失敗: ' + error.message);
            }
        }

        function previousGanttDay() {
            if (!window.ganttDate) {
                const today = new Date();
                const weekStart = new Date(today);
                weekStart.setDate(weekStart.getDate() - weekStart.getDay() + 1 + currentWeek * 7);
                window.ganttDate = new Date(weekStart);
            }
            window.ganttDate.setDate(window.ganttDate.getDate() - 1);
            checkAndUpdateWeekIfNeeded();
            updateGanttDateDisplay();
        }

        function nextGanttDay() {
            if (!window.ganttDate) {
                const today = new Date();
                const weekStart = new Date(today);
                weekStart.setDate(weekStart.getDate() - weekStart.getDay() + 1 + currentWeek * 7);
                window.ganttDate = new Date(weekStart);
            }
            window.ganttDate.setDate(window.ganttDate.getDate() + 1);
            checkAndUpdateWeekIfNeeded();
            updateGanttDateDisplay();
        }

        function checkAndUpdateWeekIfNeeded() {
            // 計算當前週的日期範圍
            const today = new Date();
            const weekStart = new Date(today);
            weekStart.setDate(weekStart.getDate() - weekStart.getDay() + 1 + currentWeek * 7);
            const weekEnd = new Date(weekStart);
            weekEnd.setDate(weekEnd.getDate() + 6);
            
            // 使用正確的日期格式化方式
            const getDateStr = (date) => {
                const y = date.getFullYear();
                const m = String(date.getMonth() + 1).padStart(2, '0');
                const d = String(date.getDate()).padStart(2, '0');
                return `${y}-${m}-${d}`;
            };
            
            const ganttDateStr = getDateStr(window.ganttDate);
            const weekStartStr = getDateStr(weekStart);
            const weekEndStr = getDateStr(weekEnd);
            
            // 如果甘特圖日期超出當前週，調整currentWeek
            if (ganttDateStr < weekStartStr) {
                // 日期在當前週之前
                currentWeek--;
                initializeScheduleData();
                renderCalendarGrid();
            } else if (ganttDateStr > weekEndStr) {
                // 日期在當前週之後
                currentWeek++;
                initializeScheduleData();
                renderCalendarGrid();
            }
        }

        function updateGanttDate() {
            const dateInput = document.getElementById('ganttDatePicker').value;
            if (dateInput) {
                window.ganttDate = new Date(dateInput);
                checkAndUpdateWeekIfNeeded();
                updateGanttDateDisplay();
            }
        }

        function updateGanttDateDisplay() {
            if (!window.ganttDate) {
                const today = new Date();
                const weekStart = new Date(today);
                weekStart.setDate(weekStart.getDate() - weekStart.getDay() + 1 + currentWeek * 7);
                window.ganttDate = new Date(weekStart);
            }
            
            // 使用正確的日期格式化方式
            const y = window.ganttDate.getFullYear();
            const m = String(window.ganttDate.getMonth() + 1).padStart(2, '0');
            const d = String(window.ganttDate.getDate()).padStart(2, '0');
            const dateStr = `${y}-${m}-${d}`;
            document.getElementById('ganttDatePicker').value = dateStr;
            
            const month = String(window.ganttDate.getMonth() + 1).padStart(2, '0');
            const day = String(window.ganttDate.getDate()).padStart(2, '0');
            const dayOfWeek = ['日', '一', '二', '三', '四', '五', '六'][window.ganttDate.getDay()];
            document.getElementById('ganttTitle').textContent = `班表甘特圖 - ${window.ganttDate.getFullYear()}年${month}月${day}日(${dayOfWeek})`;
            
            renderGanttChart();
        }

        function printGantt() {
            window.print();
        }

        function setupRoleListener() {
            document.getElementById('roleSelect').addEventListener('change', function(e) {
                currentUser = e.target.value;
            });
        }

        function saveData() {
            try {
                // 準備保存到localStorage的employees（不含照片）
                const employeesForStorage = employees.map(emp => ({
                    ...emp,
                    photo: null  // 照片由IndexedDB單獨管理
                }));
                
                localStorage.setItem('scheduleData', JSON.stringify(scheduleData));
                localStorage.setItem('unavailableData', JSON.stringify(unavailableData));
                localStorage.setItem('employees', JSON.stringify(employeesForStorage));
                localStorage.setItem('shifts', JSON.stringify(shifts));
                
                // 同時將所有員工照片保存到IndexedDB
                employees.forEach(emp => {
                    if (emp.photo) {
                        savePhotoToIndexedDB(emp.id, emp.photo);
                    }
                });
                
                console.log('數據已保存（照片到IndexedDB，其他到localStorage）');
            } catch (e) {
                if (e.name === 'QuotaExceededError') {
                    console.error('localStorage已滿，嘗試清理...');
                    
                    // 第一次嘗試：移除所有照片
                    const employeesWithoutPhotos = employees.map(emp => ({
                        ...emp,
                        photo: null  // 移除照片
                    }));
                    
                    try {
                        localStorage.setItem('employees', JSON.stringify(employeesWithoutPhotos));
                        localStorage.setItem('scheduleData', JSON.stringify(scheduleData));
                        localStorage.setItem('unavailableData', JSON.stringify(unavailableData));
                        localStorage.setItem('shifts', JSON.stringify(shifts));
                        
                        // 更新內存中的employees（移除照片）
                        employees = employeesWithoutPhotos;
                        
                        alert('localStorage儲存空間已滿！\\n\\n已將所有照片轉移到IndexedDB。\\n系統仍會保留員工頭像縮寫。\\n\\n照片已自動保存到瀏覽器資料庫中。');
                        
                        // 重新渲染UI
                        renderEmployeeList();
                        renderSettingsPage();
                    } catch (e2) {
                        // 第二次嘗試失敗
                        try {
                            localStorage.setItem('scheduleData', JSON.stringify(scheduleData));
                            localStorage.setItem('unavailableData', JSON.stringify(unavailableData));
                            localStorage.setItem('employees', JSON.stringify(employeesWithoutPhotos));
                            
                            employees = employeesWithoutPhotos;
                            
                            alert('localStorage儲存空間嚴重不足！\\n\\n系統已移除照片以保存排班數據。\\n\\n建議清理瀏覽器緩存以釋放更多空間。');
                        } catch (e3) {
                            alert('儲存空間完全不足！\\n\\n請清理瀏覽器緩存：\\n設定 > 隱私權 > 清除瀏覽資料');
                            console.error('無法保存數據:', e3);
                        }
                    }
                } else {
                    console.error('保存數據時出錯:', e);
                }
            }
        }
    </script>
</body>
</html>
