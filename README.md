## Домашнє завдання до теми «Apache Spark. Оптимізація та SparkUІ»

### Частина 1

#### Job list 1

![p1](screenshots/p1.png)

**Job 0**

![p1_0](screenshots/p1_0.png)

**Job 1**

![p1_1](screenshots/p1_1.png)

**Job 2**

![p1_2](screenshots/p1_2.png)

**Job 3**

![p1_3](screenshots/p1_3.png)

**Job 4**

![p1_4](screenshots/p1_4.png)

Ми очікувано бачимо перші 2 джоби це завантаження і десереалізація файлу.

Далі ще 3 джоби без повторень, адже у нас в коді лиш одна операція дії `collect()` (пам'ятаємо, що `count()` після `groupby` - це теж трансформація).

### Частина 2

#### Job list 2

![p2](screenshots/p2.png)

**Job 0**

![p2_0](screenshots/p2_0.png)

**Job 1**

![p2_1](screenshots/p2_1.png)

**Job 2**

![p2_2](screenshots/p2_2.png)

**Job 3**

![p2_3](screenshots/p2_3.png)

**Job 4**

![p2_4](screenshots/p2_4.png)

**Job 5**

![p2_5](screenshots/p2_5.png)

**Job 6**

![p2_6](screenshots/p2_6.png)

**Job 7**

![p2_7](screenshots/p2_7.png)


Знову, очікувано бачимо перші 2 джоби це завантаження і десереалізація файлу.

Далі ще 3 джоби що виконують проміжну дію `collect()`.

Потім у нас додається ще одна команда трансформації `where("count>2")` і друга команда дії  `collect()`, але ця дія виконується з самого початку, тому ми бачимо знову 3 джоби.


### Частина 3

#### Job list 3

![p3](screenshots/p3.png)

**Job 0**

![p3_0](screenshots/p3_0.png)

**Job 1**

![p3_1](screenshots/p3_1.png)

**Job 2**

![p3_2](screenshots/p3_2.png)

**Job 3**

![p3_3](screenshots/p3_3.png)

**Job 4**

![p3_4](screenshots/p3_4.png)

**Job 5**

![p3_5](screenshots/p3_5.png)

**Job 6**

![p3_6](screenshots/p3_6.png)

Як і раніше, бачимо перші 2 джоби - це завантаження і десереалізація файлу.

Далі бачимо 4 джоби що виконують проміжну дію `collect()` і в цей час фіксує проміжний результат в пам'яті, бо ми використали команду `cache()`. Таким чином на доному етапі ми в пам'яті маємо "чекпоінт", і всі наступні дії що виконуються до поточного стану датафрейму будуть починатися не з початку, а саме з цієї точки.

Потім у нас додається ще одна команда трансформації `where("count>2")` і друга команда дії `collect()`, але оскільки у нас є закешований запис, то ця дія виконується не з самого початку і ми бачимо лиш одну додаткову джобу.