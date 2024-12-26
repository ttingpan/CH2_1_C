# [2번 과제] CH 2 OOP Summary
## 도전 기능
- 필수 기능 가이드에 있는 요구사항을 만족하는 코드를 구현했다면 아래 코드 스니펫을 보고 요구사항대로 Zoo 클래스를 정의
- 랜덤으로 동물 객체를 반환하는 createRandomAnimal()함수를 구현하세요  자세한 사항은 아래 코드 스니펫을 참조
- 동물 객체가 동적으로 생성 및 해제되어 메모리 누수가 발생하지 않도록 구현했는지 확인
- zoo 클래스 내에 animals 배열이 있으며, 해당 배열의 크기가 제한
- 배열에 새로운 항목을 추가할 때, 제한된 크기를 고려하여 배열의 크기를 동적으로 확장하거나 적절히 처리하는 예외 사항이 반영되었는지 확인

```c#
#include <iostream>
#include <cstdlib>
#include <ctime>

using namespace std;

class Animal
{
public:
    Animal()
    {
        cout << "Animal 생성자 호출!" << endl;
    }

    // 순수 가상 함수 =>자식 클래스에서 재정의 필요
    virtual void makeSound() = 0;

    // 가상 함수
    virtual ~Animal()
    {
        cout << "Animal 소멸자 호출!" << endl;
    }
};

class Zoo
{
private:
    Animal* animals[10]; // 동물 객체를 저장하는 포인터 배열
    int arrSize = 10; // 현재 배열 크기
    int size = 0; // 현재 추가된 동물 수
public:
    // 동물을 동물원에 추가하는 함수
    // - Animal 객체의 포인터를 받아 포인터 배열에 저장합니다.
    // - 같은 동물이라도 여러 번 추가될 수 있습니다.
    // - 입력 매개변수: Animal* (추가할 동물 객체)
    // - 반환값: 없음
    void addAnimal(Animal* animal)
    {
        // 현재 배열 크기보다 작으면
        if (size < arrSize)
        {
            // 동물 추가
            animals[size] = animal;

            // 추가된 동물 수 증가
            size++;
        }
    }

    // 동물원에 있는 모든 동물의 행동을 수행하는 함수
    // - 모든 동물 객체에 대해 순차적으로 소리를 내고 움직이는 동작을 실행합니다.
    // - 입력 매개변수: 없음
    // - 반환값: 없음
    void performActions()
    {
        for (int i = 0; i < size; i++)
        {
            animals[i]->makeSound();
        }
    }

    // Zoo 소멸자
    // - Zoo 객체가 소멸될 때, 동물 벡터에 저장된 모든 동물 객체의 메모리를 해제합니다.
    // - 메모리 누수를 방지하기 위해 동적 할당된 Animal 객체를 `delete` 합니다.
    // - 입력 매개변수: 없음
    // - 반환값: 없음
    ~Zoo()
    {
        for (int i = 0; i < size; i++)
        {
            delete animals[i];
        }
    }
};

class Dog : public Animal
{
public:
    Dog()
    {
        cout << "Dog 생성자 호출!" << endl;
    }

    void makeSound()
    {
        cout << "Woof! Woof!" << endl;
    }

    ~Dog()
    {
        cout << "Dog 소멸자 호출!" << endl;
    }
};

class Cat : public Animal
{
public:
    Cat()
    {
        cout << "Cat 생성자 호출!" << endl;
    }

    void makeSound()
    {
        cout << "Meow! Meow!" << endl;
    }

    ~Cat()
    {
        cout << "Cat 소멸자 호출!" << endl;
    }
};

class Cow : public Animal
{
public:
    Cow()
    {
        cout << "Cow 생성자 호출!" << endl;
    }

    void makeSound()
    {
        cout << "Mow! Mow!" << endl;
    }

    ~Cow()
    {
        cout << "Cow 소멸자 호출!" << endl;
    }
};

// 랜덤 동물을 생성하는 함수
// - 0, 1, 2 중 하나의 난수를 생성하여 각각 Dog, Cat, Cow 객체 중 하나를 동적으로 생성합니다.
// - 생성된 객체는 Animal 타입의 포인터로 반환됩니다.
// - 입력 매개변수: 없음
// - 반환값: Animal* (생성된 동물 객체의 포인터)
Animal* createRandomAnimal()
{
    Animal* animal;

    // 0~2 범위 난수 생성
    int randomNum = rand() % 3;

    cout << "생성된 난수 : " << randomNum << endl;
    
    // 생성된 난수에 따라 객체 동적 생성
    if (randomNum == 0)
    {
        animal = new Dog();
    }
    else if (randomNum == 1)
    {
        animal = new Cat();
    }
    else
    {
        animal = new Cow();
    }
    

    return animal;
}

int main()
{
    // seed설정
    srand((unsigned int)time(NULL));

    Zoo zoo;

    int n;

    bool checked = false;

    while (!checked)
    {
        // 동물원에 추가 할 동물의 수 입력
        cout << "동물원에 추가 할 동물의 수(양의 정수, 최대 10) 입력 : ";
        cin >> n;
        cout << endl;

        // 0이하의 수를 입력했을때
        if (n <= 0)
        {
            cout << "양의 정수를 입력하세요." << endl;
            continue;
        }
        // 배열의 크기를 벗어날때
        else if (n > 10)
        {
            cout << "배열의 크기(10) 보다 작은 수를 입력하세요." << endl;
            continue;
        }

        checked = true;
    }

    for (int i = 0; i < n; i++)
    {
        zoo.addAnimal(createRandomAnimal());
    }

    zoo.performActions();

	return 0;
}
```
