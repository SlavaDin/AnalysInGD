# АНАЛИЗ ДАННЫХ И ИСКУССТВЕННЫЙ ИНТЕЛЛЕКТ [in GameDev]
Отчет по лабораторной работе #4 выполнил(а):
- Стрежнев Егор Сергеевич
- РИ-230918
Отметка о выполнении заданий (заполняется студентом):

| Задание | Выполнение | Баллы |
| ------ | ------ | ------ |
| Задание 1 | * | * |
| Задание 2 | * | * |
| Задание 3 | * | * |

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
Познакомиться с одной из первых моделей нейросетей - перцептроном и реализовать эту модель в Unity

## 1 Поиск агентом объекта на сцене
После сделанных пунктов по методичке объект на сцене Unity начал двигаться
![image](https://github.com/user-attachments/assets/f35ed42a-d042-4611-a669-dd6092d62447)
1 модель

![image](https://github.com/user-attachments/assets/e3d4e075-2b2c-4021-a529-9a294c74fce4)
3 модели

![image](https://github.com/user-attachments/assets/162e864a-2d01-4ce7-bb77-e289dfa13447)
9 моделей

![image](https://github.com/user-attachments/assets/1dfb7803-80a9-4e59-a625-0c455bb2db7c)
27 моделей

![image](https://github.com/user-attachments/assets/7527a6b7-9d36-4b8e-9b3a-2e209bfe7f96)
Работа модели

После обучения, работа модели не всегда выдает удачный результат, при некоторых позициях шар начинает идти в абсолютно противоположную сторону, скорее всего это связанно с тем, что в процессе обучения, из за небольших пробелов между плитами на которых происходило обучение, шар заезжал на другую платформу. Но это только мое предположение. В целом обучение прошло удачно

## 2 Симулятор добычи ресурсов
После сделанных пунктов по методичке объект "перетаскивал" ресурсы

![image](https://github.com/user-attachments/assets/8a893886-a4cc-47c1-bf9e-1022f84837c1)
При запуске сцены, все объекты перетаскивали ресурсы абсолютно точно, скорее всего из-за того что я скачал проект с обученной моделью


![image](https://github.com/user-attachments/assets/168191e0-f2fd-45a1-9dc2-e8ef6bbb4372)
Графики оценки результатов обучения

## Задание 1. Найдите внутри C# скрипта “коэффициент корреляции” и сделать выводы о том, как он влияет на обучение модели
Ход работы:
- Внутри скрипта есть поле forceMultiplier - это поправочный коэффицент силы, она нужна для того чтобы сила не была (-1 и 1) слишком маленькая 

## Задание 2. Изменить параметры файла yaml-агента и определить какие параметры и как влияют на обучение модели. Привести описание не менее трех параметров.
Ход работы:
- max_steps: - Этот параметр определяет максимальное количество шагов, чем выше число тем лучше обучиться модель
- trainer_type: -  Этот параметр определяет, какой алгоритм интеллектуального обучения используется
- num_layers: Этот параметр определяет количество слоёв вашей обучающей сети. Чем больше это число, тем глубже модель
  
## Задание 3. Приведите примеры, для каких игровых задачи и ситуаций могут использоваться примеры 1 и 2 с ML-Agent’ом. В каких случаях проще использовать ML-агент, а не писать программную реализацию решения?
Ход работы:
- Если модель обучиться следовать за целью, то 1 модель может использоваться, например, для преследования за игроком. Или же направление любого снаряда (например пули или файербола) в цель
- 2 модель может быть представленна в виде определенного курьера между двуям точками
- ML-агента удобно использовать если задача имеет множество видов решения, или не имеет определенного алгоритма решения, т.е. множество исходов событий. Или если алгоритм невероятно сложный и его реализация в ручную требует большое количество ресурсов и времени.

## Выводы
Я изучил систему ML-агента в юнити, научился обучать готовые модели и тестировать их
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