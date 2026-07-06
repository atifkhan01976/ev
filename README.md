Here is the complete code for a single-file, highly modern comparison website. It is designed to be completely **standalone**—copy the code, save it with an `.html` extension (e.g., `ev-comparison.html`), and open it directly in any web browser.

The site includes an elegant data dashboard, responsive design, interactive tabs, dynamic filters, and clean CSS styling.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Canada EV Buyer's Hub | 2026 Comparison</title>
    <style>
        :root {
            --bg-color: #0f172a;
            --card-bg: #1e293b;
            --accent-blue: #38bdf8;
            --accent-green: #34d399;
            --text-main: #f8fafc;
            --text-muted: #94a3b8;
            --border-color: #334155;
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
        }

        body {
            background-color: var(--bg-color);
            color: var(--text-main);
            padding: 2rem 1rem;
            line-height: 1.5;
        }

        .container {
            max-width: 1200px;
            margin: 0 auto;
        }

        header {
            text-align: center;
            margin-bottom: 3rem;
        }

        header h1 {
            font-size: 2.5rem;
            color: var(--text-main);
            margin-bottom: 0.5rem;
        }

        header h1 span {
            color: var(--accent-blue);
        }

        header p {
            color: var(--text-muted);
            font-size: 1.1rem;
        }

        /* Interactive Filter Control Tabs */
        .filter-controls {
            display: flex;
            justify-content: center;
            gap: 1rem;
            margin-bottom: 2rem;
        }

        .filter-btn {
            background-color: var(--card-bg);
            color: var(--text-main);
            border: 1px solid var(--border-color);
            padding: 0.75rem 1.5rem;
            border-radius: 8px;
            cursor: pointer;
            font-weight: 600;
            transition: all 0.2s ease;
        }

        .filter-btn:hover {
            border-color: var(--accent-blue);
        }

        .filter-btn.active {
            background-color: var(--accent-blue);
            color: var(--bg-color);
            border-color: var(--accent-blue);
        }

        /* Showcase Cards Grid */
        .showcase-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
            gap: 2rem;
            margin-bottom: 4rem;
        }

        .ev-card {
            background-color: var(--card-bg);
            border-radius: 12px;
            border: 1px solid var(--border-color);
            padding: 1.5rem;
            display: flex;
            flex-direction: column;
            justify-content: space-between;
        }

        .ev-card h3 {
            font-size: 1.4rem;
            margin-bottom: 0.25rem;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .badge {
            font-size: 0.75rem;
            padding: 0.25rem 0.5rem;
            border-radius: 4px;
            font-weight: bold;
        }
        .badge-suv { background-color: #818cf8; color: #fff; }
        .badge-car { background-color: #f472b6; color: #fff; }

        .ev-card .vendor {
            color: var(--accent-blue);
            font-size: 0.9rem;
            text-transform: uppercase;
            letter-spacing: 1px;
            margin-bottom: 1rem;
            font-weight: 700;
        }

        .metrics {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 1rem;
            margin-bottom: 1rem;
            background: rgba(0,0,0,0.2);
            padding: 0.75rem;
            border-radius: 8px;
        }

        .metric-label {
            font-size: 0.8rem;
            color: var(--text-muted);
        }

        .metric-value {
            font-size: 1.1rem;
            font-weight: 700;
        }

        /* Modern Table Styling */
        .table-section {
            background-color: var(--card-bg);
            border-radius: 12px;
            border: 1px solid var(--border-color);
            padding: 1.5rem;
            overflow-x: auto;
        }

        .table-section h2 {
            margin-bottom: 1.5rem;
            font-size: 1.8rem;
            border-left: 4px solid var(--accent-green);
            padding-left: 0.75rem;
        }

        table {
            width: 100%;
            border-collapse: collapse;
            text-align: left;
        }

        th, td {
            padding: 1rem;
            border-bottom: 1px solid var(--border-color);
        }

        th {
            color: var(--text-muted);
            font-weight: 600;
            text-transform: uppercase;
            font-size: 0.85rem;
        }

        tr:hover {
            background-color: rgba(255, 255, 255, 0.02);
        }

        .winner-cell {
            color: var(--accent-green);
            font-weight: bold;
        }

        .rebate-yes { color: var(--accent-green); }
        .rebate-no { color: #f87171; }

        @media (max-width: 768px) {
            body { padding: 1rem 0.5rem; }
            header h1 { font-size: 2rem; }
            th, td { padding: 0.5rem; font-size: 0.9rem; }
        }
    </style>
</head>
<body>

<div class="container">
    <header>
        <h1>Canada EV <span>Buyer's Resource</span></h1>
        <p>Market snapshot of affordable electric cars & SUVs with calculated incentive rules.</p>
    </header>

    <!-- Filter Buttons -->
    <div class="filter-controls">
        <button class="filter-btn active" onclick="filterEVs('all')">All Budget Models</button>
        <button class="filter-btn" onclick="filterEVs('suv')">SUVs & Crossovers</button>
        <button class="filter-btn" onclick="filterEVs('car')">Sedans & Hatchbacks</button>
    </div>

    <!-- EV Showcase Grid -->
    <div class="showcase-grid" id="evGrid">
        <!-- Subaru -->
        <div class="ev-card" data-type="suv">
            <div>
                <h3>Subaru Uncharted <span class="badge badge-suv">SUV</span></h3>
                <div class="vendor">Subaru Canada</div>
                <div class="metrics">
                    <div>
                        <div class="metric-label">Base MSRP</div>
                        <div class="metric-value">$42,995</div>
                    </div>
                    <div>
                        <div class="metric-label">Max Range</div>
                        <div class="metric-value">Up to 496 km</div>
                    </div>
                </div>
            </div>
            <p style="color: var(--text-muted); font-size: 0.9rem;">Features native Tesla NACS port charging. High 208mm ground clearance tailored perfectly for tough Canadian trail environments.</p>
        </div>

        <!-- Kia EV4 -->
        <div class="ev-card" data-type="car">
            <div>
                <h3>Kia EV4 <span class="badge badge-car">Sedan</span></h3>
                <div class="vendor">Kia Canada</div>
                <div class="metrics">
                    <div>
                        <div class="metric-label">Base MSRP</div>
                        <div class="metric-value">$38,995</div>
                    </div>
                    <div>
                        <div class="metric-label">Max Range</div>
                        <div class="metric-value">Up to 552 km</div>
                    </div>
                </div>
            </div>
            <p style="color: var(--text-muted); font-size: 0.9rem;">Massive value setup fully eligible for the $5,000 Federal rebate. Outstanding 552 km long range rating on the Wind LR trim.</p>
        </div>

        <!-- VW ID.4 -->
        <div class="ev-card" data-type="suv">
            <div>
                <h3>Volkswagen ID.4 <span class="badge badge-suv">SUV</span></h3>
                <div class="vendor">Volkswagen</div>
                <div class="metrics">
                    <div>
                        <div class="metric-label">Base MSRP</div>
                        <div class="metric-value">$48,495</div>
                    </div>
                    <div>
                        <div class="metric-label">Max Range</div>
                        <div class="metric-value">Up to 468 km</div>
                    </div>
                </div>
            </div>
            <p style="color: var(--text-muted); font-size: 0.9rem;">Spacious, classic cargo utility offering up to 1,818L maximum trunk configuration. Well insulated cold-weather battery architecture.</p>
        </div>

        <!-- Tesla Model 3 -->
        <div class="ev-card" data-type="car">
            <div>
                <h3>Tesla Model 3 <span class="badge badge-car">Sedan</span></h3>
                <div class="vendor">Tesla</div>
                <div class="metrics">
                    <div>
                        <div class="metric-label">Base MSRP</div>
                        <div class="metric-value">$39,490</div>
                    </div>
                    <div>
                        <div class="metric-label">Max Range</div>
                        <div class="metric-value">Up to 570 km</div>
                    </div>
                </div>
            </div>
            <p style="color: var(--text-muted); font-size: 0.9rem;">Highly optimized performance software and instant Supercharger access. Imported platform completely exempt from federal rebates.</p>
        </div>
    </div>

    <!-- Comparative Table Section -->
    <div class="table-section">
        <h2>Head-to-Head: Premium Long Range Sedan Battle</h2>
        <table>
            <thead>
                <tr>
                    <th>Feature / Metric</th>
                    <th>2026 Kia EV4 (Wind LR)</th>
                    <th>2026 Tesla Model 3 (Base)</th>
                    <th>Edge</th>
                </tr>
            </thead>
            <tbody>
                <tr>
                    <td><strong>Base Sticker Price</strong></td>
                    <td>$42,995 CAD</td>
                    <td>$39,490 CAD</td>
                    <td>Tesla</td>
                </tr>
                <tr>
                    <td><strong>Federal iZEV Rebate</strong></td>
                    <td class="rebate-yes">Eligible (-$5,000)</td>
                    <td class="rebate-no">Ineligible ($0)</td>
                    <td class="winner-cell">Kia</td>
                </tr>
                <tr>
                    <td><strong>Effective Price</strong></td>
                    <td class="winner-cell">$37,995 CAD</td>
                    <td>$39,490 CAD</td>
                    <td class="winner-cell">Kia (-$1,500)</td>
                </tr>
                <tr>
                    <td><strong>Rated Driving Range</strong></td>
                    <td class="winner-cell">552 km</td>
                    <td>463 km</td>
                    <td class="winner-cell">Kia (+89 km)</td>
                </tr>
                <tr>
                    <td><strong>Charging Hardware</strong></td>
                    <td>Native NACS Port</td>
                    <td>Native NACS Port</td>
                    <td>Tie</td>
                </tr>
                <tr>
                    <td><strong>Battery Warranty</strong></td>
                    <td>10 Years / 160,000 km</td>
                    <td>8 Years / 160,000 km</td>
                    <td class="winner-cell">Kia</td>
                </tr>
            </tbody>
        </table>
    </div>
</div>

<script>
    function filterEVs(type) {
        // Toggle Active Button State
        const buttons = document.querySelectorAll('.filter-btn');
        buttons.forEach(btn => btn.classList.remove('active'));
        event.target.classList.add('active');

        // Filter Logic
        const cards = document.querySelectorAll('.ev-card');
        cards.forEach(card => {
            if (type === 'all' || card.getAttribute('data-type') === type) {
                card.style.display = 'flex';
            } else {
                card.style.display = 'none';
            }
        });
    }
</script>

</body>
</html>

```
