:# diplom

Нужно скачать: дамп dbPedia https://drive.google.com/file/d/18OaFbzUT0kjVRTcgS8Dp9SY7t_nzUPao/view?usp=sharing и закинуть его в dbpediaparser

дамп opencorpora https://drive.google.com/file/d/1gq_EqHCSj_ZW-mA55RWCmbCFkDJFqvBh/view?usp=sharing в ner

поставить dbpediaEnquirerPy на место папки с тем же именем.

Две папки dbpediaparser и ner.

В dbpediaparser есть parser.py, она создаёт из дампа dbpedia файл csv, вида: имя сущности/тип сущности, достаточно запустить main

В ner:\
Databuilder-строит из дампа опенкорпоры данные. Дамп должен быть в той же директории. Создает файл corpus2, в котором каждая строка-предложение, а предложение-список троек [слово, часть речи, тип сущности в опенкорпоре] 

test2 часть из corpus2, на которой тестируются алгоритмы.


Alg, alg+dic, alg+wiki+checker-собственно чистые алгоритмы, алгоритм со словарём и со словарём+чекером.

Alg:
  на вход подаются corpus2(обучающая выборка) и test2(тестовая выборка)\
  word2feature извлекает признаки из каждого слова в предложении\
  sent2feature- для предложения возвращает список признаков слов\
  sent2label- для предложения возвращает список типов слов в предложении(имеются в виду типы именованых сущностей)\
  sent2token- для предоложения возвращетс список частей речи слов.

main-запускает процесс обучения различных алгоритмов МО и тестирует их, вывод-показатели precision,recall,fscore

Alg+dic:\
  make_dict-строит dict на основе построенного словаря из parser.py Словарь: "имя сущности":"тип сущности"\
  in_dict проверяет слово на наличие в словаре.\
  word2feature-добавляется признак, находится ли слово в словаре\
  main- Аналогично Alg, только из алгоритмов остался только CRF.\
  Остальные методы аналогичны\

Alg+wiki+checker:\

  make_dict словарь строится немного по-другому, теперь для Person записи в словаре выглядят: "Фамилия":"Остаток"\
  sent2feature-добавляется предобработка текста, а именно, связывание сущностей и исправление ошибок\
  correction_sent- для слова генерирует всевозможные ошибки(только одну за раз) и проверяет на наличие слова в словаре. Если исправление  одно, то происходит замена.\
  full_name_builder- составляет список полных имен в предложении. Для каждого слова в предложении записывает, является ли оно частью полного имени какой-либо сущности или нет, если является, то в список полных имен для каждого такого слова пишется полное имя сущности.\
  word2feature добавляется признак full_name, отвечающий за полное имя сущности\
остальное аналогично аналогично alg+dic\




