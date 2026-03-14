# 《城堡守护者》实现计划

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** 实现一个3D第三人称中世纪基地防御生存游戏的核心玩法原型

**Architecture:** 使用Unity引擎，采用组件化架构。玩家控制、建造系统、敌人AI、波浪系统分别作为独立模块开发，通过事件系统通信。

**Tech Stack:** Unity 2022+, C#, Unity Test Framework

---

## 阶段一：基础框架搭建

### Task 1: 创建Unity项目结构

**Files:**
- 创建: `Assets/Scripts/Player/PlayerController.cs`
- 创建: `Assets/Scripts/Player/PlayerStats.cs`
- 创建: `Assets/Scripts/Game/GameManager.cs`

**Step 1: 创建项目文件夹结构**

```bash
# 在Unity编辑器中创建以下文件夹
Assets/Scripts/Player/
Assets/Scripts/Game/
Assets/Scripts/Building/
Assets/Scripts/Enemy/
Assets/Scripts/UI/
Assets/Prefabs/
Assets/Scenes/
```

**Step 2: 创建基础GameManager**

```csharp
using UnityEngine;

public class GameManager : MonoBehaviour
{
    public static GameManager Instance { get; private set; }
    
    public enum GameState { Playing, Paused, GameOver, Victory }
    public GameState CurrentState { get; private set; }
    
    void Awake()
    {
        if (Instance == null) Instance = this;
        else Destroy(gameObject);
    }
    
    public void SetGameState(GameState newState)
    {
        CurrentState = newState;
    }
}
```

**Step 3: 提交**

```bash
git add .
git commit -m "feat: 创建基础项目结构和GameManager"
```

---

### Task 2: 创建玩家控制器

**Files:**
- 修改: `Assets/Scripts/Player/PlayerController.cs`
- 创建: `Assets/Tests/PlayMode/PlayerControllerTests.cs`

**Step 1: 编写玩家移动测试**

```csharp
using NUnit.Framework;
using UnityEngine;

[TestFixture]
public class PlayerControllerTests
{
    [Test]
    public void Player_MoveTowards_Direction_MovesPlayer()
    {
        // Arrange
        var player = new GameObject().AddComponent<PlayerController>();
        Vector3 startPos = player.transform.position;
        Vector3 targetPos = startPos + Vector3.forward * 5f;
        
        // Act
        player.MoveTowards(targetPos);
        
        // Assert
        Assert.AreNotEqual(startPos, player.transform.position);
    }
}
```

**Step 2: 运行测试验证失败**

Run: 在Unity中运行测试
Expected: 测试通过（方法需要实现）

**Step 3: 实现玩家移动**

```csharp
using UnityEngine;

public class PlayerController : MonoBehaviour
{
    [SerializeField] private float moveSpeed = 5f;
    private Rigidbody rb;
    
    void Awake()
    {
        rb = GetComponent<Rigidbody>();
    }
    
    public void MoveTowards(Vector3 target)
    {
        Vector3 direction = (target - transform.position).normalized;
        rb.MovePosition(transform.position + direction * moveSpeed * Time.deltaTime);
    }
    
    void Update()
    {
        float h = Input.GetAxis("Horizontal");
        float v = Input.GetAxis("Vertical");
        Vector3 moveDir = new Vector3(h, 0, v).normalized;
        rb.MovePosition(transform.position + moveDir * moveSpeed * Time.deltaTime);
    }
}
```

**Step 4: 运行测试验证通过**

**Step 5: 提交**

```bash
git add .
git commit -feat: 实现玩家移动控制器
```

---

## 阶段二：武器系统

### Task 3: 武器系统基础

**Files:**
- 创建: `Assets/Scripts/Player/WeaponSystem.cs`
- 创建: `Assets/Scripts/Player/Weapon.cs`
- 创建: `Assets/Scripts/Player/WeaponType.cs`

**Step 1: 创建武器类型枚举**

```csharp
public enum WeaponType
{
    Sword,    // 剑 - 攻速快，范围攻击
    Axe,      // 斧 - 伤害高，单体输出
    Bow,      // 弓 - 射速快，单发伤害低
    Crossbow  // 弩 - 射速慢，伤害高，可穿透
}
```

**Step 2: 创建Weapon基类**

```csharp
using UnityEngine;

public abstract class Weapon : MonoBehaviour
{
    public WeaponType weaponType;
    public float damage = 10f;
    public float attackSpeed = 1f;
    public float range = 2f;
    
    public abstract void Attack(Transform target);
}
```

**Step 3: 实现剑（近战）**

