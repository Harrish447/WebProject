# Network Device Health Monitoring System

## Mini-Project Report — Network Management System (NMS)

---

## 1. Project Title

**Network Device Health Monitoring System**

A web-based network monitoring dashboard that tracks the health and performance of simulated network devices — routers, switches, and servers — in real time.

---

## 2. Abstract

Network administrators need continuous visibility into the health of infrastructure devices to prevent outages and performance degradation. This project presents a Network Device Health Monitoring System that simulates monitoring of network devices including routers, switches, and servers. The system evaluates device status based on CPU usage, memory usage, and reachability, automatically classifying each device as Online, Warning, Critical, or Offline. A real-time dashboard displays summary statistics, device cards with resource usage bars, fault alerts, and 24-hour performance trend charts. The system updates metrics periodically to simulate live monitoring and generates alerts when devices exceed configured thresholds. The application is built using React, TypeScript, Tailwind CSS, Chart.js, and Supabase (PostgreSQL) for data storage. The project demonstrates core network management concepts including device monitoring, threshold-based alerting, status evaluation algorithms, and performance visualization — making it suitable for a Network Management System course mini-project.

---

## 3. Introduction

In modern enterprise networks, the number of devices — routers, switches, servers, firewalls — can range from dozens to thousands. Monitoring each device manually is impractical. Network Management Systems (NMS) automate this by continuously polling devices for health metrics and presenting the data in a centralized dashboard.

This project implements a simplified NMS that monitors 8 simulated network devices. Each device has CPU usage, memory usage, uptime, and response time metrics. The system evaluates these metrics against configured thresholds to determine device status, generates alerts for devices exceeding thresholds, and displays everything in an interactive web dashboard with charts and notifications.

The goal is to demonstrate the core concepts of network monitoring — data collection, status evaluation, alerting, and visualization — in a functional, presentable application suitable for a college mini-project demonstration.

---

## 4. Problem Statement

In network environments, device failures and resource exhaustion can cause service outages, data loss, and user disruption. Without a monitoring system, administrators have no early warning of problems — they only learn about issues when users report them. There is a need for a system that:

- Continuously monitors device health metrics (CPU, memory, reachability)
- Automatically detects when devices exceed safe operating thresholds
- Alerts administrators to problems before they cause outages
- Presents device status in a clear, visual dashboard
- Maintains historical data for trend analysis

This project addresses these needs with a simulated monitoring system that demonstrates all these capabilities.

---

## 5. Objectives

1. To design and develop a web-based network device monitoring dashboard
2. To simulate monitoring of network devices (routers, switches, servers) with realistic metrics
3. To implement a status detection algorithm that classifies devices as Online, Warning, Critical, or Offline based on CPU and memory thresholds
4. To generate and display fault alerts for devices exceeding thresholds or going offline
5. To visualize device performance trends using Chart.js line charts
6. To implement filtering by device type and status, and searching by name or IP
7. To provide a responsive, professional user interface suitable for demonstration
8. To demonstrate core Network Management System concepts including monitoring, alerting, and visualization

---

## 6. Existing System and Proposed System

### Existing System

In many small to medium organizations, network monitoring is done manually:

- Administrators periodically log into individual devices to check status
- No centralized view of all devices
- Problems are discovered after they cause outages
- No historical data for trend analysis
- Time-consuming and error-prone

Enterprise NMS tools like Nagios, Zabbix, and PRTG exist but are complex to set up and require physical infrastructure.

### Proposed System

This project proposes a lightweight, web-based monitoring dashboard that:

- Provides a centralized view of all network devices in one dashboard
- Automatically evaluates device status using threshold-based rules
- Generates real-time alerts for problematic devices
- Displays performance trends over 24 hours using interactive charts
- Simulates live monitoring with periodic metric updates
- Requires no physical devices — all monitoring is simulated
- Is easy to set up and demonstrate in a classroom environment

---

## 7. System Architecture

### Architecture Diagram

