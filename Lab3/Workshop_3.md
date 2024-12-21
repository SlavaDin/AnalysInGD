# АНАЛИЗ ДАННЫХ И ИСКУССТВЕННЫЙ ИНТЕЛЛЕКТ [in GameDev]
Отчет по лабораторной работе #1 выполнил(а):
- Брюхов Вячеслав Романович
- РИ-230934
Отметка о выполнении заданий (заполняется студентом):

| Задание | Выполнение | Баллы |
| ------ | ------ | ------ |
| Задание 1 | * | ? |
| Задание 2 | * | ? |
| Задание 3 | * | ? |

знак "*" - задание выполнено; знак "#" - задание не выполнено;

Работу проверили:
- к.т.н., доцент Денисов Д.В.
- к.э.н., доцент Панов М.А.
- ст. преп., Фадеев В.О.

[![N|Solid](https://cldup.com/dTxpPi9lDf.thumb.png)](https://nodesource.com/products/nsolid)

[![Build Status](https://travis-ci.org/joemccann/dillinger.svg?branch=master)](https://travis-ci.org/joemccann/dillinger)

Структура отчета

- Данные о работе: название работы, фио, группа, выполненные задания.
- Цель работы.
- Задание 1.
- Код реализации выполнения задания. Визуализация результатов выполнения (если применимо).
- Задание 2.
- Код реализации выполнения задания. Визуализация результатов выполнения (если применимо).
- Задание 3.
- Код реализации выполнения задания. Визуализация результатов выполнения (если применимо).
- Выводы.
- ✨Magic ✨

## Цель работы
Разработать оптимальный баланс изменения сложности для десяти уровней игры Dragon Picker.

## Задание 1. Начало работы с прототипом
### Ознакомьтесь с презентацией третьей лекции, оцените структуры игры Dragon Picker. Скачайте и ознакомьтесь с прототипом игры Dragon Picker на движке Unity. Запустите игровую сцену, проанализируйте движение дракона:
### - Какие переменные влияют на движение дракона на сцене? Укажите эти переменные.
### - Какие переменные влияют на сложность игры на сцене? Укажите эти переменные.
Ход работы:
- На движение дракончика влияют параметры скорости и рандомное значение, отвечающее за то, будет ли дракон менять траекторию в определенный момент времени
- На сложность игры на сцене влияют параметры: скорость дракона, частота появления яйца, скорость его падения, количество щитов, частота изменения направления движения дракона и размер щита

## Задание 2. Начало работы с шаблоном google-таблицы для балансировки игры
### Ознакомьтесь с презентацией третьей лекции, оцените структуры шаблона таблицы для балансировки. Отметьте, как может быть использован шаблон таблицы для визуализации изменения уровней сложности в игре Dragon Picker?
Ход работы:
- Таблица может быть использовать для того чтобы отследить как меняются переменные "баланса" в зависимости от каждого уровня. Так же для удобства их можно визуализировать различными графиками

## Задание 3. Задания к работе
## - Задание 1. Предложите вариант изменения найденных переменных для 10 уровней в игре. Визуализируйте изменение уровня сложности в таблице.
Ход работы:
- Для работы я выбрал следующие переменные: скорость дракона, частота появления яйца, количество щитов
- Эти параметры можно изменять как "в цифрах" (например, скорость дракона + 10), так и в "процентах" (например, скорость дракона * 1,(какой то процент))
- Для работы я решил что изменение в процентах будет лучше, т.к. поймать примерные "балансные" значения будет проще
- Автоматическое заполнение таблицы выполняется следющим кодом:
```Python
import gspread
import numpy as np
gc = gspread.service_account(filename='dragonpickeranalis-75d63a0aadb4.json')
sh = gc.open("DragonPickerAnalis")

def update_indexs():
    for i in range(1, 11):
        sh.sheet1.update_acell("A" + str(i+1), str(i))

def update_dragon_speed(percent: str):
    current_speed = float(sh.sheet1.acell("B2").value)
    for i in range(3, 12):
        current_speed = str(current_speed * float(str(f"1.{percent}")))
        current_speed = current_speed.split(".")
        current_speed = current_speed[0] + "," + current_speed[1]
        sh.sheet1.update_acell("B" + str(i), str(current_speed))
        current_speed = current_speed.split(",")
        current_speed = float(current_speed[0] + "." + current_speed[1])

def update_time_egg_drop(percent: str):
    tail = 100 - int(percent)
    print(tail)
    current_time = float(sh.sheet1.acell("C2").value)
    for i in range(3, 12):
        current_time = str(current_time * float(str(f"0.{tail}")))
        current_time = current_time.split(".")
        current_time = current_time[0] + "," + current_time[1]
        sh.sheet1.update_acell("C" + str(i), str(current_time))
        current_time = current_time.split(",")
        current_time = float(current_time[0] + "." + current_time[1])

def update_shields_count():
    current_count = int(sh.sheet1.acell("E2").value)
    repeat = 1
    for i in range(3, 12):
        sh.sheet1.update_acell("E" + str(i), str(current_count))
        repeat += 1
        if repeat == 3:
            current_count -= 1
            repeat = 0

if __name__ == "__main__":
    update_dragon_speed("20")
    update_time_egg_drop("10")
    update_egg_speed("05")
    update_shields_count()
```
- Таблица описывающая изменения переменных
![alt text](Graph.png)
Ссылка на таблицу: https://docs.google.com/spreadsheets/d/1efO-KsePNR6pN_EIpBTdKl5vsMA5N4R0OKUmpqiDzdQ/edit?gid=0#gid=0
- Из таблицы видно что скорость дракона и частота появления яйца изменяются равномерно. Это является как и плюсом, т.к. не происходит резких скачков в сложности игры, так и минусом, потому что рассчитано на казуальных игроков.
- Количество щитов меняется каждые 3 уровня, ее изменение влияет на то, что игроку дается меньше шансов ошибиться. В контексте игры Dragon Picker это не выходит за пределы разумного и только мотивирует оттачивать свои игровые навыки

## - Задание 2. Создайте 10 сцен на Unity с изменяющимся уровнем сложности.
## - Задание 3. Решение в 80+ баллов должно визуализировать данные из google-таблицы, и с помощью Python передавать в проект Unity. В Python данные также должны быть визуализированы.
P.S. Задания 2 и Задание 3 имеют взаимодействие друг с другом, поэтому я их объеденил в один подраздел. 
- В предыдущем заданий я уже описал изменение всех выбранных игровых параметров влияющих на баланс, а также визуализировал изменения. Для того чтобы передать эти параметры в заранее сделанные игровые сцены (я просто скопировал 1 сцену Dragon Picker 10 раз) я сделал следующие шаги:
1. Создать пустой объект на сцене и прикрепить к нему компонент отвечающий за загрузку данных из таблицы, хранение информации об уровне, выбор и хранение параметров "баланса". Выборка параметров баланса зависит от указанном в сериализованном поле значении уровня.
Так же этот скрипт, после того как считал и определил нужные нам параметры, вызывает событие которое означает что данныве нужно поменять. Это делается для того, чтобюы выдержать паузу перед тем как прочитаются данные из гугл таблицы.

Скрипт компонента:
```C#  
using System;
using System.Collections;
using UnityEngine;
using UnityEngine.Networking;
using SimpleJSON;
using System.Globalization;

public class LoadSettings : MonoBehaviour
{
    [SerializeField] private int _sceneNumber;
    #region Private Fields
    private float _dragonSpeed;
    private float _timeEggDrop;
    private float _mass;
    private int _countShield;
    #endregion

    #region Getters
    public float DragonSpeed { get { return _dragonSpeed; } }
    public float TimeEggDrop { get { return _timeEggDrop; } }
    public float Mass { get { return _mass; } }
    public int CountShield { get { return _countShield; } }
    #endregion

    public event Action CoroutineIsStopped;

    void Awake()
    {
        StartCoroutine(GoogleSheets());
    }
    private float ConvertStrToFloat(string str)
    {
        //Debug.Log(str);
        str = str.Replace(',', '.');
        float result = float.Parse(str, CultureInfo.InvariantCulture.NumberFormat); ;
        return result;
    }
    IEnumerator GoogleSheets()
    {
        UnityWebRequest curentResp = UnityWebRequest.Get("https://sheets.googleapis.com/v4/spreadsheets/1aw64RwvLdCHzxuFXzrZL03ffxSlYch_INkygCQ_HYMU/values/Лист1?key=AIzaSyBzuQEHb02mqr_SodKQpAWk5PL6YdZdXAc");
        yield return curentResp.SendWebRequest();
        string rawResp = curentResp.downloadHandler.text;
        var rawJson = JSON.Parse(rawResp);
        foreach (var itemRawJson in rawJson["values"])
        {
            var parseJson = JSON.Parse(itemRawJson.ToString())[0];
            //Debug.Log(parseJson);
            string lvl = parseJson[0];
            //Debug.Log(lvl);
            var tryStr = int.TryParse(lvl, out var number);
            if (tryStr == true)
            {
                int intLvl = int.Parse(lvl);
                if (_sceneNumber == intLvl)
                {
                    Debug.Log(intLvl);
                    _dragonSpeed = ConvertStrToFloat(parseJson[1]);
                    _timeEggDrop = ConvertStrToFloat(parseJson[2]);
                    _mass = ConvertStrToFloat(parseJson[3]);
                    string strCount = parseJson[4];
                    _countShield = Convert.ToInt32(strCount);
                    //Debug.Log($"Скорость дракона: {_dragonSpeed}, " +
                    //    $"Скорость появления яйца: {_timeEggDrop}, " +
                    //    $"Скорость падения яйца: {_mass}, " +
                    //    $"Количество щитов: {_countShield}");
                }
            }
        }
        CoroutineIsStopped?.Invoke();

    }
}
```
Объект отвечающий за загрузку параметров, их хранение и их передачу:
![alt text](1.png)

2. Далее нужно залезть в код в котором определяется и реализуется поведение дракона, а так же другой скрипт который делает тоже самое но с щитом
3. 
Нужные нам скрипты в файловой системе юнити:
![alt text(2.png))

В скрипте отвечающим за дракона мы создаем обработчик события, который будет обновлять нужные нам параметры:
```C#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class EnemyDragon : MonoBehaviour
{
    public GameObject dragonEggPrefab;
    [SerializeField] private LoadSettings _settings;
    public float speed;
    public float timeBetweenEggDrops;
    public float leftRightDistance = 10f;
    public float chanceDirection = 0.1f;

    private void OnEnable()
    {
        _settings.CoroutineIsStopped += UpdateFields;
    }


    void Start()
    {
        Invoke("DropEgg", 2f);
    }

    private void UpdateFields()
    {
        speed = _settings.DragonSpeed;
        //Debug.Log(speed);
        timeBetweenEggDrops = _settings.TimeEggDrop;
        //Debug.Log(timeBetweenEggDrops);
    }

    void DropEgg(){
        Vector3 myVector = new Vector3(0.0f, 5.0f, 0.0f);
        GameObject egg = Instantiate<GameObject>(dragonEggPrefab);
        egg.transform.position = transform.position + myVector;
        Invoke("DropEgg", timeBetweenEggDrops);
    }

    // Update is called once per frame
    void Update()
    {
        Vector3 pos = transform.position;
        pos.x += speed * Time.deltaTime;
        transform.position = pos;

        if (pos.x < -leftRightDistance){
            speed = Mathf.Abs(speed);
        }
        else if (pos.x > leftRightDistance){
            speed = -Mathf.Abs(speed);
        }
    }

    private void FixedUpdate() {
        if (Random.value < chanceDirection){
            speed *= -1;
        }
    }
}
```

И делаем тоже самое в скрипте который создает щит:
```C#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.SceneManagement;

public class DragonPicker : MonoBehaviour
{
    [SerializeField] private LoadSettings _settings;
    public GameObject energyShieldPrefab;
    public int numEnergyShield;
    public float energyShieldBottomY = -6f;
    public float energyShieldRadius = 1.5f;
    
    public List<GameObject> shieldList;

    private void OnEnable()
    {
        _settings.CoroutineIsStopped += UpdateShield;
    }

    void CreateShield()
    {
        shieldList = new List<GameObject>();

        for (int i = 1; i <= numEnergyShield; i++)
        {
            GameObject tShieldGo = Instantiate<GameObject>(energyShieldPrefab);
            tShieldGo.transform.position = new Vector3(0, energyShieldBottomY, 0);
            tShieldGo.transform.localScale = new Vector3(1 * i, 1 * i, 1 * i);
            shieldList.Add(tShieldGo);
        }
    }

    private void UpdateShield()
    {
        numEnergyShield = _settings.CountShield;
        CreateShield();
    }

    public void DragonEggDestroyed(){
        GameObject[] tDragonEggArray = GameObject.FindGameObjectsWithTag("Dragon Egg");
        foreach (GameObject tGO in tDragonEggArray){
            Destroy(tGO);
        }
        int shieldIndex = shieldList.Count - 1;
        GameObject tShieldGo = shieldList[shieldIndex];
        shieldList.RemoveAt(shieldIndex);
        Destroy(tShieldGo);

        if (shieldList.Count == 0){
            SceneManager.LoadScene("_0Scene");
        }
    }
}
```

Далее нужно передать каждому объекту, который реализует функционал дракона и щита, ссылку на скрипт который определяет параметры: 
![alt text](3.png)

И так нужно сделать с каждой сценой

Итог (для примера был взят последний 10 уровень, в котором отображены измененный параметры в инспекторе):
![alt text](4.png)
Поскольку я не нашел как прикрепить скринкаст в отчет, то можете поверить только на слово, что 10 уровень, не смотря на цифры, довольно сбалансированн и играется интересно и динамично

Для визуализации изменений параметров был обновлен код заполнения таблицы:
```Python
import gspread
import matplotlib.pyplot as plt
import numpy as np
gc = gspread.service_account(filename='dragonpickeranalis-75d63a0aadb4.json')
sh = gc.open("DragonPickerAnalis")

def update_indexs():
    for i in range(1, 11):
        sh.sheet1.update_acell("A" + str(i+1), str(i))

def update_dragon_speed(percent: str):
    current_speed = float(sh.sheet1.acell("B2").value)
    x = [current_speed]
    y = [1]
    for i in range(3, 12):
        current_speed = str(current_speed * float(str(f"1.{percent}")))
        current_speed = current_speed.split(".")
        current_speed = current_speed[0] + "," + current_speed[1]
        sh.sheet1.update_acell("B" + str(i), str(current_speed))
        current_speed = current_speed.split(",")
        current_speed = float(current_speed[0] + "." + current_speed[1])
        x.append(current_speed)
        y.append(i)
    plt.plot(x,y)
    plt.show()

def update_time_egg_drop(percent: str):
    tail = 100 - int(percent)
    current_time = float(sh.sheet1.acell("C2").value)
    x = [current_time]
    y = [1]
    for i in range(3, 12):
        current_time = str(current_time * float(str(f"0.{tail}")))
        current_time = current_time.split(".")
        current_time = current_time[0] + "," + current_time[1]
        sh.sheet1.update_acell("C" + str(i), str(current_time))
        current_time = current_time.split(",")
        current_time = float(current_time[0] + "." + current_time[1])
        x.append(current_time)
        y.append(i)
    plt.plot(x,y)
    plt.show()

def update_shields_count():
    current_count = int(sh.sheet1.acell("D2").value)
    repeat = 1
    x = [current_count]
    y = [1]
    for i in range(3, 12):
        sh.sheet1.update_acell("D" + str(i), str(current_count))
        repeat += 1
        if repeat == 3:
            current_count -= 1
            repeat = 0
        x.append(current_count)
        y.append(i)
    plt.plot(x, y)
    plt.show()

if __name__ == "__main__":
    update_dragon_speed("20")
    update_time_egg_drop("10")
    update_shields_count()
```

Результат: 
- Изменение скорости дракона:
![alt text](graph1.png)

- Изменение частоты появления яйца:
![alt text](graph2.png)

- Изменение количества щитов:
![alt text](graph3.png)



## Выводы
Я научился работать с параметрами баланса, визуализировать их, расчитывать, а также применять алгоритм изменения параметров в юнити, с помощью Python иq гугл таблиц

| Plugin | README |
| ------ | ------ |
| Dropbox | [plugins/dropbox/README.md][PlDb] |
| GitHub | [plugins/github/README.md][PlGh] |
| Google Drive | [plugins/googledrive/README.md][PlGd] |
| OneDrive | [plugins/onedrive/README.md][PlOd] |
| Medium | [plugins/medium/README.md][PlMe] |
| Google Analytics | [plugins/googleanalytics/README.md][PlGa] |

## Powered by

**BigDigital Team: Denisov | Fadeev | Panov**