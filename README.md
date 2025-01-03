# АНАЛИЗ ДАННЫХ И ИСКУССТВЕННЫЙ ИНТЕЛЛЕКТ [in GameDev]
Отчет по лабораторной работе #2 выполнил(а):
- Темербаев Константин Валерьевич
- РИ230949

| Задание | Выполнение | Баллы |
| ------ | ------ | ------ |
| Задание 1 | * | 60 |
| Задание 2 | * | 20 |
| Задание 3 | * | 20 |

Работу проверили:



Структура отчета

- Данные о работе: название работы, фио, группа, выполненные задания.
- Цель работы.
- Задание 1.
- Код реализации выполнения задания. Визуализация результатов выполнения (если применимо).
- Задание 2.
- Код реализации выполнения задания. Визуализация результатов выполнения (если применимо).
- Задание 3.
- Код реализации выполнения задания. Визуализация результатов выполнения (если применимо).

## Цель работы
Научиться передавать в Unity данные из Google Sheets с помощью Python.

## Задание 1
### Выберите одну из игровых переменных в игре СПАСТИ РТФ: Выживание (HP, SP, игровая валюта, здоровье и т.д.), опишите её роль в игре, условия изменения / появления и диапазон допустимых значений. Постройте схему экономической модели в игре и укажите место выбранного ресурса в ней.

##HP (здоровье)
Одна из самых главных переменных, которая отвечает за условия продолжения игры, если количество hp становится равное или меньше нуля, то игровой процесс заканчивается.

Очки здоровья можно потерять, при получении урона или же восполнить можно при помощи покупки в магазине/прокачкой навыков

![Image alt](https://github.com/TemKost/AD-in-GameDev-lab2/blob/main/HP%20rtf.png)



## Задание 2
###  С помощью скрипта на языке Python заполните google-таблицу данными, описывающими выбранную игровую переменную в игре “СПАСТИ РТФ:Выживание”. Средствами google-sheets визуализируйте данные в google-таблице (постройте график / диаграмму и пр.) для наглядного представления выбранной игровой величины. Опишите характер изменения этой величины, опишите недостатки в реализации этой величины (например, в игре может произойти условие наступления эксплойта) и предложите до 3-х вариантов модификации условий работы с переменной, чтобы сделать игровой опыт лучше.

https://docs.google.com/spreadsheets/d/1ifU3RndTpyBalDr_yfdVHkoCAQodYp9CicQFVVw5sTw/edit?gid=0#gid=0

При получении урона отнимается 10 или 20 здоровья, при этом не может стать меньше нуля
Количество жизней можно полностью восплнить при покупке за валюту или при прокачке навыков

```py

from random import random, Random

import numpy as np
import gspread
gc = gspread.service_account(filename='unitydatasciense-445418-2398f2d9be3c.json')
sh = gc.open("UnitySheets")
price = np.random.randint(2000, 10000, 11)
mon = list(range(1,11))
hp = 30
i = 0
while i <= len(mon):
    j = np.random.randint(1,4)
    i += 1
    if j == 1:
        dmg = 10
        hp = hp - dmg
        if hp <= 0:
            hp = 0
            tempHP = str(hp)
            sh.sheet1.update(('A' + str(i)), str(i))
            sh.sheet1.update(('B' + str(i)), str(j))
            sh.sheet1.update(('C' + str(i)), str(tempHP))
            print(tempHP)
            continue
        else:
            tempHP = str(hp)
            sh.sheet1.update(('A' + str(i)), str(i))
            sh.sheet1.update(('B' + str(i)), str(j))
            sh.sheet1.update(('C' + str(i)), str(tempHP))
            print(tempHP)
    if j == 2:
        dmg = 20
        hp = hp - dmg
        if hp <= 0:
            hp = 0
            tempHP = str(hp)
            sh.sheet1.update(('A' + str(i)), str(i))
            sh.sheet1.update(('B' + str(i)), str(j))
            sh.sheet1.update(('C' + str(i)), str(tempHP))
            print(tempHP)
            continue
        else:
            tempHP = str(hp)
            sh.sheet1.update(('A' + str(i)), str(i))
            sh.sheet1.update(('B' + str(i)), str(j))
            sh.sheet1.update(('C' + str(i)), str(tempHP))
            print(tempHP)
    if j == 3:
        hp = 30
        tempHP = str(hp)
        sh.sheet1.update(('A' + str(i)), str(i))
        sh.sheet1.update(('B' + str(i)), str(j))
        sh.sheet1.update(('C' + str(i)), str(tempHP))
        print(tempHP)

```

## Задание 3
### Настройте на сцене Unity воспроизведение звуковых файлов, описывающих динамику изменения выбранной переменной. Например, если выбрано здоровье главного персонажа вы можете выводить сообщения, связанные с его состоянием.

```c#

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

    // Start is called before the first frame update
    void Start()
    {
        StartCoroutine(GoogleSheets());
    }

    // Update is called once per frame
    void Update()
    {
        if (dataSet["Mon_" + i.ToString()] == 30 & statusStart == false & i != dataSet.Count)
        {
            StartCoroutine(PlaySelectAudioGood());
            Debug.Log(dataSet["Mon_" + i.ToString()]);
        }

        if (dataSet["Mon_" + i.ToString()] == 20 & statusStart == false & i != dataSet.Count)
        {
            StartCoroutine(PlaySelectAudioNormal());
            Debug.Log(dataSet["Mon_" + i.ToString()]);
        }

        if (dataSet["Mon_" + i.ToString()] <= 10 & statusStart == false & i != dataSet.Count)
        {
            StartCoroutine(PlaySelectAudioBad());
            Debug.Log(dataSet["Mon_" + i.ToString()]);
        }
    }

    IEnumerator GoogleSheets()
    {
        UnityWebRequest curentResp = UnityWebRequest.Get("https://sheets.googleapis.com/v4/spreadsheets/1ifU3RndTpyBalDr_yfdVHkoCAQodYp9CicQFVVw5sTw/values/Лист1?key=AIzaSyDo02p5v_h3I7wtX_KMxvBwwtdBj3HBAfs");
        yield return curentResp.SendWebRequest();
        string rawResp = curentResp.downloadHandler.text;
        var rawJson = JSON.Parse(rawResp);
        foreach (var itemRawJson in rawJson["values"])
        {
            var parseJson = JSON.Parse(itemRawJson.ToString());
            var selectRow = parseJson[0].AsStringList;
            dataSet.Add(("Mon_" + selectRow[0]), float.Parse(selectRow[2]));
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

