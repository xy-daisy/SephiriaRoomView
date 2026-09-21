# Sephiria Room View（房间视野）v1.0.3 — 安装说明

把镜头拉远到「当前所在的整个房间」并锁定在那里，走进下一个房间时自动重新取景。
遇到战斗时改为框住整块战斗区域，战斗中镜头完全静止，不会被角色拖着走。

## 现在的取景规则

- **普通房间 / 长房间**：走到哪就显示哪一间，**整块显示、不再开窗分区**（多长多大都一样）。
- **普通小怪战斗房**：进战斗**直接显示整个战斗区域**，一次定框、**全程不再变化**，
  不会切窗口、不会中途收紧、不会跟着角色漂移。
- **小 boss 房**（游戏自己标记为 `Miniboss` 的怪）：改为只框**真正的战斗区域**
  （怪实际刷出来的那块，不是连走廊一起画进去的检测区），四周留一点点余量。
  一场战斗只重框一次，之后不再动；普通小怪房不受影响，仍然整块显示、零切换。
- **boss 房 / 终 boss 房**：整块显示竞技场，居中且占比更大；竞技场本体永远完整在画面内。
- **角色跑出画面时会跟着走**：战斗中途角色真的离开画面，镜头会解除锁定开始跟随
  （并在本场战斗内不再钉死），不会一直把人晾在屏幕外。
- **练功稻草人 / 测试人偶**：打它不会切换视野（游戏把它当成敌对单位，会误判进战斗）。
- **boss 进场动画后不会卡在原版缩放**：动画结束把缩放抢回去时会重新抢回来。
- **过渡小房间**：按真实墙体测量房间边界，走进去时跟正常房间一样切换视野。

- 单机 / 联机都可用。镜头是每台客户端各自的，不走网络，不影响他人。
- 游戏中按 **F10** 随时开关（想回原版视野按一下即可）。
- NPC 对话、过场动画、脚本镜头会自动让位给游戏本身。

---

## 一、前置条件：需要 BepInEx 6

本 mod 是 BepInEx 插件，**必须先装好 BepInEx 6（Unity Mono / x64）**。

怎么判断自己有没有装：打开游戏根目录（Steam 里右键 Sephiria → 管理 → 浏览本地文件），
如果同时看到下面这些，就说明已经装好了，直接跳到第二节：

```text
Sephiria.exe
winhttp.dll
doorstop_config.ini
BepInEx/
```

> 如果你装过 **Sephiria Together**（多人联机 mod）的 `with-BepInEx` 整合包，
> BepInEx 已经自带了，不需要再装。

如果没装：从 BepInEx 官方仓库获取 **v6 的 Unity Mono x64 构建**（本游戏用的是
`BepInEx 6.0.0-be.697`，Unity 6000.3.x），把压缩包里的文件**直接解压到游戏根目录**，
即 `winhttp.dll` 要和 `Sephiria.exe` 在同一层。然后**先启动一次游戏**，
让 BepInEx 生成它自己的目录和缓存（首次启动会慢一些），退出后再装本 mod。

---

## 二、安装

1. 在 Steam 中右键 **Sephiria** → **管理** → **浏览本地文件**，打开游戏根目录。
2. 把本压缩包里的 **`BepInEx` 文件夹整个拖进游戏根目录**，和已有的 `BepInEx` 合并。
3. 确认最终路径长这样（`Sephiria` 是游戏根目录）：

```text
Sephiria/
└─ BepInEx/
   ├─ plugins/
   │  └─ SephiriaRoomView.dll      ← 本体
   └─ config/
      └─ com.sephiria.roomview.cfg ← 配置（可选，首次启动会自动生成）
```

4. 启动游戏。进入关卡后视野会立刻变宽 —— 说明生效了。

> **不要把整个 zip 放进 `BepInEx/plugins`**，也不要放进 `Sephiria_Data`。
> 只要 `SephiriaRoomView.dll` 这一个文件在 `BepInEx/plugins/` 下就行。

配置文件是可选的：不放也会在第一次启动时自动生成一份带注释的默认值。
压缩包里这份只是方便你先改好再启动。
**升级时如果沿用旧 cfg，新增/改过默认值的项不会被自动套用** —— 想拿新默认值就删掉旧 cfg
（或只改你要改的那几项）再启动一次。

---

## 三、卸载

删掉这一个文件即可，游戏立刻恢复原版视野：

```text
Sephiria/BepInEx/plugins/SephiriaRoomView.dll
```

配置文件 `Sephiria/BepInEx/config/com.sephiria.roomview.cfg` 留着无害，想彻底清理就一起删。

---

## 四、常用配置

配置文件：`Sephiria/BepInEx/config/com.sephiria.roomview.cfg`
（用记事本/VSCode 打开，改完**重启游戏**生效；游戏运行时改会被覆盖）

