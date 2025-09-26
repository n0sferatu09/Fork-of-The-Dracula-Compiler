# Отчёт по практике



## Выбранное ПО
The Dracula Compiler (пока что только лексер) - https://github.com/n0sferatu09/DraculaCompiler
Так как это мой проект, то найденные в нем баги я буду также исправлять, как и в форке, так и в оригинальном репозитории (там же лежит инструкция по эксплуатации.



## Найденные баги

### Баг №1

**Описание**: Малое кол-во символов для идентификаторов.

**Шаги воспроизведения**:
1. Вписать в компилируемый файл идентификатор длиной более 16 символов.
2. Скомпилировать файл по инструкции на оригинальном репозитории проекта.
3. Увидеть, что название идентификатора вывелось не полностью.

**Ожидаемое поведение**: Вывод в терминал полное название идентификатора.

**Фактическое поведение**: Вывод в терминал только первые 15 символов названия идентификатора.

**Серьёзность**: Высокая, так как в таблицу символов должно попадать полное название идентификатора, иначе парсер запутается.

**Скриншот/Видео**: Скриншот в папке screenshots/VeryFewCharacters.png, а так же screenshots/defines


### Баг №2

**Описание**: Переполнение массива из-за отсутствия проверки в цикле.

**Шаги воспроизведения**:
1. Вписать в компилируемый файл идентификатор длиной более 16 символов.
2. Скомпилировать файл по инструкции на оригинальном репозитории проекта.
3. Увидеть ошибку переполнения.

**Ожидаемое поведение**: Вывод в терминал полное название идентификатора.

**Фактическое поведение**: Ошибка из-за переполнения.

**Серьёзность**: Высокая, из-за переполнения падает прод.

**Скриншот/Видео**: Скриншот в папке screenshots/overflow.png


### Баг №3

**Описание**: Отстутствие проверки перед добавлением нулевого терминатора в конец строки. Из-за этого тоже может случится переполнение.

**Шаги воспроизведения**:
1. Вписать в компилируемый файл идентификатор длиной равной 16 символам или более.
2. Скомпилировать файл по инструкции на оригинальном репозитории проекта.
3. Увидеть ошибку переполнения из-за того, что в 17 ячейку массива(которой не существует) добавляется нулевой терминатор.

**Ожидаемое поведение**: Добавление нулевого терминатора в конец строки.

**Фактическое поведение**: Большая вероятность переполнения из-за отстутствия проверки.

**Серьёзность**: Высокая.

**Скриншот/Видео**: Скриншот в папке screenshots/OverflowWithTerminator.png или screenshots/Terminator.png


### Баг №4

**Описание**: Малое кол-во символов для чисел.

**Шаги воспроизведения**:
1. Вписать в компилируемый файл число длиной более 64 символов.
2. Скомпилировать файл по инструкции на оригинальном репозитории проекта.
3. В терминал выведутся только первые 63 символа.

**Ожидаемое поведение**: Вывод полного числа. 

**Фактическое поведение**: Вывод только первых 63 символов.

**Серьёзность**: Высокая, не правильная семантика программы и в последующем неправильная генерация кода.

**Скриншот/Видео**: Скриншот в папке screenshots/VeryFewDigits.png, а так же screenshots/defines


### Баг №5

**Описание**: Переполнение массива из-за отсутствия проверки в цикле.

**Шаги воспроизведения**:
1. Вписать в компилируемый файл число длиной более 64 символов.
2. Скомпилировать файл по инструкции на оригинальном репозитории проекта.
3. Увидеть ошибку переполнения.

**Ожидаемое поведение**: Вывод в терминал полного числа.

**Фактическое поведение**: Ошибка из-за переполнения.

**Серьёзность**: Высокая, утечка памяти.

**Скриншот/Видео**: Скриншот в папке screenshots/OverflowWithNumbers.png


### Баг №6

**Описание**: Отстутствие проверки перед добавлением нулевого терминатора в конец числа.

**Шаги воспроизведения**:
1. Вписать в компилируемый файл идентификатор длиной равной 64 символам или более.
2. Скомпилировать файл по инструкции на оригинальном репозитории проекта.
3. Увидеть ошибку переполнения из-за того, что в 65 ячейку массива(которой не существует) добавляется нулевой терминатор.

**Ожидаемое поведение**: Добавление нулевого терминатора в конец числа.

**Фактическое поведение**: Большая вероятность переполнения из-за отстутствия проверки.

**Серьёзность**: Высокая, утечка памяти.

**Скриншот/Видео**: Скриншот в папке screenshots/OverflowWithDigitTerminator.png или screenshots/Terminator.png


### Баг №7

**Описаниe**: Отсутствие отдельной проверки для однострочных('//') и многострочных('/*  */') комментариев.

**Шаги воспроизведения**:
1. Вписать в компилируемый файл однострочные или многострочные комментарии.
2. Скомпилировать файл по инструкции на оригинальном репозитории проекта.
3. Увидеть, что лексер в таком случае выведет в терминал "UKNOWN OPERATOR: /*".

**Ожидаемое поведение**: Вывод в терминал "FOUND A COMMENT: /* или */ или //".

**Фактическое поведение**: Выводит "UNKNOW OPERATOR".

**Серьёзность**: Высокая, будет ломаться синтаксис, семантика, да и сам компилятор вероятно загнется на стадии лексического анализа.


### Баг №8

**Описаниe**: Функция генерации пунктуаторов генерирует один большой неизвестный токен, если вдруг операторы находятся в строке. Из ("<<=") получается токен <<=");

**Шаги воспроизведения**:
1. Вписать в компилируемый файл подобную строку ("->");.
2. Скомпилировать файл по инструкции на оригинальном репозитории проекта.
3. Увидеть, что лексер выводит "UNKNOW OPERATOR".

**Ожидаемое поведение**: Вывод в терминал "FOUND A OPERATOR: ->".

**Фактическое поведение**: Выводит "UNKNOW OPERATOR".

**Серьёзность**: Высокая, компилятор будет сворачивать свою работу еще на стадии лексического анализа.




## Тесты

### Тест: Проверка на переполнение в случае генерации длинных идентификаторов.

**Описание теста**: Обработка переполнения.

**Шаги**:
1. Вписать в компилируемый файл длинный идентификатор.
2. Скомпилировать.
3. Выходит ли полное имя идентификатора в терминал?

**Результат до исправления**: Не прошёл: выводятся только первые 16 символов.

**Код до исправления** (если применимо):
```C
    int index = 0;
    char buffer[KEYWORD_SIZE + 1] = {0};

    buffer[index++] = first_letter;

    char current = fgetc(file);

    while(current != EOF && (isalnum(current) || current == '_')) {
        buffer[index++] = current;
        current = fgetc(file);
    }

    buffer[index] = '\0';

```
**Исправление** (если применимо): Добавил динамический массив. Теперь если index будет больше capacity, то capacity будет увеличиваться в двойне, благодаря этому, мы можем через функцию realloc делать массив больше в 2 раза. Так же это экономит память, если в случае с макросом KEYWORD_SIEZ мы выделяли память для 16 символов, то теперь из-за того, что стартуем с capacity = 3, условный if будет идеально вписываться в эти 3 символа, 2 на if и 1 на '\0'.

**Код после исправления** (если применимо):
```C
    int capacity = 3;
    int index = 0;
    char* buffer = malloc(sizeof(char) * capacity);

    if (buffer == NULL) {
        fprintf(stderr, "Memory allocation failed!\n");
        free(token);
        return NULL;
    }

    buffer[index++] = first_letter;

    char current = fgetc(file);

    while(current != EOF && (isalnum(current) || current == '_')) {
        if (index >= capacity) {
            capacity *= 2;
            char* new_buffer = realloc(buffer, sizeof(char) * capacity);

            if (new_buffer == NULL) {
                fprintf(stderr, "Memory reallocation failed!\n");
                free(buffer);
                free(token);
                return NULL;
            }

            buffer = new_buffer;
        }

        buffer[index++] = current;
        current = fgetc(file);
    }

    if (index >= capacity) {
        capacity++;
        char* new_buffer = realloc(buffer, sizeof(char) * capacity);

        if (new_buffer == NULL) {
            fprintf(stderr, "Memory reallocation failed!\n");
            free(buffer);
            free(token);
            return NULL;
        }

        buffer = new_buffer;
    }

    buffer[index] = '\0';

```

**Результат после исправления**: Прошёл, теперь можно вписывать идентификаторы любой длины, пока память не кончится)))


