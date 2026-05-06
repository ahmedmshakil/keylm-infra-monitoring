# KeyLM Infra Monitoring

This repository provides comprehensive infrastructure monitoring for the KeyLM project using Prometheus and Grafana. It allows real-time tracking of system performance, application metrics, and database queries.

## Live Site & Configuration

- **Live Website:** [https://keylm.shakilahmed.tech/](https://keylm.shakilahmed.tech/)
- **Setup Guide:** Check out the **[SETUP.md](./SETUP.md)** file for detailed instructions on configuring and running this project.

## Dashboard Overview

Explore the various monitoring visualizations available in our deployment. Click on any dashboard preview below for a closer look.

<div align="center">
  <table>
    <tr>
      <td colspan="2" align="center">
        <b>Main Setup Dashboard</b><br>
        <a href="./img/keylm_grafana_dashboard.png">
          <img src="./img/keylm_grafana_dashboard.png" width="900" alt="Main Dashboard">
        </a><br>
        <i>Overview of the KeyLM main application status and general configuration.</i>
      </td>
    </tr>
    <tr>
      <td align="center">
        <b>OS Monitor</b><br>
        <a href="./img/keylm_grafana_os_monitor_01.png">
          <img src="./img/keylm_grafana_os_monitor_01.png" width="650" alt="OS Monitor 01">
        </a><br>
        <i>Detailed operating system metrics including disk I/O and network traffic trends.</i>
      </td>
      <td align="center">
        <b>Network & Disk Monitor</b><br>
        <a href="./img/keylm_grafana_os_monitor_02.png">
          <img src="./img/keylm_grafana_os_monitor_02.png" width="650" alt="OS Monitor 02">
        </a><br>
        <i>Additional OS-level visualizations tracking process counts and system uptime elements.</i>
      </td>
    </tr>
    <tr>
      <td align="center">
        <b>Database Queries</b><br>
        <a href="./img/keylm_grafana_queries.png">
          <img src="./img/keylm_grafana_queries.png" width="650" alt="Database Queries">
        </a><br>
        <i>Tracks database query performance, latency, and execution times on our servers.</i>
      </td>
      <td align="center">
        <b>Prometheus Up Status</b><br>
        <a href="./img/keylm_prometheus_status.png">
          <img src="./img/keylm_prometheus_status.png" width="650" alt="Prometheus Status">
        </a><br>
        <i>Shows the real-time health and scrape configuration status of all metrics targets.</i>
      </td>
    </tr>
    <tr>
      <td align="center">
        <b>Go Runtime Metrics</b><br>
        <a href="./img/keylm_grafana_go_metrics.png">
          <img src="./img/keylm_grafana_go_metrics.png" width="650" alt="Go Metrics">
        </a><br>
        <i>Monitors Go application performance including memory usage and garbage collection.</i>
      </td>
      <td align="center">
        <b>General System Metrics</b><br>
        <a href="./img/keylm_grafana_metrics.png">
          <img src="./img/keylm_grafana_metrics.png" width="650" alt="General Metrics">
        </a><br>
        <i>Displays system-level resource utilization like CPU, RAM, and container metrics.</i>
      </td>
    </tr>
  </table>
</div>