```
┌─────────────────────────────────────────────────────────────────┐
│                        USER (Administrator)                      │
│                              │                                   │
│                              ▼                                   │
│  ┌──────────────────────────────────────────────────────────┐   │
│  │              WEB DASHBOARD (React + Tailwind)             │   │
│  │                                                            │   │
│  │  ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌────────────┐  │   │
│  │  │ Summary  │ │ Device   │ │ Alerts   │ │ Detail +   │  │   │
│  │  │ Cards    │ │ Cards    │ │ Panel    │ │ Chart.js   │  │   │
│  │  └──────────┘ └──────────┘ └──────────┘ └────────────┘  │   │
│  │                                                            │   │
│  │  ┌──────────┐ ┌──────────┐ ┌──────────┐                  │   │
│  │  │ Type     │ │ Status   │ │ Search   │                  │   │
│  │  │ Filters  │ │ Filters  │ │ Bar      │                  │   │
│  │  └──────────┘ └──────────┘ └──────────┘                  │   │
│  └──────────────────────────────────────────────────────────┘   │
│                              │                                   │
│              Read/Write via Supabase JS Client                   │
│                              │                                   │
│                              ▼                                   │
│  ┌──────────────────────────────────────────────────────────┐   │
│  │              SUPABASE (PostgreSQL Database)               │   │
│  │                                                            │   │
│  │  ┌─────────────┐     ┌──────────────────┐                │   │
│  │  │  devices     │     │ device_metrics    │                │   │
│  │  │──────────────│     │──────────────────│                │   │
│  │  │ id           │◄───►│ device_id (FK)   │                │   │
│  │  │ name         │     │ cpu_usage        │                │   │
│  │  │ type         │     │ memory_usage     │                │   │
│  │  │ ip_address   │     │ uptime_seconds   │                │   │
│  │  │ status       │     │ response_time_ms │                │   │
│  │  │ cpu_usage    │     │ recorded_at      │                │   │
│  │  │ memory_usage │     └──────────────────┘                │   │
│  │  │ uptime       │                                          │   │
│  │  │ last_checked │     Row Level Security: ENABLED          │   │
│  │  └─────────────┘     Policies: anon + authenticated       │   │
│  └──────────────────────────────────────────────────────────┘   │
│                              ▲                                   │
│                              │                                   │
│  ┌──────────────────────────────────────────────────────────┐   │
│  │           MONITORING ENGINE (Client-Side)                 │   │
│  │                                                            │   │
│  │  1. Simulate metric updates (CPU, memory) every 5s        │   │
│  │  2. Evaluate status using threshold rules                  │   │
│  │  3. Generate alerts for threshold violations               │   │
│  │  4. Persist updated metrics to Supabase                    │   │
│  │  5. Update dashboard + show notifications                  │   │
│  └──────────────────────────────────────────────────────────┘   │
│                                                                    │
│  ┌──────────────────────────────────────────────────────────┐   │
│  │         SIMULATED NETWORK DEVICES (8 devices)             │   │
│  │                                                            │   │
│  │  Routers:        Core Router 01, Core Router 02           │   │
│  │  Switches:       Access Switch 01, Access Switch 02       │   │
│  │  Servers:        App Server 01, App Server 02,            │   │
│  │                  Backup Server 01, Database Server 01     │   │
│  └──────────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────────┘
```

### Data Flow

1. **Monitoring Engine** simulates polling each device every 5 seconds
2. CPU and memory metrics are randomly fluctuated within realistic ranges
3. **Status Evaluation** algorithm checks thresholds and assigns status
4. Updated metrics are written to the **Supabase database** (devices + device_metrics tables)
5. **Dashboard** reads from the database and displays updated device cards
6. **Alert Generator** checks all devices and creates alerts for violations
7. **Alerts Panel** displays active faults sorted by severity
8. **Toast Notifications** inform the user of new faults or successful refreshes
9. **Chart.js** renders 24-hour trend charts in the device detail panel

---

## 8. Technologies Used

| Category | Technology | Purpose |
|----------|-----------|---------|
| Frontend Framework | React 18 | UI component-based architecture |
| Language | TypeScript | Type-safe development |
| Styling | Tailwind CSS 3 | Utility-first responsive styling |
| Charts | Chart.js 4 | Performance trend line charts |
| Icons | Lucide React | UI icons |
| Backend/Database | Supabase (PostgreSQL) | Data persistence + RLS security |
| Build Tool | Vite 5 | Fast development server + bundling |
| Fonts | Inter (Google Fonts) | Clean, professional typography |

---

## 9. Implementation

### 9.1 Database Schema

Two tables are created in Supabase PostgreSQL:

**devices table:** Stores the 8 network devices with current metrics
- `id` (UUID, primary key)
- `name` (text) — device name
- `type` (text) — router, switch, or server
- `ip_address` (text) — device IP
- `location` (text) — physical location
- `status` (text) — online, warning, critical, or offline
- `cpu_usage` (numeric) — current CPU percentage
- `memory_usage` (numeric) — current memory percentage
- `uptime_seconds` (bigint) — device uptime
- `response_time_ms` (integer) — ping response time
- `last_checked_at` (timestamptz) — last monitoring check

**device_metrics table:** Time-series metrics for trend charts
- `id` (UUID, primary key)
- `device_id` (UUID, foreign key → devices)
- `cpu_usage`, `memory_usage`, `uptime_seconds`, `response_time_ms`
- `recorded_at` (timestamptz)

