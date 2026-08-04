# MyFightingGame

一个基于 Unity 2D 的 Roguelike 动作格斗游戏。

## 🎮 游戏简介

MyFightingGame 是一款 2D 俯视角 Roguelike 动作游戏。玩家在随机生成的敌人浪潮中战斗，通过击败敌人获取经验值升级，收集掉落的装备道具提升属性，并在技能树中解锁新能力。每 N 波敌人会出现 Boss，击败敌人获得分数，挑战更高记录。

## 🕹️ 操作方式

| 按键 | 功能 |
|------|------|
| W / A / S / D | 移动（8 方向） |
| J | 近战斩击（Slash） |
| I | 弓箭射击（需先解锁 Bow 技能） |
| Q | 切换近战/远程武器 |
| K | 打开/关闭技能树面板 |
| B | 打开/关闭背包 |
| P | 打开/关闭属性面板 |
| Enter | 获得经验值（调试用） |

## ✨ 核心系统

### 玩家系统
- **8 方向移动**：支持键盘 WASD 或方向键控制
- **近战攻击**：剑刃斩击，具有冷却时间、攻击范围和击退效果
- **远程攻击**：弓箭射击，支持 8 方向瞄准，箭矢可嵌入障碍物
- **武器切换**：在近战和远程武器之间自由切换
- **属性系统**：攻击力（ATK）、移动速度、最大生命值、武器范围、击退力

### 敌人系统
- **AI 状态机**：Idle（待机）→ Chasing（追击）→ Attacking（攻击）→ Knockback（击退）
- **普通敌人**：追击玩家，近战攻击，可配置伤害和击退参数
- **Boss 敌人**：每 N 波敌人出现一次，更高威胁
- **对象池**：敌人使用对象池管理，优化性能
- **掉落系统**：击败敌人有概率掉落道具，可配置掉落概率

### 技能树系统
- **技能点**：升级获得技能点数
- **解锁机制**：前置技能达到最大等级后解锁后续技能
- **可用技能**：
  - `MaxHealthUp` — 提升最大生命值
  - `Sword Slash` — 解锁近战攻击
  - `Bow` — 解锁弓箭射击
  - `ATK_Up` — 提升攻击力
  - `SuckBlood` — 攻击时吸血

### 道具/背包系统
- **ScriptableObject 道具**：每种道具定义名称、图标、描述和属性加成
- **属性加成**：道具可提升攻击力、最大生命值、移动速度
- **掉落拾取**：敌人死亡掉落道具，玩家触碰拾取，带拾取动画
- **背包 UI**：显示当前拥有的道具

### 经验与等级
- **经验获取**：击败敌人获得经验值
- **等级成长**：经验值满后升级，所需经验按指数增长
- **等级奖励**：每次升级获得 1 个技能点

### 高低差/层级系统
- 进入特定区域可切换层级（如进入高地）
- 自动切换碰撞体和渲染排序层级
- 营造 2D 场景的深度感和层次感

### UI 系统
- 生命值条、经验条和等级显示
- 技能树面板（打开时暂停游戏）
- 属性面板（ATK、SPEED 实时显示）
- 历史最高分记录（跨场景保持）

## 🗺️ 场景结构

| 场景 | 说明 |
|------|------|
| `StartScene` | 开始菜单（开始游戏 / 退出游戏） |
| `MainScene` | 主菜单（进入战斗场景） |
| `GameScene` | 核心战斗场景 |

## 🏗️ 项目结构

