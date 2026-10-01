<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>League Dashboard & Schedule</title>
    <!-- Tailwind CSS -->
    <script src="https://cdn.jsdelivr.net/npm/@tailwindcss/browser@4"></script>
    <style>
        .active-tab {
            border-bottom: 2px solid #2563eb;
            color: #2563eb;
        }
    </style>
</head>
<body class="bg-gray-50 font-sans text-gray-900 antialiased">

    <!-- Header / Banner -->
    <header class="bg-gradient-to-r from-blue-700 to-indigo-800 text-white shadow-md">
        <div class="max-w-7xl mx-auto px-4 py-6 sm:px-6 lg:px-8 flex flex-col sm:flex-row justify-between items-center gap-4">
            <div>
                <h1 class="text-3xl font-extrabold tracking-tight">League Schedule & Standings</h1>
                <p class="text-blue-100 text-sm mt-1">Official Game Center • Fall 2026</p>
            </div>
            <div class="bg-blue-900/50 px-4 py-2 rounded-lg border border-blue-500/30 text-center sm:text-right">
                <span class="text-xs text-blue-200 block uppercase font-bold tracking-wider">Point System</span>
                <span class="text-sm font-semibold">Win: 2 PTS • Tie: 1 PT • Loss: 0 PTS</span>
            </div>
        </div>
    </header>

    <main class="max-w-7xl mx-auto px-4 py-8 sm:px-6 lg:px-8">
        
        <!-- Grid Layout for Live Standings & Quick Stats -->
        <div class="grid grid-cols-1 lg:grid-cols-3 gap-8 mb-8">
            
            <!-- Standings Table (Takes up 2 columns on large screens) -->
            <div class="lg:col-span-2 bg-white rounded-xl shadow-sm border border-gray-200 p-6">
                <div class="flex items-center justify-between mb-4">
                    <h2 class="text-xl font-bold text-gray-800 flex items-center gap-2">
                        🏆 Live Standings
                    </h2>
                    <span class="text-xs text-gray-500 italic">Auto-calculated from match scores</span>
                </div>
                <div class="overflow-x-auto">
                    <table class="min-w-full divide-y divide-gray-200">
                        <thead class="bg-gray-50">
                            <tr>
                                <th class="px-4 py-3 text-left text-xs font-semibold text-gray-600 uppercase tracking-wider">Pos</th>
                                <th class="px-4 py-3 text-left text-xs font-semibold text-gray-600 uppercase tracking-wider">Team</th>
                                <th class="px-3 py-3 text-center text-xs font-semibold text-gray-600 uppercase tracking-wider">GP</th>
                                <th class="px-3 py-3 text-center text-xs font-semibold text-gray-600 uppercase tracking-wider">W</th>
                                <th class="px-3 py-3 text-center text-xs font-semibold text-gray-600 uppercase tracking-wider">T</th>
                                <th class="px-3 py-3 text-center text-xs font-semibold text-gray-600 uppercase tracking-wider">L</th>
                                <th class="px-3 py-3 text-center text-xs font-semibold text-gray-600 uppercase tracking-wider">RS</th>
                                <th class="px-3 py-3 text-center text-xs font-semibold text-gray-600 uppercase tracking-wider">RA</th>
                                <th class="px-3 py-3 text-center text-xs font-semibold text-gray-600 uppercase tracking-wider">Diff</th>
                                <th class="px-4 py-3 text-center text-xs font-bold text-blue-600 uppercase tracking-wider bg-blue-50/50">PTS</th>
                            </tr>
                        </thead>
                        <tbody id="standings-body" class="bg-white divide-y divide-gray-200 text-sm">
                            <!-- Injected dynamically via JS -->
                        </tbody>
                    </table>
                </div>
            </div>

            <!-- Venue Maps Module -->
            <div class="bg-white rounded-xl shadow-sm border border-gray-200 p-6 flex flex-col">
                <h2 class="text-xl font-bold text-gray-800 mb-4 flex items-center gap-2">
                    📍 Field Maps & Venues
                </h2>
                
                <!-- Venue Toggles -->
                <div class="flex bg-gray-100 p-1 rounded-lg mb-4 text-xs font-medium">
                    <button id="btn-dunton" onclick="switchMap('dunton')" class="flex-1 py-2 text-center rounded-md bg-white shadow-xs text-blue-700 font-bold">
                        Dunton Fields
                    </button>
                    <button id="btn-caa" onclick="switchMap('caa')" class="flex-1 py-2 text-center rounded-md text-gray-600 hover:text-gray-900">
                        CAA Centre
                    </button>
                </div>

                <!-- Interactive Map Frame Containers -->
                <div class="flex-1 min-h-[220px] bg-gray-100 rounded-lg overflow-hidden border border-gray-200 relative mb-3">
                    <iframe id="map-frame" class="w-full h-full border-0" src="https://www.google.com/maps/embed?pb=!1m18!1m12!1m3!1d2888.745482390772!2d-79.67069172341999!3d43.6118318554284!2m3!1f0!2f0!3f0!3m2!1i1024!2i766!4f13.1!3m3!1m2!1s0x882b40673d32cb39%3A0x721db5976b92ff8!2sDunton%20Athletic%20Fields!5e0!3m2!1sen!2sca!4v1710000000000!5m2!1sen!2sca" allowfullscreen="" loading="lazy" referrerpolicy="no-referrer-when-downgrade"></iframe>
                </div>

                <div id="venue-info" class="text-xs text-gray-600 space-y-1">
                    <p class="font-bold text-gray-800 text-sm" id="venue-title">Dunton Athletic Fields</p>
                    <p id="venue-addr">6180 Kennedy Rd, Mississauga, ON L5T 2Z1</p>
                    <p id="venue-fields" class="text-blue-600 font-medium mt-1">Diamonds: Dunton 2, Dunton 3, Dunton 4</p>
                </div>
            </div>
        </div>

        <!-- Schedule Module Section -->
        <div class="bg-white rounded-xl shadow-sm border border-gray-200 p-6">
            <div class="border-b border-gray-200 pb-4 mb-6 flex flex-col md:flex-row md:items-center md:justify-between gap-4">
                <div>
                    <h2 class="text-2xl font-bold text-gray-800">Game Schedule</h2>
                    <p class="text-gray-500 text-sm">Filter, search, or update match scorecards below</p>
                </div>
                
                <!-- Filter Tool Groupings -->
                <div class="flex flex-wrap items-center gap-3">
                    <div>
                        <label class="block text-xs font-bold text-gray-500 uppercase mb-1">Search Team</label>
                        <input type="text" id="search-team" oninput="filterGames()" placeholder="e.g. Yasir" class="border border-gray-300 rounded-lg px-3 py-1.5 text-sm w-40 focus:outline-none focus:ring-2 focus:ring-blue-500">
                    </div>
                    <div>
                        <label class="block text-xs font-bold text-gray-500 uppercase mb-1">Date Filter</label>
                        <select id="filter-date" onchange="filterGames()" class="border border-gray-300 rounded-lg px-3 py-1.5 text-sm bg-white focus:outline-none focus:ring-2 focus:ring-blue-500">
                            <option value="">All Dates</option>
                            <option value="10/1/2026">Oct 1, 2026</option>
                            <option value="10/4/2026">Oct 4, 2026</option>
                            <option value="10/8/2026">Oct 8, 2026</option>
                            <option value="10/15/2026">Oct 15, 2026</option>
                        </select>
                    </div>
                    <div>
                        <label class="block text-xs font-bold text-gray-500 uppercase mb-1">Diamond</label>
                        <select id="filter-diamond" onchange="filterGames()" class="border border-gray-300 rounded-lg px-3 py-1.5 text-sm bg-white focus:outline-none focus:ring-2 focus:ring-blue-500">
                            <option value="">All Fields</option>
                            <option value="Dunton">Dunton Fields</option>
                            <option value="CAA">CAA Fields</option>
                        </select>
                    </div>
                </div>
            </div>

            <!-- Schedule Cards Grid Content (Mobile friendly layout fallback) -->
            <div id="schedule-container" class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4">
                <!-- Data injected dynamically via JS -->
            </div>
        </div>
    </main>

    <footer class="bg-gray-800 text-gray-400 text-center py-6 mt-12 border-t border-gray-700 text-xs">
        <p>© 2026 Sports Schedule Hub. Hosted on GitHub Pages.</p>
    </footer>

    <!-- Main Application Javascript & Schedules Setup -->
    <script>
        // LEAGUE DATA STORAGE ARRAY
        // EDIT SCORES HERE: Replace null with numbers (e.g., homeScore: 12, awayScore: 7)
        const games = [
            { id: 1, date: "10/1/2026", time: "7:00 PM", diamond: "Dunton 2", home: "Team Zeshan", away: "Team Ali", homeScore: null, awayScore: null },
            { id: 2, date: "10/1/2026", time: "7:00 PM", diamond: "Dunton 3", home: "Team Yasir", away: "Team Yusuf", homeScore: null, awayScore: null },
            { id: 3, date: "10/1/2026", time: "7:00 PM", diamond: "Dunton 4", home: "Team Emad", away: "Team Zohaid", homeScore: null, awayScore: null },
            { id: 4, date: "10/1/2026", time: "8:30 PM", diamond: "Dunton 2", home: "Team Ali", away: "Team Emad", homeScore: null, awayScore: null },
            { id: 5, date: "10/1/2026", time: "8:30 PM", diamond: "Dunton 3", home: "Team Yusuf", away: "Team Zeshan", homeScore: null, awayScore: null },
            { id: 6, date: "10/1/2026", time: "8:30 PM", diamond: "Dunton 4", home: "Team Zohaid", away: "Team Yasir", homeScore: null, awayScore: null },
            
            { id: 7, date: "10/4/2026", time: "7:00 PM", diamond: "CAA Red", home: "Team Ali", away: "Team Yusuf", homeScore: null, awayScore: null },
            { id: 8, date: "10/4/2026", time: "7:00 PM", diamond: "CAA Yellow", home: "Team Yasir", away: "Team Emad", homeScore: null, awayScore: null },
            { id: 9, date: "10/4/2026", time: "7:00 PM", diamond: "CAA Green", home: "Team Zohaid", away: "Team Zeshan", homeScore: null, awayScore: null },
            { id: 10, date: "10/4/2026", time: "8:30 PM", diamond: "CAA Red", home: "Team Yusuf", away: "Team Zohaid", homeScore: null, awayScore: null },
            { id: 11, date: "10/4/2026", time: "8:30 PM", diamond: "CAA Yellow", home: "Team Emad", away: "Team Ali", homeScore: null, awayScore: null },
            { id: 12, date: "10/4/2026", time: "8:30 PM", diamond: "CAA Green", home: "Team Zeshan", away: "Team Yasir", homeScore: null, awayScore: null },
            
            { id: 13, date: "10/8/2026", time: "7:00 PM", diamond: "CAA Red", home: "Team Yasir", away: "Team Ali", homeScore: null, awayScore: null },
            { id: 14, date: "10/8/2026", time: "7:00 PM", diamond: "CAA Yellow", home: "Team Zohaid", away: "Team Yusuf", homeScore: null, awayScore: null },
            { id: 15, date: "10/8/2026", time: "7:00 PM", diamond: "CAA Green", home: "Team Emad", away: "Team Zeshan", homeScore: null, awayScore: null },
            { id: 16, date: "10/8/2026", time: "8:30 PM", diamond: "CAA Red", home: "Team Zeshan", away: "Team Yasir", homeScore: null, awayScore: null },
            { id: 17, date: "10/8/2026", time: "8:30 PM", diamond: "CAA Yellow", home: "Team Ali", away: "Team Zohaid", homeScore: null, awayScore: null },
            { id: 18, date: "10/8/2026", time: "8:30 PM", diamond: "CAA Green", home: "Team Yusuf", away: "Team Emad", homeScore: null, awayScore: null },
            
            { id: 19, date: "10/15/2026", time: "7:00 PM", diamond: "CAA Red", home: "Team Yasir", away: "Team Yusuf", homeScore: null, awayScore: null },
            { id: 20, date: "10/15/2026", time: "7:00 PM", diamond: "CAA Yellow", home: "Team Zeshan", away: "Team Emad", homeScore: null, awayScore: null },
            { id: 21, date: "10/15/2026", time: "7:00 PM", diamond: "CAA Green", home: "Team Ali", away: "Team Zohaid", homeScore: null, awayScore: null },
            { id: 22, date: "10/15/2026", time: "8:30 PM", diamond: "CAA Red", home: "Team Emad", away: "Team Yasir", homeScore: null, awayScore: null },
            { id: 23, date: "10/15/2026", time: "8:30 PM", diamond: "CAA Yellow", home: "Team Zohaid", away: "Team Zeshan", homeScore: null, awayScore: null },
            { id: 24, date: "10/15/2026", time: "8:30 PM", diamond: "CAA Green", home: "Team Yusuf", away: "Team Ali", homeScore: null, awayScore: null }
        ];

        // MAP DIRECTORY COORDINATES INFRASTRUCTURE
        const mapData = {
            dunton: {
                title: "Dunton Athletic Fields",
                addr: "6180 Kennedy Rd, Mississauga, ON L5T 2Z1",
                fields: "Diamonds: Dunton 2, Dunton 3, Dunton 4",
                src: "https://www.google.com/maps/embed?pb=!1m18!1m12!1m3!1d2888.745482390772!2d-79.67069172341999!3d43.6118318554284!2m3!1f0!2f0!3f0!3m2!1i1024!2i766!4f13.1!3m3!1m2!1s0x882b40673d32cb39%3A0x721db5976b92ff8!2sDunton%20Athletic%20Fields!5e0!3m2!1sen!2sca!4v1710000000000!5m2!1sen!2sca"
            },
            caa: {
                title: "CAA Centre Sports Complex",
                addr: "7575 Kennedy Rd S, Brampton, ON L6W 4T2",
                fields: "Diamonds: CAA Red, CAA Yellow, CAA Green",
                src: "https://www.google.com/maps/embed?pb=!1m18!1m12!1m3!1d2886.7266184988755!2d-79.71960242341775!3d43.65385645271813!2m3!1f0!2f0!3f0!3m2!1i1024!2i766!4f13.1!3m3!1m2!1s0x882b3f7f02377b5d%3A0xcfdacdc6bc8cf3fa!2sCAA%20Centre!5e0!3m2!1sen!2sca!4v1710000000000!5m2!1sen!2sca"
            }
        };

        function switchMap(venueKey) {
            const data = mapData[venueKey];
            document.getElementById('map-frame').src = data.src;
            document.getElementById('venue-title').innerText = data.title;
            document.getElementById('venue-addr').innerText = data.addr;
            document.getElementById('venue-fields').innerText = data.fields;

            // Manage CSS Active classes
            const isDunton = venueKey === 'dunton';
            document.getElementById('btn-dunton').className = isDunton ? "flex-1 py-2 text-center rounded-md bg-white shadow-xs text-blue-700 font-bold" : "flex-1 py-2 text-center rounded-md text-gray-600 hover:text-gray-900";
            document.getElementById('btn-caa').className = !isDunton ? "flex-1 py-2 text-center rounded-md bg-white shadow-xs text-blue-700 font-bold" : "flex-1 py-2 text-center rounded-md text-gray-600 hover:text-gray-900";
        }

        // STANDINGS AUTOMATED COMPILER ENGINE
        function calculateStandings() {
            const standings = {};

            // Initialize all unique teams dynamically
            games.forEach(g => {
                [g.home, g.away].forEach(team => {
                    if (!standings[team]) {
                        standings[team] = { name: team, gp: 0, w: 0, t: 0, l: 0, rs: 0, ra: 0, diff: 0, pts: 0 };
                    }
                });
            });

            // Loop and add points metrics where data scores exist
            games.forEach(g => {
                if (g.homeScore !== null && g.awayScore !== null) {
                    const hs = parseInt(g.homeScore);
                    const as = parseInt(g.awayScore);

                    standings[g.home].gp++;
                    standings[g.away].gp++;
                    standings[g.home].rs += hs;
                    standings[g.home].ra += as;
                    standings[g.away].rs += as;
                    standings[g.away].ra += hs;

                    if (hs > as) {
                        standings[g.home].w++;
                        standings[g.home].pts += 2; // 2 points for a win
                        standings[g.away].l++;
                    } else if (as > hs) {
                        standings[g.away].w++;
                        standings[g.away].pts += 2;
                        standings[g.home].l++;
                    } else {
                        standings[g.home].t++;
                        standings[g.away].t++;
                        standings[g.home].pts += 1; // 1 point for a tie
                        standings[g.away].pts += 1;
                    }
                }
            });

            // Calculate run diff metrics
            Object.values(standings).forEach(t => t.diff = t.rs - t.ra);

            // Sort standard sports criteria array (PTS -> Diff -> RS)
            return Object.values(standings).sort((a, b) => {
                if (b.pts !== a.pts) return b.pts - a.pts;
                if (b.diff !== a.diff) return b.diff - a.diff;
                return b.rs - a.rs;
            });
        }

        function renderStandings() {
            const sortedData = calculateStandings();
            const tbody = document.getElementById('standings-body');
            tbody.innerHTML = '';

            sortedData.forEach((team, index) => {
                const tr = document.createElement('tr');
                tr.className = index % 2 === 0 ? "bg-white" : "bg-gray-50/50";
                tr.innerHTML = `
                    <td class="px-4 py-3 font-bold text-gray-500">${index + 1}</td>
                    <td class="px-4 py-3 font-semibold text-gray-900">${team.name}</td>
                    <td class="px-3 py-3 text-center text-gray-600">${team.gp}</td>
                    <td class="px-3 py-3 text-center text-emerald-600 font-medium">${team.w}</td>
                    <td class="px-3 py-3 text-center text-amber-600 font-medium">${team.t}</td>
                    <td class="px-3 py-3 text-center text-red-600 font-medium">${team.l}</td>
                    <td class="px-3 py-3 text-center text-gray-600">${team.rs}</td>
                    <td class="px-3 py-3 text-center text-gray-600">${team.ra}</td>
                    <td class="px-3 py-3 text-center font-medium ${team.diff >= 0 ? 'text-gray-700' : 'text-red-500'}">${team.diff > 0 ? '+' + team.diff : team.diff}</td>
                    <td class="px-4 py-3 text-center font-bold text-blue-600 bg-blue-50/30">${team.pts}</td>
                `;
                tbody.appendChild(tr);
            });
        }

        // DYNAMIC SCHEDULE RENDERING & FILTER LOGIC
        function filterGames() {
            const searchValue = document.getElementById('search-team').value.toLowerCase();
            const dateValue = document.getElementById('filter-date').value;
            const diamondValue = document.getElementById('filter-diamond').value;
            const container = document.getElementById('schedule-container');
            container.innerHTML = '';

            const filtered = games.filter(g => {
                const matchesSearch = g.home.toLowerCase().includes(searchValue) || g.away.toLowerCase().includes(searchValue);
                const matchesDate = !dateValue || g.date === dateValue;
                const matchesDiamond = !diamondValue || g.diamond.startsWith(diamondValue);
                return matchesSearch && matchesDate && matchesDiamond;
            });

            if (filtered.length === 0) {
                container.innerHTML = `<div class="col-span-full text-center py-8 text-gray-400 bg-gray-50 rounded-lg border border-dashed border-gray-300">No scheduled matches match your criteria filters.</div>`;
                return;
            }

            filtered.forEach(g => {
                const isPlayed = g.homeScore !== null && g.awayScore !== null;
                let scorecardMarkup = '';

                if (isPlayed) {
                    const homeWinner = parseInt(g.homeScore) > parseInt(g.awayScore);
                    const awayWinner = parseInt(g.awayScore) > parseInt(g.homeScore);
                    scorecardMarkup = `
                        <div class="flex items-center justify-between border-t border-b border-gray-100 py-3 my-2 text-base font-bold">
                            <span class="${homeWinner ? 'text-emerald-600 font-extrabold' : 'text-gray-700'}">${g.homeScore}</span>
                            <span class="text-xs uppercase bg-gray-100 text-gray-500 px-2 py-0.5 rounded font-semibold tracking-wider">Final</span>
                            <span class="${awayWinner ? 'text-emerald-600 font-extrabold' : 'text-gray-700'}">${g.awayScore}</span>
                        </div>
                    `;
                } else {
                    scorecardMarkup = `
                        <div class="flex items-center justify-center border-t border-b border-gray-100 py-2.5 my-2 text-xs font-bold text-blue-600">
                            <span class="bg-blue-50 px-3 py-1 rounded-full uppercase tracking-widest border border-blue-100">vs</span>
                        </div>
                    `;
                }

                const card = document.createElement('div');
                card.className = "bg-white p-5 rounded-xl border border-gray-200 shadow-2xs hover:shadow-xs transition flex flex-col justify-between";
                card.innerHTML = `
                    <div>
                        <div class="flex justify-between items-start text-xs font-semibold text-gray-400 mb-2">
                            <span class="bg-gray-100 px-2 py-0.5 rounded-sm text-gray-600">${g.date} • ${g.time}</span>
                            <span class="text-indigo-600">${g.diamond}</span>
                        </div>
                        <div class="flex justify-between items-center text-sm font-semibold py-1">
                            <span class="text-gray-900">${g.home}</span>
                            <span class="text-xs text-gray-400 uppercase font-normal">Home</span>
                        </div>
                        ${scorecardMarkup}
                        <div class="flex justify-between items-center text-sm font-semibold py-1">
                            <span class="text-gray-900">${g.away}</span>
                            <span class="text-xs text-gray-400 uppercase font-normal">Away</span>
                        </div>
                    </div>
                `;
                container.appendChild(card);
            });
        }

        // INITIAL LOAD INITIALIZATION
        window.onload = function() {
            renderStandings();
            filterGames();
        };
    </script>
</body>
</html>