Row Level Security (RLS) is enabled on both tables with `anon, authenticated` policies allowing full CRUD (single-tenant, no auth required).

### 9.2 Status Detection Algorithm

```python
function evaluateStatus(cpu, memory, isOnline):
    if not isOnline:
        return "offline"
    if cpu >= 90 or memory >= 95:
        return "critical"
    if cpu >= 70 or memory >= 80:
        return "warning"
    return "online"
```

Priority: Offline > Critical > Warning > Online

### 9.3 Simulated Monitoring Engine

- Runs every 5 seconds when "Live" mode is enabled
- For each online device, CPU and memory are fluctuated by ±6% and ±4% respectively
- Values are clamped to 0-100%
- Status is re-evaluated after each update
- Offline devices remain offline with zeroed metrics
- Updated values are persisted to the Supabase database
- A new metric record is inserted into device_metrics for history

### 9.4 Alert Generation

For each device, alerts are generated based on status:
- **Offline:** "Device is unreachable — no response to monitoring checks"
- **Critical (CPU):** "CPU usage critically high at X% (threshold: 90%)"
- **Critical (Memory):** "Memory usage critically high at X% (threshold: 95%)"
- **Warning (CPU):** "CPU usage elevated at X% (threshold: 70%)"
- **Warning (Memory):** "Memory usage elevated at X% (threshold: 80%)"

Alerts are sorted by severity: Offline > Critical > Warning

### 9.5 Frontend Components

| Component | Responsibility |
|-----------|---------------|
| Dashboard | Main layout, state management, monitoring loop, filtering |
| DeviceCard | Individual device display with CPU/memory bars + alert badge |
| DeviceDetail | Full device info + Chart.js 24-hour trend chart |
| AlertsPanel | Sorted list of active fault alerts |
| Toast | Notification system for refresh/fault events |

---

## 10. Results and Discussion

### Results

The system successfully monitors 8 simulated network devices with the following outcomes:

1. **All 8 devices are displayed** on the dashboard with correct status badges
2. **Status detection works correctly:**
   - Core Router 01 (CPU 32%, Mem 58%) → Online
   - Core Router 02 (CPU 78%, Mem 82%) → Warning (CPU ≥ 70%)
   - Access Switch 02 (CPU 92%, Mem 88%) → Critical (CPU ≥ 90%)
   - App Server 02 → Offline (unreachable)
3. **Summary cards** accurately reflect device status counts
4. **Filters** correctly narrow devices by type and status
5. **Search** finds devices by name or IP address
6. **Live monitoring** updates CPU/memory every 5 seconds with visible changes
7. **Alerts panel** displays 4-6 active alerts (offline + critical + warning devices)
8. **Chart.js trend chart** displays 24-hour CPU and memory history
9. **Toast notifications** appear on refresh and when new faults are detected
10. **Responsive design** adapts from desktop grid to mobile single-column

### Discussion

The system demonstrates all core NMS concepts:
- **Monitoring:** Periodic data collection from simulated devices
- **Threshold-based alerting:** Automatic status classification and alert generation
- **Visualization:** Interactive charts and progress bars
- **Centralized dashboard:** All device information in one view
- **Historical data:** 24-hour trend analysis capability

The simulation approach is appropriate for a college project as it demonstrates all concepts without requiring physical infrastructure. In a production environment, the monitoring engine would be replaced with SNMP polling or agent-based data collection from real devices.

---

## 11. Limitations

1. **Simulated data only** — No real network devices are monitored. All metrics are randomly generated.
2. **No SNMP/agent integration** — Real NMS tools use SNMP, WMI, or agents to collect data from physical devices.
3. **Single-tenant** — No user authentication; the dashboard is open to anyone with the URL.
4. **Limited history** — Only 24 hours of historical data is displayed in the trend chart.
5. **No email/SMS alerts** — Alerts are displayed only in the dashboard, not sent via external channels.
6. **No auto-recovery** — Offline devices remain offline; no simulation of device recovery.
7. **Browser-based monitoring** — The monitoring loop runs in the browser; if the tab is closed, monitoring stops.

---

## 12. Conclusion

The Network Device Health Monitoring System successfully demonstrates the core concepts of network management including device monitoring, threshold-based status detection, fault alerting, and performance visualization. The system monitors 8 simulated network devices (routers, switches, and servers) and provides a professional, responsive web dashboard with real-time updates, interactive charts, and automatic alert generation.

