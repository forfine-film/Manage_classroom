<html lang="th">
 <head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>ระบบจัดสอนแทน</title>
  <script src="/_sdk/data_sdk.js"></script>
  <script src="/_sdk/element_sdk.js"></script>
  <script src="https://cdn.tailwindcss.com"></script>
  <link href="https://fonts.googleapis.com/css2?family=Sarabun:wght@300;400;500;600;700&amp;display=swap" rel="stylesheet">
  <style>
    body {
      box-sizing: border-box;
      font-family: 'Sarabun', sans-serif;
    }
    * {
      box-sizing: border-box;
    }
    .tab-button {
      transition: all 0.3s ease;
    }
    .tab-button:hover {
      transform: translateY(-2px);
    }
    .card {
      transition: all 0.3s ease;
    }
    .card:hover {
      transform: translateY(-2px);
      box-shadow: 0 8px 16px rgba(0,0,0,0.1);
    }
    .btn-primary {
      transition: all 0.2s ease;
    }
    .btn-primary:hover {
      transform: scale(1.05);
    }
    .btn-danger {
      transition: all 0.2s ease;
    }
    .btn-danger:hover {
      transform: scale(1.05);
    }
    .loading {
      opacity: 0.6;
      pointer-events: none;
    }
    .stat-card {
      transition: all 0.3s ease;
    }
    .stat-card:hover {
      transform: scale(1.02);
    }
  </style>
  <style>@view-transition { navigation: auto; }</style>
 </head>
 <body class="w-full min-h-full" style="background: linear-gradient(135deg, #667eea 0%, #764ba2 100%); margin: 0; padding: 0;">
  <div class="w-full min-h-full" style="padding: 2rem 1rem;">
   <div style="max-width: 1200px; margin: 0 auto;"><!-- Header -->
    <header class="text-center mb-8">
     <h1 id="site-title" class="text-white font-bold mb-2" style="font-size: 2.5rem;">ระบบจัดสอนแทน</h1>
     <p id="site-description" class="text-white" style="font-size: 1.1rem; opacity: 0.95;">จัดการตารางสอนและการสอนแทนอย่างมีประสิทธิภาพ</p>
    </header><!-- Tabs -->
    <div class="flex gap-3 mb-6 flex-wrap justify-center"><button id="tab-dashboard-btn" class="tab-button px-6 py-3 rounded-lg font-semibold text-white" style="background-color: #4c51bf; font-size: 1.1rem;" onclick="switchTab('dashboard')"> <span id="tab-dashboard-text">📊 Dashboard</span> </button> <button id="tab-substitute-btn" class="tab-button px-6 py-3 rounded-lg font-semibold" style="background-color: rgba(255,255,255,0.3); color: white; font-size: 1.1rem;" onclick="switchTab('substitute')"> <span id="tab-substitute-text">จัดสอนแทน</span> </button> <button id="tab-teachers-btn" class="tab-button px-6 py-3 rounded-lg font-semibold" style="background-color: rgba(255,255,255,0.3); color: white; font-size: 1.1rem;" onclick="switchTab('teachers')"> <span id="tab-teachers-text">รายชื่อครู</span> </button> <button id="tab-schedule-btn" class="tab-button px-6 py-3 rounded-lg font-semibold" style="background-color: rgba(255,255,255,0.3); color: white; font-size: 1.1rem;" onclick="switchTab('schedule')"> <span id="tab-schedule-text">ตารางสอนปกติ</span> </button>
    </div><!-- Dashboard Tab -->
    <div id="dashboard-tab" class="tab-content">
     <div class="grid grid-cols-1 md:grid-cols-3 gap-4 mb-6">
      <div class="stat-card card rounded-xl p-6 text-center" style="background-color: white; box-shadow: 0 4px 6px rgba(0,0,0,0.1);">
       <div style="font-size: 3rem; margin-bottom: 0.5rem;">
        👨‍🏫
       </div>
       <div id="stat-teachers" style="font-size: 2.5rem; font-weight: bold; color: #4c51bf;">
        0
       </div>
       <div style="color: #718096; font-size: 1rem;">
        จำนวนครูทั้งหมด
       </div>
      </div>
      <div class="stat-card card rounded-xl p-6 text-center" style="background-color: white; box-shadow: 0 4px 6px rgba(0,0,0,0.1);">
       <div style="font-size: 3rem; margin-bottom: 0.5rem;">
        📚
       </div>
       <div id="stat-classes" style="font-size: 2.5rem; font-weight: bold; color: #48bb78;">
        0
       </div>
       <div style="color: #718096; font-size: 1rem;">
        คาบสอนทั้งหมด
       </div>
      </div>
      <div class="stat-card card rounded-xl p-6 text-center" style="background-color: white; box-shadow: 0 4px 6px rgba(0,0,0,0.1);">
       <div style="font-size: 3rem; margin-bottom: 0.5rem;">
        🔄
       </div>
       <div id="stat-substitutions" style="font-size: 2.5rem; font-weight: bold; color: #f56565;">
        0
       </div>
       <div style="color: #718096; font-size: 1rem;">
        การสอนแทนทั้งหมด
       </div>
      </div>
     </div>
     <div class="grid grid-cols-1 lg:grid-cols-2 gap-6"><!-- Leave Summary -->
      <div class="card rounded-xl p-6" style="background-color: white; box-shadow: 0 4px 6px rgba(0,0,0,0.1);">
       <h2 class="font-bold mb-4" style="color: #4c51bf; font-size: 1.5rem;">📋 สรุปการลาของแต่ละคน</h2>
       <div id="leave-summary" class="space-y-3"></div>
      </div><!-- Substitute Summary -->
      <div class="card rounded-xl p-6" style="background-color: white; box-shadow: 0 4px 6px rgba(0,0,0,0.1);">
       <h2 class="font-bold mb-4" style="color: #48bb78; font-size: 1.5rem;">🎯 สรุปการสอนแทนของแต่ละคน</h2>
       <div id="substitute-summary" class="space-y-3"></div>
      </div>
     </div>
    </div><!-- Substitute Tab -->
    <div id="substitute-tab" class="tab-content" style="display: none;">
     <div class="card rounded-xl p-6 mb-6" style="background-color: white; box-shadow: 0 4px 6px rgba(0,0,0,0.1);">
      <h2 class="font-bold mb-4" style="color: #4c51bf; font-size: 1.5rem;">🔄 จัดครูสอนแทนอัตโนมัติ</h2>
      <form id="substitute-form" class="space-y-4">
       <div><label for="sub-date" class="block font-semibold mb-2" style="color: #2d3748; font-size: 1rem;">วันที่</label> <input type="date" id="sub-date" required class="w-full px-4 py-2 border-2 rounded-lg" style="border-color: #e2e8f0; font-size: 1rem;">
       </div>
       <div><label for="sub-weekday" class="block font-semibold mb-2" style="color: #2d3748; font-size: 1rem;">วัน</label> <select id="sub-weekday" required class="w-full px-4 py-2 border-2 rounded-lg" style="border-color: #e2e8f0; font-size: 1rem;"> <option value="">-- เลือกวัน --</option> <option value="จันทร์">จันทร์</option> <option value="อังคาร">อังคาร</option> <option value="พุธ">พุธ</option> <option value="พฤหัสบดี">พฤหัสบดี</option> <option value="ศุกร์">ศุกร์</option> </select>
       </div>
       <div><label for="absent-teacher" class="block font-semibold mb-2" style="color: #2d3748; font-size: 1rem;">ครูที่ลา</label> <select id="absent-teacher" required class="w-full px-4 py-2 border-2 rounded-lg" style="border-color: #e2e8f0; font-size: 1rem;"> <option value="">-- เลือกครูที่ลา --</option> </select>
       </div>
       <div><label for="leave-reason" class="block font-semibold mb-2" style="color: #2d3748; font-size: 1rem;">สาเหตุการลา</label> <select id="leave-reason" required class="w-full px-4 py-2 border-2 rounded-lg" style="border-color: #e2e8f0; font-size: 1rem;"> <option value="">-- เลือกสาเหตุ --</option> <option value="ลาป่วย">ลาป่วย</option> <option value="ลากิจ">ลากิจ</option> <option value="ไปราชการ">ไปราชการ</option> <option value="ไปอบรม">ไปอบรม</option> <option value="ลาพักผ่อน">ลาพักผ่อน</option> <option value="อื่นๆ">อื่นๆ</option> </select>
       </div>
       <div id="affected-classes-container" style="display: none;">
        <div class="p-4 rounded-lg" style="background-color: #fef5e7; border: 2px solid #f9e79f;">
         <p class="font-semibold mb-2" style="color: #2d3748; font-size: 1rem;">📋 คาบที่ต้องจัดสอนแทน:</p>
         <div id="affected-classes-list" class="space-y-2"></div>
        </div>
       </div><button type="submit" class="btn-primary px-6 py-3 rounded-lg font-semibold text-white w-full" style="background-color: #48bb78; font-size: 1.1rem;"> 🤖 จัดสอนแทนอัตโนมัติ </button>
      </form>
     </div>
     <div class="card rounded-xl p-6" style="background-color: white; box-shadow: 0 4px 6px rgba(0,0,0,0.1);">
      <h2 class="font-bold mb-4" style="color: #4c51bf; font-size: 1.5rem;">รายการสอนแทน</h2>
      <div id="substitutes-list" class="space-y-3"></div>
     </div>
    </div><!-- Teachers Tab -->
    <div id="teachers-tab" class="tab-content" style="display: none;">
     <div class="card rounded-xl p-6 mb-6" style="background-color: white; box-shadow: 0 4px 6px rgba(0,0,0,0.1);">
      <h2 class="font-bold mb-4" style="color: #4c51bf; font-size: 1.5rem;">เพิ่มครู/อาจารย์</h2>
      <form id="teacher-form" class="space-y-4">
       <div><label for="teacher-name" class="block font-semibold mb-2" style="color: #2d3748; font-size: 1rem;">ชื่อ-นามสกุล</label> <input type="text" id="teacher-name" required class="w-full px-4 py-2 border-2 rounded-lg" style="border-color: #e2e8f0; font-size: 1rem;" placeholder="เช่น นายสมชาย ใจดี">
       </div>
       <div><label for="teacher-department" class="block font-semibold mb-2" style="color: #2d3748; font-size: 1rem;">กลุ่มสาระ/ภาควิชา</label> <input type="text" id="teacher-department" required class="w-full px-4 py-2 border-2 rounded-lg" style="border-color: #e2e8f0; font-size: 1rem;" placeholder="เช่น คณิตศาสตร์">
       </div><button type="submit" class="btn-primary px-6 py-3 rounded-lg font-semibold text-white w-full" style="background-color: #48bb78; font-size: 1.1rem;"> ➕ เพิ่มครู </button>
      </form>
     </div>
     <div class="card rounded-xl p-6" style="background-color: white; box-shadow: 0 4px 6px rgba(0,0,0,0.1);">
      <h2 class="font-bold mb-4" style="color: #4c51bf; font-size: 1.5rem;">รายชื่อครูทั้งหมด</h2>
      <div id="teachers-list" class="space-y-3"></div>
     </div>
    </div><!-- Schedule Tab -->
    <div id="schedule-tab" class="tab-content" style="display: none;">
     <div class="card rounded-xl p-6 mb-6" style="background-color: white; box-shadow: 0 4px 6px rgba(0,0,0,0.1);">
      <h2 class="font-bold mb-4" style="color: #4c51bf; font-size: 1.5rem;">เพิ่มคาบสอน</h2>
      <form id="class-form" class="space-y-4">
       <div><label for="class-teacher" class="block font-semibold mb-2" style="color: #2d3748; font-size: 1rem;">เลือกครูผู้สอน</label> <select id="class-teacher" required class="w-full px-4 py-2 border-2 rounded-lg" style="border-color: #e2e8f0; font-size: 1rem;"> <option value="">-- เลือกครู --</option> </select>
       </div>
       <div><label for="class-subject" class="block font-semibold mb-2" style="color: #2d3748; font-size: 1rem;">วิชา</label> <input type="text" id="class-subject" required class="w-full px-4 py-2 border-2 rounded-lg" style="border-color: #e2e8f0; font-size: 1rem;" placeholder="เช่น คณิตศาสตร์">
       </div>
       <div><label for="class-level" class="block font-semibold mb-2" style="color: #2d3748; font-size: 1rem;">ชั้นเรียน</label> <input type="text" id="class-level" required class="w-full px-4 py-2 border-2 rounded-lg" style="border-color: #e2e8f0; font-size: 1rem;" placeholder="เช่น ม.1/1, ม.3/5">
       </div>
       <div><label for="class-room" class="block font-semibold mb-2" style="color: #2d3748; font-size: 1rem;">ห้องเรียน</label> <select id="class-room" required class="w-full px-4 py-2 border-2 rounded-lg" style="border-color: #e2e8f0; font-size: 1rem;"> <option value="">-- เลือกห้อง --</option> <option value="511">511</option> <option value="521">521</option> <option value="531">531</option> <option value="533">533</option> <option value="534">534</option> <option value="536">536</option> <option value="537">537</option> <option value="541">541</option> <option value="543">543</option> <option value="544">544</option> <option value="545">545</option> <option value="546">546</option> <option value="547">547</option> </select>
       </div>
       <div><label for="class-weekday" class="block font-semibold mb-2" style="color: #2d3748; font-size: 1rem;">วัน</label> <select id="class-weekday" required class="w-full px-4 py-2 border-2 rounded-lg" style="border-color: #e2e8f0; font-size: 1rem;"> <option value="">-- เลือกวัน --</option> <option value="จันทร์">จันทร์</option> <option value="อังคาร">อังคาร</option> <option value="พุธ">พุธ</option> <option value="พฤหัสบดี">พฤหัสบดี</option> <option value="ศุกร์">ศุกร์</option> </select>
       </div>
       <div><label for="class-time" class="block font-semibold mb-2" style="color: #2d3748; font-size: 1rem;">เวลา (คาบเรียน)</label> <select id="class-time" required class="w-full px-4 py-2 border-2 rounded-lg" style="border-color: #e2e8f0; font-size: 1rem;"> <option value="">-- เลือกคาบเรียน --</option> <option value="คาบ 1 (08:30-09:25)">คาบ 1 (08:30-09:25)</option> <option value="คาบ 2 (09:25-10:20)">คาบ 2 (09:25-10:20)</option> <option value="คาบ 3 (10:20-11:15)">คาบ 3 (10:20-11:15)</option> <option value="คาบ 4 (11:15-12:10)">คาบ 4 (11:15-12:10)</option> <option value="คาบ 5 (12:10-13:00)">คาบ 5 (12:10-13:00)</option> <option value="คาบ 6 (13:00-13:55)">คาบ 6 (13:00-13:55)</option> <option value="คาบ 7 (13:55-14:50)">คาบ 7 (13:55-14:50)</option> <option value="คาบ 8 (14:50-15:45)">คาบ 8 (14:50-15:45)</option> <option value="คาบ 9 (15:45-16:40)">คาบ 9 (15:45-16:40)</option> </select>
       </div><button type="submit" class="btn-primary px-6 py-3 rounded-lg font-semibold text-white w-full" style="background-color: #48bb78; font-size: 1.1rem;"> ➕ เพิ่มคาบสอน </button>
      </form>
     </div>
     <div class="card rounded-xl p-6" style="background-color: white; box-shadow: 0 4px 6px rgba(0,0,0,0.1);">
      <h2 class="font-bold mb-4" style="color: #4c51bf; font-size: 1.5rem;">ตารางสอนทั้งหมด</h2>
      <div id="classes-list" class="space-y-3"></div>
     </div>
    </div>
   </div>
  </div>
  <script>
    // Default config
    const defaultConfig = {
      site_title: "ระบบจัดสอนแทน",
      site_description: "จัดการตารางสอนและการสอนแทนอย่างมีประสิทธิภาพ",
      tab_dashboard: "📊 Dashboard",
      tab_teachers: "รายชื่อครู",
      tab_schedule: "ตารางสอนปกติ",
      tab_substitute: "จัดสอนแทน",
      background_color: "#667eea",
      secondary_color: "#764ba2",
      surface_color: "#ffffff",
      text_color: "#2d3748",
      primary_action_color: "#4c51bf",
      secondary_action_color: "#48bb78",
      font_family: "Sarabun",
      font_size: 16
    };

    // State
    let allData = [];
    let currentTab = 'dashboard';

    // Data handler
    const dataHandler = {
      onDataChanged(data) {
        allData = data;
        renderDashboard();
        renderTeachersList();
        renderClassesList();
        renderSubstitutesList();
        updateTeacherDropdowns();
      }
    };

    // Initialize SDKs
    async function initApp() {
      const dataResult = await window.dataSdk.init(dataHandler);
      if (!dataResult.isOk) {
        console.error("Failed to initialize data SDK");
      }

      if (window.elementSdk) {
        window.elementSdk.init({
          defaultConfig,
          onConfigChange: async (config) => {
            const baseSize = config.font_size || defaultConfig.font_size;
            const customFont = config.font_family || defaultConfig.font_family;
            const baseFontStack = 'Sarabun, sans-serif';
            
            document.getElementById('site-title').textContent = config.site_title || defaultConfig.site_title;
            document.getElementById('site-title').style.fontSize = `${baseSize * 2.5}px`;
            document.getElementById('site-title').style.fontFamily = `${customFont}, ${baseFontStack}`;
            
            document.getElementById('site-description').textContent = config.site_description || defaultConfig.site_description;
            document.getElementById('site-description').style.fontSize = `${baseSize * 1.1}px`;
            document.getElementById('site-description').style.fontFamily = `${customFont}, ${baseFontStack}`;
            
            document.getElementById('tab-dashboard-text').textContent = config.tab_dashboard || defaultConfig.tab_dashboard;
            document.getElementById('tab-teachers-text').textContent = config.tab_teachers || defaultConfig.tab_teachers;
            document.getElementById('tab-schedule-text').textContent = config.tab_schedule || defaultConfig.tab_schedule;
            document.getElementById('tab-substitute-text').textContent = config.tab_substitute || defaultConfig.tab_substitute;
            
            document.querySelectorAll('.tab-button').forEach(btn => {
              btn.style.fontSize = `${baseSize * 1.1}px`;
              btn.style.fontFamily = `${customFont}, ${baseFontStack}`;
            });
            
            document.body.style.background = `linear-gradient(135deg, ${config.background_color || defaultConfig.background_color} 0%, ${config.secondary_color || defaultConfig.secondary_color} 100%)`;
            
            document.querySelectorAll('.card').forEach(card => {
              card.style.backgroundColor = config.surface_color || defaultConfig.surface_color;
            });
            
            document.querySelectorAll('h2').forEach(h2 => {
              h2.style.fontSize = `${baseSize * 1.5}px`;
              h2.style.fontFamily = `${customFont}, ${baseFontStack}`;
            });
            
            document.querySelectorAll('label, input, select, button, p, div').forEach(el => {
              el.style.fontFamily = `${customFont}, ${baseFontStack}`;
            });
            
            document.querySelectorAll('.btn-primary').forEach(btn => {
              btn.style.backgroundColor = config.secondary_action_color || defaultConfig.secondary_action_color;
            });
            
            const activeTabBtn = document.getElementById(`tab-${currentTab}-btn`);
            if (activeTabBtn) {
              activeTabBtn.style.backgroundColor = config.primary_action_color || defaultConfig.primary_action_color;
            }
          },
          mapToCapabilities: (config) => ({
            recolorables: [
              {
                get: () => config.background_color || defaultConfig.background_color,
                set: (value) => {
                  if (window.elementSdk && window.elementSdk.config) {
                    window.elementSdk.config.background_color = value;
                    window.elementSdk.setConfig({ background_color: value });
                  }
                }
              },
              {
                get: () => config.secondary_color || defaultConfig.secondary_color,
                set: (value) => {
                  if (window.elementSdk && window.elementSdk.config) {
                    window.elementSdk.config.secondary_color = value;
                    window.elementSdk.setConfig({ secondary_color: value });
                  }
                }
              },
              {
                get: () => config.surface_color || defaultConfig.surface_color,
                set: (value) => {
                  if (window.elementSdk && window.elementSdk.config) {
                    window.elementSdk.config.surface_color = value;
                    window.elementSdk.setConfig({ surface_color: value });
                  }
                }
              },
              {
                get: () => config.primary_action_color || defaultConfig.primary_action_color,
                set: (value) => {
                  if (window.elementSdk && window.elementSdk.config) {
                    window.elementSdk.config.primary_action_color = value;
                    window.elementSdk.setConfig({ primary_action_color: value });
                  }
                }
              },
              {
                get: () => config.secondary_action_color || defaultConfig.secondary_action_color,
                set: (value) => {
                  if (window.elementSdk && window.elementSdk.config) {
                    window.elementSdk.config.secondary_action_color = value;
                    window.elementSdk.setConfig({ secondary_action_color: value });
                  }
                }
              }
            ],
            borderables: [],
            fontEditable: {
              get: () => config.font_family || defaultConfig.font_family,
              set: (value) => {
                if (window.elementSdk && window.elementSdk.config) {
                  window.elementSdk.config.font_family = value;
                  window.elementSdk.setConfig({ font_family: value });
                }
              }
            },
            fontSizeable: {
              get: () => config.font_size || defaultConfig.font_size,
              set: (value) => {
                if (window.elementSdk && window.elementSdk.config) {
                  window.elementSdk.config.font_size = value;
                  window.elementSdk.setConfig({ font_size: value });
                }
              }
            }
          }),
          mapToEditPanelValues: (config) => new Map([
            ["site_title", config.site_title || defaultConfig.site_title],
            ["site_description", config.site_description || defaultConfig.site_description],
            ["tab_dashboard", config.tab_dashboard || defaultConfig.tab_dashboard],
            ["tab_teachers", config.tab_teachers || defaultConfig.tab_teachers],
            ["tab_schedule", config.tab_schedule || defaultConfig.tab_schedule],
            ["tab_substitute", config.tab_substitute || defaultConfig.tab_substitute]
          ])
        });
      }

      // Set today's date
      const today = new Date().toISOString().split('T')[0];
      document.getElementById('sub-date').value = today;
    }

    // Tab switching
    function switchTab(tab) {
      currentTab = tab;
      document.querySelectorAll('.tab-content').forEach(content => {
        content.style.display = 'none';
      });
      document.getElementById(`${tab}-tab`).style.display = 'block';
      
      document.querySelectorAll('.tab-button').forEach(btn => {
        btn.style.backgroundColor = 'rgba(255,255,255,0.3)';
      });
      
      const activeBtn = document.getElementById(`tab-${tab}-btn`);
      const config = window.elementSdk?.config || defaultConfig;
      activeBtn.style.backgroundColor = config.primary_action_color || defaultConfig.primary_action_color;
    }

    // Dashboard rendering
    function renderDashboard() {
      const teachers = allData.filter(d => d.type === 'teacher');
      const classes = allData.filter(d => d.type === 'class');
      const substitutions = allData.filter(d => d.type === 'substitution');
      
      // Update stats
      document.getElementById('stat-teachers').textContent = teachers.length;
      document.getElementById('stat-classes').textContent = classes.length;
      document.getElementById('stat-substitutions').textContent = substitutions.length;
      
      // Leave summary
      const leaveSummary = {};
      substitutions.forEach(sub => {
        const teacherId = sub.original_teacher_id;
        if (!leaveSummary[teacherId]) {
          leaveSummary[teacherId] = {
            count: 0,
            reasons: {}
          };
        }
        leaveSummary[teacherId].count++;
        const reason = sub.leave_reason || 'ไม่ระบุ';
        leaveSummary[teacherId].reasons[reason] = (leaveSummary[teacherId].reasons[reason] || 0) + 1;
      });
      
      const leaveSummaryContainer = document.getElementById('leave-summary');
      if (Object.keys(leaveSummary).length === 0) {
        leaveSummaryContainer.innerHTML = '<p style="color: #718096; text-align: center; padding: 2rem;">ยังไม่มีข้อมูลการลา</p>';
      } else {
        leaveSummaryContainer.innerHTML = Object.entries(leaveSummary)
          .sort((a, b) => b[1].count - a[1].count)
          .map(([teacherId, data]) => {
            const teacher = teachers.find(t => t.__backendId === teacherId);
            const reasonsText = Object.entries(data.reasons)
              .map(([reason, count]) => `${reason} (${count})`)
              .join(', ');
            return `
              <div class="p-4 rounded-lg" style="background-color: #fef5e7; border-left: 4px solid #f39c12;">
                <div class="flex justify-between items-center mb-2">
                  <p class="font-semibold" style="color: #2d3748; font-size: 1.1rem;">${teacher ? teacher.teacher_name : 'ไม่ระบุ'}</p>
                  <span class="px-3 py-1 rounded-full font-bold" style="background-color: #f39c12; color: white; font-size: 0.9rem;">${data.count} ครั้ง</span>
                </div>
                <p style="color: #718096; font-size: 0.9rem;">สาเหตุ: ${reasonsText}</p>
              </div>
            `;
          }).join('');
      }
      
      // Substitute summary
      const substituteSummary = {};
      substitutions.forEach(sub => {
        const teacherId = sub.substitute_teacher_id;
        if (!substituteSummary[teacherId]) {
          substituteSummary[teacherId] = 0;
        }
        substituteSummary[teacherId]++;
      });
      
      const substituteSummaryContainer = document.getElementById('substitute-summary');
      if (Object.keys(substituteSummary).length === 0) {
        substituteSummaryContainer.innerHTML = '<p style="color: #718096; text-align: center; padding: 2rem;">ยังไม่มีข้อมูลการสอนแทน</p>';
      } else {
        substituteSummaryContainer.innerHTML = Object.entries(substituteSummary)
          .sort((a, b) => b[1] - a[1])
          .map(([teacherId, count]) => {
            const teacher = teachers.find(t => t.__backendId === teacherId);
            return `
              <div class="p-4 rounded-lg" style="background-color: #e8f5e9; border-left: 4px solid #48bb78;">
                <div class="flex justify-between items-center">
                  <p class="font-semibold" style="color: #2d3748; font-size: 1.1rem;">${teacher ? teacher.teacher_name : 'ไม่ระบุ'}</p>
                  <span class="px-3 py-1 rounded-full font-bold" style="background-color: #48bb78; color: white; font-size: 0.9rem;">${count} ครั้ง</span>
                </div>
              </div>
            `;
          }).join('');
      }
    }

    // Teacher form
    document.getElementById('teacher-form').addEventListener('submit', async (e) => {
      e.preventDefault();
      
      const name = document.getElementById('teacher-name').value.trim();
      const department = document.getElementById('teacher-department').value.trim();
      
      if (!name || !department) return;
      
      const btn = e.target.querySelector('button[type="submit"]');
      btn.classList.add('loading');
      btn.textContent = '⏳ กำลังบันทึก...';
      
      const result = await window.dataSdk.create({
        type: 'teacher',
        teacher_name: name,
        department: department,
        created_at: new Date().toISOString()
      });
      
      btn.classList.remove('loading');
      btn.textContent = '➕ เพิ่มครู';
      
      if (result.isOk) {
        document.getElementById('teacher-form').reset();
        showMessage('เพิ่มครูสำเร็จ', 'success');
      } else {
        showMessage('เกิดข้อผิดพลาดในการบันทึก กรุณาลองใหม่อีกครั้ง', 'error');
      }
    });

    // Class form
    document.getElementById('class-form').addEventListener('submit', async (e) => {
      e.preventDefault();
      
      const teacherId = document.getElementById('class-teacher').value;
      const subject = document.getElementById('class-subject').value.trim();
      const classLevel = document.getElementById('class-level').value.trim();
      const room = document.getElementById('class-room').value;
      const weekday = document.getElementById('class-weekday').value;
      const timeRange = document.getElementById('class-time').value;
      
      if (!teacherId || !subject || !classLevel || !room || !weekday || !timeRange) return;
      
      const btn = e.target.querySelector('button[type="submit"]');
      btn.classList.add('loading');
      btn.textContent = '⏳ กำลังบันทึก...';
      
      const result = await window.dataSdk.create({
        type: 'class',
        teacher_id: teacherId,
        subject: subject,
        class_level: classLevel,
        room: room,
        weekday: weekday,
        time_range: timeRange,
        created_at: new Date().toISOString()
      });
      
      btn.classList.remove('loading');
      btn.textContent = '➕ เพิ่มคาบสอน';
      
      if (result.isOk) {
        document.getElementById('class-form').reset();
        showMessage('เพิ่มคาบสอนสำเร็จ', 'success');
      } else {
        showMessage('เกิดข้อผิดพลาดในการบันทึก กรุณาลองใหม่อีกครั้ง', 'error');
      }
    });

    // Update absent teacher dropdown
    document.getElementById('sub-weekday').addEventListener('change', () => {
      updateAbsentTeacherDropdown();
      document.getElementById('affected-classes-container').style.display = 'none';
    });

    // Show affected classes when teacher is selected
    document.getElementById('absent-teacher').addEventListener('change', showAffectedClasses);

    // Substitute form - Auto assign
    document.getElementById('substitute-form').addEventListener('submit', async (e) => {
      e.preventDefault();
      
      const date = document.getElementById('sub-date').value;
      const weekday = document.getElementById('sub-weekday').value;
      const absentTeacherId = document.getElementById('absent-teacher').value;
      const leaveReason = document.getElementById('leave-reason').value;
      
      if (!date || !weekday || !absentTeacherId || !leaveReason) return;
      
      const btn = e.target.querySelector('button[type="submit"]');
      btn.classList.add('loading');
      btn.textContent = '⏳ กำลังจัดสอนแทน...';
      
      // Get all classes of absent teacher on that day
      const affectedClasses = allData.filter(d => 
        d.type === 'class' && 
        d.teacher_id === absentTeacherId && 
        d.weekday === weekday
      );
      
      if (affectedClasses.length === 0) {
        btn.classList.remove('loading');
        btn.textContent = '🤖 จัดสอนแทนอัตโนมัติ';
        showMessage('ไม่พบคาบสอนของครูที่ลาในวันนี้', 'error');
        return;
      }
      
      // Auto assign substitute teachers
      let successCount = 0;
      for (const classData of affectedClasses) {
        const substituteTeacher = findBestSubstituteTeacher(classData, date, weekday);
        
        if (substituteTeacher) {
          const result = await window.dataSdk.create({
            type: 'substitution',
            date: date,
            class_id: classData.__backendId,
            original_teacher_id: absentTeacherId,
            substitute_teacher_id: substituteTeacher.__backendId,
            leave_reason: leaveReason,
            created_at: new Date().toISOString()
          });
          
          if (result.isOk) {
            successCount++;
          }
        }
      }
      
      btn.classList.remove('loading');
      btn.textContent = '🤖 จัดสอนแทนอัตโนมัติ';
      
      if (successCount > 0) {
        document.getElementById('substitute-form').reset();
        document.getElementById('affected-classes-container').style.display = 'none';
        const today = new Date().toISOString().split('T')[0];
        document.getElementById('sub-date').value = today;
        showMessage(`จัดสอนแทนสำเร็จ ${successCount} คาบ`, 'success');
      } else {
        showMessage('ไม่สามารถจัดสอนแทนได้ กรุณาตรวจสอบตารางสอน', 'error');
      }
    });

    // Render functions
    function renderTeachersList() {
      const teachers = allData.filter(d => d.type === 'teacher');
      const container = document.getElementById('teachers-list');
      
      if (teachers.length === 0) {
        container.innerHTML = '<p style="color: #718096; text-align: center; padding: 2rem;">ยังไม่มีข้อมูลครู กรุณาเพิ่มครูก่อน</p>';
        return;
      }
      
      container.innerHTML = teachers.map(teacher => `
        <div class="flex justify-between items-center p-4 rounded-lg" style="background-color: #f7fafc; border: 2px solid #e2e8f0;">
          <div>
            <p class="font-semibold" style="color: #2d3748; font-size: 1.1rem;">${teacher.teacher_name}</p>
            <p style="color: #718096; font-size: 0.95rem;">${teacher.department}</p>
          </div>
          <button onclick="deleteTeacher('${teacher.__backendId}')" class="btn-danger px-4 py-2 rounded-lg font-semibold text-white" style="background-color: #f56565;">
            🗑️ ลบ
          </button>
        </div>
      `).join('');
    }

    function renderClassesList() {
      const classes = allData.filter(d => d.type === 'class');
      const teachers = allData.filter(d => d.type === 'teacher');
      const container = document.getElementById('classes-list');
      
      if (classes.length === 0) {
        container.innerHTML = '<p style="color: #718096; text-align: center; padding: 2rem;">ยังไม่มีตารางสอน กรุณาเพิ่มคาบสอนก่อน</p>';
        return;
      }
      
      container.innerHTML = classes.map(cls => {
        const teacher = teachers.find(t => t.__backendId === cls.teacher_id);
        return `
          <div class="p-4 rounded-lg" style="background-color: #f7fafc; border: 2px solid #e2e8f0;">
            <div class="flex justify-between items-start mb-2">
              <div class="flex-1">
                <p class="font-semibold" style="color: #2d3748; font-size: 1.1rem;">${cls.subject} - ${cls.class_level || 'ไม่ระบุชั้น'}</p>
                <p style="color: #718096; font-size: 0.95rem;">ครูผู้สอน: ${teacher ? teacher.teacher_name : 'ไม่ระบุ'}</p>
                <p style="color: #718096; font-size: 0.95rem;">ห้อง: ${cls.room} | ${cls.weekday} | ${cls.time_range}</p>
              </div>
              <button onclick="deleteClass('${cls.__backendId}')" class="btn-danger px-4 py-2 rounded-lg font-semibold text-white" style="background-color: #f56565;">
                🗑️ ลบ
              </button>
            </div>
          </div>
        `;
      }).join('');
    }

    function renderSubstitutesList() {
      const substitutions = allData.filter(d => d.type === 'substitution');
      const teachers = allData.filter(d => d.type === 'teacher');
      const classes = allData.filter(d => d.type === 'class');
      const container = document.getElementById('substitutes-list');
      
      if (substitutions.length === 0) {
        container.innerHTML = '<p style="color: #718096; text-align: center; padding: 2rem;">ยังไม่มีรายการสอนแทน</p>';
        return;
      }
      
      // Group by date and teacher
      const grouped = {};
      substitutions.forEach(sub => {
        const key = `${sub.date}_${sub.original_teacher_id}`;
        if (!grouped[key]) {
          grouped[key] = {
            date: sub.date,
            originalTeacherId: sub.original_teacher_id,
            leaveReason: sub.leave_reason,
            substitutions: []
          };
        }
        grouped[key].substitutions.push(sub);
      });
      
      container.innerHTML = Object.values(grouped).map(group => {
        const originalTeacher = teachers.find(t => t.__backendId === group.originalTeacherId);
        
        const subsHtml = group.substitutions.map(sub => {
          const classData = classes.find(c => c.__backendId === sub.class_id);
          const substituteTeacher = teachers.find(t => t.__backendId === sub.substitute_teacher_id);
          
          return `
            <div class="flex justify-between items-center p-3 rounded" style="background-color: #ffffff; margin-bottom: 0.5rem;">
              <div class="flex-1">
                <p style="color: #2d3748; font-size: 0.95rem;"><strong>${classData ? classData.time_range : 'ไม่ระบุ'}</strong> - ${classData ? classData.subject : 'ไม่ระบุ'} ${classData && classData.class_level ? `(${classData.class_level})` : ''}</p>
                <p style="color: #718096; font-size: 0.85rem;">ห้อง: ${classData ? classData.room : 'ไม่ระบุ'} | ครูสอนแทน: ${substituteTeacher ? substituteTeacher.teacher_name : 'ไม่ระบุ'}</p>
              </div>
              <button onclick="deleteSubstitution('${sub.__backendId}')" class="btn-danger px-3 py-1 rounded font-semibold text-white" style="background-color: #f56565; font-size: 0.85rem;">
                🗑️
              </button>
            </div>
          `;
        }).join('');
        
        return `
          <div class="p-4 rounded-lg" style="background-color: #fef5e7; border: 2px solid #f9e79f; margin-bottom: 1rem;">
            <div class="mb-3">
              <p class="font-semibold" style="color: #2d3748; font-size: 1.1rem;">📅 ${group.date}</p>
              <p style="color: #e67e22; font-size: 0.95rem;">
                <strong>ครูที่ลา:</strong> ${originalTeacher ? originalTeacher.teacher_name : 'ไม่ระบุ'} 
                <span style="color: #718096;">| สาเหตุ: ${group.leaveReason || 'ไม่ระบุ'}</span>
              </p>
            </div>
            <div>
              ${subsHtml}
            </div>
          </div>
        `;
      }).join('');
    }

    function updateTeacherDropdowns() {
      const teachers = allData.filter(d => d.type === 'teacher');
      
      const classTeacherSelect = document.getElementById('class-teacher');
      
      const teacherOptions = teachers.map(t => 
        `<option value="${t.__backendId}">${t.teacher_name} (${t.department})</option>`
      ).join('');
      
      classTeacherSelect.innerHTML = '<option value="">-- เลือกครู --</option>' + teacherOptions;
    }

    function updateAbsentTeacherDropdown() {
      const weekday = document.getElementById('sub-weekday').value;
      if (!weekday) {
        document.getElementById('absent-teacher').innerHTML = '<option value="">-- เลือกวันก่อน --</option>';
        return;
      }
      
      const teachers = allData.filter(d => d.type === 'teacher');
      const classesOnDay = allData.filter(d => d.type === 'class' && d.weekday === weekday);
      
      // Get teachers who have classes on this day
      const teachersWithClasses = teachers.filter(t => 
        classesOnDay.some(c => c.teacher_id === t.__backendId)
      );
      
      const teacherOptions = teachersWithClasses.map(t => 
        `<option value="${t.__backendId}">${t.teacher_name} (${t.department})</option>`
      ).join('');
      
      document.getElementById('absent-teacher').innerHTML = '<option value="">-- เลือกครูที่ลา --</option>' + teacherOptions;
    }

    function showAffectedClasses() {
      const weekday = document.getElementById('sub-weekday').value;
      const absentTeacherId = document.getElementById('absent-teacher').value;
      
      if (!weekday || !absentTeacherId) {
        document.getElementById('affected-classes-container').style.display = 'none';
        return;
      }
      
      const affectedClasses = allData.filter(d => 
        d.type === 'class' && 
        d.teacher_id === absentTeacherId && 
        d.weekday === weekday
      );
      
      if (affectedClasses.length === 0) {
        document.getElementById('affected-classes-container').style.display = 'none';
        return;
      }
      
      const listHtml = affectedClasses.map(c => {
        return `<p style="color: #718096; font-size: 0.9rem;">• ${c.time_range} - ${c.subject} ${c.class_level ? `(${c.class_level})` : ''} (ห้อง ${c.room})</p>`;
      }).join('');
      
      document.getElementById('affected-classes-list').innerHTML = listHtml;
      document.getElementById('affected-classes-container').style.display = 'block';
    }

    // Helper function to get period number from time range
    function getPeriodNumber(timeRange) {
      const periodMap = {
        'คาบ 1 (08:30-09:25)': 1,
        'คาบ 2 (09:25-10:20)': 2,
        'คาบ 3 (10:20-11:15)': 3,
        'คาบ 4 (11:15-12:10)': 4,
        'คาบ 5 (12:10-13:00)': 5,
        'คาบ 6 (13:00-13:55)': 6,
        'คาบ 7 (13:55-14:50)': 7,
        'คาบ 8 (14:50-15:45)': 8,
        'คาบ 9 (15:45-16:40)': 9
      };
      return periodMap[timeRange] || 0;
    }

    // Find best substitute teacher (not teaching 3 consecutive periods)
    function findBestSubstituteTeacher(classToSubstitute, date, weekday) {
      const teachers = allData.filter(d => d.type === 'teacher');
      const allClasses = allData.filter(d => d.type === 'class' && d.weekday === weekday);
      const existingSubstitutions = allData.filter(d => 
        d.type === 'substitution' && 
        d.date === date
      );
      
      const targetPeriod = getPeriodNumber(classToSubstitute.time_range);
      
      // Find available teachers
      for (const teacher of teachers) {
        // Skip the absent teacher
        if (teacher.__backendId === classToSubstitute.teacher_id) continue;
        
        // Get all periods this teacher is teaching (original + substitutions)
        const teacherPeriods = [];
        
        // Original classes
        allClasses.forEach(c => {
          if (c.teacher_id === teacher.__backendId) {
            teacherPeriods.push(getPeriodNumber(c.time_range));
          }
        });
        
        // Substitution classes
        existingSubstitutions.forEach(sub => {
          if (sub.substitute_teacher_id === teacher.__backendId) {
            const subClass = allClasses.find(c => c.__backendId === sub.class_id);
            if (subClass) {
              teacherPeriods.push(getPeriodNumber(subClass.time_range));
            }
          }
        });
        
        // Check if teacher is already teaching at this period
        if (teacherPeriods.includes(targetPeriod)) continue;
        
        // Check if adding this period would create 3 consecutive periods
        const allPeriods = [...teacherPeriods, targetPeriod].sort((a, b) => a - b);
        
        let hasThreeConsecutive = false;
        for (let i = 0; i < allPeriods.length - 2; i++) {
          if (allPeriods[i+1] === allPeriods[i] + 1 && allPeriods[i+2] === allPeriods[i] + 2) {
            hasThreeConsecutive = true;
            break;
          }
        }
        
        if (!hasThreeConsecutive) {
          return teacher;
        }
      }
      
      return null;
    }

    // Delete functions
    async function deleteTeacher(id) {
      const record = allData.find(d => d.__backendId === id);
      if (!record) return;
      
      const hasClasses = allData.some(d => d.type === 'class' && d.teacher_id === id);
      if (hasClasses) {
        showMessage('ไม่สามารถลบครูที่มีคาบสอนอยู่ กรุณาลบคาบสอนก่อน', 'error');
        return;
      }
      
      const result = await window.dataSdk.delete(record);
      if (!result.isOk) {
        showMessage('เกิดข้อผิดพลาดในการลบ กรุณาลองใหม่อีกครั้ง', 'error');
      }
    }

    async function deleteClass(id) {
      const record = allData.find(d => d.__backendId === id);
      if (!record) return;
      
      const hasSubs = allData.some(d => d.type === 'substitution' && d.class_id === id);
      if (hasSubs) {
        showMessage('ไม่สามารถลบคาบที่มีการสอนแทนอยู่ กรุณาลบการสอนแทนก่อน', 'error');
        return;
      }
      
      const result = await window.dataSdk.delete(record);
      if (!result.isOk) {
        showMessage('เกิดข้อผิดพลาดในการลบ กรุณาลองใหม่อีกครั้ง', 'error');
      }
    }

    async function deleteSubstitution(id) {
      const record = allData.find(d => d.__backendId === id);
      if (!record) return;
      
      const result = await window.dataSdk.delete(record);
      if (!result.isOk) {
        showMessage('เกิดข้อผิดพลาดในการลบ กรุณาลองใหม่อีกครั้ง', 'error');
      }
    }

    // Message helper
    function showMessage(text, type) {
      const color = type === 'success' ? '#48bb78' : '#f56565';
      const msg = document.createElement('div');
      msg.textContent = text;
      msg.style.cssText = `
        position: fixed;
        top: 20px;
        right: 20px;
        background-color: ${color};
        color: white;
        padding: 1rem 1.5rem;
        border-radius: 0.5rem;
        font-weight: 600;
        z-index: 1000;
        box-shadow: 0 4px 6px rgba(0,0,0,0.1);
      `;
      document.body.appendChild(msg);
      setTimeout(() => msg.remove(), 3000);
    }

    // Initialize
    initApp();
  </script>
 <script>(function(){function c(){var b=a.contentDocument||a.contentWindow.document;if(b){var d=b.createElement('script');d.innerHTML="window.__CF$cv$params={r:'9a8029b17384731f',t:'MTc2NDczNDQ4Ny4wMDAwMDA='};var a=document.createElement('script');a.nonce='';a.src='/cdn-cgi/challenge-platform/scripts/jsd/main.js';document.getElementsByTagName('head')[0].appendChild(a);";b.getElementsByTagName('head')[0].appendChild(d)}}if(document.body){var a=document.createElement('iframe');a.height=1;a.width=1;a.style.position='absolute';a.style.top=0;a.style.left=0;a.style.border='none';a.style.visibility='hidden';document.body.appendChild(a);if('loading'!==document.readyState)c();else if(window.addEventListener)document.addEventListener('DOMContentLoaded',c);else{var e=document.onreadystatechange||function(){};document.onreadystatechange=function(b){e(b);'loading'!==document.readyState&&(document.onreadystatechange=e,c())}}}})();</script></body>
</html># Manage_classroom
