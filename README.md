<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vahan Market Share Dashboard</title>
    
    <!-- Fonts and Bootstrap -->
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;600;700&display=swap" rel="stylesheet">
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet">
    
    <style>
        body {
            font-family: 'Inter', sans-serif;
            background-color: #f1f5f9;
            color: #334155;
            padding-bottom: 50px;
        }

        /* Elegant Header */
        .dashboard-header {
            background: linear-gradient(135deg, #1e293b 0%, #0f172a 100%);
            color: #ffffff;
            padding: 2.5rem 0;
            margin-bottom: 2.5rem;
            box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1);
        }
        .dashboard-header h1 {
            font-weight: 700;
            letter-spacing: -0.5px;
            margin: 0;
        }
        .dashboard-header p {
            color: #94a3b8;
            margin-top: 0.5rem;
            margin-bottom: 0;
        }

        /* Filter Section */
        .filter-card {
            background: #ffffff;
            border-radius: 12px;
            padding: 1.5rem;
            box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.05);
            margin-bottom: 2rem;
            border: 1px solid #e2e8f0;
        }

        /* KPI Cards */
        .kpi-card {
            background: #ffffff;
            border-radius: 12px;
            padding: 2rem;
            box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.05);
            border: 1px solid #e2e8f0;
            text-align: center;
            height: 100%;
            transition: transform 0.2s ease;
        }
        .kpi-card:hover {
            transform: translateY(-5px);
        }
        .kpi-title {
            font-size: 0.9rem;
            font-weight: 600;
            text-transform: uppercase;
            color: #64748b;
            letter-spacing: 0.05em;
            margin-bottom: 1rem;
        }
        .kpi-value {
            font-size: 3rem;
            font-weight: 700;
            color: #0f172a;
            margin: 0;
            line-height: 1;
        }

        /* Tables (No small windows, full visibility) */
        .table-card {
            background: #ffffff;
            border-radius: 12px;
            box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.05);
            margin-bottom: 2.5rem;
            border: 1px solid #e2e8f0;
            overflow: hidden; /* Only for border radius */
        }
        .table-header {
            background-color: #ffffff;
            padding: 1.25rem 1.5rem;
            border-bottom: 1px solid #e2e8f0;
        }
        .table-header h5 {
            margin: 0;
            font-weight: 600;
            color: #0f172a;
        }
        .table {
            margin-bottom: 0;
        }
        .table thead th {
            background-color: #f8fafc;
            color: #475569;
            font-weight: 600;
            font-size: 0.85rem;
            text-transform: uppercase;
            letter-spacing: 0.05em;
            border-bottom: 2px solid #e2e8f0;
            padding: 1rem 1.5rem;
        }
        .table tbody td {
            padding: 1rem 1.5rem;
            vertical-align: middle;
            color: #334155;
            border-bottom: 1px solid #e2e8f0;
        }
        .table tbody tr:hover {
            background-color: #f8fafc;
        }

        /* Badges & Indicators */
        .badge-positive {
            background-color: #dcfce7;
            color: #166534;
            padding: 0.35rem 0.6rem;
            border-radius: 6px;
            font-weight: 600;
            font-size: 0.85rem;
        }
        .badge-negative {
            background-color: #fee2e2;
            color: #991b1b;
            padding: 0.35rem 0.6rem;
            border-radius: 6px;
            font-weight: 600;
            font-size: 0.85rem;
        }
        .status-high { background-color: #fee2e2; color: #991b1b; padding: 0.35rem 0.6rem; border-radius: 6px; font-weight: 600; font-size: 0.85rem; }
        .status-normal { background-color: #e0f2fe; color: #075985; padding: 0.35rem 0.6rem; border-radius: 6px; font-weight: 600; font-size: 0.85rem; }

    </style>
</head>
<body>

<!-- Header -->
<div class="dashboard-header text-center">
    <div class="container">
        <h1>Vahan Market Share Analytics</h1>
        <p>Comprehensive Insights by Cluster, City, RTO, OEM, and Fuel Type</p>
    </div>
</div>

<div class="container">
    
    <!-- Controls Section -->
    <div class="filter-card">
        <div class="row g-3 align-items-center">
            <div class="col-md-5">
                <label for="clusterSelect" class="form-label fw-bold">Select Region / Cluster (Col C Ref):</label>
                <select id="clusterSelect" class="form-select form-select-lg" onchange="populateCities()">
                    <option value="MP_REGION" selected>MP / Region Total (Default)</option>
                    <option value="Bhopal_Cluster">Bhopal Cluster</option>
                    <option value="Indore_Cluster">Indore Cluster</option>
                    <option value="Gwalior_Cluster">Gwalior Cluster</option>
                </select>
            </div>
            <div class="col-md-5">
                <label for="citySelect" class="form-label fw-bold">Select City (Col C Ref):</label>
                <select id="citySelect" class="form-select form-select-lg" onchange="updateDashboard()">
                    <option value="ALL">All Cities (Region Level)</option>
                </select>
            </div>
            <div class="col-md-2 text-end">
                <button class="btn btn-primary btn-lg w-100 mt-4" onclick="updateDashboard()">Refresh Data</button>
            </div>
        </div>
    </div>

    <!-- KPI Cards (NEXA Market Share) -->
    <div class="row mb-5 g-4">
        <div class="col-md-6">
            <div class="kpi-card">
                <div class="kpi-title">NEXA September Market Share</div>
                <div class="kpi-value" id="nexaSeptMs">--%</div>
                <div class="mt-3">
                    <span class="text-muted me-2">YoY Change:</span>
                    <span id="nexaSeptChange">--</span>
                </div>
            </div>
        </div>
        <div class="col-md-6">
            <div class="kpi-card">
                <div class="kpi-title">NEXA Till September Market Share</div>
                <div class="kpi-value" id="nexaTillSeptMs">--%</div>
                <div class="mt-3">
                    <span class="text-muted me-2">YoY Change:</span>
                    <span id="nexaTillSeptChange">--</span>
                </div>
            </div>
        </div>
    </div>

    <!-- Vahan Pendency Table -->
    <div class="table-card">
        <div class="table-header">
            <h5>Vahan Pendency (Selected View)</h5>
        </div>
        <table class="table" id="pendencyTable">
            <thead>
                <tr>
                    <th>Dealer Name</th>
                    <th>City</th>
                    <th>Pending Vehicles</th>
                    <th>Status</th>
                </tr>
            </thead>
            <tbody>
                <!-- Populated dynamically -->
            </tbody>
        </table>
    </div>

    <!-- RTO & OEM Wise Industry & Market Share -->
    <div class="table-card">
        <div class="table-header">
            <h5>RTO & OEM Wise Industry, MS & YoY Change (OEMs ref Row 6)</h5>
        </div>
        <table class="table" id="rtoOemTable">
            <thead>
                <tr>
                    <th>RTO (Col C)</th>
                    <th>OEM (Row 6)</th>
                    <th>Industry Total</th>
                    <th>Sept MS %</th>
                    <th>Sept YoY Change</th>
                    <th>Till Sept MS %</th>
                    <th>Till Sept YoY Change</th>
                </tr>
            </thead>
            <tbody>
                <!-- Populated dynamically -->
            </tbody>
        </table>
    </div>

    <!-- RTO, OEM, Fuel Wise Market Share -->
    <div class="table-card">
        <div class="table-header">
            <h5>RTO, OEM & Fuel Wise Vahan MS (OEMs ref Row 5)</h5>
        </div>
        <table class="table" id="rtoOemFuelTable">
            <thead>
                <tr>
                    <th>RTO (Col C / B)</th>
                    <th>OEM (Row 5)</th>
                    <th>Fuel Type</th>
                    <th>Sept MS %</th>
                    <th>Sept YoY Change</th>
                    <th>Till Sept MS %</th>
                    <th>Till Sept YoY Change</th>
                </tr>
            </thead>
            <tbody>
                <!-- Populated dynamically -->
            </tbody>
        </table>
    </div>

</div>

<!-- JavaScript Engine -->
<script>
    /* 
       MOCK DATA STRUCTURE 
       In production, replace this by parsing your Excel files.
       - Clusters, Cities, and RTOs emulate Excel Column C & B.
       - OEM names emulate Excel Rows 5 & 6.
    */
    const db = {
        "MP_REGION": {
            cities: ["ALL"],
            nexaSeptMs: "12.8", nexaSeptChange: "+1.4",
            nexaTillSeptMs: "12.1", nexaTillSeptChange: "+0.9",
            pendency: [
                { dealer: "NEXA Bhopal Central", city: "Bhopal", pending: 85, status: "High" },
                { dealer: "NEXA Indore Premium", city: "Indore", pending: 42, status: "Normal" },
                { dealer: "NEXA Gwalior Motors", city: "Gwalior", pending: 28, status: "Normal" },
                { dealer: "NEXA Jabalpur Auto", city: "Jabalpur", pending: 95, status: "High" }
            ],
            rtoOem: [
                { rto: "MP Total", oem: "Maruti Suzuki", ind: "12,500", septMs: "43.5", septChange: "+1.2", tillSeptMs: "42.8", tillSeptChange: "+0.8" },
                { rto: "MP Total", oem: "Hyundai", ind: "4,200", septMs: "14.6", septChange: "-0.5", tillSeptMs: "15.0", tillSeptChange: "-0.2" },
                { rto: "MP Total", oem: "Tata Motors", ind: "3,800", septMs: "13.2", septChange: "+2.1", tillSeptMs: "12.5", tillSeptChange: "+1.5" },
                { rto: "MP Total", oem: "Mahindra", ind: "2,900", septMs: "10.0", septChange: "+1.5", tillSeptMs: "9.5", tillSeptChange: "+1.2" }
            ],
            rtoOemFuel: [
                { rto: "MP Total", oem: "Maruti Suzuki", fuel: "Petrol", septMs: "35.0", septChange: "+0.5", tillSeptMs: "34.5", tillSeptChange: "+0.2" },
                { rto: "MP Total", oem: "Maruti Suzuki", fuel: "CNG", septMs: "8.5", septChange: "+0.7", tillSeptMs: "8.3", tillSeptChange: "+0.6" },
                { rto: "MP Total", oem: "Tata Motors", fuel: "EV", septMs: "2.1", septChange: "+1.5", tillSeptMs: "1.8", tillSeptChange: "+1.0" }
            ]
        },
        "Bhopal_Cluster": {
            cities: ["Bhopal", "Vidisha", "Sehore"],
            "Bhopal": {
                nexaSeptMs: "14.5", nexaSeptChange: "+2.1", nexaTillSeptMs: "13.8", nexaTillSeptChange: "+1.5",
                pendency: [
                    { dealer: "NEXA Bhopal Central", city: "Bhopal", pending: 85, status: "High" },
                    { dealer: "NEXA MP Nagar", city: "Bhopal", pending: 30, status: "Normal" }
                ],
                rtoOem: [
                    { rto: "MP04 (Bhopal)", oem: "Maruti Suzuki", ind: "3,200", septMs: "45.2", septChange: "+2.0", tillSeptMs: "44.1", tillSeptChange: "+1.5" },
                    { rto: "MP04 (Bhopal)", oem: "Hyundai", ind: "1,100", septMs: "15.5", septChange: "-0.8", tillSeptMs: "16.0", tillSeptChange: "-0.5" }
                ],
                rtoOemFuel: [
                    { rto: "MP04 (Bhopal)", oem: "Maruti Suzuki", fuel: "Petrol", septMs: "38.2", septChange: "+1.0", tillSeptMs: "37.5", tillSeptChange: "+0.8" },
                    { rto: "MP04 (Bhopal)", oem: "Maruti Suzuki", fuel: "CNG", septMs: "7.0", septChange: "+1.0", tillSeptMs: "6.6", tillSeptChange: "+0.7" }
                ]
            },
            "Vidisha": {
                nexaSeptMs: "11.2", nexaSeptChange: "-0.5", nexaTillSeptMs: "11.5", nexaTillSeptChange: "-0.2",
                pendency: [{ dealer: "NEXA Vidisha Auto", city: "Vidisha", pending: 15, status: "Normal" }],
                rtoOem: [{ rto: "MP40 (Vidisha)", oem: "Maruti Suzuki", ind: "450", septMs: "48.0", septChange: "+1.5", tillSeptMs: "47.2", tillSeptChange: "+1.0" }],
                rtoOemFuel: [{ rto: "MP40 (Vidisha)", oem: "Maruti Suzuki", fuel: "Petrol", septMs: "45.0", septChange: "+1.2", tillSeptMs: "44.0", tillSeptChange: "+0.9" }]
            },
            "Sehore": {
                nexaSeptMs: "9.5", nexaSeptChange: "+0.2", nexaTillSeptMs: "9.0", nexaTillSeptChange: "+0.1",
                pendency: [],
                rtoOem: [{ rto: "MP37 (Sehore)", oem: "Maruti Suzuki", ind: "300", septMs: "49.0", septChange: "+2.0", tillSeptMs: "48.5", tillSeptChange: "+1.5" }],
                rtoOemFuel: [{ rto: "MP37 (Sehore)", oem: "Maruti Suzuki", fuel: "Petrol", septMs: "49.0", septChange: "+2.0", tillSeptMs: "48.5", tillSeptChange: "+1.5" }]
            }
        },
        "Indore_Cluster": {
            cities: ["Indore", "Ujjain", "Dewas"],
            "Indore": {
                nexaSeptMs: "15.8", nexaSeptChange: "+3.2", nexaTillSeptMs: "14.5", nexaTillSeptChange: "+2.0",
                pendency: [{ dealer: "NEXA Indore Premium", city: "Indore", pending: 42, status: "Normal" }],
                rtoOem: [
                    { rto: "MP09 (Indore)", oem: "Maruti Suzuki", ind: "4,500", septMs: "41.5", septChange: "+0.8", tillSeptMs: "41.0", tillSeptChange: "+0.5" },
                    { rto: "MP09 (Indore)", oem: "Tata Motors", ind: "1,200", septMs: "14.0", septChange: "+2.5", tillSeptMs: "13.2", tillSeptChange: "+1.8" }
                ],
                rtoOemFuel: [
                    { rto: "MP09 (Indore)", oem: "Maruti Suzuki", fuel: "Petrol", septMs: "32.0", septChange: "-0.5", tillSeptMs: "32.5", tillSeptChange: "-0.2" },
                    { rto: "MP09 (Indore)", oem: "Maruti Suzuki", fuel: "CNG", septMs: "9.5", septChange: "+1.3", tillSeptMs: "8.5", tillSeptChange: "+0.7" }
                ]
            },
            "Ujjain": {
                nexaSeptMs: "10.5", nexaSeptChange: "+0.5", nexaTillSeptMs: "10.0", nexaTillSeptChange: "+0.2",
                pendency: [], rtoOem: [], rtoOemFuel: []
            },
            "Dewas": {
                nexaSeptMs: "12.0", nexaSeptChange: "-1.0", nexaTillSeptMs: "12.5", nexaTillSeptChange: "-0.5",
                pendency: [], rtoOem: [], rtoOemFuel: []
            }
        },
        "Gwalior_Cluster": {
            cities: ["Gwalior", "Shivpuri"],
            "Gwalior": {
                nexaSeptMs: "11.0", nexaSeptChange: "-1.2", nexaTillSeptMs: "11.4", nexaTillSeptChange: "-0.5",
                pendency: [{ dealer: "NEXA Gwalior Motors", city: "Gwalior", pending: 28, status: "Normal" }],
                rtoOem: [{ rto: "MP07 (Gwalior)", oem: "Maruti Suzuki", ind: "1,800", septMs: "40.5", septChange: "-1.5", tillSeptMs: "41.2", tillSeptChange: "-0.8" }],
                rtoOemFuel: [{ rto: "MP07 (Gwalior)", oem: "Maruti Suzuki", fuel: "Petrol", septMs: "36.5", septChange: "-1.0", tillSeptMs: "37.0", tillSeptChange: "-0.5" }]
            },
            "Shivpuri": {
                nexaSeptMs: "8.5", nexaSeptChange: "+1.0", nexaTillSeptMs: "8.0", nexaTillSeptChange: "+0.5",
                pendency: [], rtoOem: [], rtoOemFuel: []
            }
        }
    };

    // UI Logic

    // 1. Populate City Dropdown Based on Cluster Selection
    function populateCities() {
        const cluster = document.getElementById("clusterSelect").value;
        const citySelect = document.getElementById("citySelect");
        citySelect.innerHTML = ""; 

        if (cluster === "MP_REGION") {
            citySelect.innerHTML = '<option value="ALL">All Cities (Region Level)</option>';
        } else {
            const cities = db[cluster].cities;
            citySelect.innerHTML = '<option value="" disabled selected>-- Select a City --</option>';
            cities.forEach(city => {
                citySelect.innerHTML += `<option value="${city}">${city}</option>`;
            });
        }
        updateDashboard();
    }

    // 2. Format YoY Change with Elegant Badges
    function formatChange(val) {
        if (!val) return "-";
        if (val.includes('+')) return `<span class="badge-positive">${val}%</span>`;
        if (val.includes('-')) return `<span class="badge-negative">${val}%</span>`;
        return val + "%";
    }

    // 3. Update the Entire Dashboard
    function updateDashboard() {
        const cluster = document.getElementById("clusterSelect").value;
        const city = document.getElementById("citySelect").value;

        let data = null;

        // Determine which dataset to pull
        if (cluster === "MP_REGION" || city === "ALL") {
            data = db["MP_REGION"];
        } else if (city && city !== "ALL" && city !== "") {
            data = db[cluster][city];
        } else {
            return; // Exit if a city isn't selected yet in cluster mode
        }

        // Update KPIs
        document.getElementById("nexaSeptMs").innerText = data.nexaSeptMs + "%";
        document.getElementById("nexaSeptChange").innerHTML = formatChange(data.nexaSeptChange);
        document.getElementById("nexaTillSeptMs").innerText = data.nexaTillSeptMs + "%";
        document.getElementById("nexaTillSeptChange").innerHTML = formatChange(data.nexaTillSeptChange);

        // Update Pendency Table
        const pendencyTbody = document.querySelector("#pendencyTable tbody");
        pendencyTbody.innerHTML = "";
        if (data.pendency.length === 0) {
            pendencyTbody.innerHTML = `<tr><td colspan="4" class="text-center text-muted py-4">No pendency data available for this selection.</td></tr>`;
        } else {
            data.pendency.forEach(item => {
                const statusClass = item.status === 'High' ? 'status-high' : 'status-normal';
                pendencyTbody.innerHTML += `<tr>
                    <td class="fw-bold">${item.dealer}</td>
                    <td>${item.city}</td>
                    <td>${item.pending}</td>
                    <td><span class="${statusClass}">${item.status}</span></td>
                </tr>`;
            });
        }

        // Update RTO & OEM Table
        const rtoOemTbody = document.querySelector("#rtoOemTable tbody");
        rtoOemTbody.innerHTML = "";
        if (data.rtoOem.length === 0) {
            rtoOemTbody.innerHTML = `<tr><td colspan="7" class="text-center text-muted py-4">No RTO/OEM data available for this selection.</td></tr>`;
        } else {
            data.rtoOem.forEach(item => {
                rtoOemTbody.innerHTML += `<tr>
                    <td class="fw-bold">${item.rto}</td>
                    <td>${item.oem}</td>
                    <td>${item.ind}</td>
                    <td class="fw-bold text-dark">${item.septMs}%</td>
                    <td>${formatChange(item.septChange)}</td>
                    <td class="fw-bold text-dark">${item.tillSeptMs}%</td>
                    <td>${formatChange(item.tillSeptChange)}</td>
                </tr>`;
            });
        }

        // Update RTO, OEM & Fuel Table
        const rtoOemFuelTbody = document.querySelector("#rtoOemFuelTable tbody");
        rtoOemFuelTbody.innerHTML = "";
        if (data.rtoOemFuel.length === 0) {
            rtoOemFuelTbody.innerHTML = `<tr><td colspan="7" class="text-center text-muted py-4">No Fuel data available for this selection.</td></tr>`;
        } else {
            data.rtoOemFuel.forEach(item => {
                rtoOemFuelTbody.innerHTML += `<tr>
                    <td class="fw-bold">${item.rto}</td>
                    <td>${item.oem}</td>
                    <td class="text-primary fw-bold">${item.fuel}</td>
                    <td class="fw-bold text-dark">${item.septMs}%</td>
                    <td>${formatChange(item.septChange)}</td>
                    <td class="fw-bold text-dark">${item.tillSeptMs}%</td>
                    <td>${formatChange(item.tillSeptChange)}</td>
                </tr>`;
            });
        }
    }

    // Initialize Default State on Load
    window.onload = () => {
        populateCities(); 
    };

</script>

<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/js/bootstrap.bundle.min.js"></script>
</body>
</html>