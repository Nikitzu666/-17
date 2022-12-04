Выполнил: Смирнов Н.М

Группа: ЭВТ-70

Игровой движок: Unity 2021.3.15

Название работы: разработка игры Puzzle

Лабораторная работа №17

Тема: разработка игры Puzzle

Цель: разработать игру Puzzle 

Оборудование: компьютер

Ход работы 

1. Выполнение работы
![image](https://user-images.githubusercontent.com/119733911/205501423-d6b5462c-014c-4aba-9c92-968a3f5ddca0.png)
Рисунок 17.1 Импортировали компоненты

Спрайты труб и кубиков распологаем в сцене как нам нужно и делаем бэкграунд из кубиков 

![image](https://user-images.githubusercontent.com/119733911/205501210-84b71264-f2ca-4926-b8af-d721792912ed.png)
Рисунок 17.2 Расположили трубы и кубы 

Листинг 17.1 PipeScript.cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class PipeScript : MonoBehaviour
{
    float[] rotations = { 0, 90, 180, 270 };

    public float[] correctRotation;
    [SerializeField]
    bool isPlaced = false;

    int PossibleRots = 1;

    GameManager gameManager;

    private void Awake()
    {
        gameManager = GameObject.Find("GameManager").GetComponent<GameManager>();
    }
    private void Start()
    {
        PossibleRots = correctRotation.Length;
        int rand = Random.Range(0, rotations.Length);
        transform.eulerAngles = new Vector3(0, 0, rotations[rand]);

        if(PossibleRots > 1)
        {
            if (transform.eulerAngles.z == correctRotation[0] || transform.eulerAngles.z == correctRotation[1])
            {
                isPlaced = true;
                gameManager.correctMove();
            }
        }
        else
        {
            if (transform.eulerAngles.z == correctRotation[0])
            {
                isPlaced = true;
                gameManager.correctMove();
            }
        }




    }

    private void OnMouseDown()
    {
        transform.Rotate(new Vector3(0, 0, 90));

        if (PossibleRots > 1)
        {
            if (transform.eulerAngles.z == correctRotation[0] || transform.eulerAngles.z == correctRotation[1] && isPlaced == false)
            {
                isPlaced = true;
                gameManager.correctMove();
            }
            else if (isPlaced == true)
            {
                isPlaced = false;
                gameManager.wrongMove();
            }
        }
        else
        {
            if (transform.eulerAngles.z == correctRotation[0] && isPlaced == false)
            {
                isPlaced = true;
                gameManager.correctMove();
            }
            else if (isPlaced == true)
            {
                isPlaced = false;
                gameManager.wrongMove();
            }
        }

    }
}

Листинг 17.2 GameManager.cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class GameManager : MonoBehaviour
{
    public GameObject PipesHolder;
    public GameObject[] Pipes;

    [SerializeField]
    int totalPipes = 0;
    [SerializeField]
    int correctedPipes = 0;

    
    void Start()
    {
        totalPipes = PipesHolder.transform.childCount;

        Pipes = new GameObject[totalPipes];

        for (int i= 0; i < Pipes.Length; i++)
        {
            Pipes[i] = PipesHolder.transform.GetChild(i).gameObject;
        }
    }

    public void correctMove()
    {
        correctedPipes += 1;

        Debug.Log("correct Move");

        if(correctedPipes == totalPipes)
        {
            Debug.Log("You win");
        }
    }

    public void wrongMove()
    {
        correctedPipes -= 1;
    }
}

2.Вывод:
В ходе проделанной работы, мы разработали игру Puzzle.