```csharp
public class Sword : Weapon
{
    void Awake()
    {
        weaponType = WeaponType.Sword;
        damage = 15f;
        attackSpeed = 2f;
        range = 1.5f;
    }
    
    public override void Attack(Transform target)
    {
        // 实现挥砍攻击
    }
}
```

**Step 4: 实现弓（远程）**

```csharp
public class Bow : Weapon
{
    public GameObject arrowPrefab;
    
    void Awake()
    {
        weaponType = WeaponType.Bow;
        damage = 10f;
        attackSpeed = 3f;
        range = 20f;
    }
    
    public override void Attack(Transform target)
    {
        // 实现射箭
    }
}
```

**Step 5: 创建武器切换系统**

```csharp
public class WeaponSystem : MonoBehaviour
{
    private List<Weapon> weapons = new List<Weapon>();
    private int currentWeaponIndex = 0;
    
    public void AddWeapon(Weapon weapon)
    {
        weapons.Add(weapon);
    }
    
    public void SwitchWeapon(int index)
    {
        if (index >= 0 && index < weapons.Count)
        {
            currentWeaponIndex = index;
            // 切换武器显示
        }
    }
    
    public void SwitchToNextWeapon()
    {
        currentWeaponIndex = (currentWeaponIndex + 1) % weapons.Count;
    }
}
```

**Step 6: 提交**

```bash
git add .
git commit - "feat: 实现武器系统基础"
```

---

## 阶段三：资源系统

### Task 4: 资源管理

**Files:**
- 创建: `Assets/Scripts/Game/ResourceManager.cs`
- 创建: `Assets/Scripts/Game/ResourceType.cs`
- 创建: `Assets/Scripts/Game/ResourceData.cs`

**Step 1: 创建资源类型**

```csharp
public enum ResourceType
{
    Wood,    // 木材
    Stone,   // 石头
    Food,    // 食物
    Metal    // 金属
}
```

**Step 2: 创建ResourceManager**

```csharp
using UnityEngine;
using System.Collections.Generic;

public class ResourceManager : MonoBehaviour
{
    public static ResourceManager Instance { get; private set; }
    
    private Dictionary<ResourceType, int> resources = new Dictionary<ResourceType, int>()
    {
        { ResourceType.Wood, 0 },
        { ResourceType.Stone, 0 },
        { ResourceType.Food, 0 },
        { ResourceType.Metal, 0 }
    };
    
    public int GetResource(ResourceType type) => resources[type];
    
    public bool SpendResource(ResourceType type, int amount)
    {
        if (resources[type] >= amount)
        {
            resources[type] -= amount;
            return true;
        }
        return false;
    }
    
    public void AddResource(ResourceType type, int amount)
    {
        resources[type] += amount;
    }
}
```

**Step 3: 创建资源收集点**

```csharp
public class ResourceNode : MonoBehaviour
{
    public ResourceType resourceType;
    public int amount = 10;
    
    public int Collect()
    {
        int collected = amount;
        amount = 0;
        Destroy(gameObject);
        return collected;
    }
}
```

**Step 4: 提交**

```bash
git add .
git commit - "feat: 实现资源管理系统"
```

---

## 阶段四：建造系统

### Task 5: 建筑系统基础

**Files:**
- 创建: `Assets/Scripts/Building/BuildingSystem.cs`
- 创建: `Assets/Scripts/Building/Building.cs`
- 创建: `Assets/Scripts/Building/BuildingType.cs`

**Step 1: 创建建筑类型**

```csharp
public enum BuildingType
{
    Wall,        // 城墙
    ArrowTower,  // 箭塔
    Gate,        // 大门
    Spike,       // 拒马
    Trap,        // 陷阱
    Warehouse,   // 仓库
    House        // 民居
}
```

**Step 2: 创建Building基类**

```csharp
using UnityEngine;

public class Building : MonoBehaviour
{
    public BuildingType buildingType;
    public int maxHealth = 100;
    public int currentHealth;
    public int level = 1;
    
    public int[] upgradeCost = { 50, 100, 200 };
    
    protected virtual void Start()
    {
        currentHealth = maxHealth;
    }
    
    public virtual void TakeDamage(int damage)
    {
        currentHealth -= damage;
        if (currentHealth <= 0)
        {
            DestroyBuilding();
        }
    }
    
    public virtual bool Upgrade()
    {
        if (level >= 3) return false;
        
        int cost = upgradeCost[level - 1];
        if (ResourceManager.Instance.SpendResource(ResourceType.Wood, cost))
        {
            level++;
            maxHealth += 50;
            currentHealth = maxHealth;
            return true;
        }
        return false;
    }
    
    protected virtual void DestroyBuilding()
    {
        Destroy(gameObject);
    }
}
```

