<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>League Game Schedule & Map Dashboard</title>
    <!-- Tailwind CSS for sleek, modern UI styling -->
    <script src="https://cdn.jsdelivr.net/npm/@tailwindcss/browser@4"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap');
        body { font-family: 'Inter', sans-serif; }
    </style>
</head>
<body class="bg-slate-50 text-slate-800 min-h-screen">

    <!-- Header Navigation -->
    <header class="bg-indigo-900 text-white shadow-md sticky top-0 z-50">
        <div class="max-w-7xl mx-auto px-4 py-4 flex flex-col sm:flex-row justify-between items-center gap-4">
            <div class="flex items-center gap-3">
                <div class="bg-amber-500 p-2 rounded-lg text-indigo-950 font-bold text-xl shadow-inner">⚾</div>
                <div>
                    <h1 class="text-xl font-bold tracking-tight">League Dashboard</h1>
                    <p class="text-xs text-indigo-200">Official Game Schedule & Venues</p>
                </div>
            </div>
            <div class="text-xs sm:text-sm bg-indigo-950/50 px-3 py-1.5 rounded-full text-indigo-200 border border-indigo-700">
                Season: <span class="text-amber-400 font-semibold">Fall 2026</span>
            </div>
        </div>
    </header>

    <main class="max-w-7xl mx-auto px-4 py-8 grid grid-cols-1 lg:grid-cols-3 gap-8">
        
        <!-- Left 2 Columns: Filter and Schedule Table -->
        <div class="lg:col-span-2 space-y-6">
            
            <!-- Filter Controls Panel -->
            <div class="bg-white p-5 rounded-xl shadow-sm border border-slate-100">
                <h2 class="text-sm font-semibold text-slate-500 uppercase tracking-wider mb-4 flex items-center gap-2">
                    <span>🔍</span> Filter Schedule
                </h2>
                <div class="grid grid-cols-1 sm:grid-cols-3 gap-4">
                    <div>
                        <label class="block text-xs font-medium text-slate-600 mb-1">Search Team</label>
                        <select id="teamFilter" class="w-full text-sm bg-slate-50 border border-slate-200 rounded-lg p-2.5 focus:ring-2 focus:ring-indigo-500 focus:outline-none">
                            <option value="all">All Teams</option>
                            <option value="Team Ali">Team Ali</option>
                            <option value="Team Emad">Team Emad</option>
                            <option value="Team Yasir">Team Yasir</option>
                            <option value="Team Yusuf">Team Yusuf</option>
                            <option value="Team Zeshan">Team Zeshan</option>
                            <option value="Team Zohaid">Team Zohaid</option>
                        </select>
                    </div>
                    <div>
                        <label class="block text-xs font-medium text-slate-600 mb-1">Game Date</label>
                        <select id="dateFilter" class="w-full text-sm bg-slate-50 border border-slate-200 rounded-lg p-2.5 focus:ring-2 focus:ring-indigo-500 focus:outline-none">
                            <option value="all">All Dates</option>
                            <option value="10/1/2026">Oct 1, 2026</option>
                            <option value="10/4/2026">Oct 4, 2026</option>
                            <option value="10/8/2026">Oct 8, 2026</option>
                            <option value="10/15/2026">Oct 15, 2026</option>
                        </select>
                    </div>
                    <div>
                        <label class="block text-xs font-medium text-slate-600 mb-1">Diamond / Venue</label>
                        <select id="diamondFilter" class="w-full text-sm bg-slate-50 border border-slate-200 rounded-lg p-2.5 focus:ring-2 focus:ring-indigo-500 focus:outline-none">
                            <option value="all">All Fields</option>
                            <option value="Dunton">Dunton Fields (2, 3, 4)</option>
                            <option value="CAA">CAA Complex (Red, Yellow, Green)</option>
                        </select>
                    </div>
                </div>
                <div class="mt-4 flex justify-between items-center pt-2 border-t border-slate-100 text-xs text-slate-400">
                    <span id="matchCount">Showing 24 of 24 games</span>
                    <button id="resetBtn" class="text-indigo-600 hover:text-indigo-800 font-medium cursor-pointer">Reset Filters</button>
                </div>
            </div>

            <!-- Schedule Container -->
            <div class="bg-white rounded-xl shadow-sm border border-slate-100 overflow-hidden">
                <div class="overflow-x-auto">
                    <table class="w-full text-left border-collapse">
                        <thead>
                            <tr class="bg-slate-50 text-slate-500 font-semibold text-xs uppercase tracking-wider border-b border-slate-100">
                                <th class="py-3.5 px-5">Date</th>
                                <th class="py-3.5 px-4">Time</th>
                                <th class="py-3.5 px-4">Diamond</th>
                                <th class="py-3.5 px-4 text-right">Home</th>
                                <th class="py-3.5 px-2 text-center text-slate-300">vs</th>
                                <th class="py-3.5 px-4 text-left">Away</th>
                            </tr>
                        </thead>
                        <tbody id="scheduleBody" class="divide-y divide-slate-100 text-sm text-slate-700">
                            <!-- Dynamic rows injected by JS -->
                        </tbody>
                    </table>
                </div>
                <!-- Empty State -->
                <div id="noGamesRow" class="hidden py-12 text-center text-slate-400 text-sm">
                    ⚠️ No games match your selected filters.
                </div>
            </div>
        </div>

        <!-- Right 1 Column: Interactive Venue Maps -->
        <div class="space-y-6">
            <div class="bg-white p-5 rounded-xl shadow-sm border border-slate-100 space-y-4">
                <h2 class="text-sm font-semibold text-slate-500 uppercase tracking-wider flex items-center gap-2">
                    <span>📍</span> Interactive Maps & Venues
                </h2>
                <p class="text-xs text-slate-500 leading-relaxed">
                    Click the selector below to switch interactive satellite map frames between the two league park venues.
                </p>

                <!-- Map Switcher Tabs -->
                <div class="flex rounded-lg bg-slate-100 p-1 text-xs font-medium">
                    <button id="tabDunton" class="flex-1 py-2 text-center rounded-md cursor-pointer transition bg-white text-indigo-950 shadow-sm" onclick="switchMap('dunton')">
                        Dunton Fields
                    </button>
                    <button id="tabCAA" class="flex-1 py-2 text-center rounded-md cursor-pointer transition text-slate-600 hover:text-slate-900" onclick="switchMap('caa')">
                        CAA Centre
                    </button>
                </div>

                <!-- Live Embedding Embed Boxes -->
                <div class="relative w-full h-64 bg-slate-100 rounded-lg overflow-hidden border border-slate-200">
                    <!-- Dunton Map -->
                    <iframe id="mapDunton" class="w-full h-full border-0 absolute top-0 left-0 transition-opacity duration-300 opacity-100" 
                        src="https://www.google.com/maps/embed?pb=!1m18!1m12!1m3!1d2888.647710323384!2d-79.6738981!3d43.6138763!2m3!1f0!2f0!3f0!3m2!1i1024!2i768!4f13.1!3m3!1m2!1s0x882b40bd42eb8331%3A0x71fc99430c44ec8!2sDunton%20Athletic%20Fields!5e0!3m2!1sen!2sca!4v1700000000000!5m2!1sen!2sca" 
                        allowfullscreen="" loading="lazy" referrerpolicy="no-referrer-when-downgrade">
                    </iframe>
                    <!-- CAA Map -->
                    <iframe id="mapCAA" class="w-full h-full border-0 absolute top-0 left-0 transition-opacity duration-300 opacity-0 pointer-events-none" 
                        src="https://www.google.com/maps/embed?pb=!1m18!1m12!1m3!1d2886.537233261642!2d-79.7042583!3d43.6577884!2m3!1f0!2f0!3f0!3m2!1i1024!2i768!4f13.1!3m3!1m2!1s0x882b3f7f893e1b7b%3A0xcfeaa4d2e8251efa!2sCAA%20Centre!5e0!3m2!1sen!2sca!4v1700000000001!5m2!1sen!2sca" 
                        allowfullscreen="" loading="lazy" referrerpolicy="no-referrer-when-downgrade">
                    </iframe>
                </div>

                <!-- Info Snippets Dynamic Footer -->
                <div id="infoDunton" class="bg-slate-50 p-3.5 rounded-lg border border-slate-200/60 space-y-1">
                    <h3 class="text-xs font-bold text-slate-700">Dunton Athletic Fields</h3>
                    <p class="text-[11px] text-slate-500 leading-tight">6180 Kennedy Rd, Mississauga, ON L5T 2Z1</p>
                    <p class="text-[11px] font-medium text-indigo-600 pt-1">Fields used: Dunton 2, Dunton 3, Dunton 4</p>
                </div>
                <div id="infoCAA" class="hidden bg-slate-50 p-3.5 rounded-lg border border-slate-200/60 space-y-1">
                    <h3 class="text-xs font-bold text-slate-700">CAA Centre Sports Complex</h3>
                    <p class="text-[11px] text-slate-500 leading-tight">7575 Kennedy Rd S, Brampton, ON L6W 4T2</p>
                    <p class="text-[11px] font-medium text-emerald-600 pt-1">Fields used: CAA Red, CAA Yellow, CAA Green</p>
                </div>
            </div>
        </div>
    </main>

    <footer class="bg-white border-t border-slate-200 text-center py-6 mt-12 text-xs text-slate-400">
        <p>© 2026 League Schedule Website. Generated for simple public distribution.</p>
    </footer>

    <!-- Logic Scripting for Dynamic Filtering and Map Tabs -->
    <script>
        // Data Structure Matrix Array
        const rawGames = [
            { date: "10/1/2026", time: "7:00 PM", diamond: "Dunton 2", home: "Team Zeshan", away: "Team Ali" },
            { date: "10/1/2026", time: "7:00 PM", diamond: "Dunton 3", home: "Team Yasir", away: "Team Yusuf" },
            { date: "10/1/2026", time: "7:00 PM", diamond: "Dunton 4", home: "Team Emad", away: "Team Zohaid" },
            { date: "10/1/2026", time: "8:30 PM", diamond: "Dunton 2", home: "Team Ali", away: "Team Emad" },
            { date: "10/1/2026", time: "8:30 PM", diamond: "Dunton 3", home: "Team Yusuf", away: "Team Zeshan" },
            { date: "10/1/2026", time: "8:30 PM", diamond: "Dunton 4", home: "Team Zohaid", away: "Team Yasir" },
            { date: "10/4/2026", time: "7:00 PM", diamond: "CAA Red", home: "Team Ali", away: "Team Yusuf" },
            { date: "10/4/2026", time: "7:00 PM", diamond: "CAA Yellow", home: "Team Yasir", away: "Team Emad" },
            { date: "10/4/2026", time: "7:00 PM", diamond: "CAA Green", home: "Team Zohaid", away: "Team Zeshan" },
            { date: "10/4/2026", time: "8:30 PM", diamond: "CAA Red", home: "Team Yusuf", away: "Team Zohaid" },
            { date: "10/4/2026", time: "8:30 PM", diamond: "CAA Yellow", home: "Team Emad", away: "Team Ali" },
            { date: "10/4/2026", time: "8:30 PM", diamond: "CAA Green", home: "Team Zeshan", away: "Team Yasir" },
            { date: "10/8/2026", time: "7:00 PM", diamond: "CAA Red", home: "Team Yasir", away: "Team Ali" },
            { date: "10/8/2026", time: "7:00 PM", diamond: "CAA Yellow", home: "Team Zohaid", away: "Team Yusuf" },
            { date: "10/8/2026", time: "7:00 PM", diamond: "CAA Green", home: "Team Emad", away: "Team Zeshan" },
            { date: "10/8/2026", time: "8:30 PM", diamond: "CAA Red", home: "Team Zeshan", away: "Team Yasir" },
            { date: "10/8/2026", time: "8:30 PM", diamond: "CAA Yellow", home: "Team Ali", away: "Team Zohaid" },
            { date: "10/8/2026", time: "8:30 PM", diamond: "CAA Green", home: "Team Yusuf", away: "Team Emad" },
            { date: "10/15/2026", time: "7:00 PM", diamond: "CAA Red", home: "Team Yasir", away: "Team Yusuf" },
            { date: "10/15/2026", time: "7:00 PM", diamond: "CAA Yellow", home: "Team Zeshan", away: "Team Emad" },
            { date: "10/15/2026", time: "7:00 PM", diamond: "CAA Green", home: "Team Ali", away: "Team Zohaid" },
            { date: "10/15/2026", time: "8:30 PM", diamond: "CAA Red", home: "Team Emad", away: "Team Yasir" },
            { date: "10/15/2026", time: "8:30 PM", diamond: "CAA Yellow", home: "Team Zohaid", away: "Team Zeshan" },
            { date: "10/15/2026", time: "8:30 PM", diamond: "CAA Green", home: "Team Yusuf", away: "Team Ali" }
        ];

        // Element Selectors
        const teamFilter = document.getElementById('teamFilter');
        const dateFilter = document.getElementById('dateFilter');
        const diamondFilter = document.getElementById('diamondFilter');
        const scheduleBody = document.getElementById('scheduleBody');
        const noGamesRow = document.getElementById('noGamesRow');
        const matchCount = document.getElementById('matchCount');
        const resetBtn = document.getElementById('resetBtn');

        // Render Table Function
        function renderSchedule() {
            const teamVal = teamFilter.value;
            const dateVal = dateFilter.value;
            const diamondVal = diamondFilter.value;

            let filtered = rawGames.filter(game => {
                const matchesTeam = teamVal === 'all' || game.home === teamVal || game.away === teamVal;
                const matchesDate = dateVal === 'all' || game.date === dateVal;
                const matchesDiamond = diamondVal === 'all' || game.diamond.includes(diamondVal);
                return matchesTeam && matchesDate && matchesDiamond;
            });

            // Clean previous rows
            scheduleBody.innerHTML = '';
            
            if (filtered.length === 0) {
                noGamesRow.classList.remove('hidden');
                matchCount.textContent = "Showing 0 games";
                return;
            }
            
            noGamesRow.classList.add('hidden');
            matchCount.textContent = `Showing ${filtered.length} of ${rawGames.length} games`;

            filtered.forEach(game => {
                const tr = document.createElement('tr');
                tr.className = "hover:bg-slate-50 transition-colors";
                
                // Color coding tags for diamonds
                const tagColor = game.diamond.includes('Dunton') 
                    ? 'bg-indigo-50 text-indigo-700 border-indigo-100' 
                    : 'bg-emerald-50 text-emerald-700 border-emerald-100';

                tr.innerHTML = `
                    <td class="py-3.5 px-5 font-medium whitespace-nowrap text-slate-500">${game.date}</td>
                    <td class="py-3.5 px-4 text-slate-600">${game.time}</td>
                    <td class="py-3.5 px-4">
                        <span class="px-2 py-0.5 border text-xs font-medium rounded-full ${tagColor}">${game.diamond}</span>
                    </td>
                    <td class="py-3.5 px-4 text-right font-semibold text-slate-900">${game.home}</td>
                    <td class="py-3.5 px-2 text-center text-slate-400 text-xs font-normal">vs</td>
                    <td class="py-3.5 px-4 text-left font-semibold text-slate-900">${game.away}</td>
                `;
                scheduleBody.appendChild(tr);
            });
        }

        // Map Switcher Mechanism
        function switchMap(venue) {
            const tabDunton = document.getElementById('tabDunton');
            const tabCAA = document.getElementById('tabCAA');
            const mapDunton = document.getElementById('mapDunton');
            const mapCAA = document.getElementById('mapCAA');
            const infoDunton = document.getElementById('infoDunton');
            const infoCAA = document.getElementById('infoCAA');

            if (venue === 'dunton') {
                tabDunton.className = "flex-1 py-2 text-center rounded-md cursor-pointer transition bg-white text-indigo-950 shadow-sm";
                tabCAA.className = "flex-1 py-2 text-center rounded-md cursor-pointer transition text-slate-600 hover:text-slate-900";
                
                mapDunton.classList.replace('opacity-0', 'opacity-100');
                mapDunton.classList.remove('pointer-events-none');
                mapCAA.classList.replace('opacity-100', 'opacity-0');
                mapCAA.classList.add('pointer-events-none');
                
                infoDunton.classList.remove('hidden');
                infoCAA.classList.add('hidden');
            } else {
                tabCAA.className = "flex-1 py-2 text-center rounded-md cursor-pointer transition bg-white text-indigo-950 shadow-sm";
                tabDunton.className = "flex-1 py-2 text-center rounded-md cursor-pointer transition text-slate-600 hover:text-slate-900";
                
                mapCAA.classList.replace('opacity-0', 'opacity-100');
                mapCAA.classList.remove('pointer-events-none');
                mapDunton.classList.replace('opacity-100', 'opacity-0');
                mapDunton.classList.add('pointer-events-none');
                
                infoCAA.classList.remove('hidden');
                infoDunton.classList.add('hidden');
            }
        }

        // Event Hookups
        teamFilter.addEventListener('change', renderSchedule);
        dateFilter.addEventListener('change', renderSchedule);
        diamondFilter.addEventListener('change', renderSchedule);
        
        resetBtn.addEventListener('click', () => {
            teamFilter.value = 'all';
            dateFilter.value = 'all';
            diamondFilter.value = 'all';
            renderSchedule();
        });

        // Initialize Call
        renderSchedule();
    </script>
</body>
</html>
