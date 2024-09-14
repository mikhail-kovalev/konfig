![image](https://github.com/user-attachments/assets/b264bee7-f1f9-4e3b-b99a-405e0af326b4)# CONFIG-prac-1
# Задание 1

![image](https://github.com/user-attachments/assets/f547c21c-2bf9-4fef-a0b5-3866bc80fa20)
![image](https://github.com/user-attachments/assets/4a56c8dd-e351-4955-81d2-90b8a3525514)
```
grep '.*' /etc/passwd | cut -d':' -f1 | sort
```

# Задание 2

![image](https://github.com/user-attachments/assets/68c1cf81-56a8-47cc-bc8a-02a0f645bdad)
![image](https://github.com/user-attachments/assets/7c1aca4d-8786-4187-ad79-ae6d7c5d554c)

```
cat /etc/protocols | awk '{print $2, $1}' | sort -nr | head -5
```

# Задание 3

![image](https://github.com/user-attachments/assets/e636f362-bbca-4e23-8cb5-ade36fbc37d9)
![image](https://github.com/user-attachments/assets/5f579ab6-4b8d-41b8-87c8-3130059ae606)

```
nano banner.sh
```

```
#!/bin/bash
 
if [ $# exit 1]; then
        echo "Использование: $0 \"Hello from RTU MIREA!\""
        exit 1
fi
 
text="$1"
text_length=${#text}
border_length=$((text_length + 4))
 
#Вывод верхней границы
echo "+$(printf -- '-%.0s' $(seq 1 $border_length))+"
 
# Вывод текста в баннере
echo "| $text |"
 
# Вывод нижней границы
echo "+$(printf -- '-%.0s' $(seq 1 $border_length))+"
```
```
#сделали файл исполняемым
chmod +x banner.sh
```
```
#запуск скрипта
./banner.sh "Hello from RTU MIREA!"
```

# Задание 4

![image](https://github.com/user-attachments/assets/7c545689-ad3d-4174-ab87-7c343b4055a7)
![image](https://github.com/user-attachments/assets/c3a1de7a-c4af-4799-814f-1557aacdd2cf)

```
nano hello.c
```
```
#include <stdio.h>

void hello() {
    printf("Hello, world!\n");
}

int main() {
    hello();
    return 0;
}
```
```
grep -o -E '\b[_a-zA-Z][_a-zA-Z0-9]*\b' hello.c | sort | uniq
```



# Задание 5

![image](https://github.com/user-attachments/assets/58db81f5-2b82-40c1-88f2-8e2e33faa725)
![image](https://github.com/user-attachments/assets/bccd1a2e-aa12-43ee-980c-edab8544749a)

```
nano reg
```
```
#!/bin/bash
 
if [ $# -ne 1 ]; then
        echo "Использование: $0 <команда>"
        exit 1
fi
 
command="$1"
 
if [ -x "$command" ]; then
        echo "Команда $command уже существует и имеет исполняемые права."
        exit 1
fi
 
if [ -f "$command" ]; then
        chmod +x "$command"
        cp "$command" /usr/local/bin/
        echo "Команда $command зарегистрирована и скопирована в /usr/local/bin/"
else
        echo "Команда $command не найдена."
        exit 1
fi
```
```
chmod +x reg
```
```
./reg banner.sh
```
```
cat reg
```
```
./reg banner.sh
```
```
sudo cp banner.sh /usr/local/bin/
```
```
ls -l /usr/local/bin/banner.sh
```
```
banner.sh "Hello from RTU MIREA!"
```

# Задание 6

![image](https://github.com/user-attachments/assets/6748d166-e95d-4f94-bb7a-852a22b31007)
![image](https://github.com/user-attachments/assets/a520db14-ca86-4f77-9339-a8bbf599d8ed)
![image](https://github.com/user-attachments/assets/e07284f1-8616-4c6d-b06a-044d0713c4ea)

```
nano check_comments.sh
```
```
#!/bin/bash
 
if [ $# -ne 1 ]; then
        echo "Использование: $0 <путь_к_каталогу>"
        exit 1
fi
 
directory="$1"
 
# Проверяем, существует ли указанный каталог
if [ ! -d "$directory" ]; then
        echo "Каталог $directory не существует."
        exit 1
fi
 
# Проверяем наличие комментария в первой строке файла
check_comment() {
        local file="$1"
        local extension="${file##*.}"  # Получаем расширение файла
 
        # Если расширение файла соответствует .c, .js или .py
        if [[ "$extension" == "c" || "$extension" == "js" || "$extension" == "py" ]]; then
                # Считываем первую строку файла и проверяем, начинается ли она с комментария
                first_line=$(head -n 1 "$file")
                if [[ "$first_line" =~ ^[[:space:]]*\/\/.* || "$first_line" =~ ^[[:space:]]*#.* ]]; then
                        echo "Файл $file содержит комментарий в первой строке."
                else
                        echo "Файл $file не содержит комментария в первой строке."
                fi
        fi
}
```
```
chmod +x check_comments.sh
```
```
pwd
```
```
ls -R
```
```
./check_comments.sh
```
```
cat check_comments.sh
```

# Задание 7
![image](https://github.com/user-attachments/assets/850f1eca-33e5-4738-aafc-a5c107c694ca)
![image](https://github.com/user-attachments/assets/cee7fcdc-ad17-4eed-9711-0acd42e7a3fb)
![image](https://github.com/user-attachments/assets/3e33c22a-7024-46ec-97e0-141b65c61d34)

![image](https://github.com/user-attachments/assets/981f38cd-3225-44ca-83c7-5c8f16843a69)

```
nano find_duplicates.sh
```
```
#!/bin/bash

if [ $# -ne 1 ]; then
  echo "Использование: $0 <путь_к_каталогу>"
  exit 1
fi

directory="$1"

# Поиск дубликатов с использованием md5sum
find "$directory" -type f -print0 | xargs -0 md5sum | sort | uniq -d -w 32 | sed -r 's/^[0-9a-f]*( )//' | while read -r duplicate; do
  echo "Дубликат: $duplicate"
done
```
```
chmod +x find_duplicates.sh
```
```
./find_duplicates.sh /root
```


# Задание 8
![image](https://github.com/user-attachments/assets/52db1d37-a2ac-4a5b-89ab-8c72dfc9bbb6)
![image](https://github.com/user-attachments/assets/f681519a-c2d5-4b77-9feb-25c7a91a5f03)
![image](https://github.com/user-attachments/assets/d4f8b70b-713c-4a35-a528-6ff524f1976f)

```
nano archive_files.sh
```
```
#!/bin/bash

if [ $# -ne 2 ]; then
  echo "Использование: $0 <расширение_файлов> <имя_архива>"
  exit 1
fi

extension="$1"
archive_name="$2"

# Поиск файлов с заданным расширением в текущем каталоге
files=$(find . -type f -name "*.$extension")

# Проверка наличия файлов
if [ -z "$files" ]; then
  echo "Файлов с расширением .$extension не найдено."
  exit 1
fi

# Архивация файлов
tar -czvf "$archive_name.tar.gz" $files

echo "Файлы с расширением .$extension были архивированы в $archive_name.tar.gz"
```
```
./archive_files.sh txt myarchive
```
```
tar -tzvf myarchive.tar.gz
```
```
ls -l
```
```
ls -ld
```


# Задание 9
![image](https://github.com/user-attachments/assets/eefe08f4-3a20-4703-a47c-486d7dd7b7f3)
![image](https://github.com/user-attachments/assets/573437e3-b05d-41d9-b8f3-7abf8452391c)

```
nano replace_spaces.sh
```
```
#!/bin/bash

if [ $# -ne 2 ]; then
  echo "Использование: $0 <входной_файл> <выходной_файл>"
  exit 1
fi

input_file="$1"
output_file="$2"

# Проверка существования входного файла
if [ ! -f "$input_file" ]; then
  echo "Входной файл $input_file не существует."
  exit 1
fi

# Замена последовательностей из 4 пробелов на символ табуляции и запись в выходной файл
sed 's/    /\t/g' "$input_file" > "$output_file"

echo "Замена завершена. Результат сохранен в $output_file."
```
```
chmod +x replace_spaces.sh
```
```
echo "This is a test file with    four spaces." > input.txt
```
```
cat input.txt
```
```
./replace_spaces.sh input.txt output.txt
```
```
cat output.txt
```
```
od -c output.txt
```


# Задание 10

![image](https://github.com/user-attachments/assets/046346ec-900d-4ba0-9a99-2d598b79cabb)
![image](https://github.com/user-attachments/assets/60e222c2-9286-4411-8276-4e50de19612d)

![image](https://github.com/user-attachments/assets/4fdd6da8-bd89-412a-9de7-0c98fa3877ac)

```
nano find_empty_files.sh
```
```
#!/bin/bash

if [ $# -ne 1 ]; then
  echo "Использование: $0 <директория>"
  exit 1
fi

directory="$1"

# Проверка существования указанной директории
if [ ! -d "$directory" ]; then
  echo "Директория $directory не существует."
  exit 1
fi

##Поиск пустых текстовых файлов
find "$directory" -type f -empty -exec file {} \; | grep "empty" | cut -d: -f1
```
```
find /home/testdir -maxdepth 1 -type f -size 0c
```
