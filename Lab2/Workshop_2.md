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
- Задание 1.
- Код реализации выполнения задания. Визуализация результатов выполнения (если применимо).
- Задание 2.
- Код реализации выполнения задания. Визуализация результатов выполнения (если применимо).
- Задание 3.
- Код реализации выполнения задания. Визуализация результатов выполнения (если применимо).
- Выводы.
- ✨Magic ✨

## Цель работы
Научиться передавать в Unity данные из Google Sheets с помощью Python. Также научиться анализировать игровые переменные, описывать их изменение и поведение

## Задание 1
### Выберите одну из игровых переменных в игре СПАСТИ РТФ: Выживание (HP, SP, игровая валюта, здоровье и т.д.), опишите её роль в игре, условия изменения / появления и диапазон допустимых значений. Постройте схему экономической модели в игре и укажите место выбранного ресурса в ней.
Ход работы:
- Для анализа выбрана характеристика "Здоровье", ее смысл в том, чтобы отображать насколько "жив" герой.
  Изменение переменной:
  - если игрок получил урон, то она уменьшается
  - если игрок во время прокачки выбрал соотвествующие навыки, то здоровье восполняется
  Диапазон допустимых значений переменной прост, от 0 и до лимита указанного в игре
- Схема эк.модели ресурса:
![alt text](Model.png)

## Задание 2
### ### С помощью скрипта на языке Python заполните google-таблицу данными, описывающими выбранную игровую переменную в игре “СПАСТИ РТФ:Выживание”. Средствами google-sheets визуализируйте данные в google-таблице (постройте график / диаграмму и пр.) для наглядного представления выбранной игровой величины. Опишите характер изменения этой величины, опишите недостатки в реализации этой величины (например, в игре может произойти условие наступления эксплойта) и предложите до 3-х вариантов модификации условий работы с переменной, чтобы сделать игровой опыт лучше.
Ход работы:
- Для начала, определим как меняется выбранная переменная:
Здоровье меняется в зависимсоти от получения им урона, это значит, что изменение этой переменной зависит от игрока и его навыка. Поскольку есть множество вариантов изменения этого параметра, возьмём ситуацию, когда игрок получает урон с некоторым промежутком времени.
- Из-за простоты выбранной ситуации график изменения переменной выглядит как обычная прямая (В более сложных ситуациях график может иметь более сложное поведене):
![alt text](график.png)
Ссылка на таблицу: https://docs.google.com/spreadsheets/d/1HICyyE5F96OtbYCe5ayZfNlHBHoZV4ns1PyzKgv7mEw/edit?gid=0#gid=0
- По графику видно, что величина изменяется от 30 (лимит здоровья) до 0 (конец игры), уменьшение происходит когда игрок получает урон (в данном примере -10), 
    а увеличение, когда игрок либо использует "Вампиризм", либо "завершает" волну.
- Заполнение таблицы было выполненно с помощью кода на Python:
```Python
import gspread
import numpy as np
gc = gspread.service_account(filename = 'unitydatascience-444810-798813615062.json')
sh = gc.open("UnityWorkshop2")
current_hp = 30
i = 0
while current_hp >= 0:
    i += 1
    if i == 0:
        continue
    else:
        sh.sheet1.update_acell(('A' + str(i)), str(i))
        sh.sheet1.update_acell(('B' + str(i)), str(current_hp))
        print(current_hp)
        current_hp -= 10
```
- Поскольку, в игре СПАСТИ РТФ довольно мало взаимодействия с хп и мало условий его изменения, то можно дополнить функционал игры следющими идеями:
    - Добавить предмет который увеличивает текущее здоровье двуями способами, либо моментально, либо переодически
    - Возможность расходовать текущее здоровье в замен на какой ресурс (например патроны), возможно придется добавлять новые предметы чтобы не ломать логику              старого предмета с этим нововведением
    - Возможность прибавлять предел здоровья (вне прокачки) на определенный промежуток времени, например, для битвы с боссом