| 分区 | 配置项 | 默认 | 作用 |
|---|---|---|---|
| General | `Enabled` | `true` | 总开关 |
| General | `ToggleKey` | `F10` | 游戏中开关热键（InputSystem 按键名，如 `F9`、`Tab`、`Backquote`） |
| General | `HeartbeatSeconds` | `10` | 每多少秒往日志打一行状态；`0` 关闭 |
| General | `YieldToGame` | `true` | 对话/过场时把镜头交还给游戏 |
| View | `Margin` | `0.5` | 房间边界外额外留白，遮住墙边 |
| View | `MinZoom` / `MaxZoom` | `8` / `18` | 房间视野的缩放上下限（原版是 `5.625`） |
| View | `MaxRoomSpanX/Y` | `0` / `0` | `0` = **任何房间都整块显示**（默认）。设成 `30`/`18` 会把大房切成窗口，走动时会平移 |
| View | `PanSpeed` | `4` | 切换房间时平移速度，越小越慢越平滑 |
| View | `TransitionTime` | `0.35` | 缩放动画时长（秒） |
| View | `CompensateCameraOffset` | `true` | 抵消游戏自带的镜头偏移，让房间真正居中 |
| View | `SingleRoomUseGeometry` | `true` | 用真实墙体测量「无房间数据」的楼层；关掉则回到 `Center/Size` |
| Boss | `BossEnabled` | `true` | boss 战改为框住竞技场 |
| Boss | `BossMinZoom` / `BossMaxZoom` | `5` / `22` | boss 战缩放上下限（整块显示，上限要 ≥ 竞技场高度的一半） |
| Boss | `BossPadding` | `1.5` | **boss 视野唯一的边距旋钮**：竞技场外每边留多少。调大（2~3）更宽松，调小（0.5~1）更贴脸。竞技场本体始终完整显示 |
| Boss | `BossTriggerOnEnter` | `true` | 一走进 boss 房就取景，不等游戏标记战斗开始 |
| Boss | `BossWholeArea` | `true` | **boss 战整块显示竞技场**，不再开窗/分区平移 |
| Combat | `CombatEnabled` | `true` | 普通战斗改为框住战斗区 |
| Combat | `CombatWholeArea` | `true` | **普通战斗一次显示整块战斗区**，不再开窗/分区平移 |
| Combat | `CombatMaxZoom` | `20` | 普通战斗最远能拉到多少；调小可在超大战斗房里让角色大一点 |
| Combat | `CombatClipToRoom` | `false` | 默认关闭：长房间整块显示。设 `true` 会把大战斗区裁回角色所在房间 |
| Combat | `CombatUseEnemyBounds` | `false` | 改成 `true` 则按**实际刷出的敌人包围盒**取景（战斗区矩形常常连走廊一起画进去） |
| Combat | `FightCenterOnArea` | `true` | **以战斗区为中心**：视野 = 战斗区 + 四周等量留白，不朝角色伸展（伸展是把走廊/门口拉进画面的元凶） |
| Combat | `FightPadding` | `2` | 普通战斗视野比战斗区每边大多少（**对 boss 不生效**，boss 只认 `BossPadding`） |
| Combat | `FightStartMargin` | `3` | 离战斗区多近才锁定视野；调大 = 更早锁定（也更容易带进门口） |
| Combat | `FightEscapeMargin` | `10` | 角色跑出画面多远就放弃锁定、改为跟随 |
| Combat | `IgnoreDummyFights` | `true` | 打测试人偶/稻草人时不切换视野 |
| Combat | `DummyRadius` | `18` | 人偶在多近的距离内才忽略战斗标记 |
| Combat | `CombatLockView` | `true` | 战斗期间镜头钉死；关掉则回到旧版跟随行为 |
| Combat | `FightZoomScale` | `1` | 战斗时额外缩放。`1` = 刚好框住；`<1` 裁掉一点边缘让战斗更大；`>1` 拉远看得更多 |
| Combat | `FightRelevanceRadius` | `26` | 角色离战斗区超过这个距离就不按战斗取景，改用房间视野 |
| MiniBoss | `MiniBossEnabled` | `true` | 小 boss 房只框真正的战斗区域（按游戏自己的 `Miniboss` 标记判定，不是按房间大小猜）。设 `false` = 所有战斗房都整块显示 |
| MiniBoss | `MiniBossPadding` | `3` | 小 boss 视野比战斗区域每边大多少；框太紧就调大（4~5） |
| MiniBoss | `MiniBossMinZoom` / `MiniBossMaxZoom` | `6` / `18` | 小 boss 战缩放上下限 |
| MiniBoss | `MiniBossWholeArea` | `true` | 小 boss 战整块显示，不再开窗 |

完整列表（含 `MiniBossEnemyPadding`、`FightSpanX/Y`、`FightRoomUnionRatio` 等）
见 cfg 文件里每一项上方的注释。

### 几种常见调法

