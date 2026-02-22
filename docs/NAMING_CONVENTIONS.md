# Naming Conventions

> Refined collaboratively during Phase 1 (Pre-Production). These are starting defaults based on [stillwwater](https://github.com/stillwwater/UnityStyleGuide) and [justinwasilenko](https://github.com/justinwasilenko/Unity-Style-Guide) guides. Customize to your team's preferences.

## Code Naming

| Element | Convention | Example |
|---------|-----------|---------|
| **Namespace** | PascalCase, match folder | `MyGame.Combat` |
| **Class** | PascalCase, noun | `PlayerController` |
| **Interface** | I + PascalCase | `IDamageable` |
| **Method** | PascalCase, verb | `TakeDamage()` |
| **Property** | PascalCase | `MaxHealth` |
| **Public field** | PascalCase (avoid — use properties) | `Health` |
| **Internal member** | Same as public (PascalCase) | `ResolveService()` |
| **Private field** | _camelCase | `_moveSpeed` |
| **Parameter** | camelCase | `damageAmount` |
| **Local variable** | camelCase | `currentHealth` |
| **Constant** | UPPER_SNAKE or PascalCase | `MAX_HEALTH` or `MaxHealth` |
| **Static field** | s_camelCase | `s_instanceCount` |
| **Enum** | PascalCase | `WeaponType` |
| **Enum value** | PascalCase | `WeaponType.AssaultRifle` |
| **Event** | On + PascalCase | `OnDeath` |
| **Boolean** | is/has/can/should prefix | `_isGrounded` |

## File & Folder Naming

| Asset Type | Convention | Example |
|-----------|-----------|---------|
| **C# Script** | PascalCase, match class | `PlayerController.cs` |
| **Assembly Def** | Namespace dot notation | `MyGame.Combat.asmdef` |
| **Scene** | PascalCase | `MainMenu.unity`, `Level01.unity` |
| **Prefab** | PascalCase | `EnemyZombie.prefab` |
| **Material** | M_PascalCase | `M_PlayerSkin.mat` |
| **Texture** | T_PascalCase_Suffix | `T_PlayerSkin_Albedo.png` |
| **Normal Map** | T_PascalCase_Normal | `T_PlayerSkin_Normal.png` |
| **Sprite** | SPR_PascalCase | `SPR_CoinGold.png` |
| **Animation Clip** | AC_PascalCase | `AC_PlayerRun.anim` |
| **Animator Controller** | ANIM_PascalCase | `ANIM_Player.controller` |
| **Audio Clip** | SFX_ or MUS_ | `SFX_Explosion.wav`, `MUS_MainTheme.ogg` |
| **UXML** | PascalCase | `PlayerHUD.uxml` |
| **USS** | PascalCase | `PlayerHUD.uss` |
| **ScriptableObject** | PascalCase | `SwordData.asset` |
| **Shader** | SH_PascalCase | `SH_CustomLit.shader` |

## Texture Suffixes

| Suffix | Map Type |
|--------|----------|
| `_Albedo` | Base color / diffuse |
| `_Normal` | Normal map |
| `_Metallic` | Metallic map |
| `_Roughness` | Roughness map |
| `_AO` | Ambient occlusion |
| `_Emission` | Emission map |
| `_Height` | Height / displacement |
| `_Mask` | Mask texture |

## Folder Structure Naming

```
Assets/_Project/
├── Features/PlayerMovement/   # Feature-based: PascalCase
├── Scripts/Combat/            # Type-based: PascalCase
├── Art/Materials/             # PascalCase
├── Audio/SFX/                 # PascalCase, uppercase abbreviations
└── Prefabs/Enemies/           # PascalCase, plural for collections
```
