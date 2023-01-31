<bold>

# Heliactyl Canary
</bold>

![GitHub release (release name instead of tag name)](https://img.shields.io/github/v/release/GHostload/Heliactyl-Canary?display_name=release&include_prereleases)

![GitHub](https://img.shields.io/github/license/GHostload/Heliactyl-Canary)
![GitHub all releases](https://img.shields.io/github/downloads/GHostload/Heliactyl-Canary/total)
![GitHub issues](https://img.shields.io/github/issues/GHostload/Heliactyl-Canary)

`Heliactyl` lets you easily create, manage, and remove Pterodactyl Servers and Users.

![Heliactyl](https://cdn.discordapp.com/attachments/1063585626022223892/1065304573058764850/PylexPlus_2.png)

Contents
========

 * [Why?](#why)
 * [Moving to Canary Version](#installation)
 * [Installation](#installation)
 * [Updating](#updating)
 * [Usage](#usage)
 * [Pterodactyl Integration](#pterodactyl-integration)
 * [Configuration](#configuration)
 * [Output Structure](#output-structure)
 * [Reinstalling](#reinstalling)
 * [3rd Party Docs](#3rd-party-docs)
 * [Want to contribute?](#want-to-contribute)

### Why?

Heliactyl itself is great but makes it hard for 3rd Party Developers to expand without modifying the base that's why I created this version of it.

The Canary Version is not fully new but more a small update or fix for any issues with the Official Heliactyl Version it also allows easy 3rd Party modifications to be made.

### Installation
---

> **Danger**
> Be careful running this with elevated privileges. Code execution can be achieved with write permissions on the config files.

#### Method 1: [`Installer`](#)
<br>

> We recommend using Debian 10 or above on a virtual private server or dedicated server (You can install Heliactyl and Pterodactyl on the same machine)

> **Warning**
> This installer is not stable yet. We recommed installing Heliactyl Canary manually

```bash
$ wget https://heliactyl.cloud/Canary/git-ae9a639/installer.sh | bash sh 
```

#### Method 2: Install From Source manually

<br>

##### Step 1:

<br>

Downloading Dependencies and Repository
```bash
$ cd /var/www/
$ git clone https://github.com/GHostload/Heliactyl-Canary
$ cd Heliactyl-Canary
$ npm install .
```

<br>

##### Step 2:

<br>

Installing Ngnix and Configuring the reverse Proxy

> Replace <Domain> with your Domain 
> Replace <Port> with the specified port in your Settings

```bash
$ apt install nginx && apt install certbot
$ curl -o /etc/nginx/sites-enabled/heliactyl.conf https://raw.githubusercontent.com/GHostload/Heliactyl-Configs/main/ngnix/heliactyl.conf 
$ nano /etc/nginx/sites-enabled/heliactyl.conf
```

##### Step 3:

<br>

SSL and Firewall Setup

```bash
$ ufw allow 80
$ ufw allow 443
$ certbot certonly -d <Your Heliactyl Domain>
$ systemctl restart nginx
```

### Updating

Just follow the normal Installation Guide but backup your database

Keep the following file:
```css

database.sqlite

```
You can optionally backup your configuration file if you don't want to lose all settings.

```css

settings.json

```


### Usage
---

To use Heliactyl navigate to your completely installed Instance and Login

To become Admin and Access the Admin Area your need to give your newly created Pterodactyl user Admin Privileges.

<br>

### Pterodactyl Integration
---

**A Word of Caution**

This External Pterodactyl Management Panel uses the Pterodactyl API to function, meaning that you can easily manage your servers and users remotely (on your website, for example.) Dotfiles and configuration files may contain sensitive information like API keys and ssh keys, and you don't want to make that public. add them to .gitingore to make sure no sensitive files are uploaded accidentally.

<br>

### Configuration

If you'd like to modify your Instance Settings, you have to edit the `JSON` config file, located at `/var/www/Heliactyl-Canary`. 

There are two ways to do this.

1. Using vim or nanao
2. Using FTP/SFTP/SCP. We recommed WinSCP.

Editing the file in a text editor will give you more control and allows you to be faster.

> **Warning**
> If untrusted users can write to your config, they may can achieve code execution!

An empty string (`""`) is the default value and is considered to be `True`. If the return value of the expression is `0`, this is considered `True`. Otherwise, it is `False`.

```json
  "stripe": {
    // Because `$ false` returns 1
    "enabled": false,
    "note": "The key is the stripe API key and the coins is the amount per £1. If the stripe API key is invalid and stripe purchases are enabled, Heliactyl will crash when attempting to do transactions.",
    "key": "", // Try to prevent keys being empty!.
    "coins": 2000
  },
```

Here's an example config based on my default `settings.json`:

```json
{
  "version": "12.7.0",
  "name": "Heliactyl",
  "letter": "H",
  "defaulttheme": "default",
  "timezone": "Europe/London",
  "resources": {
    "_note": "Options: MB, GB. This is only used on the user side, admin will use GB.",
    "type": "MB"
  },
  "website": {
    "port": 25002,
    "secret": "Default Secret (Change this to any string you want)"
  },
  "pterodactyl": {
    "domain": "https://panel.demo.heliactyl.cloud",
    "key": "ptla_Qa71q1rWZWfTwPEmlyxWKpl04g36xfZdinPoOZIx"
  },
  "linkvertise": {
    "userid": "69420",
    "coins": 3
  },
  "storelimits": {
    "ram": "8192",
    "disk": "10240",
    "cpu": "800",
    "servers": "4"
  },
  "stripe": {
    "enabled": false,
    "note": "The key is the stripe API key and the coins is the amount per £1. If the stripe API key is invalid and stripe purchases are enabled, Heliactyl will crash when attempting to do transactions.",
    "key": "5375027602355464",
    "coins": 2000
  },
  "database": "sqlite://database.sqlite",
  "api": {
    "client": {
      "api": {
        "enabled": true,
        "code": "bACOeG9ZinGThNPYYJWG"
      },
      "j4r": {
        "enabled": true,
        "ads": [
          {
            "name": "Example server 1",
            "invite": "https://discord.gg/pQDr78pACr",
            "id": "956572204101951498",
            "coins": 300
          },
          {
            "name": "Example server 2",
            "invite": "https://discord.gg/FqGKqSNXGt",
            "id": "1012034372565729280",
            "coins": 500
          }
        ]
      },
      "bot": {
        "token": "NGL_6qrZcUqja7812RVdnEKjpzOL4CvHBFG",
        "joinguild": {
          "_comment": "The Discord bot must be in these servers and have invite permissions. Automatic guild joining will not work unless role packages are configured correctly. You can always just set it to a random role & package so that only this works.",
          "enabled": true,
          "guildid": [
            "956572204101951498"
          ]
        }
      },
      "passwordgenerator": {
        "signup": true,
        "note": "Use this to disable signups",
        "length": 24
      },
      "allow": {
        "newusers": true,
        "regen": true,
        "server": {
          "create": true,
          "modify": true,
          "delete": true
        },
        "overresourcessuspend": false
      },
      "oauth2": {
        "_comment": "Go to https://discord.dev/ and create an application to set these up.",
        "id": "332269999912132097",
        "secret": "937it3ow87i4ery69876wqire",
        "link": "https://client.demo.heliactyl.cloud",
        "callbackpath": "/callback",
        "prompt": true,
        "ip": {
          "trust x-forwarded-for": true,
          "block": [],
          "duplicate check": true
        }
      },
      "ratelimits": {
        "/callback": 2,
        "/create": 1,
        "/delete": 1,
        "/modify": 1,
        "/updateinfo": 1,
        "/setplan": 2,
        "/admin": 1,
        "/regen": 1,
        "/renew": 1,
        "/api/userinfo": 1
      },
      "packages": {
        "default": "Demo-User",
        "list": {
          "default": {
            "ram": 0,
            "disk": 0,
            "cpu": 0,
            "servers": 0
          },
          "Demo-User": {
            "ram": 128,
            "disk": 512,
            "cpu": 20,
            "servers": 1
          },
          "Demo-Admin": {
            "ram": 1024,
            "disk": 1024,
            "cpu": 100,
            "servers": 1
          },
        },
        "rolePackages": {
          "note": "This allows you to set a different plan to people who have a specific role however this requires the Discord bot to be configured and functioning. This is mainly used for Boost rewards",
          "roleServer": "Server ID",
          "roles": {
            "252021900682": "Demo-User",
            "898432425634": "Demo-Admin",
          }
        }
      },
      "locations": {
        "1": {
          "name": "Germany",
          "package": null
        },
        "2": {
          "name": "France",
          "package": "Demo-Admin"
        }
      },
      "eggs": {
       "paper": {
          "display": "Minecraft Java | Paper/Spigot",
          "minimum": {
            "ram": 1024,
            "disk": 1024,
            "cpu": 100
          },
          "maximum": {
            "ram": null,
            "disk": null,
            "cpu": null
          },
          "info": {
            "egg": 3,
            "docker_image": "ghcr.io/pterodactyl/yolks:java_17",
            "startup": "java -Xms128M -Xmx{{SERVER_MEMORY}}M -Dterminal.jline=false -Dterminal.ansi=true -jar {{SERVER_JARFILE}}",
            "environment": {
              "SERVER_JARFILE": "server.jar",
              "BUILD_NUMBER": "latest"
            },
            "feature_limits": {
              "databases": 4,
              "backups": 4
            }
          }
        },
        "bungeecord": {
          "display": "Minecraft Java | BungeeCord",
          "minimum": {
            "ram": 512,
            "disk": 512,
            "cpu": 75
          },
          "maximum": {
            "ram": null,
            "disk": null,
            "cpu": null
          },
          "info": {
            "egg": 1,
            "docker_image": "ghcr.io/pterodactyl/yolks:java_17",
            "startup": "java -Xms128M -Xmx{{SERVER_MEMORY}}M -jar {{SERVER_JARFILE}}",
            "environment": {
              "SERVER_JARFILE": "bungeecord.jar",
              "BUNGEE_VERSION": "latest"
            },
            "feature_limits": {
              "databases": 4,
              "backups": 4
            }
          }
        }
      },
      "coins": {
        "enabled": true,
        "store": {
          "_comment": "The cost and per is not intended to used with 0. This is not intended to sell resources for coins. Make sure coins are enabled too, or else there can be errors.",
          "enabled": true,
          "ram": {
            "cost": 2000,
            "per": 1024
          },
          "disk": {
            "cost": 1000,
            "per": 1024
          },
          "cpu": {
            "cost": 2000,
            "per": 100
          },
          "servers": {
            "cost": 400,
            "per": 1
          }
        }
      }
    },
    "arcio": {
      "_comment": "This is no longer supported and will be removed in the future. The AFK page has worked without arc.io since v11.4.0.",
      "enabled": true,
      "widgetid": "lnqnK",
      "afk page": {
        "_comment": "This will not effect any current arc.io sessions, and will require a restart to kick everyone out of the websocket sesison.",
        "_comment2": "Make sure coins are enabled if you choose to enable this option!",
        "enabled": true,
        "_comment3": "If you change the path, you need to restart for it to take full effect.",
        "path": "afkwspath",
        "_comment4": "This afk page will give the users [coins variable] coins every [every variable] seconds.",
        "every": 30,
        "_comment5": "The coins variable is meant to not be under 1. There may be bugs if the coins variable is less than 1.",
        "coins": 3
      }
    }
  },
  "whitelist": {
    "note": "This allows only specific people to be able to use the dashboard",
    "status": false,
    "users": [
      "879823646844138148"
    ]
  },
  "servercreation": {
    "note": "You can set how much it should cost to create a server here, the default price is free",
    "cost": 1000
  },
  "renewals": {
    "note": "The cost is the amount of coins required to renew, and the delay is the amount of days before they need to renew. New servers after the 1st one will also cost the renewal amount. This has been fixed to suspend servers instead of deleting them as of v12.7.0.",
    "status": true,
    "cost": 50,
    "delay": 7
  },
  "logging": {
    "status": true,
    "webhook": "https://discord.com/api/webhooks/561797641255643429/D2pzpGe3z7CgN5q6gqC6q9Wr4oLKxf3mN8eE9a3O",
    "actions": {
      "user": {
        "signup": true,
        "create server": true,
        "gifted coins": true,
        "modify server": true,
        "buy servers": false,
        "buy ram": true,
        "buy cpu": true,
        "buy disk": false
      }, 
      "admin": {
        "set coins": true,
        "add coins": true,
        "set resources": true,
        "set plan": true,
        "create coupon": true,
        "revoke coupon": true,
        "remove account": true,
        "view ip": false
      }
    }
  },
  "antivpn": {
    "note": "For antivpn to work, generate an apikey on https://proxycheck.io/. If you put no key, Heliactyl will disable antivpn.",
    "status": false,
    "APIKey": "Proxycheck APIKey",
    "whitelistedIPs": ["IP address"]
  }
}
```
<br>

#### .gitignore

`.gitignore` includes folders and files which shouldn't be tracked for changes.

<br>

#### LICENSE

This Project / Repository has been released under the MIT License. The original code base has been created by Two. 

This Repository may include content created by or with SrydenCloud Limited and Pine Platforms Ltd
This Repository uses upstream commits from [Open Heliactyl](https://github.com/OpenHeliactyl/Heliactyl).

<br>

#### Upstream commits Disclaimer

Changes from upstream sources may include Licensed material.
If you are the License holder of any Copyrighted material consider contacting Upstream Repository Owner before contacting us.

<br>

#### Output Structure
---

```shell
📦Heliactyl-Canary
 ┣ 📂api
 ┃ ┣ 📜admin.js
 ┃ ┣ 📜api.js
 ┃ ┣ 📜arcio.js
 ┃ ┣ 📜card.js
 ┃ ┣ 📜extras.js
 ┃ ┣ 📜linkvertise.js
 ┃ ┣ 📜oauth2.js
 ┃ ┣ 📜pages.js
 ┃ ┣ 📜redeem.js
 ┃ ┣ 📜renewal.js
 ┃ ┣ 📜servers.js
 ┃ ┗ 📜store.js
 ┣ 📂assets
 ┃ ┣ 📂css
 ┃ ┃ ┣ 📂maps
 ┃ ┃ ┃ ┗ 📜style.css.map
 ┃ ┃ ┗ 📜style.css
 ┃ ┣ 📂js
 ┃ ┃ ┣ 📜ace.js
 ┃ ┃ ┣ 📜alerts.js
 ┃ ┃ ┣ 📜avgrund.js
 ┃ ┃ ┣ 📜bootstrap-table.js
 ┃ ┃ ┣ 📜bt-maxLength.js
 ┃ ┃ ┣ 📜c3.js
 ┃ ┃ ┣ 📜calendar.js
 ┃ ┃ ┣ 📜chart.js
 ┃ ┃ ┣ 📜chartist.js
 ┃ ┃ ┣ 📜circle-progress.js
 ┃ ┃ ┣ 📜clipboard.js
 ┃ ┃ ┣ 📜codeEditor.js
 ┃ ┃ ┣ 📜codemirror.js
 ┃ ┃ ┣ 📜context-menu.js
 ┃ ┃ ┣ 📜cropper.js
 ┃ ┃ ┣ 📜dashboard.js
 ┃ ┃ ┣ 📜data-table.js
 ┃ ┃ ┣ 📜db.js
 ┃ ┃ ┣ 📜desktop-notification.js
 ┃ ┃ ┣ 📜dragula.js
 ┃ ┃ ┣ 📜dropify.js
 ┃ ┃ ┣ 📜dropzone.js
 ┃ ┃ ┣ 📜editorDemo.js
 ┃ ┃ ┣ 📜file-upload.js
 ┃ ┃ ┣ 📜flot-chart.js
 ┃ ┃ ┣ 📜form-addons.js
 ┃ ┃ ┣ 📜form-repeater.js
 ┃ ┃ ┣ 📜form-validation.js
 ┃ ┃ ┣ 📜formpickers.js
 ┃ ┃ ┣ 📜google-charts.js
 ┃ ┃ ┣ 📜google-maps.js
 ┃ ┃ ┣ 📜hoverable-collapse.js
 ┃ ┃ ┣ 📜iCheck.js
 ┃ ┃ ┣ 📜inputmask.js
 ┃ ┃ ┣ 📜ion-range-slider.js
 ┃ ┃ ┣ 📜jq.tablesort.js
 ┃ ┃ ┣ 📜jquery-file-upload.js
 ┃ ┃ ┣ 📜js-grid.js
 ┃ ┃ ┣ 📜just-gage.js
 ┃ ┃ ┣ 📜light-gallery.js
 ┃ ┃ ┣ 📜listify.js
 ┃ ┃ ┣ 📜mapael.js
 ┃ ┃ ┣ 📜mapael_example_1.js
 ┃ ┃ ┣ 📜mapael_example_2.js
 ┃ ┃ ┣ 📜maps.js
 ┃ ┃ ┣ 📜misc.js
 ┃ ┃ ┣ 📜modal-demo.js
 ┃ ┃ ┣ 📜morris.js
 ┃ ┃ ┣ 📜no-ui-slider.js
 ┃ ┃ ┣ 📜off-canvas.js
 ┃ ┃ ┣ 📜owl-carousel.js
 ┃ ┃ ┣ 📜paginate.js
 ┃ ┃ ┣ 📜popover.js
 ┃ ┃ ┣ 📜profile-demo.js
 ┃ ┃ ┣ 📜progress-bar.js
 ┃ ┃ ┣ 📜rickshaw.js
 ┃ ┃ ┣ 📜select2.js
 ┃ ┃ ┣ 📜settings.js
 ┃ ┃ ┣ 📜sparkline.js
 ┃ ┃ ┣ 📜tablesorter.js
 ┃ ┃ ┣ 📜tabs.js
 ┃ ┃ ┣ 📜tight-grid.js
 ┃ ┃ ┣ 📜toastDemo.js
 ┃ ┃ ┣ 📜todolist.js
 ┃ ┃ ┣ 📜tooltips.js
 ┃ ┃ ┣ 📜typeahead.js
 ┃ ┃ ┣ 📜widgets.js
 ┃ ┃ ┣ 📜wizard.js
 ┃ ┃ ┗ 📜x-editable.js
 ┃ ┣ 📂vendors
 ┃ ┃ ┣ 📂codemirror
 ┃ ┃ ┃ ┣ 📜ambiance.css
 ┃ ┃ ┃ ┣ 📜codemirror.css
 ┃ ┃ ┃ ┣ 📜codemirror.js
 ┃ ┃ ┃ ┣ 📜javascript.js
 ┃ ┃ ┃ ┗ 📜shell.js
 ┃ ┃ ┣ 📂css
 ┃ ┃ ┃ ┗ 📜vendor.bundle.base.css
 ┃ ┃ ┣ 📂js
 ┃ ┃ ┃ ┣ 📜bootstrap.min.js.map
 ┃ ┃ ┃ ┗ 📜vendor.bundle.base.js
 ┃ ┃ ┣ 📂jvectormap
 ┃ ┃ ┃ ┣ 📜jquery-jvectormap-world-mill-en.js
 ┃ ┃ ┃ ┣ 📜jquery-jvectormap.css
 ┃ ┃ ┃ ┗ 📜jquery-jvectormap.min.js
 ┃ ┃ ┣ 📂mdi
 ┃ ┃ ┃ ┣ 📂css
 ┃ ┃ ┃ ┃ ┣ 📜materialdesignicons.min.css
 ┃ ┃ ┃ ┃ ┗ 📜materialdesignicons.min.css.map
 ┃ ┃ ┃ ┗ 📂fonts
 ┃ ┃ ┃ ┃ ┣ 📜materialdesignicons-webfont.eot
 ┃ ┃ ┃ ┃ ┣ 📜materialdesignicons-webfont.ttf
 ┃ ┃ ┃ ┃ ┣ 📜materialdesignicons-webfont.woff
 ┃ ┃ ┃ ┃ ┗ 📜materialdesignicons-webfont.woff2
 ┃ ┃ ┣ 📂owl-carousel-2
 ┃ ┃ ┃ ┣ 📜owl.carousel.min.css
 ┃ ┃ ┃ ┣ 📜owl.carousel.min.js
 ┃ ┃ ┃ ┣ 📜owl.theme.default.min.css
 ┃ ┃ ┃ ┗ 📜owl.video.play.png
 ┃ ┃ ┣ 📂pwstabs
 ┃ ┃ ┃ ┣ 📜jquery.pwstabs.min.css
 ┃ ┃ ┃ ┗ 📜jquery.pwstabs.min.js
 ┃ ┃ ┣ 📂select2
 ┃ ┃ ┃ ┣ 📜select2.min.css
 ┃ ┃ ┃ ┗ 📜select2.min.js
 ┃ ┃ ┣ 📂select2-bootstrap-theme
 ┃ ┃ ┃ ┗ 📜select2-bootstrap.min.css
 ┃ ┃ ┗ 📂typeahead.js
 ┃ ┃ ┃ ┗ 📜typeahead.bundle.min.js
 ┃ ┗ 📜info.txt
 ┣ 📂managers
 ┃ ┗ 📜Queue.js
 ┣ 📂misc
 ┃ ┣ 📜getAllServers.js
 ┃ ┣ 📜getPteroUser.js
 ┃ ┣ 📜log.js
 ┃ ┣ 📜verifyCaptchaResponse.js
 ┃ ┗ 📜vpnCheck.js
 ┣ 📂stuff
 ┃ ┗ 📜arciotext.js
 ┣ 📂themes
 ┃ ┗ 📂default
 ┃ ┃ ┣ 📂alerts
 ┃ ┃ ┃ ┣ 📜alt.ejs
 ┃ ┃ ┃ ┗ 📜vpn.ejs
 ┃ ┃ ┣ 📂boxes
 ┃ ┃ ┃ ┣ 📜1.ejs
 ┃ ┃ ┃ ┗ 📜2.ejs
 ┃ ┃ ┣ 📂components
 ┃ ┃ ┃ ┣ 📜adminsidebar.ejs
 ┃ ┃ ┃ ┣ 📜ads.ejs
 ┃ ┃ ┃ ┣ 📜alert.ejs
 ┃ ┃ ┃ ┣ 📜footer.ejs
 ┃ ┃ ┃ ┣ 📜head.ejs
 ┃ ┃ ┃ ┣ 📜scripts.ejs
 ┃ ┃ ┃ ┣ 📜sidebar.ejs
 ┃ ┃ ┃ ┣ 📜status.ejs
 ┃ ┃ ┃ ┗ 📜topnav.ejs
 ┃ ┃ ┣ 📜404.ejs
 ┃ ┃ ┣ 📜admin-coins.ejs
 ┃ ┃ ┣ 📜admin-coupons.ejs
 ┃ ┃ ┣ 📜admin-plans.ejs
 ┃ ┃ ┣ 📜admin-resources.ejs
 ┃ ┃ ┣ 📜admin.ejs
 ┃ ┃ ┣ 📜afk.ejs
 ┃ ┃ ┣ 📜buy.ejs
 ┃ ┃ ┣ 📜create.ejs
 ┃ ┃ ┣ 📜dashboard.ejs
 ┃ ┃ ┣ 📜edit.ejs
 ┃ ┃ ┣ 📜gift-coins.ejs
 ┃ ┃ ┣ 📜gift-resources.ejs
 ┃ ┃ ┣ 📜index.ejs
 ┃ ┃ ┣ 📜j4r.ejs
 ┃ ┃ ┣ 📜lv.ejs
 ┃ ┃ ┣ 📜pages.json
 ┃ ┃ ┣ 📜redeem.ejs
 ┃ ┃ ┣ 📜servers.ejs
 ┃ ┃ ┣ 📜settings.ejs
 ┃ ┃ ┗ 📜store.ejs
 ┣ 📜.gitignore
 ┣ 📜index.js
 ┣ 📜LICENSE
 ┣ 📜package-lock.json
 ┣ 📜package.json
 ┣ 📜README.md
 ┗ 📜settings.json
```
<br>

### Reinstalling
----

When reinstalling your only need to keep 2 files: 

```css

database.sqlite
&
settings.json

```

These files keep the majority of your User data.

<br>

### 3rd Party Docs
----

Currently unavailable.

<br>

### Want to Contribute?
---

Fork this Project and make your changes after that create a pull request
