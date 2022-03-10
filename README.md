# Информационная безопасность - задания

- Задания индивидуальные, выполнять их нужно самостоятельно. 
- Результат выполнения выложите в свой приватный репозиторий
- Дайте доступ к репозиторию пользователю gltronred
- В README укажите, как вас зовут, где находится решение задания, как оно работает, 
  как его запускать и т.п.

# 1

Режимы шифрования нужны, чтобы одинаковые блоки не переходили в одинаковые (как
в режиме ECB). Поскольку блоки короткие, в результате всё ещё видно внутреннюю
структуру файла, особенно если это большой медиафайл.

Кроме того, зашифрованное в режиме ECB можно расшифровать побайтово.

Напишите код, который при запуске генерирует случайный ключ K и секретную строку
S. Можно представить, например, что S - некая соль. Далее код предоставляет доступ к функции

f(x) = AES-128-ECB(x || S, K),

т.е. даёт возможность шифровать результат конкатенации x и S ключом K. Доступ
может быть, например, через ввод-вывод, http-запрос и т.п.

Ваша задача - написать код, который, имея доступ к f, получит строку S.

Для этого вы можете составить словарь блоков вида A...AA, A...AB, ..., A...AZ
(длина - один блок, последний символ пробегает все возможные символы) и сравнить
его с первым блоком результата шифрования A...A (длина на один меньше, чем длина
блока). Далее аналогичным образом вычисляется второй и последующие символы
строки S.

# 2

MAC - message authentication code - применяется для аутентификации сообщений
(проверки их подлинности). У обоих участников есть общий секретный ключ K, Алиса
генерирует (sign) метку t = S(K,m) для сообщения m, Боб проверяет (verify) метку
при помощи какого-то метода V(K,m,t).

Два свойства безопасности MAC: 

- корректная метка сообщения всегда принимается;
- злоумышленник, имея доступ к генерации и проверке меток, но не имея доступа к
  секретному ключу, не может (точнее, вероятность такого мала) сгенерировать для
  какого-либо сообщения метку, которая пройдёт проверку
  
Как видно, свойства безопасности MAC отличаются от свойств безопасности
хэш-функций. Их не надо путать

Ваша задача состоит во "взломе" CBC-MAC, который некорректно используется вместо
хэша.

CBC-MAC - способ получения MAC из блочного шифра. Строка шифруется в режиме CBC,
последний шифр-блок используется как метка.

В странном сайте вместо проверки хэша загружаемого javascript используется
AES-128-CBC-MAC, с ключом "YELLOW SUBMARINE" и IV полностью из нулевых символов.

На странице загружается код

``` javascript
alert("Hello world!");
```

Найдите такой корректный javascript-код, который запустит `alert("You are
pwned!");`, с совпадающей с CBC-MAC-меткой предыдущего кода.