The project achieves all its objectives: it implements a working status detection algorithm, generates alerts for devices exceeding CPU and memory thresholds, displays performance trends using Chart.js, and provides filtering and search capabilities. The system is functional, easy to demonstrate, and suitable for a Network Management System course mini-project presentation.

Future enhancements could include: integration with SNMP for real device monitoring, user authentication, email/SMS alert notifications, longer historical data retention, device auto-recovery simulation, and multi-tenant support for monitoring multiple network sites.

---

## 13. References

1. Stallings, W. (2013). *SNMP, SNMPv2, SNMPv3, and RMON 1 and 2*. Addison-Wesley.
2. Subramanian, M. (2000). *Network Management: Principles and Practice*. Pearson.
3. Chart.js Documentation — https://www.chartjs.org/docs/
4. React Documentation — https://react.dev/
5. Tailwind CSS Documentation — https://tailwindcss.com/docs
6. Supabase Documentation — https://supabase.com/docs
7. Lucide Icons — https://lucide.dev/

---

## 14. Sample Test Cases

| # | Test Case | Input | Expected Result | Status |
|---|-----------|-------|-----------------|--------|
| 1 | Online device | CPU 32%, Memory 58% | Status = Online (green badge) | PASS |
| 2 | High CPU warning | CPU 78%, Memory 82% | Status = Warning (amber badge) | PASS |
| 3 | High memory warning | CPU 45%, Memory 85% | Status = Warning (memory ≥ 80%) | PASS |
| 4 | Critical device | CPU 92%, Memory 88% | Status = Critical (red badge) | PASS |
| 5 | Offline device | Device unreachable | Status = Offline (grey badge, CPU/Mem = 0) | PASS |
| 6 | Device type filtering | Click "Routers" tab | Only router devices displayed | PASS |
| 7 | Status filtering | Click "Critical" filter | Only critical devices displayed | PASS |
| 8 | Search by IP | Type "10.0.0" in search | Devices with 10.0.0.x IPs shown | PASS |
| 9 | Refresh operation | Click "Refresh" button | Metrics update, toast notification appears | PASS |
| 10 | Live monitoring | Enable "Live" toggle | CPU/memory bars change every 5 seconds | PASS |
| 11 | Alert generation | Device goes critical | Alert appears in alerts panel | PASS |
| 12 | Chart display | Click a device card | 24-hour CPU/memory trend chart renders | PASS |

---

## 15. Screenshot Capture Instructions

To capture screenshots for your project report:

1. **Start the application** — Run `npm run dev` and open the URL in your browser

2. **Full Dashboard Screenshot:**
   - Open the dashboard, wait for all devices to load
   - Take a full-page screenshot (or browser screenshot)
   - Caption: "Figure 1: Network Health Monitor Dashboard"

3. **Summary Cards:**
   - Scroll to show the 5 summary cards at the top
   - Screenshot just that section
   - Caption: "Figure 2: Device Status Summary Cards"

4. **Device Cards with CPU/Memory Bars:**
   - Show the device grid with progress bars visible
   - Caption: "Figure 3: Device Cards with Resource Usage"

5. **Filters in Action:**
   - Click "Routers" tab — screenshot
   - Click "Critical" status filter — screenshot
   - Caption: "Figure 4: Device Type and Status Filtering"

6. **Search Functionality:**
   - Type "10.0.0" in the search bar
   - Screenshot showing filtered results
   - Caption: "Figure 5: Search by IP Address"

7. **Device Detail with Chart:**
   - Click on any device card (e.g., Core Router 01)
   - Show the detail panel with the Chart.js trend chart
   - Caption: "Figure 6: Device Detail with 24-Hour Performance Chart"

8. **Fault Alerts Panel:**
   - Show the alerts panel with active alerts listed
   - Caption: "Figure 7: Fault Alerts Panel"

9. **Live Monitoring:**
   - Enable "Live" mode
   - Take two screenshots 5 seconds apart showing changed CPU/memory values
   - Caption: "Figure 8: Live Monitoring — Before and After Update"

10. **Toast Notifications:**
    - Click "Refresh" button
    - Screenshot the toast notification appearing top-right
    - Caption: "Figure 9: Refresh Notification"

11. **Alert Indicators:**
    - Show device cards with red alert badges
    - Caption: "Figure 10: Alert Indicators on Device Cards"

12. **Responsive Design:**
    - Resize browser to mobile width
    - Screenshot showing single-column layout
    - Caption: "Figure 11: Responsive Mobile Layout"

**Tips for good screenshots:**
- Use a clean browser window (no bookmarks bar visible)
- Zoom to 100% for consistent sizing
- Use Windows Snipping Tool or Win+Shift+S
- Save as PNG for clarity
- Crop unnecessary whitespace
