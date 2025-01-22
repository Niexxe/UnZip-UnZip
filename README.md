# UnZip-UnZip for CTF

---

```

import zipfile
import os
 
#Установка начального архива и его пароля
current_zip = "flag_A1.zip"
password = ""
 
while True:
    #Проверка на существование файла
    if not os.path.isfile(current_zip):
        print(f"Архив {current_zip} не найден.")
        break
    #Распаковка архива с использованием текущего пароля
    try:
        with zipfile.ZipFile(current_zip, 'r') as zip_ref:
            zip_ref.extractall(pwd=bytes(password, 'utf-8'))
    except RuntimeError as e:
        print(f"Ошибка при распаковке {current_zip}: {e}")
        break
    #Получение следующего файла архива
    #предполагаем, что в каждом архиве только один следующий архив
    with zipfile.ZipFile(current_zip, 'r') as zip_ref:
        next_zip = None
        for file_name in zip_ref.namelist():
            if file_name.endswith('.zip'):
                next_zip = file_name
                break
    #Если в архиве не нашлось нового zip файла, завершаем работу
    if not next_zip:
        print(f"Достигнут конец матрёшки: в {current_zip} нет других архивов.")
        break
    #Установка пароля для следующего архива как имя текущего архива без расширения
    password = os.path.splitext(current_zip)[0]
    # Обновление текущего архива для следующей итерации
    current_zip = next_zip
    print(f"Переход к {current_zip} с паролем {password}")
 
print("Распаковка завершена.")


```
