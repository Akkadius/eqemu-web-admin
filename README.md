
# Occulus | EQEmulator Web Admin Panel

**Drone Build Status**

[![Build Status](https://cloud.drone.io/api/badges/Akkadius/eqemu-web-admin/status.svg)](https://cloud.drone.io/Akkadius/eqemu-web-admin) 

<hr>

- [What is Occulus?](#what-is-occulus)
- [Why Occulus?](#why-occulus)
- [Using Occulus](#using-occulus)
- [Features](#features)
    + [Dashboard](#dashboard)
    + [Start / Stop / Restart](#start--stop--restart)
    + [Intelligent Dynamic Server Launcher](#intelligent-dynamic-server-launcher)
    + [Quest Script Hot Reloading](#quest-script-hot-reloading)
    + [Configuration](#configuration)
    + [Manual Backups](#manual-backups)
    + [Client File Exports](#client-file-exports)
- [Requirements](#requirements)
- [Installation](#installation)
  * [Run the Web Interface](#run-the-web-interface)
  * [Post Launch](#post-launch)
- [Feature Requests](#feature-requests)
- [Contributing](#contributing)
- [Developing](#developing)

<hr>

<p align="center">
  <a href="https://nodejs.org/">
    <img
      alt="Node.js"
      src="https://nodejs.org/static/images/logo-light.svg"
      width="400"
    />
  </a>
    <a href="https://vuejs.org" target="_blank" rel="noopener noreferrer"><img width="250" src="https://vuejs.org/images/logo.png" alt="Vue logo">
    
</p>

<hr>

![image](https://user-images.githubusercontent.com/3319450/87236540-8c13f500-c3b0-11ea-87f6-756e60fa61ed.png)

![image](https://user-images.githubusercontent.com/3319450/87236581-d39a8100-c3b0-11ea-94e3-6224330a8b4e.png)

# What is Occulus?

A web based administrator console for [EverQuest Emulator servers](https://github.com/EQEmu/Server)

# Why Occulus?

To simplify administrative and development needs of operators

# Using Occulus

Occulus is open source, built to help others in the EverQuest Emulator community. If you use this admin panel, please remember to contribute back to the community in whichever  way that you can

# Features

### Dashboard

* Statistics
* Players Online
* Server Process counts
* System Info (CPU / Disk / Network)
* Process Management (Server Start, Stop, Reboot)

### Start / Stop / Restart

Start, stop, restart your server with ease

![image](https://user-images.githubusercontent.com/3319450/87236870-fb3f1880-c3b3-11ea-9702-5584cd75867c.png)

### Intelligent Dynamic Server Launcher

Only boot as many zones as you need to be idle at a time (Default: 3) (Shown: 6)

As more zones become booted by players; the server launcher will spawn additional to keep the standby pool

![image](https://user-images.githubusercontent.com/3319450/87236671-94206480-c3b1-11ea-89fb-4110270802f1.png)

### Quest Script Hot Reloading

Instantly reload zone quests, repop when quest files are changed, with configurable behaviors via server rules

<p align="center"><a href="https://vuejs.org" target="_blank" rel="noopener noreferrer"><img width="700" src="https://user-images.githubusercontent.com/3319450/87236747-7e5f6f00-c3b2-11ea-84c0-0bfd8437e2a9.png" alt="Vue logo"></a></p>

<p align="center"><a href="https://vuejs.org" target="_blank" rel="noopener noreferrer"><img width="700" src="https://user-images.githubusercontent.com/3319450/87236754-891a0400-c3b2-11ea-84d6-c923af0b0727.png" alt="Vue logo"></a></p>

### Configuration

Easily edit your server configuration

![image](https://user-images.githubusercontent.com/3319450/87236778-dc8c5200-c3b2-11ea-8a9a-69c79731ec82.png)

### Manual Backups

Manually backup your server assets with a click of a button

![image](https://user-images.githubusercontent.com/3319450/87236781-f9288a00-c3b2-11ea-96d4-505f313f300b.png)

### Client File Exports

Tired of constantly manually exporting new client text file changes? Give your players a direct link that exports fresh data and only refreshes once a minute to prevent constant re-exports

![image](https://user-images.githubusercontent.com/3319450/87236783-fded3e00-c3b2-11ea-9fee-28c8f7be8938.png)

# Requirements

* Linux or Windows
* An installation of the emulator server which runs server binaries out of the `bin` folder
* Server code that has been kept up to date as there are backend API's that the admin panel relies on the function to the fullest

# Installation

As of this writing the admin launcher is not included with the server installer, but there is plans for it to be. 

In order to install this admin panel you'll need to download a **Windows** or **Linux** release from [Releases](https://github.com/Akkadius/eqemu-web-admin/releases)

## Run the Web Interface

When you have the binary installed to the `bin` directory, you can run the web server. You can optionally pass a port number to run it on a different port

An example of what this looks like:

```
eqemu@f8905f80723c:~/server$ ./bin/eqemu-admin web
[www] Listening on port 3000
[database] MySQL Attempting to connect (default) | Host [mariadb:3306] Database [peq] User [eqemu]
[Process Manager] Starting server launcher...
[database] MySQL Connected (default) | Host [mariadb:3306] Database [peq] User [eqemu]
[database] MySQL Attempting to connect (content) | Host [content-cdn.projecteq.net:16033] Database [peq_content] User [peq_server_tgc]
[database] MySQL Connected (content) | Host [content-cdn.projecteq.net:16033] Database [peq_content] User [peq_server_tgc]
[HRM] Starting HRM listener (Hot-Reload Module) v1.0
[HRM] Watching scripts [/home/eqemu/server/quests]
[HRM] Watching scripts [/home/eqemu/server/lua_modules]
[HRM] Watching scripts [/home/eqemu/server/plugins]
[EQEmuConfig] File change detected, reloading...
GET /api/v1/server/process_counts 200 28.475 ms - 59
GET /api/v1/world/clients 200 51.359 ms - 711026
GET /api/v1/server/process_counts 200 31.330 ms - 59
GET /api/v1/world/clients 200 53.814 ms - 710599
GET /api/v1/server/sysinfo 200 46.122 ms - 6712
GET /api/v1/server/sysinfo 200 51.343 ms - 6712
GET /api/v1/server/sysinfo 200 47.942 ms - 6712
```

That's it! You're ready to go! 

You will need to start the admin panel using Microsoft Services for Windows or using a process manager for Linux. Instructions for making this more "installable" as a system service will come later (Or feel free to submit a PR)

## Post Launch

Your admin panel will initialize itself when first booted. It will create an admin password you can find in your **eqemu_config.json** that can be found in its own section of the config

Different sections of the configuration will be intialize on first-use. For example the launcher configuration won't be present until you try to use the Start / Stop / Restart features of the admin panel

```
eqemu@f8905f80723c:~/server$ cat eqemu_config.json | jq '.["web-admin"]'
{
  "application": {
    "key": "<auto-generated-key>",
    "admin": {
      "password": "<auto-generated-password>"
    }
  },
  "launcher": {
    "minZoneProcesses": 3,
    "runSharedMemory": true,
    "runLoginserver": false,
    "runQueryServ": false,
    "isRunning": true
  },
  "serverCodePath": "/home/eqemu/code/"
}
```

# Feature Requests

Want a feature that isn't already available? Open an issue with the title "[Feature Request]" and we will see about getting it added

# Contributing

If you want to contribute to the admin panel, please submit **Pull Requests**

Occulus is open source, built to help others in the EverQuest Emulator community. If you use this admin panel, please remember to contribute back to the community in whichever way that you can

# Developing

To develop the admin panel, clone the repository in the base of your EQEmulator server

The project is a Vue SPA with a NodeJS backend; so you will need to have two watcher processes watching for file changes to see the changes locally

Make sure you have [NodeJS](https://nodejs.org/en/download/) installed and install nodemon `npm install -g nodemon`

## Create Watches

For the backend, at the base of the project run `npm run watch`

```
eqemu@e155719debeb:~/server/eqemu-web-admin$ npm run watch

> eqemu-admin@2.0.13 watch /home/eqemu/server/eqemu-web-admin
> nodemon --ignore public/ --exec node ./app/bin/admin web

[nodemon] 2.0.4
[nodemon] to restart at any time, enter `rs`
[nodemon] watching path(s): *.*
[nodemon] watching extensions: js,mjs,json
[nodemon] starting `node ./app/bin/admin web`
[www] Listening on port 3000
[database] MySQL Attempting to connect (default) | Host [mariadb:3306] Database [peq] User [eqemu]
[database] MySQL Connected (default) | Host [mariadb:3306] Database [peq] User [eqemu]
[HRM] Starting HRM listener (Hot-Reload Module) v1.0
[HRM] Watching scripts [/home/eqemu/server/quests]
[HRM] Watching scripts [/home/eqemu/server/lua_modules]
[HRM] Watching scripts [/home/eqemu/server/plugins]
```

For the frontend, you will run `npm run serve`

```
eqemu@e155719debeb:~/server/eqemu-web-admin/frontend$ npm run serve                                                                                               
DONE  Compiled successfully in 5744ms
 
No type errors found
Version: typescript 3.4.5
Time: 3661ms

  App running at:
  - Local:   http://localhost:8080/

  It seems you are running Vue CLI inside a container.
  Access the dev server via http://localhost:<your container's external mapped port>/

  Note that the development build is not optimized.
  To create a production build, run npm run build.
```

## Frontend Development Configuration

**Note** For the frontend, you will need to make sure that you set `VUE_APP_BACKEND_BASE_URL` environment variable in a `.env` file at the base of the `frontend` folder, this will tell your web client what endpoint it needs to talk to for API calls

`eqemu@e155719debeb:~/server/eqemu-web-admin/frontend$ cp .env.example .env`

In my case; I have my development environment living in a VM and I need to point my web client to where the backend lives

```
VUE_APP_BACKEND_BASE_URL=http://192.168.98.200:3000
```

The VM is 192.168.98.200, usually if you have this running on the same machine, you would just use `localhost`

If you have are developing against a backend that is remote, you will need to make sure that you add your host / ip to the CORS config on the backend under `app.js` (temporarily)

Other than these small considerations, you should be on your way to developing fairly quickly

```
var cors = require('cors')
app.use(
  cors(
    {
      origin: [
        'http://localhost:8080',
        'http://192.168.98.200:8080',
        'http://docker:8080',
        'http://localhost:5000'
      ],
      exposedHeaders: ['Content-Disposition']
    }
  )
);
```