## Задание 3
### Настройте на сцене Unity воспроизведение звуковых файлов, описывающих динамику изменения выбранной переменной. Например, если выбрано здоровье главного персонажа вы можете выводить сообщения, связанные с его состоянием.
Ход работы:
- Для реализации данного задания я выделил правильно по которым описывается динамика изменения здоровья:
    - Если хп около лимита (в моем случае hp >= 30), то воспроизводится звук "хороший"
    - Если хп около среднего значения (в моем случае 30 > hp >= 20), то воспроизводится звук "средний"
    - Если хп около минимума (в моем случае hp <= 0), то воспроизводится звук "плохой"
- Для отображения этой динамики в Unity я создал компонент с таким скриптом (Это скрипт из методички, с небольшими изменениями):
```C#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.Networking;
using SimpleJSON;

public class NewBehaviourScript : MonoBehaviour
{
    public AudioClip goodSpeak;
    public AudioClip normalSpeak;
    public AudioClip badSpeak;
    private AudioSource selectAudio;
    private Dictionary<string, float> dataSet = new Dictionary<string, float>();
    private bool statusStart = false;
    private int i = 1;
    void Start()
    {
        StartCoroutine(GoogleSheets());
    }
    void Update()
    {
        if (dataSet["Mon_" + i.ToString()] >= 30 & statusStart == false & i != dataSet.Count)
        {
            StartCoroutine(PlaySelectAudioGood());
            Debug.Log(dataSet["Mon_" + i.ToString()]);
            Debug.Log("Проигрывается звук 'Horosho'");
        }
        if (dataSet["Mon_" + i.ToString()] < 30 & dataSet["Mon_" + i.ToString()] >= 20 & statusStart == false & i != dataSet.Count)
        {
            StartCoroutine(PlaySelectAudioNormal());
            Debug.Log(dataSet["Mon_" + i.ToString()]);
            Debug.Log("Проигрывается звук 'Sredne'");
        }
        if (dataSet["Mon_" + i.ToString()] < 20 & statusStart == false & i != dataSet.Count)
        {
            StartCoroutine(PlaySelectAudioBad());
            Debug.Log(dataSet["Mon_" + i.ToString()]);
            Debug.Log("Проигрывается звук 'Ploho'");
        }
    }

    IEnumerator GoogleSheets()
    {
        UnityWebRequest curentResp = UnityWebRequest.Get("https://sheets.googleapis.com/v4/spreadsheets/1-TBYwyDcrPz_f4pHD6Jv8I1gbH_UNFmD-HplJS98Nkk/values/Лист1?key=AIzaSyBFrX6sxQ8GY5gv57Az0AXnpU1rQKbjwJU");
        yield return curentResp.SendWebRequest();
        string rawResp = curentResp.downloadHandler.text;
        var rawJson = JSON.Parse(rawResp);
        foreach (var itemRawJson in rawJson["values"])
        {
            var parseJson = JSON.Parse(itemRawJson.ToString());
            var selectRow = parseJson[0].AsStringList;
            dataSet.Add(("Mon_" + selectRow[0]), float.Parse(selectRow[1]));
        }
    }
    IEnumerator PlaySelectAudioGood()
    {
        statusStart = true;
        selectAudio = GetComponent<AudioSource>();
        selectAudio.clip = goodSpeak;
        selectAudio.Play();
        yield return new WaitForSeconds(3);
        statusStart = false;
        i++;
    }
    IEnumerator PlaySelectAudioNormal()
    {
        statusStart = true;
        selectAudio = GetComponent<AudioSource>();
        selectAudio.clip = normalSpeak;
        selectAudio.Play();
        yield return new WaitForSeconds(3);
        statusStart = false;
        i++;
    }
    IEnumerator PlaySelectAudioBad()
    {
        statusStart = true;
        selectAudio = GetComponent<AudioSource>();
        selectAudio.clip = badSpeak;
        selectAudio.Play();
        yield return new WaitForSeconds(4);
        statusStart = false;
        i++;
    }
}
```
- Как видно на изображении ниже, скрипт работает без проблем и в Unity наглядно изображена динамика изменения здоровья
![alt text](unity.png)

## Выводы

Научился ананлизировать ресурсы в играх,а также визуализировать изменения и поведение этого ресурса, при помощи Google Sheets.

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
