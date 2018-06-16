---
title: 【Unity 研究院】PoolManager使用案例
abbrlink: e01df37c
tags:
  - Unity
  - Poolmanager
categories:
  - Unity
date: 2018-07-01 00:00:00
comments:
---
# PoolManager使用案例

## 引言
对象池技术——是一种广泛通用的内存优化手段。在首次建立对象时，便将其存储在池子中，需要再次使用时，不是每次都实例化一个新的对象，而是查找回收池中是否存在闲置的对象，将其重新激活利用。  
PoolManager 应该是个很常用的对象池插件，最近拿来用，还是有一些值得注意的地方的

```
using PathologicalGames;
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class SnakeSpawner : MonoBehaviour
{
    private static SnakeSpawner instance;
    private Dictionary<SnakePrefabType, SpawnPool> spawnPools = new Dictionary<SnakePrefabType, SpawnPool>();
    private Dictionary<SnakePrefabType, Transform> spawnFather = new Dictionary<SnakePrefabType, Transform>();
    public List<SnakePrefab> Prefabs;
    public static SnakeSpawner Instance
    {
        get
        {
            if (instance == null)
            {
                instance = FindObjectOfType<SnakeSpawner>();
                instance.gameObject.name = "SnakeSpawner";
            }
            return instance;
        }
    }
    private void Awake()
    {
        instance = this;
        AddPrefabToPool(SnakePrefabType.Player, 6, 10, true, 6, 10, 2);
        AddPrefabToPool(SnakePrefabType.Body, 30, 100, false, 50, 10, 5);
        AddPrefabToPool(SnakePrefabType.Food, 50, 100, true, 50, 5, 5);
        AddPrefabToPool(SnakePrefabType.Prop, 100, 150, true, 100, 5, 5, false, true);
    }
    // Use this for initialization
    void Start()
    {
    }

    // Update is called once per frame
    void Update()
    {
        if (Input.GetKeyDown(KeyCode.P))
        {
            GetPlayer(new Vector3(Random.Range(-5f, 5f), 0, 0));
        }
    }
    /// <summary>
    /// 给对象池添加对象
    /// </summary>
    /// <param name="type">预制体类型 </param>
    /// <param name="preloadAmount">默认初始化个数</param>
    /// <param name="limitAmount">限制池子里最大的Prefab数量</param>
    /// <param name="cullDespawned">开启自动清理池子</param>
    /// <param name="cullAbove">最终保留</param>
    /// <param name="cullDelay">多久清理一次</param>
    /// <param name="cullMaxPerPass">每次清理几个</param>
    /// <param name="limitInstances">开启限制后，当limitAmount数量不够时，实例化失败</param>
    /// <param name="limitFIFO">无限取Prefab(limitAmount数量不够时会把最初实例化的拿过来)</param>
    /// <returns></returns>
    public bool AddPrefabToPool(SnakePrefabType type, int preloadAmount, int limitAmount, bool cullDespawned, int cullAbove, int cullDelay, int cullMaxPerPass, bool limitInstances = false, bool limitFIFO = true)
    {
        SpawnPool spawnPool;
        if (!spawnPools.ContainsKey(type))
        {
            GameObject obj = new GameObject();
            obj.name = type.ToString() + "Pool";
            obj.transform.parent = transform;
            spawnPool = obj.AddComponent<SpawnPool>();
            spawnPools.Add(type, spawnPool);
        }
        spawnPool = spawnPools[type];
        for (int i = 0; i < Prefabs.Count; i++)
        {
            SnakePrefab item = Prefabs[i];
            if (item.type == type)
            {
                if (spawnPool.GetPrefabPool(item.Prefab) != null)
                {
                    return true;
                }
                item.Prefab.name = item.type.ToString();
                PrefabPool refabPool = new PrefabPool(item.Prefab);
                //默认初始化个数
                refabPool.preloadAmount = preloadAmount;
                //开启限制
                refabPool.limitInstances = limitInstances;
                //关闭无限取Prefab
                refabPool.limitFIFO = limitFIFO;
                //限制池子里最大的Prefab数量
                refabPool.limitAmount = limitAmount;
                //开启自动清理池子
                refabPool.cullDespawned = cullDespawned;
                //最终保留
                refabPool.cullAbove = cullAbove;
                //多久清理一次
                refabPool.cullDelay = cullDelay;
                //每次清理几个
                refabPool.cullMaxPerPass = cullMaxPerPass;
                spawnPool.CreatePrefabPool(refabPool);
                spawnPool._perPrefabPoolOptions.Add(refabPool);
                return true;
            }
        }
        return false;
    }
    public SnakePlayer GetPlayer(Vector3 pos = new Vector3(), Quaternion rot = new Quaternion())
    {
        SnakePlayer player = SpawnAllByType(SnakePrefabType.Player, pos, rot).GetComponent<SnakePlayer>();
        player.Init();
        return player;
    }
    public SpawnPool GetSpawnPoolByType(SnakePrefabType type)
    {
        return spawnPools[type];
    }
    public SnakeBody GetBody(Vector3 pos = new Vector3(), Quaternion rot = new Quaternion())
    {
        return SpawnAllByType(SnakePrefabType.Body, pos, rot).GetComponent<SnakeBody>();
    }
    public SnakeFood GetFood(Vector3 pos = new Vector3(), Quaternion rot = new Quaternion())
    {
        return SpawnAllByType(SnakePrefabType.Food, pos, rot).GetComponent<SnakeFood>();
    }
    public Transform SpawnAllByType(SnakePrefabType type, Vector3 pos = new Vector3(), Quaternion rot = new Quaternion())
    {
        return spawnPools[type].Spawn(type.ToString(), pos, rot, GetSpawnParent(type));
    }
    public static void Despawn(SnakePrefabType type, Transform trans)
    {
        instance.spawnPools[type].Despawn(trans, instance.spawnPools[type].transform);
    }

    /// <summary>
    /// 回收某一类型所有对象
    /// </summary>
    /// <param name="type"></param>
    public void DespawnTypeAll(SnakePrefabType type)
    {
        //spawnPools[type].DespawnAll(spawnPools[type].transform);
    }
    public Transform GetSpawnParent(SnakePrefabType type)
    {
        if (!spawnFather.ContainsKey(type))
        {
            Transform parent = new GameObject().transform;
            parent.name = "All" + type.ToString();
            spawnFather.Add(type, parent);
        }
        return spawnFather[type];
    }
}
[System.Serializable]
public struct SnakePrefab
{
    public SnakePrefabType type;
    public Transform Prefab;
}
public enum SnakePrefabType
{
    Player,
    Body,
    Food,
    Prop,
}
```