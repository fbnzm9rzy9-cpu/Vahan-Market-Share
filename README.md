
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vahan Market Share Dashboard</title>
    <!-- Bootstrap 5 CSS for Styling -->
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet">
    <style>
        body {
            background-color: #f4f7f6;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }
        .card {
            box-shadow: 0 4px 6px rgba(0,0,0,0.1);
            margin-bottom: 20px;
        }
        .header-title {
            color: #0d6efd;
            font-weight: bold;
        }
        .ms-change-positive {
            color: green;
            font-weight: bold;
        }
        .ms-change-negative {
            color: red;
            font-weight: bold;
        }
        .table th {
            background-color: #0d6efd;
            color: white;
        }
    </style>
</head>
<body>

<div class="container-fluid py-4">
    <h2 class="text-center header-title mb-4">Vahan Market Share Dashboard</h2>

    <!-- Controls Section -->
    <div class="row mb-4">
        <div class="col-md-4 offset-md-2">
            <label for="clusterSelect" class="form-label fw-bold">Select Cluster:</label>
            <select id="clusterSelect" class="form-select" onchange="populateCities()">
                <option value="MP_REGION" selected>MP/Region Total (Default)</option>
                <option value="Cluster_1">Cluster 1</option>
                <option value="Cluster_2">Cluster 2</option>
            </select>
        </div>
        <div class="col-md-4">
            <label for="citySelect" class="form-label fw-bold">Select City:</label>
            <select id="citySelect" class="form-select" onchange="updateDashboard()">
                <option value="ALL">All Cities</option>
            </select>
        </div>
    </div>

    <!-- KPI Cards (Points 1 & 2) -->
    <div class="row">
        <div class="col-md-6">
            <div class="card text-center bg-light">
                <div class="card-body">
                    <h5 class="card-title text-muted">NEXA September Market Share</h5>
                    <h2 id="nexaSeptMs" class="mb-0">--%</h2>
                    <p class="mt-2 mb-0">YoY Change: <span id="nexaSeptChange">--%</span></p>
                </div>
            </div>
        </div>
        <div class="col-md-6">
            <div class="card text-center bg-light">
                <div class="card-body">
                    <h5 class="card-title text-muted">NEXA Till September Market Share</h5>
                    <h2 id="nexaTillSeptMs" class="mb-0">--%</h2>
                    <p class="mt-2 mb-0">YoY Change: <span id="nexaTillSeptChange">--%</span></p>
                </div>
            </div>
        </div>
    </div>

    <!-- Vahan Pendency (Point 3) -->
    <div class="card">
        <div class="card-header bg-dark text-white fw-bold">
            Vahan Pendency (Dealers in Selected City/Region)
        </div>
        <div class="card-body p-0">
            <div class="table-responsive">
                <table class="table table-striped table-hover mb-0" id="pendencyTable">
                    <thead>
                        <tr>
                            <th>Dealer Name</th>
                            <th>City</th>
                            <th>Pending Vehicles</th>
                            <th>Status</th>
                        </tr>
                    </thead>
                    <tbody>
                        <!-- Populated by JS -->
                    </tbody>
                </table>
            </div>
        </div>
    </div>

    <!-- RTO & OEM Wise Data (Point 4) -->
    <div class="card">
        <div class="card-header bg-primary text-white fw-bold">
            RTO & OEM Wise Industry, Market Share & YoY Change
        </div>
        <div class="card-body p-0">
            <div class="table-responsive">
                <table class="table table-bordered mb-0" id="rtoOemTable">
                    <thead>
                        <tr>
                            <th>RTO</th>
                            <th>OEM</th>
                            <th>Sept MS %</th>
                            <th>Sept YoY Change %</th>
                            <th>Till Sept MS %</th>
                            <th>Till Sept YoY Change %</th>
                        </tr>
                    </thead>
                    <tbody>
                         <!-- Populated by JS -->
                    </tbody>
                </table>
            </div>
        </div>
    </div>

    <!-- RTO, OEM, Fuel Wise Data (Point 5) -->
    <div class="card">
        <div class="card-header bg-success text-white fw-bold">
            RTO, OEM & Fuel Wise Vahan Market Share
        </div>
        <div class="card-body p-0">
            <div class="table-responsive">
                <table class="table table-bordered mb-0" id="rtoOemFuelTable">
                    <thead>
                        <tr>
                            <th>RTO</th>
                            <th>OEM</th>
                            <th>Fuel Type</th>
                            <th>Sept MS %</th>
                            <th>Sept YoY Change %</th>
                            <th>Till Sept MS %</th>
                            <th>Till Sept YoY Change %</th>
                        </tr>
                    </thead>
                    <tbody>
                         <!-- Populated by JS -->
                    </tbody>
                </table>
            </div>
        </div>
    </div>

</div>

<!-- JavaScript for Logic -->
<script>
    // --- MOCK DATA SIMULATING EXCEL SHEETS ---
    const mockData = {
        "MP_REGION": {
            cities: ["ALL"],
            nexaSeptMs: "12.4", nexaSeptChange: "+1.2",
            nexaTillSeptMs: "11.8", nexaTillSeptChange: "+0.8",
            pendency: [
                { dealer: "Dealer A", city: "Bhopal", pending: 45, status: "High" },
                { dealer: "Dealer B", city: "Indore", pending: 20, status: "Normal" },
                { dealer: "Dealer C", city: "Gwalior", pending: 15, status: "Normal" }
            ],
            rtoOem: [
                { rto: "MP04 (Bhopal)", oem: "Maruti", septMs: "45.0", septChange: "+2.1", tillSeptMs: "44.5", tillSeptChange: "+1.5" },
                { rto: "MP09 (Indore)", oem: "Hyundai", septMs: "15.0", septChange: "-0.5", tillSeptMs: "15.5", tillSeptChange: "-0.2" }
            ],
            rtoOemFuel: [
                { rto: "MP04 (Bhopal)", oem: "Maruti", fuel: "Petrol", septMs: "40.0", septChange: "+1.0", tillSeptMs: "39.0", tillSeptChange: "+0.5" },
                { rto: "MP04 (Bhopal)", oem: "Maruti", fuel: "CNG", septMs: "5.0", septChange: "+1.1", tillSeptMs: "5.5", tillSeptChange: "+1.0" }
            ]
        },
        "Cluster_1": {
            cities: ["Bhopal", "Gwalior"],
            "Bhopal": {
                nexaSeptMs: "14.1", nexaSeptChange: "+2.5",
                nexaTillSeptMs: "13.5", nexaTillSeptChange: "+1.5",
                pendency: [{ dealer: "Dealer A", city: "Bhopal", pending: 45, status: "High" }],
                rtoOem: [{ rto: "MP04", oem: "Maruti", septMs: "47.0", septChange: "+3.1", tillSeptMs: "46.5", tillSeptChange: "+2.0" }],
                rtoOemFuel: [{ rto: "MP04", oem: "Maruti", fuel: "Petrol", septMs: "42.0", septChange: "+2.0", tillSeptMs: "41.0", tillSeptChange: "+1.0" }]
            },
            "Gwalior": {
                nexaSeptMs: "10.0", nexaSeptChange: "-1.0",
                nexaTillSeptMs: "10.2", nexaTillSeptChange: "-0.5",
                pendency: [{ dealer: "Dealer C", city: "Gwalior", pending: 15, status: "Normal" }],
                rtoOem: [{ rto: "MP07", oem: "Maruti", septMs: "40.0", septChange: "-1.1", tillSeptMs: "41.5", tillSeptChange: "-0.5" }],
                rtoOemFuel: [{ rto: "MP07", oem: "Maruti", fuel: "Petrol", septMs: "35.0", septChange: "-1.0", tillSeptMs: "36.0", tillSeptChange: "-0.5" }]
            }
        },
        "Cluster_2": {
            cities: ["Indore", "Ujjain"],
            "Indore": {
                nexaSeptMs: "15.0", nexaSeptChange: "+3.0",
                nexaTillSeptMs: "14.0", nexaTillSeptChange: "+2.0",
                pendency: [{ dealer: "Dealer B", city: "Indore", pending: 20, status: "Normal" }],
                rtoOem: [{ rto: "MP09", oem: "Maruti", septMs: "43.0", septChange: "+1.1", tillSeptMs: "42.5", tillSeptChange: "+1.0" }],
                rtoOemFuel: [{ rto: "MP09", oem: "Maruti", fuel: "Petrol", septMs: "38.0", septChange: "+1.0", tillSeptMs: "37.0", tillSeptChange: "+0.5" }]
            },
            "Ujjain": {
                 // Simulated blank data for example purposes
                 nexaSeptMs: "8.0", nexaSeptChange: "-2.0",
                 nexaTillSeptMs: "8.5", nexaTillSeptChange: "-1.5",
                 pendency: [], rtoOem: [], rtoOemFuel: []
            }
        }
    };

    // --- LOGIC ---
    
    // 1. Populate Cities Dropdown based on Cluster selection
    function populateCities() {
        const cluster = document.getElementById("clusterSelect").value;
        const citySelect = document.getElementById("citySelect");
        citySelect.innerHTML = ""; // Clear existing

        if (cluster === "MP_REGION") {
            citySelect.innerHTML = '<option value="ALL">All Cities</option>';
        } else {
            const cities = mockData[cluster].cities;
            citySelect.innerHTML = '<option value="" disabled selected>Select City</option>';
            cities.forEach(city => {
                citySelect.innerHTML += `<option value="${city}">${city}</option>`;
            });
        }
        updateDashboard();
    }

    // 2. Helper to format YoY text (Red for -, Green for +)
    function formatYoY(value) {
        if(value.includes('+')) return `<span class="ms-change-positive">${value}</span>`;
        if(value.includes('-')) return `<span class="ms-change-negative">${value}</span>`;
        return value;
    }

    // 3. Update all Tables and Cards
    function updateDashboard() {
        const cluster = document.getElementById("clusterSelect").value;
        const city = document.getElementById("citySelect").value;

        let dataToDisplay = null;

        if (cluster === "MP_REGION" || city === "ALL") {
            dataToDisplay = mockData["MP_REGION"];
        } else if (city && city !== "ALL") {
            dataToDisplay = mockData[cluster][city];
        } else {
            return; // Wait for city selection
        }

        // Update KPI Cards
        document.getElementById("nexaSeptMs").innerText = dataToDisplay.nexaSeptMs + "%";
        document.getElementById("nexaSeptChange").innerHTML = formatYoY(dataToDisplay.nexaSeptChange + "%");
        
        document.getElementById("nexaTillSeptMs").innerText = dataToDisplay.nexaTillSeptMs + "%";
        document.getElementById("nexaTillSeptChange").innerHTML = formatYoY(dataToDisplay.nexaTillSeptChange + "%");

        // Update Pendency Table
        const pendencyTbody = document.querySelector("#pendencyTable tbody");
        pendencyTbody.innerHTML = "";
        dataToDisplay.pendency.forEach(item => {
            pendencyTbody.innerHTML += `<tr>
                <td>${item.dealer}</td><td>${item.city}</td><td>${item.pending}</td>
                <td><span class="badge ${item.status === 'High' ? 'bg-danger' : 'bg-success'}">${item.status}</span></td>
            </tr>`;
        });
        if(dataToDisplay.pendency.length === 0) pendencyTbody.innerHTML = `<tr><td colspan="4" class="text-center">No Data Available</td></tr>`;

        // Update RTO OEM Table
        const rtoOemTbody = document.querySelector("#rtoOemTable tbody");
        rtoOemTbody.innerHTML = "";
        dataToDisplay.rtoOem.forEach(item => {
            rtoOemTbody.innerHTML += `<tr>
                <td>${item.rto}</td><td>${item.oem}</td><td>${item.septMs}%</td>
                <td>${formatYoY(item.septChange + '%')}</td><td>${item.tillSeptMs}%</td>
                <td>${formatYoY(item.tillSeptChange + '%')}</td>
            </tr>`;
        });
        if(dataToDisplay.rtoOem.length === 0) rtoOemTbody.innerHTML = `<tr><td colspan="6" class="text-center">No Data Available</td></tr>`;

        // Update RTO OEM Fuel Table
        const rtoOemFuelTbody = document.querySelector("#rtoOemFuelTable tbody");
        rtoOemFuelTbody.innerHTML = "";
        dataToDisplay.rtoOemFuel.forEach(item => {
            rtoOemFuelTbody.innerHTML += `<tr>
                <td>${item.rto}</td><td>${item.oem}</td><td>${item.fuel}</td>
                <td>${item.septMs}%</td><td>${formatYoY(item.septChange + '%')}</td>
                <td>${item.tillSeptMs}%</td><td>${formatYoY(item.tillSeptChange + '%')}</td>
            </tr>`;
        });
        if(dataToDisplay.rtoOemFuel.length === 0) rtoOemFuelTbody.innerHTML = `<tr><td colspan="7" class="text-center">No Data Available</td></tr>`;
    }

    // Initialize Dashboard on page load
    window.onload = () => {
        populateCities(); // Sets defaults to Region total
    };
</script>

<!-- Bootstrap JS (Optional, for advanced dropdowns/modals) -->
<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/js/bootstrap.bundle.min.js"></script>
</body>
</html>