Тестовый блокнот для пробы загрузки файла с google диска


```python
from google.colab import drive

drive.mount('g', force_remount=True)
```

    Mounted at g
    

Вывод списка содержимого диска Google


```python
import os

directory: str = 'g/MyDrive'
files: list = os.listdir(directory)
print(files)
```

    ['PICT2148.JPG', 'FOREX', 'Биржа', 'Сканы документов', 'trudnye-dialogi.pdf', 'Google-Drive.ico', 'Temp', 'ИП', 'SBPro', 'BIRTHDAY', '2017-04-14_032553 Тест SSD PCI-E.png', '2017-04-14_033617_Тест SSD SATA.png', '2017-04-14_034215 HDD E.png', '2017-04-14_034215 HDD F.png', 'Folder Download.ico', 'Вакансия Электромеханик в Санкт-Петербурге, работа в ОКИЛ, полиграфический холдинг.pdf', 'Календарь рабочие смены 2017.xlsx', 'Личная', 'Андрис', 'Book', 'Программы', 'Календарь рабочие смены 2018.xlsx', 'Книга1.xlsx', 'Торговый план.gsheet', 'Новый документ.gdoc', '001.gsheet', '2018-04-14_231443.png', '2018-04-15_104702.png', '2018-04-15_222533.png', '2018-04-17_205858.png', 'Внук_28-06-2018_12-26-39.zip', 'Заявка.gsheet', 'Ланс Бегс', 'Календарь рабочие смены 2018 Виади.xlsx', 'Инструкции', 'Работа СВЕЗА', 'Налог 2018 uved-25549821.pdf', '2019 Рабочие смены.png', 'Календарь рабочие смены 2019 Виади.xlsx', '2019-04-11_191947.png', '2019-04-11_194637.png', 'UOPilot', '2019-05-03_18h40_21.png', 'Книга KAPTUR.pdf', 'PSB Промсвязьбанк', 'wow', 'Пожелания для внесения изменений после модернизации КГУ.docx', 'Инвестиции', 'Крипта', '2020 Рабочие смены.png', 'Календарь рабочие смены 2020 Виади.xlsx', 'Покупка Ноотропов', '[obuka] [Джим Квик] Программа «Супермозг» Доп. Материалы', 'Ноотропы', 'garibyan-samvel-chudo-slovar-klyuchey-zapominaniya-3500-angliyskih-slov-142217.pdf', '01.02.2020.xls', '01.02.2020_01.xls', '01.02.2020_Edit.xls', 'Копия Калькулятор вложений в S&P500 VOO или FXUS а может на СПБ бирже собрать самостоятельно?.gsheet', 'Python learn', 'quik_doc.zip', 'Quik8 Doc', 'Python', 'Бизнес идеи', 'Python_SkillBox', 'Витамины.xlsx', 'Ставки', 'Копия FAQ по курсу “Пайтон с нуля”.gdoc', 'Тест опционов.gsheet', 'CherryTree_Data (1).ctb~~~', 'data_saients_webinar', 'TsLab', 'Основы статистики StepIk', 'Data Science', 'ИИС ВТБ Портфель ЦБ (ver. 2).gsheet', 'Билет на конференцию.gdoc', 'Colab Notebooks', 'Мемы', 'Deep-Learning-School', 'Профессия_Python-разработчик.pdf', 'Typora', 'Blog', 'Alaracroft', 'CherryTree_Data.ctb~~~', 'CherryTree_Data.ctb~~', 'CherryTree_Data.ctb', 'CherryTree_Data.ctb~', 'Python_SkillBox_code_solutions', 'Заявление в ИНГОССТРАХ 4071792.gdoc', 'ПТЭ_2021', '2021 Рабочие смены.png', 'Фильмы.gdoc', 'Python_SkillBox_Basic_Jupiter', 'descript.ion', 'Биткоин обмен', 'QUIK_real_time', 'gpedit_install.bat', 'Дом', 'ПЭС', 'crypto', '2021_01 Рабочие смены.png', 'Календарь рабочие смены 2021 Виади.xlsx', 'Natali', '5d9c7eb8bf2a4f55846fe26f0d6a2924_210409T082919.938_pdf.pdf', '32445833d7ee48868f437ee756dbec4e_210904T203318.671_pdf.pdf', 'Файл Jam без названия.gjam', 'Майнинг', 'SM.gsheet', 'Подбор комплектующих.gsheet', 'RIG_001.gsheet', '2021-10-14_115851.png', '2021-10-21_213416.png', 'Бизнес-план по Рижским ценам.gsheet', '2021-11-15_191335 Илья Огородов.png', 'Matvienko.png', 'Солнечные панели.gsheet', 'SQL_skillbox', '111', 'Проценты по стейкингу крипты.gsheet', '2021-12-22_144150.png', 'AAA', 'Vires Finance Waves-USDN стейкинг USDN.gsheet', 'SkillBox_Диплом_Basic', 'IPTV_Sharovoz']
    

Чтение файла и вывод содержимого файла с гугл диска


```python
with open('g/MyDrive/Temp/test_google_drive.txt', 'r') as file:
  file_contents: str = file.read()
  print (file_contents)  # Вывод содержимого файла
```

    Это тестовый файл на google диске
    