```
Assets/
├── AIGC/                    # AI 辅助生成工具
│   └── Enemy/               # 敌人波次生成器
├── Prefabs/                 # 预制体
│   ├── Enemy_Red.prefab     # 普通敌人
│   ├── Boss.prefab          # Boss 敌人
│   ├── Arrow.prefab         # 箭矢
│   ├── ItemPrefab.prefab    # 掉落道具
│   ├── SkillButton.prefab   # 技能按钮
│   └── UIPrefab/            # UI 预制体
├── Scenes/                  # 场景文件
├── Scripts/
│   ├── Player/              # 玩家脚本
│   │   ├── PlayerMove.cs    # 玩家移动
│   │   ├── Player_Combat.cs # 近战攻击
│   │   ├── Player_Bow.cs    # 弓箭射击
│   │   ├── Player_Hp.cs     # 生命值
│   │   ├── Player_ChangeEquipment.cs  # 武器切换
│   │   ├── Arrow.cs         # 箭矢行为
│   │   ├── StatsManager.cs  # 属性管理器（单例）
│   │   ├── ExpManager.cs    # 经验与等级
│   │   └── StatsUI.cs       # 属性显示
│   ├── Enemy/               # 敌人脚本
│   │   ├── Enemy_Movement.cs  # AI 状态机与移动
│   │   ├── Enemy_Combat.cs    # 敌人攻击
│   │   ├── Enemy_Hp.cs        # 敌人生命值与掉落
│   │   └── Enemy_Konckback.cs # 敌人击退
│   ├── SkillTree/           # 技能树脚本
│   │   ├── SkillSO.cs         # 技能数据（ScriptableObject）
│   │   ├── SkillSlot.cs       # 技能槽
│   │   ├── SkillTreeManager.cs # 技能树管理器
│   │   ├── SkillManager.cs    # 技能效果处理
│   │   └── ToggleSkillTree.cs # 技能树开关
│   ├── Item/                # 道具/背包脚本
│   │   ├── ItemSO.cs        # 道具数据（ScriptableObject）
│   │   ├── ItemInfo.cs      # 道具信息
│   │   ├── Loot.cs          # 掉落拾取
│   │   ├── InventoryData.cs # 背包数据（单例）
│   │   ├── InventoryManager.cs # 背包 UI 管理
│   │   └── InventorySlot.cs # 背包槽位
│   ├── UI/                  # UI 脚本
│   │   ├── StartMenuController.cs  # 开始菜单
│   │   ├── MainMenuController.cs   # 主菜单
│   │   ├── StatsMenuController.cs  # 结算/退出菜单
│   │   ├── UI_Open.cs       # 通用 UI 开关
│   │   └── SetHighestScore.cs # 最高分显示
│   ├── Elevation/           # 层级/高低差
│   │   ├── Elevation_Entry.cs # 进入高层
│   │   └── Elevation_Exit.cs  # 退出高层
│   └── User/                # 用户数据
│       └── UserInfoData.cs  # 持久化数据（最高分）
└── AIGC/                    # AI 辅助脚本
    ├── EnemySpawner_2D.cs   # 2D 敌人生成器
    ├── GrassPlacer.cs       # 草地放置工具
    ├── TilemapTexturePlacer.cs # Tilemap 纹理放置
    ├── TilemapAutoFill.cs   # Tilemap 自动填充
    └── aicode.cs            # AI 代码辅助
```

## 🔧 技术特性

- **Unity 2D**：使用 SpriteRenderer、Rigidbody2D、Collider2D
- **对象池**：敌人使用对象池复用，减少 GC 压力
- **ScriptableObject**：技能和道具使用 SO 实现数据驱动
- **单例模式**：StatsManager、InventoryData、UserInfoData 使用单例保证全局唯一
- **事件驱动**：使用 C# event 实现模块间解耦通信
- **协程**：击退、延时等效果使用协程实现
- **TextMeshPro**：使用 TMP 渲染 UI 文本
- **Animator 层级**：近战/远程武器切换通过 Animator Layer 实现

## 🚀 快速开始

1. 使用 **Unity 2022.3+**（项目基于 Unity 2022.3.62 / Tuanjie 1.8.3）打开项目
2. 在 Build Settings 中确认三个场景已添加：`StartScene` → `MainScene` → `GameScene`
3. 打开 `StartScene` 场景，点击 Play 运行
4. 也可以直接运行 `GAME/Luna.exe` 体验已构建的可执行文件

**核心依赖：**
- Cinemachine 2.10.6
- TextMesh Pro 3.0.9
- Unity 2D Feature Set（Tilemap、Sprite、2D Physics）

**层级设置：**
- Layer 6：`Player`
- Layer 7：`Enemy`

**美术资源：** 使用 "Tiny Swords" 像素风格素材包

## 📋 开发路线

- [x] 基础移动与战斗
- [x] 敌人 AI 与波次生成
- [x] 技能树系统
- [x] 道具掉落与背包
- [x] Boss 敌人
- [x] 经验与等级系统
- [x] 武器切换（近战/远程）
- [x] 高低差层级系统
- [x] 分数与最高分记录
- [ ] 更多敌人类型
- [ ] 更多技能与道具
- [ ] 音效与背景音乐
- [ ] 更多关卡场景

## 📝 更新日志

### v0.4 — 添加 Boss
- 新增 Boss 敌人预制体与生成逻辑
- 每 N 波敌人生成一个 Boss

### v0.3 — 掉落与手感优化
- 添加敌人掉落物品功能
- 修复场景切换时 Time.timeScale 的 Bug
- 优化战斗手感

### v0.2 — Bug 修复
- 修复多个 Bug
- 添加可执行文件

### v0.1 — 游戏基础功能
- 玩家移动、近战攻击、弓箭系统
- 敌人 AI 与波次生成
- 技能树系统
- 道具掉落与背包系统
- 经验与等级系统

---

*Made with Unity*
