```html
<!DOCTYPE html>
<html lang="ru" class="h-full bg-slate-950 text-slate-100">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Турнир по хоккею «Золотая шайба» & «Золотой конёк»</title>
  <!-- Tailwind CSS -->
  <script src="https://cdn.tailwindcss.com"></script>
  <!-- Lucide Icons -->
  <script src="https://unpkg.com/lucide@latest"></script>
  <!-- Google Fonts -->
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Teko:wght@500;700&family=Inter:wght@400;500;600;700;800&display=swap" rel="stylesheet">

  <script>
    tailwind.config = {
      theme: {
        extend: {
          fontFamily: {
            sans: ['Inter', 'sans-serif'],
            display: ['Teko', 'sans-serif'],
          },
          colors: {
            ice: {
              50: '#f0f9ff',
              100: '#e0f2fe',
              300: '#7dd3fc',
              500: '#0ea5e9',
              600: '#0284c7',
              900: '#0c4a6e',
            },
            puck: '#0b1320',
          }
        }
      }
    }
  </script>
  <style>
    /* Custom scrollbars */
    ::-webkit-scrollbar {
      width: 8px;
      height: 8px;
    }
    ::-webkit-scrollbar-track {
      background: #0f172a;
    }
    ::-webkit-scrollbar-thumb {
      background: #334155;
      border-radius: 4px;
    }
    ::-webkit-scrollbar-thumb:hover {
      background: #0284c7;
    }
    .bracket-line {
      position: relative;
    }
    .bracket-line::after {
      content: '';
      position: absolute;
      right: -1rem;
      top: 50%;
      width: 1rem;
      height: 2px;
      background-color: #334155;
    }
  </style>
</head>
<body class="min-h-full flex flex-col bg-slate-950 font-sans text-slate-100 selection:bg-cyan-500 selection:text-white">

  <!-- HEADER / NAVBAR -->
  <header class="sticky top-0 z-30 bg-slate-900/90 backdrop-blur border-b border-slate-800">
    <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 h-20 flex items-center justify-between">
      
      <!-- Logo & Title -->
      <div class="flex items-center gap-3">
        <div class="w-12 h-12 bg-gradient-to-tr from-cyan-600 to-blue-700 rounded-xl flex items-center justify-center shadow-lg shadow-cyan-500/20 border border-cyan-400/30">
          <i data-lucide="trophy" class="w-7 h-7 text-amber-400"></i>
        </div>
        <div>
          <h1 class="font-display uppercase tracking-wider text-2xl sm:text-3xl font-bold bg-gradient-to-r from-white via-slate-200 to-cyan-400 bg-clip-text text-transparent leading-none">
            Золотая Шайба & Золотой Конёк
          </h1>
          <p class="text-xs text-cyan-400/80 font-medium tracking-wide flex items-center gap-1 mt-0.5">
            <span class="inline-block w-2 h-2 rounded-full bg-emerald-400 animate-pulse"></span>
            Всероссийский турнир по хоккею с шайбой
          </p>
        </div>
      </div>

      <!-- Header Controls / Admin Status -->
      <div class="flex items-center gap-3">
        <div id="adminBadge" class="hidden items-center gap-2 px-3 py-1.5 bg-amber-500/10 border border-amber-500/30 text-amber-400 rounded-lg text-xs font-semibold">
          <i data-lucide="shield-check" class="w-4 h-4"></i>
          <span class="hidden sm:inline">Режим Администратора</span>
        </div>

        <button id="adminLoginBtn" onclick="openLoginModal()" class="flex items-center gap-2 px-4 py-2 bg-gradient-to-r from-cyan-600 to-blue-600 hover:from-cyan-500 hover:to-blue-500 text-white rounded-lg text-sm font-semibold transition-all shadow-md shadow-cyan-900/40 border border-cyan-400/30">
          <i data-lucide="key-round" class="w-4 h-4"></i>
          <span id="loginBtnText">Вход для админа</span>
        </button>
      </div>
    </div>
  </header>

  <!-- NAVIGATION TABS -->
  <div class="bg-slate-900/50 border-b border-slate-800/80">
    <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 flex space-x-1 sm:space-x-4 overflow-x-auto py-2" id="navigationTabs">
      <button onclick="switchTab('playoff')" id="tab-playoff" class="tab-btn active px-4 py-2.5 rounded-lg text-sm font-medium whitespace-nowrap transition-all flex items-center gap-2 bg-cyan-600 text-white shadow-md shadow-cyan-900/30">
        <i data-lucide="git-fork" class="w-4 h-4"></i>
        Сетка Плей-офф
      </button>
      <button onclick="switchTab('standings')" id="tab-standings" class="tab-btn px-4 py-2.5 rounded-lg text-sm font-medium whitespace-nowrap transition-all flex items-center gap-2 text-slate-400 hover:text-slate-200 hover:bg-slate-800/50">
        <i data-lucide="table" class="w-4 h-4"></i>
        Турнирная таблица
      </button>
      <button onclick="switchTab('matches')" id="tab-matches" class="tab-btn px-4 py-2.5 rounded-lg text-sm font-medium whitespace-nowrap transition-all flex items-center gap-2 text-slate-400 hover:text-slate-200 hover:bg-slate-800/50">
        <i data-lucide="calendar" class="w-4 h-4"></i>
        Матчи группы
      </button>
      <button onclick="switchTab('scorers')" id="tab-scorers" class="tab-btn px-4 py-2.5 rounded-lg text-sm font-medium whitespace-nowrap transition-all flex items-center gap-2 text-slate-400 hover:text-slate-200 hover:bg-slate-800/50">
        <i data-lucide="award" class="w-4 h-4"></i>
        Бомбардиры
      </button>
      <button onclick="switchTab('teams')" id="tab-teams" class="tab-btn px-4 py-2.5 rounded-lg text-sm font-medium whitespace-nowrap transition-all flex items-center gap-2 text-slate-400 hover:text-slate-200 hover:bg-slate-800/50">
        <i data-lucide="shield" class="w-4 h-4"></i>
        Команды
      </button>
    </div>
  </div>

  <!-- MAIN CONTENT CONTAINER -->
  <main class="flex-grow max-w-7xl w-full mx-auto px-4 sm:px-6 lg:px-8 py-8">

    <!-- ADMIN BANNER (Visible when logged in) -->
    <div id="adminControlsBanner" class="hidden mb-6 p-4 rounded-xl bg-amber-500/10 border border-amber-500/30 flex flex-wrap items-center justify-between gap-4">
      <div class="flex items-center gap-3">
        <i data-lucide="edit-3" class="w-5 h-5 text-amber-400"></i>
        <div>
          <h4 class="text-sm font-bold text-amber-300">Режим редактирования включен</h4>
          <p class="text-xs text-amber-200/70">Вы можете изменять счет, добавлять команды, игроков и продвигать команды по сетке плей-офф.</p>
        </div>
      </div>
      <div class="flex items-center gap-2">
        <button onclick="resetDataToDefault()" class="px-3 py-1.5 bg-slate-800 hover:bg-slate-700 text-slate-300 border border-slate-700 rounded-lg text-xs font-semibold transition">
          Сбросить к начальным данным
        </button>
      </div>
    </div>

    <!-- TAB 1: PLAYOFF BRACKET -->
    <section id="section-playoff" class="tab-content">
      <div class="flex items-center justify-between mb-6">
        <div>
          <h2 class="text-2xl font-bold font-display uppercase tracking-wide text-white">Плей-офф Кубка</h2>
          <p class="text-xs text-slate-400">Сетка финальной стадии турнира</p>
        </div>
      </div>

      <div class="bg-slate-900/60 border border-slate-800 rounded-2xl p-6 shadow-xl overflow-x-auto">
        <div class="min-w-[850px] flex justify-between gap-8 py-4" id="playoffBracketContainer">
          <!-- Rendered dynamically by JS -->
        </div>
      </div>
    </section>

    <!-- TAB 2: STANDINGS TABLE -->
    <section id="section-standings" class="tab-content hidden">
      <div class="flex items-center justify-between mb-6">
        <div>
          <h2 class="text-2xl font-bold font-display uppercase tracking-wide text-white">Групповой этап</h2>
          <p class="text-xs text-slate-400">Положение команд в таблице</p>
        </div>
      </div>

      <div class="bg-slate-900/60 border border-slate-800 rounded-2xl overflow-hidden shadow-xl">
        <div class="overflow-x-auto">
          <table class="w-full text-left border-collapse">
            <thead>
              <tr class="border-b border-slate-800 bg-slate-900/80 text-xs uppercase text-slate-400 tracking-wider">
                <th class="p-4 text-center w-12">№</th>
                <th class="p-4">Команда</th>
                <th class="p-4 text-center">И</th>
                <th class="p-4 text-center">В</th>
                <th class="p-4 text-center">В Овер/Б</th>
                <th class="p-4 text-center">П</th>
                <th class="p-4 text-center">Шайбы</th>
                <th class="p-4 text-center font-bold text-cyan-400">Очки</th>
              </tr>
            </thead>
            <tbody id="standingsTableBody" class="divide-y divide-slate-800/60 text-sm">
              <!-- Rendered via JS -->
            </tbody>
          </table>
        </div>
      </div>
    </section>

    <!-- TAB 3: GROUP MATCHES -->
    <section id="section-matches" class="tab-content hidden">
      <div class="flex items-center justify-between mb-6">
        <div>
          <h2 class="text-2xl font-bold font-display uppercase tracking-wide text-white">Календарь и результаты</h2>
          <p class="text-xs text-slate-400">Все матчи групповой стадии</p>
        </div>
        <button onclick="openMatchModal()" class="admin-only hidden items-center gap-2 px-4 py-2 bg-cyan-600 hover:bg-cyan-500 text-white rounded-lg text-sm font-semibold transition">
          <i data-lucide="plus" class="w-4 h-4"></i> Добавить матч
        </button>
      </div>

      <div id="matchesList" class="grid grid-cols-1 md:grid-cols-2 gap-4">
        <!-- Rendered via JS -->
      </div>
    </section>

    <!-- TAB 4: TOP SCORERS -->
    <section id="section-scorers" class="tab-content hidden">
      <div class="flex items-center justify-between mb-6">
        <div>
          <h2 class="text-2xl font-bold font-display uppercase tracking-wide text-white">Бомбардиры турнира</h2>
          <p class="text-xs text-slate-400">Статистика игроков (Гол + Пас = Очки)</p>
        </div>
        <button onclick="openPlayerModal()" class="admin-only hidden items-center gap-2 px-4 py-2 bg-cyan-600 hover:bg-cyan-500 text-white rounded-lg text-sm font-semibold transition">
          <i data-lucide="user-plus" class="w-4 h-4"></i> Добавить игрока
        </button>
      </div>

      <div class="bg-slate-900/60 border border-slate-800 rounded-2xl overflow-hidden shadow-xl">
        <div class="overflow-x-auto">
          <table class="w-full text-left border-collapse">
            <thead>
              <tr class="border-b border-slate-800 bg-slate-900/80 text-xs uppercase text-slate-400 tracking-wider">
                <th class="p-4 text-center w-12">№</th>
                <th class="p-4">Игрок</th>
                <th class="p-4">Команда</th>
                <th class="p-4 text-center">Голы (Г)</th>
                <th class="p-4 text-center">Передачи (П)</th>
                <th class="p-4 text-center font-bold text-cyan-400">Очки (Г+П)</th>
                <th class="p-4 text-center">Штраф (мин)</th>
                <th class="admin-only hidden p-4 text-right">Действия</th>
              </tr>
            </thead>
            <tbody id="scorersTableBody" class="divide-y divide-slate-800/60 text-sm">
              <!-- Rendered via JS -->
            </tbody>
          </table>
        </div>
      </div>
    </section>

    <!-- TAB 5: TEAMS -->
    <section id="section-teams" class="tab-content hidden">
      <div class="flex items-center justify-between mb-6">
        <div>
          <h2 class="text-2xl font-bold font-display uppercase tracking-wide text-white">Участники турнира</h2>
          <p class="text-xs text-slate-400">Хоккейные клубы «Золотая шайба»</p>
        </div>
        <button onclick="openTeamModal()" class="admin-only hidden items-center gap-2 px-4 py-2 bg-cyan-600 hover:bg-cyan-500 text-white rounded-lg text-sm font-semibold transition">
          <i data-lucide="shield-plus" class="w-4 h-4"></i> Добавить команду
        </button>
      </div>

      <div id="teamsGrid" class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-4">
        <!-- Rendered via JS -->
      </div>
    </section>

  </main>

  <!-- FOOTER -->
  <footer class="mt-auto border-t border-slate-800/80 bg-slate-900/40 py-6">
    <div class="max-w-7xl mx-auto px-4 text-center text-xs text-slate-500">
      <p>© 2026 Хоккейный турнир «Золотая Шайба» и «Золотой Конёк». Все права защищены.</p>
      <p class="mt-1">Официальная платформа учета результатов хоккейных матчей.</p>
    </div>
  </footer>

  <!-- ================= MODALS ================= -->

  <!-- 1. LOGIN MODAL -->
  <div id="loginModal" class="fixed inset-0 z-50 bg-slate-950/80 backdrop-blur-sm hidden items-center justify-center p-4">
    <div class="bg-slate-900 border border-slate-800 rounded-2xl w-full max-w-md p-6 shadow-2xl relative animate-in fade-in zoom-in duration-200">
      <button onclick="closeLoginModal()" class="absolute top-4 right-4 text-slate-400 hover:text-white">
        <i data-lucide="x" class="w-5 h-5"></i>
      </button>
      <div class="flex items-center gap-3 mb-4">
        <div class="w-10 h-10 rounded-xl bg-cyan-500/10 border border-cyan-500/30 flex items-center justify-center text-cyan-400">
          <i data-lucide="lock" class="w-5 h-5"></i>
        </div>
        <div>
          <h3 class="text-lg font-bold text-white">Вход для Администратора</h3>
          <p class="text-xs text-slate-400">Введите пароль доступа к управлению</p>
        </div>
      </div>
      
      <form onsubmit="handleLogin(event)" class="space-y-4">
        <div>
          <label class="block text-xs font-medium text-slate-300 mb-1">Пароль администратора</label>

          <!-- Hidden Password Field -->
          <div class="relative">
            <input type="password" id="adminPasswordInput" required placeholder="••••••••" class="w-full bg-slate-950 border border-slate-800 rounded-lg px-4 py-2.5 text-white text-sm focus:outline-none focus:border-cyan-500 transition">
            <button type="button" onclick="togglePasswordVisibility()" class="absolute right-3 top-2.5 text-slate-500 hover:text-slate-300">
              <i data-lucide="eye" id="eyeIcon" class="w-4 h-4"></i>
            </button>
          </div>
          <p id="loginError" class="text-xs text-rose-500 mt-1.5 hidden">Неверный пароль. Попробуйте еще раз.</p>
        </div>

        <div class="flex justify-end gap-2 pt-2">
          <button type="button" onclick="closeLoginModal()" class="px-4 py-2 bg-slate-800 hover:bg-slate-700 text-slate-300 rounded-lg text-sm font-medium transition">Отмена</button>
          <button type="submit" class="px-5 py-2 bg-cyan-600 hover:bg-cyan-500 text-white rounded-lg text-sm font-semibold transition shadow-md shadow-cyan-900/30">Войти</button>
        </div>
      </form>
    </div>
  </div>

  <!-- 2. TEAM MODAL (Add/Edit Team) -->
  <div id="teamModal" class="fixed inset-0 z-50 bg-slate-950/80 backdrop-blur-sm hidden items-center justify-center p-4">
    <div class="bg-slate-900 border border-slate-800 rounded-2xl w-full max-w-lg p-6 shadow-2xl relative">
      <button onclick="closeTeamModal()" class="absolute top-4 right-4 text-slate-400 hover:text-white">
        <i data-lucide="x" class="w-5 h-5"></i>
      </button>
      <h3 id="teamModalTitle" class="text-lg font-bold text-white mb-4">Добавить команду</h3>
      
      <form onsubmit="saveTeam(event)" class="space-y-4">
        <input type="hidden" id="teamId">
        <div>
          <label class="block text-xs font-medium text-slate-300 mb-1">Название хоккейного клуба</label>
          <input type="text" id="teamName" required placeholder="Например: ХК Авангард" class="w-full bg-slate-950 border border-slate-800 rounded-lg px-4 py-2 text-white text-sm focus:outline-none focus:border-cyan-500">
        </div>
        <div>
          <label class="block text-xs font-medium text-slate-300 mb-1">Город / Регион</label>
          <input type="text" id="teamCity" required placeholder="Например: Москва" class="w-full bg-slate-950 border border-slate-800 rounded-lg px-4 py-2 text-white text-sm focus:outline-none focus:border-cyan-500">
        </div>
        <div>
          <label class="block text-xs font-medium text-slate-300 mb-1">URL Логотипа / Эмблемы команды</label>
          <input type="url" id="teamLogo" placeholder="https://example.com/logo.png" class="w-full bg-slate-950 border border-slate-800 rounded-lg px-4 py-2 text-white text-sm focus:outline-none focus:border-cyan-500">
          <p class="text-[11px] text-slate-500 mt-1">Оставьте пустым, чтобы использовать эмблему по умолчанию</p>
        </div>
        
        <div class="flex justify-end gap-2 pt-2">
          <button type="button" onclick="closeTeamModal()" class="px-4 py-2 bg-slate-800 text-slate-300 rounded-lg text-sm">Отмена</button>
          <button type="submit" class="px-5 py-2 bg-cyan-600 text-white rounded-lg text-sm font-semibold">Сохранить</button>
        </div>
      </form>
    </div>
  </div>

  <!-- 3. PLAYOFF MATCH EDIT MODAL -->
  <div id="playoffMatchModal" class="fixed inset-0 z-50 bg-slate-950/80 backdrop-blur-sm hidden items-center justify-center p-4">
    <div class="bg-slate-900 border border-slate-800 rounded-2xl w-full max-w-md p-6 shadow-2xl relative">
      <button onclick="closePlayoffMatchModal()" class="absolute top-4 right-4 text-slate-400 hover:text-white">
        <i data-lucide="x" class="w-5 h-5"></i>
      </button>
      <h3 class="text-lg font-bold text-white mb-2">Результат матча плей-офф</h3>
      <p id="playoffStageLabel" class="text-xs text-cyan-400 mb-4 font-semibold uppercase tracking-wider"></p>
      
      <form onsubmit="savePlayoffMatchScore(event)" class="space-y-4">
        <input type="hidden" id="playoffMatchId">
        
        <div class="grid grid-cols-2 gap-4 bg-slate-950 p-4 rounded-xl border border-slate-800">
          <div class="text-center">
            <div id="pTeam1Logo" class="w-10 h-10 mx-auto mb-2 rounded-full overflow-hidden flex items-center justify-center bg-slate-800"></div>
            <p id="pTeam1Name" class="text-xs font-bold text-slate-200 mb-2 truncate"></p>
            <input type="number" id="pTeam1Score" min="0" required class="w-16 mx-auto text-center font-display text-2xl font-bold bg-slate-900 border border-slate-700 rounded-lg py-1 text-white">
          </div>
          <div class="text-center">
            <div id="pTeam2Logo" class="w-10 h-10 mx-auto mb-2 rounded-full overflow-hidden flex items-center justify-center bg-slate-800"></div>
            <p id="pTeam2Name" class="text-xs font-bold text-slate-200 mb-2 truncate"></p>
            <input type="number" id="pTeam2Score" min="0" required class="w-16 mx-auto text-center font-display text-2xl font-bold bg-slate-900 border border-slate-700 rounded-lg py-1 text-white">
          </div>
        </div>

        <div class="flex items-center gap-2">
          <input type="checkbox" id="playoffMatchOT" class="rounded border-slate-800 text-cyan-600 focus:ring-cyan-500">
          <label for="playoffMatchOT" class="text-xs text-slate-300">Матч завершился в овертайме / по буллитам (ОТ/Б)</label>
        </div>

        <div class="flex justify-end gap-2 pt-2">
          <button type="button" onclick="closePlayoffMatchModal()" class="px-4 py-2 bg-slate-800 text-slate-300 rounded-lg text-sm">Отмена</button>
          <button type="submit" class="px-5 py-2 bg-cyan-600 text-white rounded-lg text-sm font-semibold">Сохранить и продвинуть победителя</button>
        </div>
      </form>
    </div>
  </div>

  <!-- 4. PLAYER MODAL (Add Player) -->
  <div id="playerModal" class="fixed inset-0 z-50 bg-slate-950/80 backdrop-blur-sm hidden items-center justify-center p-4">
    <div class="bg-slate-900 border border-slate-800 rounded-2xl w-full max-w-md p-6 shadow-2xl relative">
      <button onclick="closePlayerModal()" class="absolute top-4 right-4 text-slate-400 hover:text-white">
        <i data-lucide="x" class="w-5 h-5"></i>
      </button>
      <h3 class="text-lg font-bold text-white mb-4">Добавить бомбардира</h3>
      
      <form onsubmit="savePlayer(event)" class="space-y-4">
        <div>
          <label class="block text-xs font-medium text-slate-300 mb-1">Имя и Фамилия хоккеиста</label>
          <input type="text" id="playerName" required placeholder="Иван Иванов" class="w-full bg-slate-950 border border-slate-800 rounded-lg px-4 py-2 text-white text-sm focus:outline-none focus:border-cyan-500">
        </div>
        <div>
          <label class="block text-xs font-medium text-slate-300 mb-1">Команда</label>
          <select id="playerTeamId" required class="w-full bg-slate-950 border border-slate-800 rounded-lg px-4 py-2 text-white text-sm focus:outline-none focus:border-cyan-500">
            <!-- Populated via JS -->
          </select>
        </div>
        <div class="grid grid-cols-3 gap-3">
          <div>
            <label class="block text-xs font-medium text-slate-300 mb-1">Голы</label>
            <input type="number" id="playerGoals" min="0" value="0" required class="w-full bg-slate-950 border border-slate-800 rounded-lg px-3 py-2 text-white text-sm">
          </div>
          <div>
            <label class="block text-xs font-medium text-slate-300 mb-1">Пасы</label>
            <input type="number" id="playerAssists" min="0" value="0" required class="w-full bg-slate-950 border border-slate-800 rounded-lg px-3 py-2 text-white text-sm">
          </div>
          <div>
            <label class="block text-xs font-medium text-slate-300 mb-1">Штраф (мин)</label>
            <input type="number" id="playerPim" min="0" value="0" required class="w-full bg-slate-950 border border-slate-800 rounded-lg px-3 py-2 text-white text-sm">
          </div>
        </div>

        <div class="flex justify-end gap-2 pt-2">
          <button type="button" onclick="closePlayerModal()" class="px-4 py-2 bg-slate-800 text-slate-300 rounded-lg text-sm">Отмена</button>
          <button type="submit" class="px-5 py-2 bg-cyan-600 text-white rounded-lg text-sm font-semibold">Сохранить</button>
        </div>
      </form>
    </div>
  </div>

  <!-- 5. MATCH EDIT MODAL -->
  <div id="matchModal" class="fixed inset-0 z-50 bg-slate-950/80 backdrop-blur-sm hidden items-center justify-center p-4">
    <div class="bg-slate-900 border border-slate-800 rounded-2xl w-full max-w-md p-6 shadow-2xl relative">
      <button onclick="closeMatchModal()" class="absolute top-4 right-4 text-slate-400 hover:text-white">
        <i data-lucide="x" class="w-5 h-5"></i>
      </button>
      <h3 class="text-lg font-bold text-white mb-4">Добавить / Изменить матч</h3>
      
      <form onsubmit="saveMatch(event)" class="space-y-4">
        <input type="hidden" id="matchId">
        
        <div>
          <label class="block text-xs font-medium text-slate-300 mb-1">Команда 1 (Хозяева)</label>
          <select id="matchTeam1Id" required class="w-full bg-slate-950 border border-slate-800 rounded-lg px-4 py-2 text-white text-sm focus:outline-none focus:border-cyan-500"></select>
        </div>
        <div>
          <label class="block text-xs font-medium text-slate-300 mb-1">Команда 2 (Гости)</label>
          <select id="matchTeam2Id" required class="w-full bg-slate-950 border border-slate-800 rounded-lg px-4 py-2 text-white text-sm focus:outline-none focus:border-cyan-500"></select>
        </div>

        <div class="grid grid-cols-2 gap-4 bg-slate-950 p-3 rounded-xl border border-slate-800">
          <div>
            <label class="block text-xs font-medium text-slate-400 mb-1 text-center">Счет К1</label>
            <input type="number" id="matchScore1" min="0" placeholder="0" class="w-full text-center font-bold bg-slate-900 border border-slate-700 rounded-lg py-1.5 text-white">
          </div>
          <div>
            <label class="block text-xs font-medium text-slate-400 mb-1 text-center">Счет К2</label>
            <input type="number" id="matchScore2" min="0" placeholder="0" class="w-full text-center font-bold bg-slate-900 border border-slate-700 rounded-lg py-1.5 text-white">
          </div>
        </div>

        <div class="flex items-center gap-2">
          <input type="checkbox" id="matchIsOT" class="rounded border-slate-800 text-cyan-600 focus:ring-cyan-500">
          <label for="matchIsOT" class="text-xs text-slate-300">Победа в овертайме или по буллитам (ОТ/Б)</label>
        </div>

        <div>
          <label class="block text-xs font-medium text-slate-300 mb-1">Статус матча</label>
          <select id="matchPlayed" class="w-full bg-slate-950 border border-slate-800 rounded-lg px-4 py-2 text-white text-sm">
            <option value="scheduled">Запланирован</option>
            <option value="played">Завершен</option>
            <option value="postponed">Перенесён</option>
            <option value="cancelled">Отменён</option>
          </select>
        </div>

        <div class="flex justify-between items-center pt-2">
          <button type="button" id="deleteMatchModalBtn" onclick="deleteMatchFromModal()" class="px-3 py-2 bg-rose-600/20 text-rose-400 hover:bg-rose-600/30 rounded-lg text-sm font-semibold transition border border-rose-500/30 hidden">
            Удалить матч
          </button>
          <div class="flex gap-2 ml-auto">
            <button type="button" onclick="closeMatchModal()" class="px-4 py-2 bg-slate-800 text-slate-300 rounded-lg text-sm">Отмена</button>
            <button type="submit" class="px-5 py-2 bg-cyan-600 text-white rounded-lg text-sm font-semibold">Сохранить</button>
          </div>
        </div>
      </form>
    </div>
  </div>


  <!-- JS LOGIC -->
  <script>
    // INITIAL DEFAULT DATA (HOCKEY ONLY)
    // REAL-TIME BROADCAST SYNCHRONIZATION ACROSS TABS/USERS
    window.addEventListener('storage', (event) => {
      if (['hockey_teams', 'hockey_players', 'hockey_matches', 'hockey_playoff'].includes(event.key)) {
        teams = JSON.parse(localStorage.getItem('hockey_teams')) || defaultTeams;
        players = JSON.parse(localStorage.getItem('hockey_players')) || defaultPlayers;
        matches = JSON.parse(localStorage.getItem('hockey_matches')) || defaultMatches;
        playoff = JSON.parse(localStorage.getItem('hockey_playoff')) || defaultPlayoff;
        renderAll();
      }
    });

    // LOCAL STORAGE SAVER
    function saveData() {
      localStorage.setItem('hockey_teams', JSON.stringify(teams));
      localStorage.setItem('hockey_players', JSON.stringify(players));
      localStorage.setItem('hockey_matches', JSON.stringify(matches));
      localStorage.setItem('hockey_playoff', JSON.stringify(playoff));
    }

    function resetDataToDefault() {
      if (confirm('Сбросить все данные к исходным?')) {
        teams = defaultTeams;
        players = defaultPlayers;
        matches = defaultMatches;
        playoff = defaultPlayoff;
        saveData();
        renderAll();
      }
    }

    // AUTHENTICATION LOGIC (HIDDEN PASSWORD FIELD)
    function openLoginModal() {
      if (isAdmin) {
        // Logout if already logged in
        isAdmin = false;
        localStorage.setItem('hockey_admin', 'false');
        updateAdminUI();
      } else {
        document.getElementById('adminPasswordInput').value = '';
        document.getElementById('loginError').classList.add('hidden');
        document.getElementById('loginModal').classList.remove('hidden');
        document.getElementById('loginModal').classList.add('flex');
      }
    }

    function closeLoginModal() {
      document.getElementById('loginModal').classList.add('hidden');
      document.getElementById('loginModal').classList.remove('flex');
    }

    function handleLogin(e) {
      e.preventDefault();
      const pwd = document.getElementById('adminPasswordInput').value;
      if (pwd === 'admin2026') {
        isAdmin = true;
        localStorage.setItem('hockey_admin', 'true');
        closeLoginModal();
        updateAdminUI();
      } else {
        document.getElementById('loginError').classList.remove('hidden');
      }
    }

    function togglePasswordVisibility() {
      const pwdInput = document.getElementById('adminPasswordInput');
      const eyeIcon = document.getElementById('eyeIcon');
      if (pwdInput.type === 'password') {
        pwdInput.type = 'text';
        eyeIcon.setAttribute('data-lucide', 'eye-off');
      } else {
        pwdInput.type = 'password';
        eyeIcon.setAttribute('data-lucide', 'eye');
      }
      lucide.createIcons();
    }

    function updateAdminUI() {
      const loginBtnText = document.getElementById('loginBtnText');
      const adminBadge = document.getElementById('adminBadge');
      const adminBanner = document.getElementById('adminControlsBanner');
      const adminElements = document.querySelectorAll('.admin-only');

      if (isAdmin) {
        loginBtnText.textContent = 'Выйти из админа';
        adminBadge.classList.remove('hidden');
        adminBadge.classList.add('flex');
        adminBanner.classList.remove('hidden');
        adminElements.forEach(el => {
          el.classList.remove('hidden');
          if (el.tagName === 'BUTTON') el.classList.add('flex');
        });
      } else {
        loginBtnText.textContent = 'Вход для админа';
        adminBadge.classList.add('hidden');
        adminBadge.classList.remove('flex');
        adminBanner.classList.add('hidden');
        adminElements.forEach(el => el.classList.add('hidden'));
      }
      renderAll();
    }

    // TABS SWITCHING
    function switchTab(tabName) {
      document.querySelectorAll('.tab-btn').forEach(btn => {
        btn.classList.remove('bg-cyan-600', 'text-white', 'shadow-md', 'shadow-cyan-900/30');
        btn.classList.add('text-slate-400', 'hover:text-slate-200', 'hover:bg-slate-800/50');
      });
      const activeBtn = document.getElementById(`tab-${tabName}`);
      activeBtn.classList.add('bg-cyan-600', 'text-white', 'shadow-md', 'shadow-cyan-900/30');
      activeBtn.classList.remove('text-slate-400', 'hover:text-slate-200', 'hover:bg-slate-800/50');

      document.querySelectorAll('.tab-content').forEach(sec => sec.classList.add('hidden'));
      document.getElementById(`section-${tabName}`).classList.remove('hidden');
    }

    // HELPER: RENDER LOGO OR DEFAULT EMBLEM
    function getTeamLogoHTML(logoUrl, teamName, sizeClass = "w-6 h-6") {
      if (logoUrl && logoUrl.trim() !== '') {
        return `<img src="${logoUrl}" alt="${teamName}" class="${sizeClass} object-cover rounded-full border border-slate-700/60 bg-slate-800" onerror="this.onerror=null; this.src=''; this.parentElement.innerHTML='<div class=\'${sizeClass} rounded-full bg-slate-800 flex items-center justify-center text-cyan-400 border border-slate-700\'><i data-lucide=\'shield\' class=\'w-3.5 h-3.5\'></i></div>';">`;
      }
      return `<div class="${sizeClass} rounded-full bg-slate-800 flex items-center justify-center text-cyan-400 border border-slate-700"><i data-lucide="shield" class="w-3.5 h-3.5"></i></div>`;
    }

    // RENDER ALL DATA
    function renderAll() {
      renderStandings();
      renderMatches();
      renderScorers();
      renderTeams();
      renderPlayoff();
      lucide.createIcons();
    }

    // 1. STANDINGS LOGIC
    function renderStandings() {
      // Calculate standings stats
      const stats = {};
      teams.forEach(t => {
        stats[t.id] = { team: t, played: 0, wins: 0, otWins: 0, losses: 0, goalsFor: 0, goalsAgainst: 0, points: 0 };
      });

      matches.forEach(m => {
        if (m.played && stats[m.team1Id] && stats[m.team2Id]) {
          const t1 = stats[m.team1Id];
          const t2 = stats[m.team2Id];

          t1.played++;
          t2.played++;
          t1.goalsFor += m.score1;
          t1.goalsAgainst += m.score2;
          t2.goalsFor += m.score2;
          t2.goalsAgainst += m.score1;

          if (m.score1 > m.score2) {
            if (m.isOT) {
              t1.otWins++;
              t1.points += 2;
              t2.points += 1;
            } else {
              t1.wins++;
              t1.points += 3;
            }
          } else if (m.score2 > m.score1) {
            if (m.isOT) {
              t2.otWins++;
              t2.points += 2;
              t1.points += 1;
            } else {
              t2.wins++;
              t2.points += 3;
            }
          }
        }
      });

      const sorted = Object.values(stats).sort((a, b) => {
        if (b.points !== a.points) return b.points - a.points;
        const diffB = b.goalsFor - b.goalsAgainst;
        const diffA = a.goalsFor - a.goalsAgainst;
        return diffB - diffA;
      });

      const tbody = document.getElementById('standingsTableBody');
      tbody.innerHTML = sorted.map((row, index) => `
        <tr class="hover:bg-slate-800/40 transition-colors">
          <td class="p-4 text-center font-bold text-slate-500">${index + 1}</td>
          <td class="p-4 font-semibold text-slate-100 flex items-center gap-3">
            ${getTeamLogoHTML(row.team.logo, row.team.name, "w-7 h-7")}
            <span>${row.team.name}</span>
            <span class="text-xs text-slate-500 font-normal">(${row.team.city})</span>
          </td>
          <td class="p-4 text-center font-medium">${row.played}</td>
          <td class="p-4 text-center text-emerald-400 font-medium">${row.wins}</td>
          <td class="p-4 text-center text-cyan-400 font-medium">${row.otWins}</td>
          <td class="p-4 text-center text-rose-400 font-medium">${row.losses}</td>
          <td class="p-4 text-center text-slate-400 font-mono text-xs">${row.goalsFor}:${row.goalsAgainst}</td>
          <td class="p-4 text-center font-bold text-cyan-400 text-base">${row.points}</td>
        </tr>
      `).join('');
    }

    // 2. GROUP MATCHES
    function renderMatches() {
      const container = document.getElementById('matchesList');
      if (matches.length === 0) {
        container.innerHTML = '<p class="text-slate-500 text-sm col-span-2 text-center py-8">Матчи пока не запланированы</p>';
        return;
      }

      container.innerHTML = matches.map(m => {
        const t1 = teams.find(t => t.id === m.team1Id) || { name: 'Неизвестно', logo: '' };
        const t2 = teams.find(t => t.id === m.team2Id) || { name: 'Неизвестно', logo: '' };

        let statusHTML = '';
        if (m.status === 'postponed' || m.played === 'postponed') {
          statusHTML = '<span class="text-xs font-semibold text-amber-400 bg-amber-500/10 border border-amber-500/30 px-2 py-0.5 rounded">Перенесён</span>';
        } else if (m.status === 'cancelled' || m.played === 'cancelled') {
          statusHTML = '<span class="text-xs font-semibold text-rose-400 bg-rose-500/10 border border-rose-500/30 px-2 py-0.5 rounded">Отменён</span>';
        } else if (m.played === true || m.played === 'played') {
          statusHTML = `
            <span class="font-display text-xl font-bold tracking-wider text-cyan-400">${m.score1} : ${m.score2}</span>
            ${m.isOT ? '<div class="text-[10px] text-amber-400 font-semibold uppercase">ОТ/Б</div>' : ''}
          `;
        } else {
          statusHTML = '<span class="text-xs font-semibold text-slate-500">Запланирован</span>';
        }

        return `
          <div class="bg-slate-900/60 border border-slate-800 rounded-xl p-4 shadow-md hover:border-slate-700 transition">
            <div class="flex items-center justify-between gap-4">
              
              <!-- Team 1 -->
              <div class="flex items-center gap-3 flex-1 min-w-0">
                ${getTeamLogoHTML(t1.logo, t1.name, "w-8 h-8")}
                <span class="font-bold text-slate-200 text-sm truncate">${t1.name}</span>
              </div>

              <!-- Score / Status -->
              <div class="text-center px-3 py-1.5 bg-slate-950 rounded-lg border border-slate-800/80 min-w-[100px] flex flex-col items-center justify-center">
                ${statusHTML}
              </div>

              <!-- Team 2 -->
              <div class="flex items-center justify-end gap-3 flex-1 min-w-0 text-right">
                <span class="font-bold text-slate-200 text-sm truncate">${t2.name}</span>
                ${getTeamLogoHTML(t2.logo, t2.name, "w-8 h-8")}
              </div>

            </div>

            ${isAdmin ? `
              <div class="mt-3 pt-2 border-t border-slate-800/60 flex justify-end items-center gap-3">
                <button onclick="editMatch('${m.id}')" class="text-xs text-cyan-400 hover:text-cyan-300 font-medium">Редактировать</button>
                <button onclick="deleteMatch('${m.id}')" class="text-xs text-rose-400 hover:text-rose-300 font-medium">Удалить</button>
              </div>
            ` : ''}
          </div>
        `;
      }).join('');
    }

    // MATCH MODAL FUNCTIONS
    function openMatchModal(id = null) {
      const select1 = document.getElementById('matchTeam1Id');
      const select2 = document.getElementById('matchTeam2Id');
      const deleteBtn = document.getElementById('deleteMatchModalBtn');
      
      const options = teams.map(t => `<option value="${t.id}">${t.name} (${t.city})</option>`).join('');
      select1.innerHTML = options;
      select2.innerHTML = options;

      document.getElementById('matchId').value = id || '';
      if (id) {
        const m = matches.find(item => item.id === id);
        select1.value = m.team1Id;
        select2.value = m.team2Id;
        document.getElementById('matchScore1').value = m.score1;
        document.getElementById('matchScore2').value = m.score2;
        document.getElementById('matchIsOT').checked = m.isOT || false;
        
        const statusVal = m.status || (m.played === true ? 'played' : 'scheduled');
        document.getElementById('matchPlayed').value = statusVal;
        
        if (deleteBtn) deleteBtn.classList.remove('hidden');
      } else {
        document.getElementById('matchScore1').value = '0';
        document.getElementById('matchScore2').value = '0';
        document.getElementById('matchIsOT').checked = false;
        document.getElementById('matchPlayed').value = 'scheduled';
        if (deleteBtn) deleteBtn.classList.add('hidden');
      }

      document.getElementById('matchModal').classList.remove('hidden');
      document.getElementById('matchModal').classList.add('flex');
    }

    function saveMatch(e) {
      e.preventDefault();
      const id = document.getElementById('matchId').value;
      const team1Id = document.getElementById('matchTeam1Id').value;
      const team2Id = document.getElementById('matchTeam2Id').value;

      if (team1Id === team2Id) {
        alert('Команды должны быть разными!');
        return;
      }

      const score1 = parseInt(document.getElementById('matchScore1').value) || 0;
      const score2 = parseInt(document.getElementById('matchScore2').value) || 0;
      const isOT = document.getElementById('matchIsOT').checked;
      const statusVal = document.getElementById('matchPlayed').value;
      const played = statusVal === 'played';

      if (id) {
        const m = matches.find(item => item.id === id);
        if (m) {
          m.team1Id = team1Id;
          m.team2Id = team2Id;
          m.score1 = score1;
          m.score2 = score2;
          m.isOT = isOT;
          m.played = played;
          m.status = statusVal;
        }
      } else {
        matches.push({ id: 'm_' + Date.now(), team1Id, team2Id, score1, score2, isOT, played, status: statusVal });
      }

      saveData();
      closeMatchModal();
      renderAll();
    }

    function deleteMatchFromModal() {
      const id = document.getElementById('matchId').value;
      if (id) {
        deleteMatch(id);
        closeMatchModal();
      }
    }

    // MATCH MODAL FUNCTIONS
    function openMatchModal(id = null) {
      const select1 = document.getElementById('matchTeam1Id');
      const select2 = document.getElementById('matchTeam2Id');
      
      const options = teams.map(t => `<option value="${t.id}">${t.name} (${t.city})</option>`).join('');
      select1.innerHTML = options;
      select2.innerHTML = options;

      document.getElementById('matchId').value = id || '';
      if (id) {
        const m = matches.find(item => item.id === id);
        select1.value = m.team1Id;
        select2.value = m.team2Id;
        document.getElementById('matchScore1').value = m.score1;
        document.getElementById('matchScore2').value = m.score2;
        document.getElementById('matchIsOT').checked = m.isOT || false;
        document.getElementById('matchPlayed').value = m.played ? 'true' : 'false';
      } else {
        document.getElementById('matchScore1').value = '0';
        document.getElementById('matchScore2').value = '0';
        document.getElementById('matchIsOT').checked = false;
        document.getElementById('matchPlayed').value = 'false';
      }

      document.getElementById('matchModal').classList.remove('hidden');
      document.getElementById('matchModal').classList.add('flex');
    }

    function closeMatchModal() {
      document.getElementById('matchModal').classList.add('hidden');
      document.getElementById('matchModal').classList.remove('flex');
    }

    function saveMatch(e) {
      e.preventDefault();
      const id = document.getElementById('matchId').value;
      const team1Id = document.getElementById('matchTeam1Id').value;
      const team2Id = document.getElementById('matchTeam2Id').value;

      if (team1Id === team2Id) {
        alert('Команды должны быть разными!');
        return;
      }

      const score1 = parseInt(document.getElementById('matchScore1').value) || 0;
      const score2 = parseInt(document.getElementById('matchScore2').value) || 0;
      const isOT = document.getElementById('matchIsOT').checked;
      const played = document.getElementById('matchPlayed').value === 'true';

      if (id) {
        const m = matches.find(item => item.id === id);
        if (m) {
          m.team1Id = team1Id;
          m.team2Id = team2Id;
          m.score1 = score1;
          m.score2 = score2;
          m.isOT = isOT;
          m.played = played;
        }
      } else {
        matches.push({ id: 'm_' + Date.now(), team1Id, team2Id, score1, score2, isOT, played });
      }

      saveData();
      closeMatchModal();
      renderAll();
    }

    function editMatch(id) { openMatchModal(id); }

    function deleteMatch(id) {
      if (confirm('Удалить этот матч?')) {
        matches = matches.filter(m => m.id !== id);
        saveData();
        renderAll();
      }
    }

    // PLAYOFF MATCH MODAL FUNCTIONS
    let currentPlayoffMatch = null;
    function openPlayoffMatchModal(matchId) {
      let m = null;
      for (const cat in playoff) {
        const found = playoff[cat].find(item => item.id === matchId);
        if (found) { m = found; break; }
      }

      if (!m) return;
      currentPlayoffMatch = m;

      const t1 = teams.find(t => t.id === m.team1Id);
      const t2 = teams.find(t => t.id === m.team2Id);

      document.getElementById('playoffMatchId').value = m.id;
      document.getElementById('playoffStageLabel').textContent = m.stage;

      document.getElementById('pTeam1Name').textContent = t1 ? t1.name : 'TBD';
      document.getElementById('pTeam2Name').textContent = t2 ? t2.name : 'TBD';

      document.getElementById('pTeam1Logo').innerHTML = t1 ? getTeamLogoHTML(t1.logo, t1.name, "w-full h-full") : '';
      document.getElementById('pTeam2Logo').innerHTML = t2 ? getTeamLogoHTML(t2.logo, t2.name, "w-full h-full") : '';

      document.getElementById('pTeam1Score').value = m.score1 !== null ? m.score1 : 0;
      document.getElementById('pTeam2Score').value = m.score2 !== null ? m.score2 : 0;

      document.getElementById('playoffMatchModal').classList.remove('hidden');
      document.getElementById('playoffMatchModal').classList.add('flex');
    }

    function closePlayoffMatchModal() {
      document.getElementById('playoffMatchModal').classList.add('hidden');
      document.getElementById('playoffMatchModal').classList.remove('flex');
    }

    function savePlayoffMatchScore(e) {
      e.preventDefault();
      if (!currentPlayoffMatch) return;

      const s1 = parseInt(document.getElementById('pTeam1Score').value) || 0;
      const s2 = parseInt(document.getElementById('pTeam2Score').value) || 0;

      if (s1 === s2) {
        alert('В хоккее плей-офф не бывает ничьих! Определите победителя в овертайме/буллитах.');
        return;
      }

      currentPlayoffMatch.score1 = s1;
      currentPlayoffMatch.score2 = s2;
      currentPlayoffMatch.played = true;

      const winnerId = s1 > s2 ? currentPlayoffMatch.team1Id : currentPlayoffMatch.team2Id;
      currentPlayoffMatch.winnerId = winnerId;

      // ADVANCE WINNER AUTOMATICALLY
      if (currentPlayoffMatch.id === 'qf1') playoff.semifinals[0].team1Id = winnerId;
      if (currentPlayoffMatch.id === 'qf2') playoff.semifinals[0].team2Id = winnerId;
      if (currentPlayoffMatch.id === 'qf3') playoff.semifinals[1].team1Id = winnerId;
      if (currentPlayoffMatch.id === 'qf4') playoff.semifinals[1].team2Id = winnerId;

      if (currentPlayoffMatch.id === 'sf1') playoff.final[0].team1Id = winnerId;
      if (currentPlayoffMatch.id === 'sf2') playoff.final[0].team2Id = winnerId;

      saveData();
      closePlayoffMatchModal();
      renderAll();
    }

    // PLAYER MODAL FUNCTIONS
    function openPlayerModal() {
      const select = document.getElementById('playerTeamId');
      select.innerHTML = teams.map(t => `<option value="${t.id}">${t.name}</option>`).join('');

      document.getElementById('playerName').value = '';
      document.getElementById('playerGoals').value = 0;
      document.getElementById('playerAssists').value = 0;
      document.getElementById('playerPim').value = 0;

      document.getElementById('playerModal').classList.remove('hidden');
      document.getElementById('playerModal').classList.add('flex');
    }

    function closePlayerModal() {
      document.getElementById('playerModal').classList.add('hidden');
      document.getElementById('playerModal').classList.remove('flex');
    }

    function savePlayer(e) {
      e.preventDefault();
      const name = document.getElementById('playerName').value.trim();
      const teamId = document.getElementById('playerTeamId').value;
      const goals = parseInt(document.getElementById('playerGoals').value) || 0;
      const assists = parseInt(document.getElementById('playerAssists').value) || 0;
      const pim = parseInt(document.getElementById('playerPim').value) || 0;

      players.push({ id: 'p_' + Date.now(), name, teamId, goals, assists, pim });
      saveData();
      closePlayerModal();
      renderAll();
    }

    function deletePlayer(id) {
      if (confirm('Удалить игрока?')) {
        players = players.filter(p => p.id !== id);
        saveData();
        renderAll();
      }
    }
  </script>
</body>
</html>
```
