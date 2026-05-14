<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=yes">
    <title>الحقني - مساعدة الطريق للشاحنات والسيارات</title>
    <!-- Font Awesome -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css">
    <!-- Google Fonts -->
    <link href="https://fonts.googleapis.com/css2?family=Tajawal:wght@400;500;700;800&display=swap" rel="stylesheet">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Tajawal', sans-serif;
            background: linear-gradient(135deg, #0A1172 0%, #0D47A1 50%, #1565C0 100%);
            min-height: 100vh;
            direction: rtl;
        }

        /* شريط جانبي للمزيد */
        .side-more {
            position: fixed;
            left: 0;
            top: 0;
            bottom: 0;
            width: 65px;
            background: linear-gradient(90deg, rgba(255,255,255,0.15) 0%, rgba(255,255,255,0.05) 100%);
            backdrop-filter: blur(8px);
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            gap: 12px;
            z-index: 100;
            cursor: pointer;
            transition: all 0.3s ease;
        }

        .side-more:hover {
            width: 80px;
            background: linear-gradient(90deg, rgba(255,255,255,0.25) 0%, rgba(255,255,255,0.1) 100%);
        }

        .side-more i {
            color: rgba(255,255,255,0.8);
            font-size: 22px;
        }

        .side-more span {
            color: rgba(255,255,255,0.8);
            font-size: 13px;
            writing-mode: vertical-rl;
            text-orientation: mixed;
            letter-spacing: 2px;
        }

        /* الأنيميشن للشاشة الرئيسية */
        @keyframes fadeInUp {
            from {
                opacity: 0;
                transform: translateY(30px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }

        @keyframes bounce {
            0%, 100% { transform: translateY(0); }
            50% { transform: translateY(-20px); }
        }

        .animate-fade-up {
            animation: fadeInUp 0.6s ease forwards;
        }

        /* الشاشات */
        .screen {
            display: none;
            min-height: 100vh;
        }

        .screen.active {
            display: block;
        }

        /* البطاقات ثلاثية الأبعاد */
        .card-3d {
            transition: transform 0.3s ease, box-shadow 0.3s ease;
            cursor: pointer;
        }

        .card-3d:hover {
            transform: translateY(-8px);
            box-shadow: 0 20px 30px -10px rgba(0,0,0,0.3);
        }

        /* تنسيق عام للنصوص */
        .text-right {
            text-align: right;
        }

        /* الكرة القافزة */
        .bouncing-ball {
            animation: bounce 1.2s ease-in-out infinite;
            cursor: pointer;
        }

        /* خلفية الصفحات الداخلية */
        .page-gradient {
            background: linear-gradient(135deg, #0A1172, #0D47A1);
        }

        /* شريط التنقل السفلي */
        .bottom-nav {
            position: fixed;
            bottom: 0;
            left: 0;
            right: 0;
            background: white;
            display: flex;
            justify-content: space-around;
            padding: 12px 20px;
            box-shadow: 0 -5px 20px rgba(0,0,0,0.1);
            z-index: 90;
        }

        .bottom-nav.dark {
            background: #1E1E1E;
        }

        .nav-item {
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 5px;
            color: #777;
            cursor: pointer;
            transition: all 0.2s;
        }

        .nav-item.active {
            color: #1A237E;
        }

        .nav-item i {
            font-size: 26px;
        }

        .nav-item span {
            font-size: 12px;
        }

        /* الوضع الليلي */
        body.dark-mode {
            background: #121212;
        }

        .dark-card {
            background: #1E1E1E !important;
            color: #eee;
        }

        /* تنسيقات إضافية */
        .location-btn {
            background: rgba(26,35,126,0.1);
            border-radius: 12px;
            padding: 8px;
            cursor: pointer;
            transition: 0.2s;
        }

        .location-btn:hover {
            background: rgba(26,35,126,0.2);
        }

        .toast-message {
            position: fixed;
            bottom: 80px;
            left: 20px;
            right: 20px;
            background: #333;
            color: white;
            padding: 12px;
            border-radius: 12px;
            text-align: center;
            z-index: 200;
            animation: fadeInUp 0.3s ease;
            display: none;
        }

        @media (max-width: 600px) {
            .side-more { width: 50px; }
            .side-more:hover { width: 60px; }
            .nav-item i { font-size: 22px; }
        }
    </style>
</head>
<body>

<!-- شريط جانبي للمزيد -->
<div class="side-more" id="moreSideBtn">
    <i class="fas fa-arrow-left"></i>
    <span>المزيد</span>
    <i class="fas fa-ellipsis-h"></i>
</div>

<!-- شاشة البداية (Splash) -->
<div id="splashScreen" class="screen active">
    <div style="min-height: 100vh; display: flex; flex-direction: column; justify-content: center; align-items: center; padding: 20px;">
        <!-- أشكال متحركة -->
        <div style="display: flex; gap: 20px; margin-bottom: 40px;">
            <div class="shape-circle" style="width: 45px; height: 45px; background: rgba(255,255,255,0.15); border-radius: 50%; border: 1px solid rgba(255,255,255,0.3); animation: pulse 2s infinite;"></div>
            <div class="shape-square" style="width: 40px; height: 40px; background: rgba(255,255,255,0.1); border-radius: 10px; transform: rotate(0deg); animation: spin 3s infinite linear;"></div>
            <div><i class="fas fa-star" style="color: rgba(255,255,255,0.5); font-size: 32px;"></i></div>
        </div>

        <!-- شعار شاحنة -->
        <div style="background: white; border-radius: 100px; padding: 25px; box-shadow: 0 0 40px rgba(13,71,161,0.5); margin-bottom: 40px; animation: bounce 2s ease-in-out infinite;">
            <i class="fas fa-truck" style="font-size: 85px; color: #0D47A1;"></i>
        </div>

        <!-- اسم التطبيق -->
        <h1 id="appName" style="color: white; font-size: 55px; font-weight: bold; letter-spacing: 5px; margin-bottom: 20px;">الحقني</h1>
        <div style="width: 60%; height: 3px; background: white; margin-bottom: 30px;"></div>

        <p style="color: #BBDEFB; font-size: 22px; font-weight: bold;">مشكلتك خليها علينا</p>
        <p style="color: rgba(255,255,255,0.8); margin-top: 10px;">..... خدمة مساعدة الشاحنات والسيارات</p>

        <div style="display: flex; gap: 30px; margin: 30px 0;">
            <i class="fas fa-car" style="font-size: 32px; color: #BBDEFB;"></i>
            <i class="fas fa-truck" style="font-size: 32px; color: #BBDEFB;"></i>
            <i class="fas fa-motorcycle" style="font-size: 32px; color: #BBDEFB;"></i>
        </div>

        <button id="startBtn" style="background: white; color: #0D47A1; border: none; padding: 15px 50px; border-radius: 40px; font-size: 20px; font-weight: bold; cursor: pointer; margin-top: 30px;">ابدأ</button>
    </div>
</div>

<!-- شاشة تسجيل الدخول -->
<div id="loginScreen" class="screen">
    <div style="min-height: 100vh; display: flex; justify-content: center; align-items: center; padding: 20px;">
        <div style="background: white; border-radius: 30px; padding: 30px 25px; max-width: 450px; width: 100%; box-shadow: 0 20px 40px rgba(0,0,0,0.2);">
            <div style="text-align: center;">
                <i id="loginIcon" class="fas fa-lock" style="font-size: 50px; color: #1E3A8A;"></i>
                <h2 id="loginTitle" style="margin: 15px 0;">تسجيل الدخول</h2>
            </div>

            <div style="margin-bottom: 15px;">
                <input type="email" id="emailInput" placeholder="البريد الإلكتروني" style="width: 100%; padding: 14px; border-radius: 12px; border: 1px solid #ddd; font-family: inherit; text-align: right;">
            </div>
            <div style="margin-bottom: 15px;">
                <input type="password" id="passwordInput" placeholder="كلمة المرور" style="width: 100%; padding: 14px; border-radius: 12px; border: 1px solid #ddd; font-family: inherit; text-align: right;">
            </div>
            <div id="confirmPasswordDiv" style="margin-bottom: 15px; display: none;">
                <input type="password" id="confirmPasswordInput" placeholder="تأكيد كلمة المرور" style="width: 100%; padding: 14px; border-radius: 12px; border: 1px solid #ddd; font-family: inherit; text-align: right;">
            </div>

            <button id="authBtn" style="width: 100%; background: #1E3A8A; color: white; padding: 14px; border-radius: 12px; border: none; font-size: 18px; font-weight: bold; cursor: pointer; margin-bottom: 15px;">دخول</button>

            <div style="text-align: center;">
                <a href="#" id="forgotPasswordLink" style="color: #1E3A8A; text-decoration: none; font-size: 14px;">نسيت كلمة السر؟</a>
            </div>

            <div style="text-align: center; margin-top: 20px;">
                <span id="toggleModeText">ليس لديك حساب؟ أنشئ واحداً</span>
            </div>
        </div>
    </div>
</div>

<!-- الشاشة الرئيسية (Home) -->
<div id="homeScreen" class="screen">
    <div style="background: #1A237E; padding: 50px 16px 20px;">
        <div style="display: flex; justify-content: space-between; align-items: center;">
            <i class="fas fa-bars" id="menuIcon" style="color: white; font-size: 28px; cursor: pointer;"></i>
            <div><i class="fas fa-truck" style="color: #FFC107; font-size: 30px;"></i><span style="color: white; font-size: 24px; font-weight: bold; margin-right: 8px;">الحقني</span></div>
            <div style="width: 40px;"></div>
        </div>
    </div>
    <div style="background: white; border-radius: 30px 30px 0 0; min-height: 80vh; padding: 30px 20px;">
        <div id="mainContent">
            <!-- البطاقات الرئيسية -->
            <div class="card-3d" data-service="equipped" style="background: #00897B; border-radius: 24px; padding: 25px; margin-bottom: 20px; color: white; display: flex; justify-content: space-between; align-items: center;">
                <div><i class="fas fa-truck-moving" style="font-size: 50px;"></i></div>
                <div style="text-align: center;"><h2>شاحنة مجهزة</h2><p>شاحنة إنقاذ مجهزة بكامل المعدات</p></div>
            </div>
            <div class="card-3d" data-service="depannage" style="background: #E65100; border-radius: 24px; padding: 25px; margin-bottom: 20px; color: white; display: flex; justify-content: space-between; align-items: center;">
                <div><i class="fas fa-fire-extinguisher" style="font-size: 50px;"></i></div>
                <div style="text-align: center;"><h2>ديبناج</h2><p>شاحنة رفع - نرفع سيارتك أينما تعطلت</p></div>
            </div>
            <div class="card-3d" data-service="repair" style="background: #3949AB; border-radius: 24px; padding: 25px; margin-bottom: 20px; color: white; display: flex; justify-content: space-between; align-items: center;">
                <div><i class="fas fa-wrench" style="font-size: 50px;"></i></div>
                <div style="text-align: center;"><h2>ورشات تصليح</h2><p>أقرب ورشات التصليح حولك</p></div>
            </div>
        </div>
    </div>

    <!-- شريط سفلي -->
    <div class="bottom-nav" id="bottomNav">
        <div class="nav-item active" data-nav="home"><i class="fas fa-home"></i><span>الرئيسية</span></div>
        <div class="nav-item" data-nav="requests"><i class="fas fa-list-alt"></i><span>طلباتي</span></div>
        <div class="nav-item" data-nav="profile"><i class="fas fa-user"></i><span>حسابي</span></div>
    </div>
</div>

<!-- شاشة المزيد (MorePage) -->
<div id="moreScreen" class="screen">
    <div style="background: linear-gradient(135deg, #0A1172, #0D47A1); min-height: 100vh;">
        <div style="padding: 20px;">
            <div style="display: flex; align-items: center; gap: 15px; margin-bottom: 20px;">
                <i class="fas fa-arrow-right" id="closeMoreBtn" style="color: white; font-size: 28px; cursor: pointer;"></i>
                <h2 style="color: white;">المزيد</h2>
            </div>
            <div style="background: rgba(255,255,255,0.1); border-radius: 20px; padding: 20px; margin-bottom: 20px;">
                <h3 style="color: #BBDEFB;">عن التطبيق</h3>
                <p style="color: white;">تطبيق "الحقني" هو تطبيق خدمي يهدف إلى تقديم المساعدة السريعة لأصحاب الشاحنات والسيارات عند تعطلهم.</p>
            </div>
            <div style="background: rgba(255,255,255,0.1); border-radius: 20px; padding: 20px; margin-bottom: 20px;">
                <h3 style="color: #BBDEFB;">مميزات التطبيق</h3>
                <ul style="color: white; list-style: none;">
                    <li><i class="fas fa-check-circle"></i> تحديد الموقع بدقة</li>
                    <li><i class="fas fa-wrench"></i> خدمة الصيانة</li>
                    <li><i class="fas fa-truck"></i> خدمة ديبناج</li>
                    <li><i class="fas fa-phone"></i> دعم 24 ساعة</li>
                </ul>
            </div>
            <div style="background: rgba(76,175,80,0.2); border-radius: 20px; padding: 20px; border: 1px solid #4CAF50;">
                <h3 style="color: #8BC34A;">👨‍💻 المطورون</h3>
                <p style="color: white;">دماني نعيمة<br>بلعدل فاطيمة</p>
                <div style="display: flex; justify-content: center; margin-top: 10px;">
                    <a href="mailto:fullvixxx@gmail.com" style="color: white; background: rgba(255,255,255,0.2); padding: 8px 15px; border-radius: 30px; text-decoration: none;"><i class="fas fa-envelope"></i> fullvixxx@gmail.com</a>
                </div>
            </div>
            <div class="bouncing-ball" style="text-align: center; margin-top: 30px;">
                <div style="width: 60px; height: 60px; background: radial-gradient(circle, #FF9800, #E65100); border-radius: 50%; margin: 0 auto; display: flex; align-items: center; justify-content: center;"><i class="fas fa-question" style="color: white; font-size: 30px;"></i></div>
            </div>
        </div>
    </div>
</div>

<!-- نافذة منبثقة للخدمات (شاحنة مجهزة - اختيار الخدمة) -->
<div id="equippedDialog" style="display: none; position: fixed; top: 0; left: 0; right: 0; bottom: 0; background: rgba(0,0,0,0.7); z-index: 300; justify-content: center; align-items: center;">
    <div style="background: white; border-radius: 30px; width: 85%; max-width: 400px; padding: 20px;">
        <h3 style="text-align: center;">اختر الخدمة</h3>
        <div class="service-option" data-type="mechanic" style="padding: 15px; border-bottom: 1px solid #eee; cursor: pointer;"><i class="fas fa-wrench"></i> ميكانيكي</div>
        <div class="service-option" data-type="fuel" style="padding: 15px; border-bottom: 1px solid #eee; cursor: pointer;"><i class="fas fa-gas-pump"></i> وقود</div>
        <div class="service-option" data-type="oil" style="padding: 15px; border-bottom: 1px solid #eee; cursor: pointer;"><i class="fas fa-oil-can"></i> زيوت</div>
        <div class="service-option" data-type="spare" style="padding: 15px; cursor: pointer;"><i class="fas fa-cogs"></i> قطع غيار</div>
        <button id="closeDialog" style="margin-top: 15px; width: 100%; padding: 10px; background: #ccc; border: none; border-radius: 12px;">إلغاء</button>
    </div>
</div>

<!-- نافذة تقديم طلب عامة -->
<div id="requestFormDialog" style="display: none; position: fixed; top: 0; left: 0; right: 0; bottom: 0; background: rgba(0,0,0,0.8); z-index: 400; overflow-y: auto; justify-content: center; align-items: center;">
    <div style="background: white; border-radius: 30px; width: 90%; max-width: 450px; padding: 25px; margin: 20px auto;">
        <h3 id="formTitle" style="text-align: center;">طلب ميكانيكي</h3>
        <textarea id="formInfo" placeholder="تفاصيل المشكلة / الطلب" rows="3" style="width: 100%; padding: 10px; border-radius: 12px; border: 1px solid #ccc; margin: 10px 0; text-align: right;"></textarea>
        <input type="text" id="formContact" placeholder="رقم الهاتف" style="width: 100%; padding: 10px; border-radius: 12px; border: 1px solid #ccc; margin: 10px 0; text-align: right;">
        <div style="display: flex; gap: 10px; align-items: center;">
            <input type="text" id="formLocation" placeholder="الموقع (GPS)" style="flex: 1; padding: 10px; border-radius: 12px; border: 1px solid #ccc; text-align: right;">
            <div class="location-btn" id="getLocationBtn"><i class="fas fa-map-marker-alt" style="font-size: 24px; color: #1A237E;"></i></div>
        </div>
        <button id="submitRequestBtn" style="background: #1A237E; color: white; width: 100%; padding: 12px; border-radius: 30px; border: none; margin-top: 20px; font-weight: bold;">إرسال الطلب</button>
        <button id="closeFormDialog" style="background: #ccc; width: 100%; padding: 10px; border-radius: 30px; border: none; margin-top: 10px;">إلغاء</button>
    </div>
</div>

<div id="toastMsg" class="toast-message"></div>

<script>
    // حالة التطبيق
    let currentUser = localStorage.getItem('currentUser') ? JSON.parse(localStorage.getItem('currentUser')) : null;
    let requests = JSON.parse(localStorage.getItem('requests') || '[]');
    let isDarkMode = localStorage.getItem('darkMode') === 'true';
    let selectedServiceType = '';

    // دوال مساعدة
    function showToast(msg, isError = false) {
        const toast = document.getElementById('toastMsg');
        toast.textContent = msg;
        toast.style.background = isError ? '#d32f2f' : '#2e7d32';
        toast.style.display = 'block';
        setTimeout(() => { toast.style.display = 'none'; }, 3000);
    }

    function showScreen(screenId) {
        document.querySelectorAll('.screen').forEach(s => s.classList.remove('active'));
        document.getElementById(screenId).classList.add('active');
    }

    // محاكاة تحديد الموقع
    function getCurrentLocation(callback) {
        if (navigator.geolocation) {
            navigator.geolocation.getCurrentPosition(
                (pos) => { callback(`${pos.coords.latitude}, ${pos.coords.longitude}`); },
                () => { showToast('لم نتمكن من تحديد موقعك', true); callback(''); }
            );
        } else { showToast('المتصفح لا يدعم تحديد الموقع', true); callback(''); }
    }

    // إضافة طلب
    function addRequest(type, info, contact, location) {
        if (!currentUser) { showToast('الرجاء تسجيل الدخول أولاً', true); return false; }
        const newReq = {
            id: Date.now(),
            userId: currentUser.email,
            type: type,
            info: info,
            contact: contact,
            location: location,
            status: 'pending',
            createdAt: new Date().toISOString()
        };
        requests.push(newReq);
        localStorage.setItem('requests', JSON.stringify(requests));
        showToast('تم إرسال الطلب بنجاح');
        return true;
    }

    // عرض طلباتي
    function renderMyRequests() {
        const container = document.getElementById('requestsList');
        if (!container) return;
        const userReqs = requests.filter(r => r.userId === currentUser?.email);
        if (userReqs.length === 0) { container.innerHTML = '<p style="text-align:center;">لا توجد طلبات بعد</p>'; return; }
        container.innerHTML = userReqs.map(req => `
            <div style="background: #f5f5f5; border-radius: 15px; padding: 15px; margin-bottom: 10px;">
                <div style="display: flex; justify-content: space-between;"><strong>${req.type === 'mechanic' ? 'ميكانيكي' : req.type === 'fuel' ? 'وقود' : req.type === 'oil' ? 'زيوت' : req.type === 'spare' ? 'قطع غيار' : 'ديبناج/ورشة'}</strong> <span style="color: ${req.status === 'pending' ? 'orange' : 'green'}">${req.status === 'pending' ? 'قيد الانتظار' : 'مكتمل'}</span></div>
                <div>📍 ${req.location}</div>
                <div>📞 ${req.contact}</div>
                <div style="font-size:12px; color:gray;">${new Date(req.createdAt).toLocaleString('ar')}</div>
            </div>
        `).join('');
    }

    function renderProfile() {
        const container = document.getElementById('profileInfo');
        if (!container) return;
        if (currentUser) {
            container.innerHTML = `
                <div style="text-align:center;"><i class="fas fa-user-circle" style="font-size: 80px; color:#1A237E;"></i></div>
                <div style="background:white; border-radius:20px; padding:20px; margin-top:20px;">
                    <p><strong>البريد:</strong> ${currentUser.email}</p>
                    <p><strong>الاسم:</strong> ${currentUser.fullName || 'غير محدد'}</p>
                    <p><strong>الهاتف:</strong> ${currentUser.phone || 'غير محدد'}</p>
                </div>
                <button id="editProfileBtn" style="background:#1A237E; color:white; width:100%; padding:12px; border-radius:30px; margin-top:15px;">تعديل الملف الشخصي</button>
                <button id="logoutBtn" style="background:#d32f2f; color:white; width:100%; padding:12px; border-radius:30px; margin-top:10px;">تسجيل خروج</button>
            `;
            document.getElementById('editProfileBtn')?.addEventListener('click', () => {
                const newName = prompt('الاسم الكامل', currentUser.fullName || '');
                const newPhone = prompt('رقم الهاتف', currentUser.phone || '');
                if (newName !== null) currentUser.fullName = newName;
                if (newPhone !== null) currentUser.phone = newPhone;
                localStorage.setItem('currentUser', JSON.stringify(currentUser));
                renderProfile();
                showToast('تم تحديث البيانات');
            });
            document.getElementById('logoutBtn')?.addEventListener('click', () => {
                currentUser = null;
                localStorage.removeItem('currentUser');
                showScreen('loginScreen');
            });
        } else { container.innerHTML = '<p style="text-align:center;">الرجاء تسجيل الدخول</p>'; }
    }

    // بناء شاشة الطلبات والحساب
    function buildRequestsScreen() {
        const requestsHtml = `<div style="background:white; min-height:100vh; padding:20px;"><h2>طلباتي</h2><div id="requestsList"></div></div>`;
        document.getElementById('requestsScreenContent').innerHTML = requestsHtml;
        renderMyRequests();
    }

    function buildProfileScreen() {
        const profileHtml = `<div style="background:white; min-height:100vh; padding:20px;"><h2>حسابي</h2><div id="profileInfo"></div></div>`;
        document.getElementById('profileScreenContent').innerHTML = profileHtml;
        renderProfile();
    }

    // تهيئة التنقل الداخلي
    function initNavigation() {
        document.querySelectorAll('[data-nav]').forEach(el => {
            el.addEventListener('click', (e) => {
                const nav = e.currentTarget.getAttribute('data-nav');
                document.querySelectorAll('.nav-item').forEach(n => n.classList.remove('active'));
                el.classList.add('active');
                if (nav === 'home') { document.getElementById('mainContent').style.display = 'block'; document.getElementById('requestsScreenContent').style.display = 'none'; document.getElementById('profileScreenContent').style.display = 'none'; }
                else if (nav === 'requests') { document.getElementById('mainContent').style.display = 'none'; document.getElementById('requestsScreenContent').style.display = 'block'; document.getElementById('profileScreenContent').style.display = 'none'; buildRequestsScreen(); }
                else if (nav === 'profile') { document.getElementById('mainContent').style.display = 'none'; document.getElementById('requestsScreenContent').style.display = 'none'; document.getElementById('profileScreenContent').style.display = 'block'; buildProfileScreen(); }
            });
        });
    }

    // ربط أحداث الخدمات
    document.addEventListener('click', (e) => {
        if (e.target.closest('[data-service]')) {
            const service = e.target.closest('[data-service]').getAttribute('data-service');
            if (service === 'equipped') { document.getElementById('equippedDialog').style.display = 'flex'; }
            else if (service === 'depannage') { selectedServiceType = 'depannage'; document.getElementById('formTitle').innerText = 'طلب ديبناج'; document.getElementById('requestFormDialog').style.display = 'flex'; }
            else if (service === 'repair') { selectedServiceType = 'repair'; document.getElementById('formTitle').innerText = 'طلب ورشة تصليح'; document.getElementById('requestFormDialog').style.display = 'flex'; }
        }
        if (e.target.closest('.service-option')) {
            const type = e.target.closest('.service-option').getAttribute('data-type');
            selectedServiceType = type;
            let title = '';
            if (type === 'mechanic') title = 'طلب ميكانيكي';
            else if (type === 'fuel') title = 'طلب وقود';
            else if (type === 'oil') title = 'طلب زيوت';
            else title = 'طلب قطع غيار';
            document.getElementById('formTitle').innerText = title;
            document.getElementById('equippedDialog').style.display = 'none';
            document.getElementById('requestFormDialog').style.display = 'flex';
        }
    });

    document.getElementById('closeDialog')?.addEventListener('click', () => { document.getElementById('equippedDialog').style.display = 'none'; });
    document.getElementById('closeFormDialog')?.addEventListener('click', () => { document.getElementById('requestFormDialog').style.display = 'none'; });
    document.getElementById('getLocationBtn')?.addEventListener('click', () => {
        getCurrentLocation((loc) => { if (loc) document.getElementById('formLocation').value = loc; });
    });
    document.getElementById('submitRequestBtn')?.addEventListener('click', () => {
        const info = document.getElementById('formInfo').value;
        const contact = document.getElementById('formContact').value;
        const location = document.getElementById('formLocation').value;
        if (!info || !contact || !location) { showToast('يرجى ملء جميع الحقول', true); return; }
        if (addRequest(selectedServiceType, info, contact, location)) {
            document.getElementById('requestFormDialog').style.display = 'none';
            document.getElementById('formInfo').value = '';
            document.getElementById('formContact').value = '';
            document.getElementById('formLocation').value = '';
        }
    });

    // تسجيل الدخول
    document.getElementById('authBtn')?.addEventListener('click', () => {
        const email = document.getElementById('emailInput').value;
        const pass = document.getElementById('passwordInput').value;
        const isLogin = document.getElementById('loginTitle').innerText === 'تسجيل الدخول';
        if (isLogin) {
            if (email === 'test@example.com' && pass === '123456') {
                currentUser = { email: email, fullName: 'مستخدم تجريبي', phone: '0555555555' };
                localStorage.setItem('currentUser', JSON.stringify(currentUser));
                showToast('تم تسجيل الدخول بنجاح');
                showScreen('homeScreen');
            } else { showToast('بيانات غير صحيحة (test@example.com / 123456)', true); }
        } else {
            const confirm = document.getElementById('confirmPasswordInput').value;
            if (pass !== confirm) { showToast('كلمة المرور غير متطابقة', true); return; }
            currentUser = { email: email, fullName: '', phone: '' };
            localStorage.setItem('currentUser', JSON.stringify(currentUser));
            showToast('تم إنشاء الحساب بنجاح');
            showScreen('homeScreen');
        }
    });

    document.getElementById('toggleModeText')?.addEventListener('click', () => {
        const isLogin = document.getElementById('loginTitle').innerText === 'تسجيل الدخول';
        if (isLogin) {
            document.getElementById('loginTitle').innerText = 'إنشاء حساب جديد';
            document.getElementById('authBtn').innerText = 'تسجيل';
            document.getElementById('confirmPasswordDiv').style.display = 'block';
            document.getElementById('toggleModeText').innerText = 'لديك حساب بالفعل؟ سجل دخولك';
            document.getElementById('loginIcon').className = 'fas fa-user-plus';
        } else {
            document.getElementById('loginTitle').innerText = 'تسجيل الدخول';
            document.getElementById('authBtn').innerText = 'دخول';
            document.getElementById('confirmPasswordDiv').style.display = 'none';
            document.getElementById('toggleModeText').innerText = 'ليس لديك حساب؟ أنشئ واحداً';
            document.getElementById('loginIcon').className = 'fas fa-lock';
        }
    });

    document.getElementById('forgotPasswordLink')?.addEventListener('click', (e) => {
        e.preventDefault();
        showToast('تم إرسال رمز استعادة إلى بريدك (محاكاة)');
    });

    document.getElementById('startBtn')?.addEventListener('click', () => { showScreen('loginScreen'); });
    document.getElementById('menuIcon')?.addEventListener('click', () => { document.getElementById('moreScreen').classList.add('active'); document.getElementById('homeScreen').classList.remove('active'); });
    document.getElementById('closeMoreBtn')?.addEventListener('click', () => { showScreen('homeScreen'); });
    document.getElementById('moreSideBtn')?.addEventListener('click', () => { showScreen('moreScreen'); });

    // إضافة محتوى الشاشات الفرعية
    const homeScreenDiv = document.getElementById('homeScreen');
    const requestsDiv = document.createElement('div'); requestsDiv.id = 'requestsScreenContent'; requestsDiv.style.display = 'none'; requestsDiv.style.background = 'white'; requestsDiv.style.minHeight = '80vh'; requestsDiv.style.padding = '20px';
    const profileDiv = document.createElement('div'); profileDiv.id = 'profileScreenContent'; profileDiv.style.display = 'none'; profileDiv.style.background = 'white'; profileDiv.style.minHeight = '80vh'; profileDiv.style.padding = '20px';
    homeScreenDiv.appendChild(requestsDiv);
    homeScreenDiv.appendChild(profileDiv);
    initNavigation();

    // تأثير كتابة اسم التطبيق
    const appNameEl = document.getElementById('appName');
    if (appNameEl) {
        const fullText = 'الحقني';
        let i = 0;
        appNameEl.innerText = '';
        function typeWriter() { if (i < fullText.length) { appNameEl.innerText += fullText.charAt(i); i++; setTimeout(typeWriter, 150); } }
        typeWriter();
    }

    // تحقق إذا كان المستخدم مسجل
    if (currentUser) showScreen('homeScreen');
    else showScreen('splashScreen');

    // أنيميشن CSS إضافية
    const style = document.createElement('style');
    style.textContent = `@keyframes pulse { 0% { opacity: 0.6; transform: scale(1); } 50% { opacity: 1; transform: scale(1.05); } 100% { opacity: 0.6; transform: scale(1); } } @keyframes spin { from { transform: rotate(0deg); } to { transform: rotate(360deg); } }`;
    document.head.appendChild(style);
</script>

</body>
</html>
