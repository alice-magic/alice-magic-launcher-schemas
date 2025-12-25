[Download Alice Magic Launcher](https://launcher.furi.moe/en) | [Furimoe](https://furi.moe)

# Alice Magic Launcher Schemas

This repository documents the JSON schemas for **Alice Magic Launcher** Minecraft instances, including:

* Instance Configuration
* Instance Manifest
* Social Links (`socials`)
* DNS-based discovery
* HTTP header verification

---

## Table of Contents

* [Alice Magic Launcher Schemas](#alice-magic-launcher-schemas)

  * [Table of Contents](#table-of-contents)
  * [Instance Configuration Schema](#instance-configuration-schema)

    * [Required Fields](#required-fields)
    * [Properties](#properties)
    * [Social Links (socials)](#social-links-socials)
    * [Example](#example)
  * [Instance Manifest Schema](#instance-manifest-schema)

    * [Item Fields](#item-fields)
    * [Manifest Example](#manifest-example)
  * [Supported Platforms Reference](#supported-platforms-reference)
  * [DNS Config](#dns-config)
  * [HTTP Header Verification in Alice Magic Launcher](#http-header-verification-in-alice-magic-launcher)

---

## Instance Configuration Schema

Schema for configuring an **Alice Magic Launcher Minecraft instance**.

* **$comment**: Schema for Alice Magic Launcher Minecraft instance configuration
* **title**: `Alice Magic Launcher Instance Schema`
* **type**: `object`

---

### Required Fields

| Field         | Type    | Description                                                                            |
| ------------- | ------- | -------------------------------------------------------------------------------------- |
| `name`        | string  | Unique identifier for the instance (lowercase letters, numbers, hyphens, underscores). |
| `displayName` | string  | Display name of the instance.                                                          |
| `description` | string  | Short description of the instance.                                                     |
| `onlineMode`  | boolean | Whether the instance requires online authentication.                                   |
| `minecraft`   | object  | Minecraft version and loader configuration.                                            |

---

### Properties

| Field                     | Type         | Description                                                  |
| ------------------------- | ------------ | ------------------------------------------------------------ |
| `icon`                    | string (URI) | URL to the instance icon image.                              |
| `minecraft.version`       | string       | Minecraft version (e.g. `1.21.8`, `latest`).                 |
| `minecraft.loader.type`   | string       | Loader type (`fabric`, `forge`, `quilt`, `neoforge`).        |
| `minecraft.loader.build`  | string       | Loader build version.                                        |
| `minecraft.loader.enable` | boolean      | Enable or disable mod loader.                                |
| `metadata.wallpaper`      | string (URI) | Wallpaper image URL.                                         |
| `metadata.[localized]`    | string       | Localized fields such as `th_displayName`, `th_description`. |
| `ignored`                 | string[]     | Files or folders ignored during launch/packaging.            |
| `readonly`                | boolean      | Whether the instance is read-only.                           |
| `gameArgs`                | string[]     | Extra JVM / game arguments.                                  |
| `socials`                 | array        | Social / community / store links for the instance.           |

---

## Social Links (`socials`)

The `socials` field allows instance authors to define official links such as:

* Community (Discord, Telegram)
* Development (GitHub, GitLab)
* Store / Donation
* Website / Embedded content

### Structure

```ts
socials: {
  url: string;
  type: string;
}[];
```

### Fields

| Field            | Type         | Description                                 |
| ---------------- | ------------ | ------------------------------------------- |
| `socials[].url`  | string (URI) | Target URL                                  |
| `socials[].type` | string       | Platform key used to determine icon & color |

* `socials` is **optional**
* Launcher will automatically map icon + color from `type`
* Unknown types fallback to default style

---

### Example

```json
{
  "$schema": "https://cdn.furimoe.com/schema/alice-magic-launcher.json",
  "name": "alice-magic",
  "displayName": "Alice Magic: Furiora's World",
  "description": "A modded Minecraft experience with adventure.",
  "icon": "https://cdn.masuru.in.th/HamF2Hsoirx4K9DL.webp",
  "onlineMode": true,

  "minecraft": {
    "version": "1.21.8",
    "loader": {
      "type": "fabric",
      "build": "0.17.2",
      "enable": true
    }
  },

  "metadata": {
    "th_displayName": "อลิซ เมจิก: โลกของฟูริโอร่า",
    "th_description": "ประสบการณ์มอด Minecraft ที่เต็มไปด้วยการผจญภัย",
    "wallpaper": "https://cdn.masuru.in.th/2025-11-18_13.webp"
  },

  "socials": [
    { "type": "github", "url": "https://github.com/aomkoyo" },
    { "type": "store", "url": "https://shop.furi.moe" },
    { "type": "support", "url": "https://furipay.me/aomkoyo" }
  ],

  "ignored": [
    "logs",
    "crash-reports",
    "screenshots"
  ],

  "readonly": false,
  "gameArgs": [
    "--quickPlayMultiplayer=play.furi.moe"
  ]
}
```

---

## Instance Manifest Schema

Defines files required to run the instance.

* **type**: `array`
* **description**: List of downloadable files with integrity data.

---

### Item Fields

| Field  | Type         | Description                              |
| ------ | ------------ | ---------------------------------------- |
| `path` | string       | Relative file path in instance directory |
| `url`  | string (URI) | Download URL                             |
| `size` | integer      | File size in bytes                       |
| `hash` | string       | SHA-1 hash                               |

---

### Manifest Example

```json
[
  {
    "path": "mods/iris-fabric-1.9.2+mc1.21.8.jar",
    "url": "https://cdn.modrinth.com/data/YL57xq9U/versions/x2f4KxP0/iris-fabric-1.9.2%2Bmc1.21.8.jar",
    "size": 2741900,
    "hash": "4964121fd2d75eebec77dd8b723bc381952fe43a"
  },
  {
    "path": "resourcepacks/Faithful 64x - Release 10.zip",
    "url": "https://cdn.modrinth.com/data/r4GILswZ/versions/5T6GekBK/Faithful%2064x%20-%20Release%2010.zip",
    "size": 18001871,
    "hash": "00413cf363e708c93a11f87dd425f0a164f3c1fb"
  }
]
```

---

## Supported Platforms Reference

Use **Key / Type** in `socials[].type`.

| Category | Platform     | Key / Type                               | Color          |
| -------- | ------------ | ---------------------------------------- | -------------- |
| Store    | Store / Shop | `store`, `shop`, `market`                | Green          |
| Dev      | GitHub       | `github`, `gh`                           | Purple         |
|          | GitLab       | `gitlab`                                 | Orange         |
| Social   | Facebook     | `facebook`, `fb`                         | Blue           |
|          | YouTube      | `youtube`, `yt`                          | Red            |
|          | X (Twitter)  | `x`, `twitter`                           | Sky Blue       |
|          | Instagram    | `instagram`, `ig`                        | Pink           |
|          | TikTok       | `tiktok`                                 | Red/Pink       |
| Gaming   | Discord      | `discord`, `dc`                          | Blurple        |
|          | Twitch       | `twitch`                                 | Purple         |
| Chat     | Telegram     | `telegram`, `tg`                         | Sky Blue       |
| Support  | Donation     | `patreon`, `kofi`, `support`             | Coral          |
| General  | Website      | `web`, `website`                         | Emerald        |
| Media    | Embed        | `iframe`                                 | Violet (Modal) |
| Default  | Other        | *any*                                    | Violet         |

---

## DNS Config

TXT record format for auto-discovery:

```txt
_alicemagiclauncher.play TXT "play.furi.moe|<Instance Config URL>|<Manifest URL>"
```

Launcher can search by domain or IP.

---

## HTTP Header Verification in Alice Magic Launcher

Every HTTP request sent by the launcher includes:

| Header   | Type    | Description           |
| -------- | ------- | --------------------- |
| `x-uuid` | string  | Player UUID  `(EX: 8518f0b2-d106-4c39-88d5-c7da11c91bbe)`          |
| `online` | boolean | Online account status |

Useful for:

* Access control
* Analytics
* Anti-leech protection