**Step 3: 创建建造系统**

```csharp
using UnityEngine;

public class BuildingSystem : MonoBehaviour
{
    public GameObject buildingPreview;
    private BuildingType currentBuildingType;
    
    public void SetBuildingToPlace(BuildingType type)
    {
        currentBuildingType = type;
    }
    
    public bool CanBuild(Vector3 position)
    {
        // 检查是否在建造区域内
        // 检查资源是否足够
        return true;
    }
    
    public void PlaceBuilding(Vector3 position)
    {
        // 实例化建筑
    }
}
```

**Step 4: 提交**

```bash
git add .
git commit - "feat: 实现建造系统基础"
```

---

## 阶段五：敌人系统

### Task 6: 敌人AI

**Files:**
- 创建: `Assets/Scripts/Enemy/Enemy.cs`
- 创建: `Assets/Scripts/Enemy/EnemyType.cs`
- 创建: `Assets/Scripts/Enemy/EnemyAI.cs`

**Step 1: 创建敌人类型**

```csharp
public enum EnemyType
{
    Infantry,   // 步兵
    Archer,     // 弓箭手
    SiegeEngine, // 投石车
    Wolf,       // 狼
    Boss        // Boss
}
```

**Step 2: 创建Enemy基类**

```csharp
using UnityEngine;

public class Enemy : MonoBehaviour
{
    public EnemyType enemyType;
    public float health = 50f;
    public float damage = 10f;
    public float moveSpeed = 2f;
    public int attackRange = 2;
    
    protected Transform target;
    
    void Start()
    {
        target = GameObject.FindWithTag("CastleHeart").transform;
    }
    
    void Update()
    {
        if (target != null)
        {
            MoveToTarget();
        }
    }
    
    protected virtual void MoveToTarget()
    {
        // 移动向目标
    }
    
    public virtual void TakeDamage(float damage)
    {
        health -= damage;
        if (health <= 0)
        {
            Die();
        }
    }
    
    protected virtual void Die()
    {
        // 掉落资源
        Destroy(gameObject);
    }
}
```

**Step 3: 创建敌人AI**

```csharp
public class EnemyAI : MonoBehaviour
{
    private Enemy enemy;
    private Transform target;
    
    void Start()
    {
        enemy = GetComponent<Enemy>();
    }
    
    void Update()
    {
        float distance = Vector3.Distance(transform.position, target.position);
        
        if (distance <= enemy.attackRange)
        {
            AttackTarget();
        }
        else
        {
            MoveTowardsTarget();
        }
    }
}
```

**Step 4: 提交**

```bash
git add .
git commit - "feat: 实现敌人AI系统"
```

---

## 阶段六：波次系统

### Task 7: 敌人波次管理

**Files:**
- 创建: `Assets/Scripts/Game/WaveManager.cs`
- 创建: `Assets/Scripts/Game/WaveConfig.cs`

**Step 1: 创建波次配置**

```csharp
[System.Serializable]
public class WaveConfig
{
    public int waveNumber;
    public int infantryCount;
    public int archerCount;
    public int siegeEngineCount;
    public float spawnInterval;
    public float timeBetweenWaves = 300f; // 5分钟
}
```

**Step 2: 创建WaveManager**

```csharp
using UnityEngine;
using System.Collections;

public class WaveManager : MonoBehaviour
{
    public static WaveManager Instance { get; private set; }
    
    public WaveConfig[] waves;
    private int currentWave = 0;
    private bool isWaveActive = false;
    
    public int CurrentWave => currentWave;
    public bool IsWaveActive => isWaveActive;
    
    void Awake()
    {
        Instance = this;
    }
    
    public void StartNextWave()
    {
        if (currentWave >= waves.Length)
        {
            GameManager.Instance.SetGameState(GameManager.GameState.Victory);
            return;
        }
        
        StartCoroutine(SpawnWave(waves[currentWave]));
    }
    
    private IEnumerator SpawnWave(WaveConfig config)
    {
        isWaveActive = true;
        
        for (int i = 0; i < config.infantryCount; i++)
        {
            SpawnEnemy(EnemyType.Infantry);
            yield return new WaitForSeconds(config.spawnInterval);
        }
        
        // ... 其他敌人类型
        
        currentWave++;
        isWaveActive = false;
    }
    
    private void SpawnEnemy(EnemyType type)
    {
        // 实例化敌人
    }
}
```

