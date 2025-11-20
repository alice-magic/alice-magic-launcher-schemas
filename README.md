
[Download Alice Magic Launcher](https://launcher.furi.moe/en) | [Furimoe](https://furi.moe)

# Alice Magic Launcher Schemas

This repository documents the JSON schemas for **Alice Magic Launcher** Minecraft instances, including both the **instance configuration** and the **instance manifest**.

---

## Table of Contents

- [Alice Magic Launcher Schemas](#alice-magic-launcher-schemas)
  - [Table of Contents](#table-of-contents)
  - [Instance Configuration Schema](#instance-configuration-schema)
    - [Required Fields](#required-fields)
    - [Properties](#properties)
    - [Example](#example)
  - [Instance Manifest Schema](#instance-manifest-schema)
    - [Item Fields](#item-fields)
    - [Manifest Example](#manifest-example)
  - [DNS Config](#dns-config)

---

## Instance Configuration Schema

Schema for configuring an **Alice Magic Launcher Minecraft instance**.

- **$comment**: Schema for Alice Magic Launcher Minecraft instance configuration  
- **title**: `Alice Magic Launcher Instance Schema`  
- **type**: `object`  

### Required Fields

| Field         | Type   | Description                                                                            |
| ------------- | ------ | -------------------------------------------------------------------------------------- |
| `name`        | string | Unique identifier for the instance (lowercase letters, numbers, hyphens, underscores). |
| `displayName` | string | Name of the instance.                                                                  |
| `description` | string | A brief description of the instance.                                                   |
| `minecraft`   | object | Minecraft version and loader configuration.                                            |

### Properties

| Field                     | Type             | Description                                                                                                                                   |
| ------------------------- | ---------------- | --------------------------------------------------------------------------------------------------------------------------------------------- |
| `icon`                    | string (URI)     | URL to an icon image representing the instance.                                                                                               |
| `minecraft.version`       | string           | Minecraft version (e.g., `1.21.8` or `latest`).                                                                                               |
| `minecraft.loader.type`   | string           | Type of mod loader (`fabric`, `forge`, `quilt`, `neoforge`).                                                                                  |
| `minecraft.loader.build`  | string           | Specific build version of the mod loader.                                                                                                     |
| `minecraft.loader.enable` | boolean          | Whether to enable the specified mod loader.                                                                                                   |
| `metadata.wallpaper`      | string (URI)     | URL to a wallpaper image for the instance.                                                                                                    |
| `metadata.[localized]`    | string           | Localized metadata fields. Supported fields include:<br>- `displayName` (e.g., `th_displayName`) <br>- `description` (e.g., `th_description`) |
| `ignored`                 | array of strings | Files/folders to ignore when packaging or launching the instance.                                                                             |
| `readonly`                | boolean          | Whether the instance is read-only.                                                                                                            |
| `gameArgs`                | array of strings | Additional arguments for the game launch (e.g., `--quickPlayMultiplayer=play.furi.moe`).                                                      |
| `manifest`                | array            | List of files required for the instance (see [Instance Manifest Schema](#instance-manifest-schema)).                                          |

### Example

```json
{
  "$schema": "https://cdn.furimoe.com/schema/alice-magic-launcher.json",
  "name": "alice-magic",
  "displayName": "Alice Magic: Furiora's World",
  "description": "A modded Minecraft experience with adventure.",
  "icon": "https://cdn.masuru.in.th/HamF2Hsoirx4K9DL.webp",
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
  "ignored": [
    "options.txt",
    "logs",
    "crash-reports",
    "optionsof.txt",
    "resourcepacks",
    "shaderpacks",
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

Schema for the manifest of required files for a Minecraft instance.

- **$comment**: Schema for the Alice Magic Launcher Minecraft instance manifest configuration  
- **title**: `Alice Magic Launcher Instance Manifest Schema`  
- **type**: `array`  
- **description**: List of files required for the instance with download information.

### Item Fields

| Field  | Type         | Description                                     |
| ------ | ------------ | ----------------------------------------------- |
| `path` | string       | Relative path of the file in the instance.      |
| `url`  | string (URI) | Download URL of the file.                       |
| `size` | integer      | File size in bytes.                             |
| `hash` | string       | SHA-1 or other hash for integrity verification. |

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

## DNS Config

DNS records can be used to advertise instance configuration and manifest URLs for Alice Magic Launcher.

Example TXT record:

```
_alicemagiclauncher.play TXT "play.furi.moe|<Instance Configuration URL>|<Instance Manifest URL>"
```

Replace `<Instance Configuration URL>` and `<Instance Manifest URL>` with the actual URLs.

You can search for an instance using the IP (e.g., `play.furi.moe`) or any IP that is configured with the above DNS TXT record.