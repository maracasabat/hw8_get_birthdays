***HW8***
---
## **Условия приёмки**

* `get_birthdays_per_week` выводит именинников в формате:

   Monday: Bill, Jill

   Friday: Kim, Jan

* Пользователей, у которых день рождения был на выходных, надо поздравить в понедельник.
* Для отладки удобно создать тестовый список users и заполнить его самостоятельно.
* Функция выводит пользователей с днями рождения на неделю вперед от текущего дня.
* Неделя начинается с понедельника.

## **Опис програми**

* Функція `get_users()` приймає на вхід текстовий файл з іменем і датою народження у вигляді: 

  Bill,18.05.1995

  Jill,12.05.1986

* На виході функція видає список словників у вигляді:
```python
[
    {
        "name":"Bill", "birthday":"18.05.1995"
    },
    {
        "name":"Jill", "birthday":"12.05.1986"
    }
]
```

1. **Функція `get_users()` для обробки текстового файлу має вигляд:**
```python
def get_users(path):
    with open(path, 'r', encoding='utf-8') as file:
        users = []
        line = file.readline()
        while True:
            data = line.removesuffix('\n').split(',')
            users.append({'name': data[0], 'birthday': data[1]})
            line = file.readline()
            if not line:
                break
    return users


print(get_users('users.txt'))
```
2. **Функція `def get_birthdays_per_week(users)` збирає та обробляє дані з попередньої функції та повертає дані у словник:**
```python
def get_birthdays_per_week(users):
    data = {
        'Monday': '',
        'Tuesday': '',
        'Wednesday': '',
        'Thursday': '',
        'Friday': '',
    }
    date_now = datetime.now()
    users = get_users('users.txt')
    for i in users:
        birthday = datetime.strptime(i.get("birthday"), '%d.%m.%Y')
        other_birthday = birthday.replace(year=date_now.year)
        d = other_birthday.date() - birthday.date()
        years = str(d.days//365)
        diff = other_birthday.date() - date_now.date()
        if timedelta(0) <= diff <= timedelta(7):
            days = datetime.weekday(other_birthday)
            if days == 0 or days == 5 or days == 6:
                data['Monday'] = data['Monday'] + i['name'] + ' ' + years + ' ' + 'years old'
                data['Monday'] = data['Monday'] + ', '
            if other_birthday.weekday() == 1:
                data['Tuesday'] = data['Tuesday'] + i['name'] + ' ' + years + ' ' + 'years old'
                data['Tuesday'] = data['Tuesday'] + ', '
            if other_birthday.weekday() == 2:
                data['Wednesday'] = data['Wednesday'] + i['name'] + ' ' + years + ' ' + 'years old'
                data['Wednesday'] = data['Wednesday'] + ', '
            if other_birthday.weekday() == 3:
                data['Thursday'] = data['Thursday'] + i['name'] + ' ' + years + ' ' + 'years old'
                data['Thursday'] = data['Thursday'] + ', '
            if other_birthday.weekday() == 4:
                data['Friday'] = data['Friday'] + i['name'] + ' ' + years + ' ' + 'years old'
                data['Friday'] = data['Friday'] + ', '

    for k, v in data.items():
        if len(v) != 0:
            print(f'{k}: {v[:-2]}')


if __name__ == '__main__':
    get_birthdays_per_week(get_users('users.txt'))
```
3. **Результатом виконання функції є вивід (за допомогою `print`) списка колег, яких треба привітати по днях, а також кількість років, яка виповнилась.**