**Step 3: 提交**

```bash
git add .
git commit - "feat: 实现波次管理系统"
```

---

## 阶段七：城堡之心

### Task 8: 核心城堡建筑

**Files:**
- 创建: `Assets/Scripts/Building/CastleHeart.cs`

**Step 1: 创建CastleHeart**

```csharp
using UnityEngine;

public class CastleHeart : Building
{
    public static CastleHeart Instance { get; private set; }
    
    protected override void Start()
    {
        base.Start();
        Instance = this;
        buildingType = BuildingType.CastleHeart;
        maxHealth = 500;
        currentHealth = maxHealth;
    }
    
    protected override void DestroyBuilding()
    {
        GameManager.Instance.SetGameState(GameManager.GameState.GameOver);
        base.DestroyBuilding();
    }
}
```

**Step 2: 提交**

```bash
git add .
git commit - "feat: 实现城堡之心"
```

---

## 阶段八：UI系统

### Task 9: 游戏UI

**Files:**
- 创建: `Assets/Scripts/UI/GameUI.cs`
- 创建: `Assets/Scripts/UI/ResourceUI.cs`
- 创建: `Assets/Scripts/UI/WaveUI.cs`

**Step 1: 创建资源UI**

```csharp
using UnityEngine;
using UnityEngine.UI;

public class ResourceUI : MonoBehaviour
{
    public Text woodText;
    public Text stoneText;
    public Text foodText;
    public Text metalText;
    
    void Update()
    {
        woodText.text = $"木材: {ResourceManager.Instance.GetResource(ResourceType.Wood)}";
        stoneText.text = $"石头: {ResourceManager.Instance.GetResource(ResourceType.Stone)}";
        foodText.text = $"食物: {ResourceManager.Instance.GetResource(ResourceType.Food)}";
        metalText.text = $"金属: {ResourceManager.Instance.GetResource(ResourceType.Metal)}";
    }
}
```

**Step 2: 创建波次UI**

```csharp
using UnityEngine;
using UnityEngine.UI;

public class WaveUI : MonoBehaviour
{
    public Text waveText;
    public Text timerText;
    public GameObject waveAnnouncement;
    
    void Start()
    {
        waveAnnouncement.SetActive(false);
    }
    
    public void ShowWaveAnnouncement(int waveNumber)
    {
        waveText.text = $"第 {waveNumber} 波";
        waveAnnouncement.SetActive(true);
        Invoke(nameof(HideAnnouncement), 3f);
    }
    
    private void HideAnnouncement()
    {
        waveAnnouncement.SetActive(false);
    }
}
```

**Step 3: 提交**

```bash
git add .
git commit - "feat: 实现游戏UI系统"
```

---

## 阶段九：关卡流程

### Task 10: 游戏流程整合

**Files:**
- 修改: `Assets/Scripts/Game/GameManager.cs`

**Step 1: 扩展GameManager**

```csharp
public class GameManager : MonoBehaviour
{
    public enum GamePhase { Day, Night }
    public GamePhase currentPhase = GamePhase.Day;
    public float dayDuration = 300f; // 5分钟
    public float nightDuration = 120f; // 2分钟
    
    private float phaseTimer;
    
    void Start()
    {
        phaseTimer = dayDuration;
    }
    
    void Update()
    {
        phaseTimer -= Time.deltaTime;
        
        if (phaseTimer <= 0)
        {
            SwitchPhase();
        }
    }
    
    private void SwitchPhase()
    {
        if (currentPhase == GamePhase.Day)
        {
            currentPhase = GamePhase.Night;
            phaseTimer = nightDuration;
            WaveManager.Instance.StartNextWave();
        }
        else
        {
            currentPhase = GamePhase.Day;
            phaseTimer = dayDuration;
        }
    }
}
```

**Step 2: 提交**

```bash
git add .
git commit - "feat: 实现日夜循环和游戏流程"
```

---

## 执行总结

完成所有任务后，游戏将具备以下核心功能：

1. ✅ 玩家可以在场景中自由移动
2. ✅ 玩家可以切换近战和远程武器
3. ✅ 玩家可以收集资源（木材、石头、食物、金属）
4. ✅ 玩家可以建造城墙、箭塔、大门等防御设施
5. ✅ 敌人按波次进攻
6. ✅ 城堡之心有血量系统
7. ✅ 胜利/失败条件正常触发

---

**Plan saved to:** `docs/plans/2026-03-14-castle-defender-implementation.md`
