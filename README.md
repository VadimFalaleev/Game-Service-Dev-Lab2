# Разработка игровых сервисов
Отчет по лабораторной работе #2 выполнил(а):
- Фалалеев Вадим Эдуардович
- РИ-300012

Отметка о выполнении заданий (заполняется студентом):

| Задание | Выполнение | Баллы |
| ------ | ------ | ------ |
| Задание 1 | * | 100 |
| Задание 2 | * | 100 |
| Задание 3 | * | 100 |

знак "*" - задание выполнено; знак "#" - задание не выполнено;

Работу проверили:
- к.т.н., доцент Денисов Д.В.
- к.э.н., доцент Панов М.А.
- ст. преп., Фадеев В.О.

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

## Цель работы
создание интерактивного приложения и изучение принципов интеграции в него игровых сервисов.

## Задание 1
### По теме видео практических работ 1-5 повторить реализацию игры на Unity. Привести описание выполненных действий.
Ход работы: 

- Я создал 3D проект в Unity версии 2022.1.0f1. В этом проекте переименовал сцену на _0Scene. В Unity asset store добавил ассет Dragon for Boss Monster : PBR. В проекте скачал и импортировал ассет. Далее создал копию префаба Grey в FourEvilDragonPBR/Prefab/DragonTerrorBringer, переименовал в Enemy и перенес в папку Scenes, после чего добавил префаб на сцену. Position и Rotation поставил на 0.

![image](https://user-images.githubusercontent.com/54228342/193024646-1fef6ccf-8c82-4407-9e8c-0948eadf7170.png)

- Далее в этой же папке нажимаем ПКМ -> create -> Animator Controller. Переименуем объект в EnemyCTRL. После чего создаем копию FlyIdle в FourEvilDragonPBR/Animations/DragonTerrorBringer и переносим в папку Scenes. Добавляем анимацию в EnemyCTRL.

![image](https://user-images.githubusercontent.com/54228342/193028111-3fed92b2-0f4f-4fc5-8f6c-a180e64b06ab.png)

- Изменяем префаб Enemy, поменяв Controller в компоненте Animator на EnemyCTRL и запустим проект, чтобы убедиться, что анимация рабоатет.

![image](https://user-images.githubusercontent.com/54228342/193028984-9894f7ee-9a62-4a60-bfdf-ee29f2bbd0ad.png)

- Создадим материал и назовем его Mat_Egg. Изменим цвет на тот, который больше подходит для яйца. Создадим на сцене объект Sphere и переименуем его в DragonEgg. Поставим все координаты на 0, Scale по Y изменим на 1.5, добавим компонент Rigidbody и материал, который только что создали.

![image](https://user-images.githubusercontent.com/54228342/193031510-4d71dd61-0271-438b-a319-642ce42ca3f0.png)

- Далее нажимаем на Untagged и Add Tag. Напротив List is Empty нажимаем на плюс и создаем тег с названием Dragon Egg, нажимая на save. Добавляем этот тег для объекта DragonEgg, делаем его префабом и удаляем со сцены.

![image](https://user-images.githubusercontent.com/54228342/193033818-b050ab01-63e6-4660-b694-09232a244d92.png)
![image](https://user-images.githubusercontent.com/54228342/193033833-f0f53f47-6de2-4e13-8313-38293f150653.png)
![image](https://user-images.githubusercontent.com/54228342/193033862-8a967215-f058-41f5-b613-fb522ffabb13.png)

- Создаем материал Mat_Shield и делаем ему произвольный цвет, меняем Rendering Mode на Transparent. После этого создаем на сцене объект Sphere, ставим ему координаты 0, -6, 0, а Scale меняем на 3, 3, 3. Добавляем объекту комопнент Rigidbody. Убираем галочку напротив Use Gravity, ставим галочку напротив Is Kinematic. Добавялем объекту материал, который только что создали и делаем его префабом.

![image](https://user-images.githubusercontent.com/54228342/193037480-29aec7af-c449-4b4f-94f7-e383999b398c.png)

- Настроим правильно камеру. в компоненте Transform ставим Position на (0, 5, 10), Rotation на (20, -180, 0), Scale на (1, 1, 0). В компоненте Camera Projection меняем на Orthographic, size ставим на 12, Far на 50. После настройки камеры меняем соотношение сторон на 16:9.

![image](https://user-images.githubusercontent.com/54228342/193250848-bf4d58ea-99b7-45ea-9cc2-a0af4efb7813.png)

- Создаем скрипт EnemyDragon и добавляем его на объект Enemy. Настроим скрипт в инспекторе, поставим speed на -4, timeBetweenEggDragon на 2, chanceDirection на 0.01.

```c#

using UnityEngine;

public class EnemyDragon : MonoBehaviour
{
    public GameObject dragonEggPrefab;
    public float speed = 1f;
    public float timeBetweenEggDrops = 1f;
    public float leftRightDistance = 10f;
    public float chanceDirection = 0.1f;

    private void Update()
    {
        Vector3 pos = transform.position;
        pos.x += speed * Time.deltaTime;
        transform.position = pos;

        if (pos.x < -leftRightDistance)
            speed = Mathf.Abs(speed);
        else if (pos.x > leftRightDistance)
            speed = -Mathf.Abs(speed);
    }

    private void FixedUpdate()
    {
        if (Random.value < chanceDirection)
            speed *= -1;
    }
}

```