### Тест: Проверка на переполнения в случае чисел

**Описание теста**: Происходит ли переполнение массива в случае, если пользователь впишет определенное число символов(64)

**Шаги**:
1. Вписать в компилируемый файл длинное число.
2. Скомпилировать.
3. Происходит ли переполнение?

**Результат до исправления**: Не прошел, переполнение происходит после 64 символа.

**Код до исправления** (если применимо):
```C
    int index = 0;
    char buffer[BUFFER_SIZE] = {0};

    int has_decimal_point = 0;
    int has_exponent = 0;
    int has_float_suffix = 0;

    token->type = TOKEN_INT;
    buffer[index++] = first_number;

    char current = fgetc(file);
    while (current != EOF && (isdigit(current) ||
            current == '.' ||
            current == 'e' || current == 'E' ||
            current == 'f' || current == 'F' ||
            current == 'd' || current == 'D')) {

        if (current == '.') {
            if (has_decimal_point) break;

            has_decimal_point = 1;
            token->type = TOKEN_DOUBLE;
        } 
        
        if (current == 'e' || current == 'E') {
            if (has_exponent) break;

            has_exponent = 1;
            token->type = TOKEN_DOUBLE;

            current = fgetc(file);
            if (current != EOF && (current == '+' || current == '-')) {
                char sign = current;
                current = fgetc(file);

                if (isdigit(current)) {
                    buffer[index++] = sign;
                    buffer[index++] = current;
                } else {
                    ungetc(current, file);
                    ungetc(sign, file);
                    fprintf(stderr, "Invalid Exponent: missing digits\n");
                    free(token);
                    return NULL;
                }
            }
        }

        if (current == 'f' || current == 'F') {
            if (has_float_suffix) break;

            has_float_suffix = 1;
            token->type = TOKEN_FLOAT;
        }

        buffer[index++] = current;
        current = fgetc(file);
    }

    buffer[index] = '\0';

```

