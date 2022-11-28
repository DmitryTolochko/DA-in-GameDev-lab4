# DA-in-GameDev-lab4
# АНАЛИЗ ДАННЫХ И ИСКУССТВЕННЫЙ ИНТЕЛЛЕКТ [in GameDev]
ПЕРЦЕПТРОН.

Отчет по лабораторной работе #4 выполнил(а):
- Толочко Дмитрий Александрович
- РИ-210944
Отметка о выполнении заданий (заполняется студентом):

| Задание | Выполнение | Баллы |
| ------ | ------ | ------ |
| Задание 1 | * | 60 |
| Задание 2 | * | 20 |
| Задание 3 | * | 20 |

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
Познакомиться с устройством перцептрона, научиться применять его на практике.

## Задание 1
### Реализовать перцептрон, который умеет производить вычисления: OR, AND, NAND, XOR

1) Создал проект в unity, создал пустой объект perceptron и навесил на него скрипт.
![image](https://user-images.githubusercontent.com/100460661/204240673-939e268d-2d56-4848-bed6-7f21d16e970c.png)

``` cs
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
		Train(8);
		Debug.Log("Test 0 0: " + CalcOutput(0,0));
		Debug.Log("Test 0 1: " + CalcOutput(0,1));
		Debug.Log("Test 1 0: " + CalcOutput(1,0));
		Debug.Log("Test 1 1: " + CalcOutput(1,1));		
	}
	
	void Update () {
		
	}
}


```

2) Создаю обучающую выборку для логического или. 

