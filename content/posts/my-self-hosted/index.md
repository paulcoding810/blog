---
date: '2024-09-08T10:06:54+07:00'
draft: false
title: 'My Self-hosted Services'
summary: 'List of my self-hosted containers and their purposes'
categories:
- Code
tags:
- self-hosted
- docker
---

This page lists the various self-hosted services I run on my home server. Each service is containerized using Docker for easy management and isolation.

![homepage](./homepage.png)

## Network

{{< icon src="https://cdn.jsdelivr.net/gh/walkxcode/dashboard-icons/png/pi-hole.png" alt="Pi-hole" width="16" height="16" >}} [Pi-hole](https://pi-hole.net/)
: Network-wide ad blocking and DNS sinkhole. It blocks ads and trackers at the network level, improving browsing speed and privacy.

## Tools

{{< icon src="https://cdn.jsdelivr.net/gh/walkxcode/dashboard-icons/png/homepage.png" alt="HomePage" width="16" height="16" >}} [HomePage](https://github.com/gethomepage/homepage)
: A modern, fully static, fast, secure fully proxied, highly customizable application dashboard. It provides a central hub for accessing all my self-hosted services.

{{< icon src="https://cdn.jsdelivr.net/gh/walkxcode/dashboard-icons/png/portainer.png" alt="Portainer" width="16" height="16" >}} [Portainer](https://www.portainer.io/)
: Making Docker and Kubernetes management easy. It provides a web interface for managing Docker environments.

{{< icon src="https://cdn.jsdelivr.net/gh/walkxcode/dashboard-icons/png/code-server.png" alt="Code Server" width="16" height="16" >}} [Code Server](https://github.com/coder/code-server)
: VS Code in the browser. Allows me to code from any device with a web browser.

{{< icon src="https://cdn.jsdelivr.net/gh/walkxcode/dashboard-icons/png/jdownloader.png" alt="JDownloader" width="16" height="16" >}} [JDownloader](https://jdownloader.org/)
: Download management tool. Automates and manages downloads from various sources.

{{< icon src="https://cdn.jsdelivr.net/gh/walkxcode/dashboard-icons/png/gitea.png" alt="Gitea" width="16" height="16" >}} [Gitea](https://about.gitea.com/)
: A painless self-hosted Git service. Provides a GitHub-like interface for personal projects.

{{< icon src="https://cdn.jsdelivr.net/gh/walkxcode/dashboard-icons/png/cloudcmd.png" alt="Cloud Commander" width="16" height="16" >}} [Cloud Commander](https://cloudcmd.io/)
: Web file manager with console and editor. Allows remote file management through a web interface.

{{< icon src="https://cdn.jsdelivr.net/gh/walkxcode/dashboard-icons/png/qbittorrent.png" alt="qBittorrent" width="16" height="16" >}} [qBittorrent](https://www.qbittorrent.org/)
: Free and reliable P2P BitTorrent client. Used for downloading and seeding torrents.

## Monitors

{{< icon src="https://cdn.jsdelivr.net/gh/walkxcode/dashboard-icons/png/glances.png" alt="Glances" width="16" height="16" >}} [Glances](https://nicolargo.github.io/glances/)
: Cross-platform system monitoring tool. Provides a comprehensive view of system resources.

{{< icon src="https://cdn.jsdelivr.net/gh/walkxcode/dashboard-icons/png/speedtest-tracker.png" alt="Speedtest Tracker" width="16" height="16" >}} [Speedtest Tracker](https://github.com/henrywhitaker3/Speedtest-Tracker)
: Continuously track your internet speed. Helps monitor ISP performance over time.

{{< icon src="https://cdn.jsdelivr.net/gh/walkxcode/dashboard-icons/png/scrutiny.png" alt="Scrutiny" width="16" height="16" >}} [Scrutiny](https://github.com/AnalogJ/scrutiny)
: WebUI for smartd S.M.A.R.T monitoring. Helps predict and prevent disk failures.

## Media

{{< icon src="https://cdn.jsdelivr.net/gh/walkxcode/dashboard-icons/png/jellyfin.png" alt="Jellyfin" width="16" height="16" >}} [Jellyfin](https://jellyfin.org/)
: The Free Software Media System. Streams my personal media collection to various devices.

{{< icon src="https://cdn.jsdelivr.net/gh/walkxcode/dashboard-icons/png/sonarr.png" alt="Sonarr" width="16" height="16" >}} [Sonarr](https://sonarr.tv/)
: Smart PVR for newsgroup and bittorrent users. Automates TV show downloads and organization.

{{< icon src="https://cdn.jsdelivr.net/gh/walkxcode/dashboard-icons/png/radarr.png" alt="Radarr" width="16" height="16" >}} [Radarr](https://radarr.video/)
: A fork of Sonarr to work with movies. Automates movie downloads and organization.

{{< icon src="https://cdn.jsdelivr.net/gh/walkxcode/dashboard-icons/png/bazarr.png" alt="Bazarr" width="16" height="16" >}} [Bazarr](https://www.bazarr.media/)
: Companion application to Sonarr and Radarr for managing subtitles. Automatically downloads and manages subtitles for my media.

{{< icon src="https://cdn.jsdelivr.net/gh/walkxcode/dashboard-icons/png/prowlarr.png" alt="Prowlarr" width="16" height="16" >}} [Prowlarr](https://github.com/Prowlarr/Prowlarr)
: Indexer manager/proxy for Sonarr, Radarr, Lidarr, etc. Centralizes and manages indexers for various *arr applications.

{{< icon src="https://cdn.jsdelivr.net/gh/walkxcode/dashboard-icons/png/lidarr.png" alt="Lidarr" width="16" height="16" >}} [Lidarr](https://lidarr.audio/)
: Music collection manager for Usenet and BitTorrent users. Automates music downloads and organization.