**Исправление** (если применимо): Как и в прошлом тесте, добавил динамический массив для решения трех багов сразу.

**Код после исправления** (если применимо):
```C
    int capacity = 2;
    int index = 0;
    char* buffer = malloc(sizeof(char) * capacity);
    if (buffer == NULL) {
        fprintf("Memory allocation failed!\n");
        free(token);
        return NULL;
    }

    int has_decimal_point = 0;
    int has_exponent = 0;
    int has_float_suffix = 0;

    token->type = TOKEN_INT;
    buffer[index++] = first_number;

    char current = fgetc(file);
    while (current != EOF && (isdigit(current) ||
            current == '.' ||
            current == 'e' || current == 'E' ||
            current == 'f' || current == 'F' ||
            current == 'd' || current == 'D')) {
        
        if (index >= capacity) {
            capacity *= 2;
            char* new_buffer = realloc(buffer, sizeof(char) * capacity);
            
            if (new_buffer == NULL) {
                fprintf("Memory reallocation failed!\n");
                free(buffer);
                free(token);
                return NULL;
            }

            buffer = new_buffer;
        }

        if (current == '.') {
            if (has_decimal_point) break;

            has_decimal_point = 1;
            token->type = TOKEN_DOUBLE;
        } 
        
        if (current == 'e' || current == 'E') {
            if (has_exponent) break;

            has_exponent = 1;
            token->type = TOKEN_DOUBLE;

            current = fgetc(file);
            if (current != EOF && (current == '+' || current == '-')) {
                char sign = current;
                current = fgetc(file);

                if (isdigit(current)) {
                    buffer[index++] = sign;
                    buffer[index++] = current;
                } else {
                    ungetc(current, file);
                    ungetc(sign, file);
                    fprintf(stderr, "Invalid Exponent: missing digits\n");
                    free(token);
                    return NULL;
                }
            }
        }

        if (current == 'f' || current == 'F') {
            if (has_float_suffix) break;

            has_float_suffix = 1;
            token->type = TOKEN_FLOAT;
        }

        buffer[index++] = current;
        current = fgetc(file);
    }

    if (index >= capacity) {
        capacity++;
        char* new_buffer = realloc(buffer, sizeof(char) * capacity);
            
        if (new_buffer == NULL) {
            fprintf("Memory reallocation failed!\n");
            free(buffer);
            free(token);
            return NULL;
        }

        buffer = new_buffer;
    }

    buffer[index] = '\0';
```

