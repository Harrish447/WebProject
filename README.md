# Network Device Health Monitoring System

A complete, functional network monitoring dashboard for the Network Management System (NMS) mini-project. Monitors simulated routers, switches, and servers with real-time status detection, alerts, and performance charts.

## Tech Stack

- **Frontend:** React 18 + TypeScript + Tailwind CSS
- **Charts:** Chart.js
- **Icons:** Lucide React
- **Backend/Data:** Supabase (PostgreSQL)
- **Build Tool:** Vite

## Features

1. **Dashboard with summary cards** — Total, Online, Warning, Critical, Offline device counts
2. **Device cards** — CPU/memory bars, status badges, uptime, IP address, last-checked time
3. **Navigation tabs** — All Devices, Routers, Switches, Servers (with counts)
4. **Status filters** — All, Online, Warning, Critical, Offline
5. **Search bar** — Search by device name or IP address
6. **Device detail panel** — Full metrics, 24-hour Chart.js trend graph, avg/peak stats
7. **Simulated live monitoring** — CPU/memory auto-update every 5 seconds (toggleable)
8. **Manual refresh** — Click refresh to force a monitoring cycle
9. **Fault alerts panel** — Severity, device name, issue description, timestamp
10. **Toast notifications** — Success/warning messages on refresh and fault detection
11. **Alert indicators** — Red pulsing badge on device cards with active alerts
12. **Responsive design** — Works on laptop and mobile screens

## Device Status Detection Algorithm

```
IF device is unreachable:
    status = OFFLINE
ELSE IF CPU >= 90% OR Memory >= 95%:
    status = CRITICAL
ELSE IF CPU >= 70% OR Memory >= 80%:
    status = WARNING
ELSE:
    status = ONLINE
```

- Critical takes priority over Warning
- Offline devices stay Offline regardless of CPU/memory values

## Sample Devices

| # | Name | Type | IP | Status |
|---|------|------|----|--------|
| 1 | Core Router 01 | Router | 10.0.0.1 | Online |
| 2 | Core Router 02 | Router | 10.0.0.2 | Warning |
| 3 | Access Switch 01 | Switch | 10.0.1.10 | Online |
| 4 | Access Switch 02 | Switch | 10.0.1.11 | Critical |
| 5 | App Server 01 | Server | 10.0.2.10 | Online |
| 6 | App Server 02 | Server | 10.0.2.11 | Offline |
| 7 | Backup Server 01 | Server | 10.0.3.50 | Online |
| 8 | Database Server 01 | Server | 10.0.3.20 | Warning |

## How to Run

### Prerequisites
- Node.js 18+ installed
- Visual Studio Code (recommended)

### Steps

1. **Open the project folder** in VS Code

2. **Open a terminal** in VS Code (Ctrl + ` or Terminal > New Terminal)

3. **Install dependencies:**
   ```
   npm install
   ```

4. **Start the development server:**
   ```
   npm run dev
   ```

5. **Open the browser** to the URL shown in the terminal (typically `http://localhost:5173`)

## How to Test Every Feature

| Feature | How to Test |
|---------|------------|
| Summary cards | Check top of dashboard — counts should match device statuses |
| Navigation tabs | Click "Routers", "Switches", "Servers" — only that type shows |
| Status filters | Click "Critical" — only critical devices appear |
| Search bar | Type "10.0.0" — only devices with that IP pattern show |
| Device details | Click any device card — detail panel opens on right |
| Chart.js chart | In detail panel, see the 24-hour CPU/memory trend line chart |
| Live monitoring | Toggle "Live" button on — watch CPU/memory bars change every 5 seconds |
| Manual refresh | Click "Refresh" button — metrics update, toast notification appears |
| Fault alerts | Check alerts panel below filters — lists all warning/critical/offline issues |
| Alert indicators | Red pulsing badge on device cards with active alerts |
| Toast notifications | Click refresh — success or warning message appears top-right |
| Responsive | Resize browser window — layout adapts for mobile |

## Troubleshooting

| Problem | Solution |
|---------|----------|
| Blank page | Check terminal for errors, ensure `npm install` ran successfully |
| Port already in use | Change port in `vite.config.ts` or close other dev servers |
| Build errors | Run `npm run build` to see detailed error messages |
| Data not loading | Ensure network connection is available (Supabase is cloud-hosted) |

## Build for Production

```
npm run build
```
Output goes to `dist/` folder. Preview with `npm run preview`.

## Project Structure

```
network-health-monitor/
├── src/
│   ├── components/
│   │   ├── Dashboard.tsx       # Main dashboard layout
│   │   ├── DeviceCard.tsx      # Individual device card
│   │   ├── DeviceDetail.tsx    # Detail panel with Chart.js
│   │   ├── AlertsPanel.tsx     # Fault alerts list
│   │   ├── Toast.tsx           # Notification system
│   │   └── MetricChart.tsx     # Legacy SVG chart (unused, kept for reference)
│   ├── lib/
│   │   ├── supabase.ts         # Supabase client + types
│   │   ├── monitoring.ts       # Status detection + alert generation
│   │   └── utils.ts            # Formatting helpers + color config
│   ├── App.tsx
│   ├── main.tsx
│   └── index.css
├── index.html
├── package.json
├── README.md
└── report.md
```

## Note

This application monitors **simulated** network devices. No physical routers, switches, or servers are required. All data is generated and stored in a Supabase PostgreSQL database. The simulated monitoring engine updates CPU and memory metrics periodically to demonstrate real-time monitoring behavior.