![image](https://user-images.githubusercontent.com/100460661/204242411-2c049c8b-7509-400b-a7b2-8d0bda63f192.png)
![image](https://user-images.githubusercontent.com/100460661/204243765-b3a4a15b-eae3-4108-bf54-4de4e3fd5bae.png)

3) Запускаю проект. Перцептрон обучился на 5 эпохе. Выводит правильные ответы.

![image](https://user-images.githubusercontent.com/100460661/204244410-0b177fa0-2945-4fa0-a933-37d9fd1572a9.png)
![image](https://user-images.githubusercontent.com/100460661/204244542-5f56390b-067f-4fce-908e-03a06c21d641.png)

4) Проделываю то же самое для логического и.

![image](https://user-images.githubusercontent.com/100460661/204246957-739f7b73-ab24-490f-b8a5-5fe13f2fe488.png)
![image](https://user-images.githubusercontent.com/100460661/204247073-1289ee81-68d3-45e3-979c-0e3611891256.png)

5) Запускаю проект. Перцептрон обучился на 7 эпохе. Выводит правильные ответы.

![image](https://user-images.githubusercontent.com/100460661/204247399-7c0c62ba-9ac0-404a-ae32-256077b9f64a.png)
![image](https://user-images.githubusercontent.com/100460661/204247425-c5ef2080-544d-43e6-953e-93aaaf046988.png)

6) То же самое для NAND.

![image](https://user-images.githubusercontent.com/100460661/204248661-cde7f649-bd0d-492f-90d5-3473a727ae00.png)
![image](https://user-images.githubusercontent.com/100460661/204248778-4981ab08-66b4-4336-9de0-c87894617ade.png)

7) Запускаю проект. Перцептрон обучился на 6 эпохе. Выводит правильные ответы.

![image](https://user-images.githubusercontent.com/100460661/204248979-e0dc0110-d914-43fa-bda7-36a3e291e18c.png)
![image](https://user-images.githubusercontent.com/100460661/204249011-56f8346a-66bd-4942-ac0e-c4eafcfa7673.png)

8) И для XOR: 

![image](https://user-images.githubusercontent.com/100460661/204249792-90f26795-bcf7-4470-9595-8c35c99ebe35.png)
![image](https://user-images.githubusercontent.com/100460661/204249950-80678928-5568-4cd4-a2a1-d682281c2a10.png)

9) Запускаю проект. Перцептрон не обучается при любом количестве эпох. Выводит неправильные ответы.

![image](https://user-images.githubusercontent.com/100460661/204252152-f37b2b4b-744f-4fde-b048-1412231971dd.png)

## Задание 2
### Построить графики зависимости количества эпох от ошибки обучения.

![image](https://user-images.githubusercontent.com/100460661/204253114-759aa2e0-87e2-452f-990e-783064266709.png)

Необходимое количество эпох обучения напрямую зависит от ошибки, которую выдает программа на тестовом наборе данных.

## Задание 3
### Построить визуальную модель работы перцептрона на сцене Unity.

Я попытаюсь сделать модель из 2 шаров, которые после соприкосновения действуют согласно логике "И": приятгиваются или отталиваются.

1) Добавил плоскость и два шара на сцену, удалил пустой объект с перцептроном

![image](https://user-images.githubusercontent.com/100460661/204265748-535e8b67-8bb5-4357-affa-4431cdbd0416.png)

2) Создал enum.

``` cs 

public enum Type
{
    Zero,
    One
}
```

А также скрипт для шара item, который наследует класс Perceptron:

``` cs 
using System;
using UnityEngine;

public class item : Perceptron
{
    public Type type;
    private float speed = 0.5f;

    public GameObject target;

    private void Update() 
    {
        transform.position = Vector3.MoveTowards(transform.position, target.transform.position, speed*Time.deltaTime);
    }

    private void OnCollisionEnter(Collision other) 
    {
        try
        {
            if (CalcOutput((int)this.type,(int)other.gameObject?.GetComponent<item>().type) == 0)
            {
                speed *= -1;
            }
        }
        catch (NullReferenceException)
        {
            print("не шар");
        }
    }
}
```

Шары будут притягиваться, если оба будут One.

3) На шары навесил скрипт item, сделал настройку для логики "И" и добавил по RigidBody. Также добавил ссылку на друг друга, чтобы они в начале двигались навстречу.

![image](https://user-images.githubusercontent.com/100460661/204271747-3759bb36-d23e-43fc-9606-e89a201da4d2.png)

4) Пробуем запустить проект, добавив разные цифры для шаров.

![image](https://user-images.githubusercontent.com/100460661/204271792-9b757582-a7bf-4e74-9aa7-604168f3f605.png)
![image](https://user-images.githubusercontent.com/100460661/204271819-ac0b6ad7-2cfa-4dc7-a2ff-4c83c7877a8b.png)

https://user-images.githubusercontent.com/100460661/204271926-7f3980cd-573e-456a-968f-431744932288.mp4

5) Поменяем цифры местами.

![image](https://user-images.githubusercontent.com/100460661/204272005-6427f6a5-2a4c-41d2-bf41-296e20b42820.png)
![image](https://user-images.githubusercontent.com/100460661/204272044-230545c5-6efc-4060-a841-dcbfa864ea12.png)

https://user-images.githubusercontent.com/100460661/204272116-ad797f96-a7bd-4ae6-a98e-9350cdd158d5.mp4

6) Теперь сделаем оба шара единицами.

![image](https://user-images.githubusercontent.com/100460661/204272195-5c32b49e-a567-48d9-a540-c9b23ab7861a.png)
![image](https://user-images.githubusercontent.com/100460661/204272213-c1ba5a17-ae80-4ab9-a829-980192a1c625.png)

https://user-images.githubusercontent.com/100460661/204270327-49a67879-c287-48c8-a588-d59709797486.mp4

Шары притягиваются.

7) Теперь сделаем оба шара нулями.

![image](https://user-images.githubusercontent.com/100460661/204272282-75b10327-1b02-4c6e-b025-452e49a759c5.png)
![image](https://user-images.githubusercontent.com/100460661/204272311-d3bba0d9-6039-4367-be77-e1d207357aee.png)

https://user-images.githubusercontent.com/100460661/204272421-4c45bb78-1aa2-4e2b-aad2-8588fcc5a48c.mp4

Шары после столкновения не притягиваются, поскольку скрипт действует согласно логике "И"



## Выводы
В этой лабораторной работе я реализовал двоичную логику на перцептроне, а также построил визуальную модель его работы в юнити. Выяснилось, что с помощью перцептрона невозможно реализовать логику XOR.

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