- **boss 房还是嫌大 / 想更贴脸** → 调小 `BossPadding`（`1` 或 `0.5`）。
- **boss 房嫌窄、边缘看不到** → 调大 `BossPadding`（`2.5`~`4`）。
- **战斗区域想再大一点（更满屏）** → `FightZoomScale` 调到 `0.9`（会裁掉一点边缘）。
- **大战斗房里角色太小** → 调小 `CombatMaxZoom`（会裁一点，但角色更大）。
- **视野想比战斗区更大/更小** → `FightPadding`（每边加多少，默认 `2`）。
- **战斗框又把走廊/隔壁房间带进来了** → 确认 `FightCenterOnArea = true`；仍有问题就调小 `FightStartMargin`（如 `1`）。
- **小 boss 房还是一大片空地** → 确认 `MiniBossEnabled = true`，日志里应有
  `fight kind: ... -> MINI-BOSS` 和 `framed from the zone's spawn area: ...`。
- **小 boss 框太紧** → 调大 `MiniBossPadding`（4~5）。
- **战斗时角色贴到屏幕边缘** → 这是「以战斗区为中心」的代价，调大 `FightPadding` 可缓解。
- **不规则战斗房间有角落被裁掉** → `CombatRoomRatio` 调到 `3`（少裁），或 `FightRoomUnionRatio` 调到 `3`。
- **过渡小房间的视野还是跟着角色走** → 确认 `SingleRoomUseGeometry = true`；
  日志里会有 `measured from scene geometry` 一行。
- **觉得换房间太快/太慢** → `TransitionTime` 和 `PanSpeed`。
- **进 boss 楼层后角色不在画面里** → 调小 `FightRelevanceRadius`（例如 `16`）。

---

## 五、排错

**完全没反应**
- 确认 `SephiriaRoomView.dll` 在 `BepInEx/plugins/`，而不是在某个子文件夹里。
- 看 `Sephiria/BepInEx/LogOutput.log`，启动成功会有：
  `[Info :Sephiria Room View] [Sephiria Room View] v1.0.3 loaded. Toggle with F10.`
- 如果日志里出现 `error, disabling: ...`，mod 会自动关闭；把那一行发给我。

**想看 mod 到底选了哪个区域**
把 `HeartbeatSeconds` 保持默认 `10`，日志里每 10 秒会有一行，形如：

```text
[RoomView] Room pos=(3543.8,509.1) region=[(3522.0,500.0)-(3552.0,518.0) 30.0x18.0] center=(3537.0,509.0) ortho=9.50 (cam 9.50) camOffset=(0.0,0.0)
```

战斗类型的判定也会打出来，形如：

```text
[RoomView] fight kind: spawn data = spawns a MINIBOSS | a mini-boss unit is on the field -> MINI-BOSS, frame the measured fight border
[RoomView] mini-boss fight - re-framing the pinned view onto its fight area
[RoomView] framed from the zone's spawn area: [...] -> [...]
```

boss 房会打出原始竞技场尺寸和最终加边结果：

```text
[RoomView] boss arena raw=(...)-(...) 32.0x32.0 outbound(l,r,t,b)=(0.0,0.0,0.0,0.0) padding=1.5 -> [(...)-(...)]
```

**游戏内报错 / 崩溃**
本 mod 的日志走 BepInEx（`Sephiria/BepInEx/LogOutput.log`），
但 Unity 自己的异常在 Player.log：
`C:\Users\<你的用户名>\AppData\LocalLow\TEAMHORAY\Sephiria\Player.log`

---

## 六、English (quick version)

**What it does:** widens the camera to fit the whole room you are standing in and locks it there.
During a fight it frames the whole fight area and pins the camera still — normal fights are framed
once when they start and never change. Mini-boss fights are framed from the zone's real fight area
(the box the monsters spawn in, not the larger detect box that reaches into the corridor). Boss
fights show the whole arena. If the character walks off screen mid-fight the pin is dropped and the
camera follows again. Works in single player and multiplayer (the camera is per-client; nothing is
sent over the network).

**Requirements:** BepInEx 6 (Unity Mono, x64) must already be installed in the game folder.

**Install:** drag the `BepInEx` folder from this archive into the game root directory so that the
file ends up here:

```text
Sephiria/BepInEx/plugins/SephiriaRoomView.dll
```

Do not put the whole ZIP inside `BepInEx/plugins`, and do not put anything into `Sephiria_Data`.

**Toggle in game:** `F10` (configurable via `ToggleKey`).

**Uninstall:** delete `SephiriaRoomView.dll`.

**Config:** `Sephiria/BepInEx/config/com.sephiria.roomview.cfg` (auto-generated on first launch if
absent). Edit, then restart the game. Note that BepInEx does not overwrite values that already
exist in the file, so after an upgrade delete the cfg (or the entries you want reset) to pick up
new defaults.

Key knobs: `BossPadding` (only margin a boss frame gets), `FightPadding` (margin around normal
fight areas), `MiniBossEnabled` / `MiniBossPadding` (mini-boss framing).
