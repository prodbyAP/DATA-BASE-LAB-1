# Лабораторная работа 1 по дисциплине "Базы данных"
## ФИО студента, номер учебной группы
Притечко Арсений Николаевич, 2262-ДБ
## Вариант 20: "Зарплата"
Имеются сведения об отделах (название отдела) и тарифная сетка
(должность, разряд (от 7 до 15),ставка (руб/час)). Работник, имеющий
табельный номер, ФИО, должность и разряд, работает в некотором
отделе. Ежемесячно заполняется табель, определяющий, какое число
часов отработал определенный работник.

Выходные документы:
* Список зарплат сотрудников с указанием их должности за
определенный месяц, упорядоченный по отделам и ФИО
сотрудников
* Список отделов с указанием количества лиц с определенным
разрядом, упорядоченный по отделам

## ER-Диаграмма
```mermaid
erDiagram
    Отдел {
        int id PK
        string название
    }
    
    Сотрудник {
        int id PK
        int табельный_номер
        string ФИО
        int отдел_id FK
        int должность_id FK
        int разряд
    }
    
    Должность {
        int id PK
        string название
    }
    
    Тарифная_сетка {
        int id PK
        int должность_id FK
        int разряд
        decimal ставка
    }
    
    Табель {
        int id PK
        int сотрудник_id FK
        date месяц
        decimal часы
    }

    Отдел ||--o{ Сотрудник : содержит
    Сотрудник }o--|| Должность : имеет
    Должность ||--o{ Тарифная_сетка : имеет_ставки
    Сотрудник ||--o{ Табель : работает
```
## Сущности:
* Отдел - единица предприятия, где работают сотрудники. Имеет уникальное название и может содержать несколько сотрудников.
* Должность - определяет требования, которые выполняет сотрудник. Имеет тарифные ставки по разрядам.
* Тарифная стека - определяет ставку в час.
* Сотрудник - работник предприятия со своим табельным номером.
* Табель - фиксирует количество отработанных часов для расчета заработной платы.

## Логическая модель в виде диаграммы классов UML-2.4
```mermaid
classDiagram
    direction LR
    
    class Отдел {
        +int id
        +string название
        +getСотрудники() List
    }
    
    class Должность {
        +int id
        +string название
        +getСтавки() List
    }
    
    class ТарифнаяСетка {
        +int id
        +int разряд
        +decimal ставка
    }
    
    class Сотрудник {
        +int id
        +int табельныйНомер
        +string ФИО
        +int разряд
        +расчитатьЗарплату() decimal
    }
    
    class Табель {
        +int id
        +date месяц
        +decimal часы
    }

    Отдел "1" *-- "*" Сотрудник : содержит
    Должность "1" *-- "*" Сотрудник : занимают
    Должность "1" *-- "*" ТарифнаяСетка : имеет
    Сотрудник "1" *-- "*" Табель : работает
```

## Связи между сущностями:
* Отдел -> Сотрудник (Тип: Один ко многим 1:N)
* Должность -> Сотрудник (Тип: Один ко многим 1:N)
* Должность -> Тарифная сетка (Тип: Один ко многим 1:N)
* Сотрудник -> Табель (Тип: Один ко многим 1:N)

## Физическая модель:
```mermaid
erDiagram
    Отдел {
        integer id PK "INT PRIMARY KEY"
        string название "NVARCHAR(100) NOT NULL UNIQUE"
    }
    
    Должность {
        integer id PK "INT PRIMARY KEY"
        string название "NVARCHAR(100) NOT NULL UNIQUE"
    }
    
    ТарифнаяСетка {
        integer id PK "INT PRIMARY KEY"
        integer должность_id FK "INT NOT NULL"
        integer разряд "INT NOT NULL"
        decimal ставка "DECIMAL(10,2) NOT NULL"
    }
    
    Сотрудник {
        integer id PK "INT PRIMARY KEY"
        integer табельный_номер "INT NOT NULL UNIQUE"
        string ФИО "NVARCHAR(150) NOT NULL"
        integer отдел_id FK "INT NOT NULL"
        integer должность_id FK "INT NOT NULL"
        integer разряд "INT NOT NULL"
    }
    
    Табель {
        integer id PK "INT PRIMARY KEY"
        integer сотрудник_id FK "INT NOT NULL"
        date месяц "DATE NOT NULL"
        decimal часы "DECIMAL(6,2) NOT NULL"
    }

    Отдел ||--o{ Сотрудник : "FOREIGN KEY (отдел_id)"
    Должность ||--o{ Сотрудник : "FOREIGN KEY (должность_id)"
    Должность ||--o{ ТарифнаяСетка : "FOREIGN KEY (должность_id)"
    Сотрудник ||--o{ Табель : "FOREIGN KEY (сотрудник_id)"
```
# Лабораторная работа 2

## Создание DDL-запросов для PostgreSQL
## 1) Создал структуру Базы данных в pgAdmin:
### Создал таблицу "Отдел":
![1](https://github.com/user-attachments/assets/66d8b839-61c4-4822-a300-21ee996445e3)
### Создал таблицу "Должность":
![2](https://github.com/user-attachments/assets/2010a8b4-8406-4193-b325-d268bb8d30c5)
### Создал таблицу "ТарифнаяСетка":
![3](https://github.com/user-attachments/assets/0ba08ce2-dc17-42e1-9f47-0dc0871f1288)
### Создал таблицу "Сотрудник":
![4](https://github.com/user-attachments/assets/8c11af54-6272-42a0-be17-e6a548b4d379)
### Создал таблицу "Табель":
![5](https://github.com/user-attachments/assets/d5ae6418-9153-4a49-851f-6443c8bf9572)

### Заполнил таблицу "Отдел" данными
![данные 1 1](https://github.com/user-attachments/assets/ac0c7c0c-2a14-4e9c-bec5-a11e2481b93b)
![данные 1 2](https://github.com/user-attachments/assets/cef4fb09-d5d6-42c0-9e0e-195ba1c33217)
### Заполнил таблицу "Должность" данными
![данные 2 1](https://github.com/user-attachments/assets/cab9027e-e0b0-409a-891c-39793970a74a)
![данные 2 2](https://github.com/user-attachments/assets/6cfa3b00-93a7-4088-aba6-63cc4016fe9a)
### Заполнил таблицу "ТарифнаяСетка" данными
![данные 3 1](https://github.com/user-attachments/assets/290d59ea-8c63-41fd-9799-14108dd99b1f)
![данные 3 2](https://github.com/user-attachments/assets/2ca091cc-a137-4c84-8fed-05edd5ad4ade)
### Заполнил таблицу "Сотрудник" данными
![данные 4 1](https://github.com/user-attachments/assets/715f12c7-aad0-450d-bce0-3d38c6138fe1)
![данные 4 2](https://github.com/user-attachments/assets/baecc955-79e0-4ea0-ac03-1609fa033ff3)
### Заполнил таблицу "Табель" данными
![данные 5 1](https://github.com/user-attachments/assets/ade64cb9-2908-4a5b-9247-c84eaa38edeb)
![данные 5 2](https://github.com/user-attachments/assets/ff6d6e22-3b27-4c79-abfa-a96dcb8852c1)

## 2) Выполнил SELECT-запросы по варианту задания:
![запрос 1](https://github.com/user-attachments/assets/cab4cee5-043c-4a3f-99ea-1424a7e239ca)
![запрос 2](https://github.com/user-attachments/assets/763f071c-0cce-423b-8305-95171ed3fe0e)