**Результат после исправления**: Прошёл, вроде работает.


### Тест: Строки

**Описание теста**: Генерирует ли лексер строковые литералы.

**Шаги**:
1. Написать в компилируемый файл строку, которая находится между двух '"'.
2. Скомпилировать.
3. Генерирует ли строки?

**Результат до исправления**: Не прошел, лексер видит строки как 2 токена оператора '"' и индектификаторы, по-мимо этого, из-за того, что мы можем засунуть в строку оператор то происходит так, что функция генерации этих самых операторов работает не очень корректно. Например, если мы напишем printf(">>=");, то лексер выведет:
FOUND A IDENTIFIER: printf
FOUND A OPERATOR: (
FOUND A OPERATOR: "
UNKNOWN OPERATOR:>>=");

**Код до исправления** (если применимо):
``` C
// Тут ничего нет
```

**Исправление** (если применимо): [Опишите изменения]

**Код после исправления** (если применимо):
```C
Token* generate_string(FILE* file, char first_letter) {
    if (first_letter == EOF) return NULL;

    Token* token = malloc(sizeof(*token));
    if (token == NULL) return NULL;

    int capacity = 3;
    int index = 0;
    char* buffer = malloc(sizeof(char) * capacity);

    if (buffer == NULL) {
        fprintf(stderr, "Memory allocation failed!\n");
        free(token);
        return NULL;
    }

    buffer[index++] = first_letter;

    char current = fgetc(file);
    while (current != EOF && current != '"') {
        if (index >= capacity) {
            capacity *= 2;
            char* new_buffer = realloc(buffer, sizeof(char) * capacity);

            if (new_buffer == NULL) {
                fprintf(stderr, "Memory reallocation failed!\n");
                free(buffer);
                free(token);
                return NULL;
            }

            buffer = new_buffer;
        }

        buffer[index++] = current;
        current = fgetc(file);
    }

    if (index >= capacity) {
        capacity++;
        char* new_buffer = realloc(buffer, sizeof(char) * capacity);

        if (new_buffer == NULL) {
            fprintf(stderr, "Memory reallocation failed!\n");
            free(buffer);
            free(token);
            return NULL;
        }
    }

    buffer[index++] = current;
    buffer[index] = '\0';
    
    if (buffer != NULL) {
        token->type = TOKEN_STRING_LITERAL;
        token->value.string_value = strdup(buffer);
    } else {
        free(token);
        return NULL;
    }

    return token;
}
```

**Результат после исправления**: Прошёл, теперь генерирует строки и не мешает функции для генерации операций.



## Процесс исправления

