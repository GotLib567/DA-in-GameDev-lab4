# АНАЛИЗ ДАННЫХ И ИСКУССТВЕННЫЙ ИНТЕЛЛЕКТ [in GameDev]
Отчет по лабораторной работе #4 выполнил(а):
- Широков Глеб Игоревич
- РИ-210933
Отметка о выполнении заданий (заполняется студентом):

| Задание | Выполнение | Баллы |
| ------ | ------ | ------ |
| Задание 1 | * | 60 |
| Задание 2 | # | 20 |
| Задание 3 | # | 20 |

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

В проекте Unity реализовать перцептрон, который умеет производить вычисления:
- OR | Дать комментарий о корректности работы
- And | Дать комментарий о корректности работы
- NAND | Дать комментарий о корректности работы
- XOR | Дать комментарий о корректности работы

## Задание 1

- Навесил файл с кодом перцептрона на пустой GameObject:

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

[System.Serializable]
public class TrainingSet
{
	public double[] input;
	public double output;
}

public class Perceptron : MonoBehaviour {

	public TrainingSet[] ts;
	double[] weights = {0,0};
	double bias = 0;
	double totalError = 0;

	double DotProductBias(double[] v1, double[] v2) 
	{
		if (v1 == null || v2 == null)
			return -1;
	 
		if (v1.Length != v2.Length)
			return -1;
	 
		double d = 0;
		for (int x = 0; x < v1.Length; x++)
		{
			d += v1[x] * v2[x];
		}

		d += bias;
	 
		return d;
	}

	double CalcOutput(int i)
	{
		double dp = DotProductBias(weights,ts[i].input);
		if(dp > 0) return(1);
		return (0);
	}

	void InitialiseWeights()
	{
		for(int i = 0; i < weights.Length; i++)
		{
			weights[i] = Random.Range(-1.0f,1.0f);
		}
		bias = Random.Range(-1.0f,1.0f);
	}

	void UpdateWeights(int j)
	{
		double error = ts[j].output - CalcOutput(j);
		totalError += Mathf.Abs((float)error);
		for(int i = 0; i < weights.Length; i++)
		{			
			weights[i] = weights[i] + error*ts[j].input[i]; 
		}
		bias += error;
	}

	double CalcOutput(double i1, double i2)
	{
		double[] inp = new double[] {i1, i2};
		double dp = DotProductBias(weights,inp);
		if(dp > 0) return(1);
		return (0);
	}

	void Train(int epochs)
	{
		InitialiseWeights();
		
		for(int e = 0; e < epochs; e++)
		{
			totalError = 0;
			for(int t = 0; t < ts.Length; t++)
			{
				UpdateWeights(t);
				Debug.Log("W1: " + (weights[0]) + " W2: " + (weights[1]) + " B: " + bias);
			}
			Debug.Log("TOTAL ERROR: " + totalError);
		}
	}

	void Start () {
		Train(4);

		Debug.Log("Test 0 0: " + CalcOutput(0,0));
		Debug.Log("Test 0 1: " + CalcOutput(0,1));
		Debug.Log("Test 1 0: " + CalcOutput(1,0));
		Debug.Log("Test 1 1: " + CalcOutput(1,1));
	}
	
	void Update () {
		
	}
}
```

![image](https://user-images.githubusercontent.com/80561050/208313733-8929710c-2360-425b-a2f6-47a701eb4cbb.png)

**Подсчёт для OR**

- Создал массив из 4 логических элементов и запустил код с Train(1). Total Error вышла 2:

![image](https://user-images.githubusercontent.com/80561050/208314127-551015ee-170f-4569-9ea0-3cf99d403c6b.png)


- Прогнал значения через CalcOutput с Train(4), TotalError в большинстве случаев к *4й итерцаии* становится 0:

![image](https://user-images.githubusercontent.com/80561050/208314351-cb3fa0ff-f3de-4014-9b69-0747cf2cf1d2.png)


**Подсчёт для AND**

- Изменил значения в массиве логических элементов:

![image](https://user-images.githubusercontent.com/80561050/209116122-e5962baa-9108-4530-99fa-72308bea2e04.png)

- TotalError в большинстве случаев к *7й итерцаии* становится 0:

![image](https://user-images.githubusercontent.com/80561050/209116218-ced750db-4402-48a8-a89b-c57707710da8.png)


**Подсчёт для NAND**

- Изменил значения в массиве логических элементов:

![image](https://user-images.githubusercontent.com/80561050/209117033-86f22115-3011-4645-bf25-0d8f119f5a3a.png)

- TotalError в большинстве случаев к *9й итерцаии* становится 0:

![image](https://user-images.githubusercontent.com/80561050/209117070-e770a5a8-04ed-4c08-a1f4-acfa808d11b7.png)


**Подсчёт для XOR**

- Изменил значения в массиве логических элементов:

![image](https://user-images.githubusercontent.com/80561050/209118313-d34e1d6a-72c1-4764-b867-baed031247cb.png)

- TotalError во всех случаях ровно 4:

![image](https://user-images.githubusercontent.com/80561050/209118434-4fd4ca55-89bb-4731-bb75-559a2a0f5bf5.png)

Для логической операции XOR подсчёт невозможен. Перцептрон не сожет справиться с этой задачей.

## Задание 2

-

## Задание 3

-

## Выводы

Я смог реализовать перцептрон, который умеет производить вычисления OR, AND и NAND. В ходе лабораторной работе выяснилось, что перцептрон не может справиться с задачей вычисления логической операции XOR. Разобрался как работает перцептрон.


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
