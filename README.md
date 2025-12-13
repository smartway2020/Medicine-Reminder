<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <meta name="apple-mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
    <meta name="apple-mobile-web-app-title" content="Medicine Reminder">
    <title>Medicine Reminder</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        
        body {
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif;
            background: linear-gradient(to bottom right, #dbeafe, #e0e7ff);
            min-height: 100vh;
            padding: 20px;
            -webkit-user-select: none;
            user-select: none;
        }
        
        .container {
            max-width: 500px;
            margin: 0 auto;
        }
        
        .header {
            text-align: center;
            margin-bottom: 30px;
            padding-top: 20px;
        }
        
        .emoji {
            font-size: 64px;
            margin-bottom: 10px;
        }
        
        .header h1 {
            font-size: 32px;
            color: #1f2937;
            margin-bottom: 10px;
        }
        
        .card {
            background: white;
            border-radius: 20px;
            padding: 30px;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
            margin-bottom: 20px;
        }
        
        .setup-screen {
            text-align: center;
        }
        
        .question {
            font-size: 22px;
            font-weight: 600;
            color: #1f2937;
            margin-bottom: 30px;
        }
        
        .frequency-options {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 12px;
            margin-bottom: 20px;
        }
        
        .frequency-btn {
            padding: 20px;
            border: 3px solid #e5e7eb;
            background: white;
            border-radius: 12px;
            font-size: 18px;
            font-weight: 600;
            color: #374151;
            cursor: pointer;
            transition: all 0.2s;
        }
        
        .frequency-btn:active {
            transform: scale(0.95);
        }
        
        .frequency-btn.selected {
            background: #3b82f6;
            color: white;
            border-color: #3b82f6;
        }
        
        .time-inputs {
            margin-top: 30px;
        }
        
        .time-input-group {
            margin-bottom: 16px;
        }
        
        .time-label {
            font-size: 14px;
            font-weight: 600;
            color: #6b7280;
            margin-bottom: 8px;
            display: block;
        }
        
        .time-input {
            width: 100%;
            padding: 14px;
            border: 2px solid #e5e7eb;
            border-radius: 10px;
            font-size: 18px;
            text-align: center;
        }
        
        .time-input:focus {
            outline: none;
            border-color: #3b82f6;
        }
        
        .btn {
            width: 100%;
            padding: 16px;
            border: none;
            border-radius: 12px;
            font-size: 18px;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.2s;
        }
        
        .btn-primary {
            background: #10b981;
            color: white;
            margin-top: 20px;
        }
        
        .btn-primary:active {
            background: #059669;
            transform: scale(0.98);
        }
        
        .btn-secondary {
            background: #6b7280;
            color: white;
            margin-top: 10px;
        }
        
        .btn-secondary:active {
            background: #4b5563;
            transform: scale(0.98);
        }
        
        .active-screen {
            text-align: center;
        }
        
        .status-emoji {
            font-size: 80px;
            margin-bottom: 20px;
            animation: pulse 2s infinite;
        }
        
        @keyframes pulse {
            0%, 100% { transform: scale(1); }
            50% { transform: scale(1.05); }
        }
        
        .status-title {
            font-size: 28px;
            font-weight: bold;
            color: #1f2937;
            margin-bottom: 10px;
        }
        
        .status-subtitle {
            font-size: 16px;
            color: #6b7280;
            margin-bottom: 30px;
        }
        
        .reminder-info {
            background: #f0fdf4;
            border: 2px solid #10b981;
            border-radius: 12px;
            padding: 20px;
            margin-bottom: 20px;
        }
        
        .reminder-label {
            font-size: 14px;
            color: #065f46;
            font-weight: 600;
            margin-bottom: 8px;
        }
        
        .reminder-value {
            font-size: 24px;
            font-weight: bold;
            color: #047857;
        }
        
        .schedule-list {
            background: #f9fafb;
            border-radius: 12px;
            padding: 20px;
            margin-bottom: 20px;
        }
        
        .schedule-title {
            font-size: 16px;
            font-weight: 600;
            color: #374151;
            margin-bottom: 12px;
        }
        
        .schedule-item {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 10px 0;
            border-bottom: 1px solid #e5e7eb;
        }
        
        .schedule-item:last-child {
            border-bottom: none;
        }
        
        .schedule-time {
            font-size: 18px;
            font-weight: 600;
            color: #1f2937;
        }
        
        .schedule-dose {
            font-size: 14px;
            color: #6b7280;
        }
        
        .hidden {
            display: none;
        }
        
        .notification-badge {
            background: #fef3c7;
            border: 2px solid #fbbf24;
            border-radius: 10px;
            padding: 12px;
            margin-bottom: 20px;
            text-align: center;
            font-size: 14px;
            color: #92400e;
        }
        
        .notification-badge.enabled {
            background: #d1fae5;
            border-color: #10b981;
            color: #065f46;
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="header">
            <div class="emoji">💊</div>
            <h1>Medicine Reminder</h1>
        </div>

        <!-- Setup Screen -->
        <div id="setupScreen" class="card setup-screen">
            <div class="question">How many times per day?</div>
            
            <div class="frequency-options">
                <button class="frequency-btn" onclick="selectFrequency(1)">1x</button>
                <button class="frequency-btn" onclick="selectFrequency(2)">2x</button>
                <button class="frequency-btn" onclick="selectFrequency(3)">3x</button>
                <button class="frequency-btn" onclick="selectFrequency(4)">4x</button>
                <button class="frequency-btn" onclick="selectFrequency(5)">5x</button>
                <button class="frequency-btn" onclick="selectFrequency(6)">6x</button>
            </div>
            
            <div id="timeInputsContainer" class="time-inputs hidden"></div>
            
            <button id="startBtn" class="btn btn-primary hidden" onclick="startReminders()">
                Start Reminders
            </button>
        </div>

        <!-- Active Screen -->
        <div id="activeScreen" class="card active-screen hidden">
            <div class="status-emoji">✅</div>
            <div class="status-title">Reminders Active</div>
            <div class="status-subtitle">You'll get alerts at your scheduled times</div>
            
            <div id="notificationBadge" class="notification-badge">
                ⚠️ Enable notifications for sound alerts
            </div>
            
            <div class="reminder-info">
                <div class="reminder-label">Taking medicine</div>
                <div class="reminder-value"><span id="frequencyDisplay">0</span> times per day</div>
            </div>
            
            <div class="schedule-list">
                <div class="schedule-title">📅 Your Schedule</div>
                <div id="scheduleList"></div>
            </div>
            
            <button class="btn btn-secondary" onclick="changeSchedule()">
                Change Schedule
            </button>
        </div>
    </div>

    <!-- Audio for alarm sound -->
    <audio id="alarmSound" preload="auto">
        <!-- Using a data URI for a simple beep sound -->
        <source src="data:audio/wav;base64,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" type="audio/wav">
    </audio>

    <script>
        let selectedFrequency = 0;
        let reminderTimes = [];
        let activeReminders = [];

        // Select frequency
        function selectFrequency(freq) {
            selectedFrequency = freq;
            
            // Update button styles
            const buttons = document.querySelectorAll('.frequency-btn');
            buttons.forEach(btn => btn.classList.remove('selected'));
            event.target.classList.add('selected');
            
            // Show time inputs
            generateTimeInputs(freq);
            document.getElementById('startBtn').classList.remove('hidden');
        }

        // Generate time input fields
        function generateTimeInputs(freq) {
            const container = document.getElementById('timeInputsContainer');
            container.classList.remove('hidden');
            
            const defaultTimes = ['08:00', '13:00', '18:00', '23:00', '10:00', '16:00'];
            
            let html = '';
            for (let i = 0; i < freq; i++) {
                html += `
                    <div class="time-input-group">
                        <label class="time-label">Dose ${i + 1} - Set Time</label>
                        <input type="time" class="time-input" id="time${i}" value="${defaultTimes[i] || '08:00'}">
                    </div>
                `;
            }
            
            container.innerHTML = html;
        }

        // Start reminders
        function startReminders() {
            if (selectedFrequency === 0) {
                alert('Please select how many times per day');
                return;
            }
            
            // Get all times
            reminderTimes = [];
            for (let i = 0; i < selectedFrequency; i++) {
                const timeInput = document.getElementById(`time${i}`);
                if (timeInput && timeInput.value) {
                    reminderTimes.push(timeInput.value);
                }
            }
            
            if (reminderTimes.length === 0) {
                alert('Please set at least one time');
                return;
            }
            
            // Request notification permission
            requestNotificationPermission();
            
            // Save to storage
            saveToStorage();
            
            // Schedule all reminders
            scheduleAllReminders();
            
            // Show active screen
            showActiveScreen();
        }

        // Request notification permission
        async function requestNotificationPermission() {
            if ('Notification' in window) {
                const permission = await Notification.requestPermission();
                updateNotificationBadge(permission === 'granted');
            }
        }

        // Update notification badge
        function updateNotificationBadge(enabled) {
            const badge = document.getElementById('notificationBadge');
            if (enabled) {
                badge.className = 'notification-badge enabled';
                badge.innerHTML = '✅ Sound alerts enabled';
            } else {
                badge.className = 'notification-badge';
                badge.innerHTML = '⚠️ Enable notifications for sound alerts';
                badge.onclick = requestNotificationPermission;
            }
        }

        // Show active screen
        function showActiveScreen() {
            document.getElementById('setupScreen').classList.add('hidden');
            document.getElementById('activeScreen').classList.remove('hidden');
            
            document.getElementById('frequencyDisplay').textContent = selectedFrequency;
            
            // Display schedule
            const scheduleList = document.getElementById('scheduleList');
            let html = '';
            reminderTimes.forEach((time, index) => {
                html += `
                    <div class="schedule-item">
                        <span class="schedule-dose">Dose ${index + 1}</span>
                        <span class="schedule-time">${formatTime(time)}</span>
                    </div>
                `;
            });
            scheduleList.innerHTML = html;
        }

        // Format time for display
        function formatTime(time) {
            const [hours, minutes] = time.split(':');
            const hour = parseInt(hours);
            const ampm = hour >= 12 ? 'PM' : 'AM';
            const displayHour = hour % 12 || 12;
            return `${displayHour}:${minutes} ${ampm}`;
        }

        // Schedule all reminders
        function scheduleAllReminders() {
            // Clear existing reminders
            activeReminders.forEach(id => clearTimeout(id));
            activeReminders = [];
            
            reminderTimes.forEach(time => {
                scheduleReminder(time);
            });
        }

        // Schedule individual reminder
        function scheduleReminder(time) {
            const now = new Date();
            const [hours, minutes] = time.split(':');
            
            const scheduledTime = new Date();
            scheduledTime.setHours(parseInt(hours));
            scheduledTime.setMinutes(parseInt(minutes));
            scheduledTime.setSeconds(0);
            scheduledTime.setMilliseconds(0);
            
            // If time has passed today, schedule for tomorrow
            if (scheduledTime <= now) {
                scheduledTime.setDate(scheduledTime.getDate() + 1);
            }
            
            const delay = scheduledTime - now;
            
            const timerId = setTimeout(() => {
                triggerReminder();
                // Reschedule for next day
                scheduleReminder(time);
            }, delay);
            
            activeReminders.push(timerId);
        }

        // Trigger reminder (notification + sound)
        function triggerReminder() {
            // Play sound
            const audio = document.getElementById('alarmSound');
            audio.play().catch(e => console.log('Could not play sound:', e));
            
            // Show notification
            if (Notification.permission === 'granted') {
                const notification = new Notification('💊 Medicine Time!', {
                    body: 'Time to take your medicine',
                    requireInteraction: true,
                    tag: 'medicine-reminder',
                    vibrate: [200, 100, 200]
                });
                
                notification.onclick = () => {
                    window.focus();
                    notification.close();
                };
            }
            
            // Show alert if notification not available
            if (Notification.permission !== 'granted') {
                alert('💊 Medicine Time!\n\nTime to take your medicine');
            }
        }

        // Change schedule
        function changeSchedule() {
            // Clear reminders
            activeReminders.forEach(id => clearTimeout(id));
            activeReminders = [];
            
            // Keep current values to show them
            const currentFreq = selectedFrequency;
            const currentTimes = [...reminderTimes];
            
            // Show setup screen
            document.getElementById('activeScreen').classList.add('hidden');
            document.getElementById('setupScreen').classList.remove('hidden');
            
            // Pre-select the current frequency
            if (currentFreq > 0) {
                const buttons = document.querySelectorAll('.frequency-btn');
                buttons.forEach((btn, index) => {
                    if (index + 1 === currentFreq) {
                        btn.classList.add('selected');
                    } else {
                        btn.classList.remove('selected');
                    }
                });
                
                // Generate time inputs with current times
                generateTimeInputs(currentFreq);
                
                // Fill in current times
                currentTimes.forEach((time, index) => {
                    const input = document.getElementById(`time${index}`);
                    if (input) {
                        input.value = time;
                    }
                });
                
                document.getElementById('startBtn').classList.remove('hidden');
            }
        }

        // Save to storage
        function saveToStorage() {
            const data = {
                frequency: selectedFrequency,
                times: reminderTimes
            };
            localStorage.setItem('medicineSchedule', JSON.stringify(data));
        }

        // Load from storage
        function loadFromStorage() {
            const saved = localStorage.getItem('medicineSchedule');
            if (saved) {
                const data = JSON.parse(saved);
                selectedFrequency = data.frequency;
                reminderTimes = data.times;
                
                // Check notification permission
                if (Notification.permission === 'granted') {
                    updateNotificationBadge(true);
                }
                
                scheduleAllReminders();
                showActiveScreen();
            }
        }

        // Initialize on load
        window.addEventListener('load', () => {
            loadFromStorage();
            
            // Re-schedule when page becomes visible
            document.addEventListener('visibilitychange', () => {
                if (!document.hidden && reminderTimes.length > 0) {
                    scheduleAllReminders();
                }
            });
        });
    </script>
</body>
</html># Medicine-Reminder
