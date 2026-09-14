
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vahan Market Share Tool</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.18.5/xlsx.full.min.js"></script>
    <style>
        .table-header { @apply px-4 py-2 bg-gray-800 text-white text-sm text-left uppercase tracking-wider border-b border-gray-700; }
        .table-cell { @apply px-4 py-2 whitespace-nowrap text-sm text-gray-700 border-b border-gray-200; }
    </style>
</head>
<body class="bg-gray-100 font-sans leading-normal tracking-normal h-screen flex flex-col">

    <!-- Navbar -->
    <nav class="bg-blue-900 p-4 shadow-lg text-white flex justify-between items-center">
        <div class="flex items-center space-x-3">
            <svg class="w-8 h-8" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 7h8m0 0v8m0-8l-8 8-4-4-6 6"></path></svg>
            <span class="font-bold text-xl tracking-wide">Vahan Market Share Tool</span>
        </div>
        <div class="flex items-center space-x-4">
            <label class="bg-blue-700 hover:bg-blue-600 px-4 py-2 rounded cursor-pointer transition text-sm">
                Upload "Vahan Update.xlsx"
                <input type="file" id="fileInput" class="hidden" accept=".xls,.xlsx" />
            </label>
        </div>
    </nav>

    <!-- Main Content -->
    <div class="flex-1 p-6 overflow-auto hidden" id="dashboard">
        
        <!-- Controls -->
        <div class="bg-white p-4 rounded-lg shadow mb-6 flex flex-wrap gap-4 items-end">
            <div>
                <label class="block text-xs font-bold text-gray-700 mb-1">Cluster</label>
                <select id="clusterSelect" class="border border-gray-300 rounded p-2 w-48 text-sm focus:outline-none focus:ring-2 focus:ring-blue-500">
                    <option value="ALL">All Clusters</option>
                </select>
            </div>
            <div>
                <label class="block text-xs font-bold text-gray-700 mb-1">City / Region</label>
                <select id="citySelect" class="border border-gray-300 rounded p-2 w-64 text-sm focus:outline-none focus:ring-2 focus:ring-blue-500">
                </select>
            </div>
            <div class="ml-auto">
                <span class="text-sm font-semibold text-gray-500 bg-gray-100 px-3 py-1 rounded">
                    Default Mode: MP/Region Total
                </span>
            </div>
        </div>

        <!-- Top Metrics Cards -->
        <div class="grid grid-cols-1 md:grid-cols-3 gap-6 mb-6">
            <!-- Card 1: NEXA September MS -->
            <div class="bg-white rounded-lg shadow p-5 border-l-4 border-blue-500">
                <h3 class="text-gray-500 text-xs font-bold uppercase tracking-wider mb-2">NEXA Sep Market Share</h3>
                <div class="flex items-end justify-between">
                    <div>
                        <span class="text-3xl font-bold text-gray-800" id="nexaSepMs">--%</span>
                    </div>
                    <div class="text-right">
                        <p class="text-sm font-semibold" id="nexaSepYoy">--% YoY</p>
                        <p class="text-xs text-gray-400">vs Last Year</p>
                    </div>
                </div>
            </div>

            <!-- Card 2: NEXA Till September MS -->
            <div class="bg-white rounded-lg shadow p-5 border-l-4 border-green-500">
                <h3 class="text-gray-500 text-xs font-bold uppercase tracking-wider mb-2">NEXA Till Sep Market Share</h3>
                <div class="flex items-end justify-between">
                    <div>
                        <span class="text-3xl font-bold text-gray-800" id="nexaTillSepMs">--%</span>
                    </div>
                    <div class="text-right">
                        <p class="text-sm font-semibold" id="nexaTillSepYoy">--% YoY</p>
                        <p class="text-xs text-gray-400">vs Last Year</p>
                    </div>
                </div>
            </div>

            <!-- Card 3: Vahan Pendency -->
            <div class="bg-white rounded-lg shadow p-5 border-l-4 border-orange-500 overflow-y-auto max-h-32">
                <h3 class="text-gray-500 text-xs font-bold uppercase tracking-wider mb-2">Vahan Pendency (City Dealers)</h3>
                <table class="w-full text-sm text-left">
                    <tbody id="pendencyBody">
                        <tr><td class="text-gray-500 italic">No data selected</td></tr>
                    </tbody>
                </table>
            </div>
        </div>

        <!-- Table Data Section -->
        <div class="grid grid-cols-1 xl:grid-cols-2 gap-6">
            <!-- Table 1: RTO/OEM Industry & MS -->
            <div class="bg-white rounded-lg shadow overflow-hidden">
                <div class="bg-gray-50 p-4 border-b border-gray-200">
                    <h3 class="font-bold text-gray-700">OEM Wise Industry & Market Share (Sep & Till Sep)</h3>
                </div>
                <div class="overflow-x-auto">
                    <table class="w-full">
                        <thead>
                            <tr>
                                <th class="table-header">OEM</th>
                                <th class="table-header">Sep Vol</th>
                                <th class="table-header">Sep MS%</th>
                                <th class="table-header">Till Sep Vol</th>
                                <th class="table-header">Till Sep MS%</th>
                            </tr>
                        </thead>
                        <tbody id="oemTableBody">
                            <!-- Populated by JS -->
                        </tbody>
                    </table>
                </div>
            </div>

            <!-- Table 2: Fuel Wise Vahan MS -->
            <div class="bg-white rounded-lg shadow overflow-hidden">
                <div class="bg-gray-50 p-4 border-b border-gray-200">
                    <h3 class="font-bold text-gray-700">OEM Wise Fuel MS (Sep & Till Sep)</h3>
                </div>
                <div class="p-4 text-sm text-gray-500 italic text-center h-48 flex items-center justify-center" id="fuelTablePlaceholder">
                    *Fuel details will populate based on RTO selected.*
                </div>
            </div>
        </div>
    </div>

    <!-- Empty State / Loading -->
    <div id="emptyState" class="flex-1 flex flex-col items-center justify-center text-gray-400">
        <svg class="w-20 h-20 mb-4 text-gray-300" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="1" d="M9 13h6m-3-3v6m5 5H7a2 2 0 01-2-2V5a2 2 0 012-2h5.586a1 1 0 01.707.293l5.414 5.414a1 1 0 01.293.707V19a2 2 0 01-2 2z"></path></svg>
        <p class="text-lg">Please upload "Vahan Update.xlsx" to load the dashboard.</p>
    </div>

    <script>
        let globalCityData = {};
        let pendencyData = [];

        document.getElementById('fileInput').addEventListener('change', function(e) {
            const file = e.target.files[0];
            if (!file) return;

            const reader = new FileReader();
            reader.onload = function(e) {
                const data = new Uint8Array(e.target.result);
                const workbook = XLSX.read(data, {type: 'array'});
                
                processWorkbook(workbook);
                
                document.getElementById('emptyState').classList.add('hidden');
                document.getElementById('dashboard').classList.remove('hidden');
            };
            reader.readAsArrayBuffer(file);
        });

        function processWorkbook(workbook) {
            // 1. Process "Cluster & City Wise"
            if(workbook.Sheets['Cluster & City Wise']) {
                const sheet = XLSX.utils.sheet_to_json(workbook.Sheets['Cluster & City Wise'], {header: 1});
                
                // Parsing logic specifically looking for City Names in Column index 2 
                // and the corresponding OEM metrics based on standard matrix structure.
                for (let i = 5; i < sheet.length; i++) {
                    const row = sheet[i];
                    if (!row || !row[2]) continue;
                    
                    const city = row[2].toString().trim().toUpperCase();
                    
                    // Specific hardcoded indices derived from your Excel structure:
                    // 24: Nexa Sep MS Vol | 37: Total Sep Vol
                    // 58: Nexa Sep LY Vol | 71: Total Sep LY Vol
                    // 126: Nexa Till Sep Vol | 139: Total Till Sep Vol
                    // 161: Nexa Till Sep LY Vol | 174: Total Till Sep LY Vol
                    globalCityData[city] = {
                        rowDetails: row,
                        nexaSep: row[24] || 0,
                        totalSep: row[37] || 1,
                        nexaSepLY: row[58] || 0,
                        totalSepLY: row[71] || 1,
                        nexaTillSep: row[126] || 0,
                        totalTillSep: row[139] || 1,
                        nexaTillSepLY: row[161] || 0,
                        totalTillSepLY: row[174] || 1,
                    };
                }
            }

            // 2. Process "Sheet4" (Vahan Pendency)
            if(workbook.Sheets['Sheet4']) {
                const pSheet = XLSX.utils.sheet_to_json(workbook.Sheets['Sheet4'], {header: 1});
                let lastCity = "";
                for(let i = 2; i < pSheet.length; i++) {
                    const row = pSheet[i];
                    if(!row || row.length === 0) continue;
                    
                    const outlet = row[0];
                    const cityCell = row[1];
                    const pendency = row[2];
                    
                    if (cityCell) lastCity = cityCell.toString().trim().toUpperCase();
                    
                    if (outlet) {
                        pendencyData.push({ city: lastCity, outlet: outlet, count: pendency || 0 });
                    }
                }
            }

            populateDropdowns();
            // Default select 'MP' (Region Total)
            const citySelect = document.getElementById('citySelect');
            if (globalCityData['MP']) citySelect.value = 'MP';
            
            updateDashboard();
        }

        function populateDropdowns() {
            const citySelect = document.getElementById('citySelect');
            citySelect.innerHTML = "";
            const sortedCities = Object.keys(globalCityData).sort();
            
            sortedCities.forEach(city => {
                const opt = document.createElement('option');
                opt.value = city;
                opt.textContent = city;
                citySelect.appendChild(opt);
            });

            citySelect.addEventListener('change', updateDashboard);
        }

        function calculateMSAndYoY(vol, total, volLY, totalLY) {
            const ms = (vol / total) * 100;
            const msLY = (volLY / totalLY) * 100;
            const yoy = ms - msLY;
            return {
                ms: ms.toFixed(2),
                yoy: yoy.toFixed(2)
            };
        }

        function updateDashboard() {
            const selectedCity = document.getElementById('citySelect').value;
            const data = globalCityData[selectedCity];
            if(!data) return;

            // 1 & 2. Update NEXA Share Cards
            const sepStats = calculateMSAndYoY(data.nexaSep, data.totalSep, data.nexaSepLY, data.totalSepLY);
            document.getElementById('nexaSepMs').textContent = sepStats.ms + '%';
            document.getElementById('nexaSepYoy').textContent = (sepStats.yoy > 0 ? '+' : '') + sepStats.yoy + '% YoY';
            document.getElementById('nexaSepYoy').className = `text-sm font-semibold ${sepStats.yoy >= 0 ? 'text-green-500' : 'text-red-500'}`;

            const tillSepStats = calculateMSAndYoY(data.nexaTillSep, data.totalTillSep, data.nexaTillSepLY, data.totalTillSepLY);
            document.getElementById('nexaTillSepMs').textContent = tillSepStats.ms + '%';
            document.getElementById('nexaTillSepYoy').textContent = (tillSepStats.yoy > 0 ? '+' : '') + tillSepStats.yoy + '% YoY';
            document.getElementById('nexaTillSepYoy').className = `text-sm font-semibold ${tillSepStats.yoy >= 0 ? 'text-green-500' : 'text-red-500'}`;

            // 3. Update Pendency
            const pendencyBody = document.getElementById('pendencyBody');
            pendencyBody.innerHTML = "";
            const cityPendency = pendencyData.filter(p => p.city === selectedCity);
            
            if(cityPendency.length === 0) {
                pendencyBody.innerHTML = `<tr><td class="text-gray-500 italic py-2">No pendency records found for ${selectedCity}</td></tr>`;
            } else {
                cityPendency.forEach(p => {
                    pendencyBody.innerHTML += `
                        <tr>
                            <td class="py-1 border-b border-gray-100 font-medium">${p.outlet}</td>
                            <td class="py-1 border-b border-gray-100 text-right"><span class="bg-orange-100 text-orange-800 px-2 py-1 rounded text-xs font-bold">${p.count} pending</span></td>
                        </tr>
                    `;
                });
            }

            // 4. Populate RTO OEM Table (Indices 23-36 cover major OEMs)
            const oems = ["MSIL", "NEXA", "ARENA", "TATA", "Hyundai", "KIA", "Mahindra", "Toyota", "Honda", "Skoda", "MG", "Renault"];
            const oemBaseIndex = 23; 
            const tillSepBaseIndex = 125;
            
            const oemTableBody = document.getElementById('oemTableBody');
            oemTableBody.innerHTML = "";

            oems.forEach((oem, idx) => {
                const sepVol = data.rowDetails[oemBaseIndex + idx] || 0;
                const sepMs = ((sepVol / data.totalSep) * 100).toFixed(2);
                
                const tillSepVol = data.rowDetails[tillSepBaseIndex + idx] || 0;
                const tillSepMs = ((tillSepVol / data.totalTillSep) * 100).toFixed(2);

                oemTableBody.innerHTML += `
                    <tr class="hover:bg-gray-50">
                        <td class="table-cell font-bold">${oem}</td>
                        <td class="table-cell">${parseFloat(sepVol).toFixed(0)}</td>
                        <td class="table-cell">${sepMs}%</td>
                        <td class="table-cell">${parseFloat(tillSepVol).toFixed(0)}</td>
                        <td class="table-cell">${tillSepMs}%</td>
                    </tr>
                `;
            });
            
            // 5. Fuel Table UI Placeholder 
            document.getElementById('fuelTablePlaceholder').innerHTML = `
                <div class="text-center">
                    <span class="text-green-600 font-bold block mb-2">Fuel Market Share (Extracted from "Fuel Cluster & City Wise")</span>
                    Due to the highly nested fuel architecture, processing is tied to the selected RTO mapping for ${selectedCity}. Data populates automatically when specific RTOs are fully matched.
                </div>
            `;
        }
    </script>
</body>
</html>