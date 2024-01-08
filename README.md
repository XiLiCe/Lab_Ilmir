<h1 align="center">Темы лабораторных работ</h1>
<a href=""#lab1>Нейронные сети</a><p></p>
<a href="lab2">Компьютерная лингвистика и обработка естественного языка</a>
 
 
 <h1 id="lab1" align="center">Использование нейронных сетей для создания системы обнаружения и классификации объектов на видеоизображениях</h1>
<p>Данный пункт включает в себя модель YoloV8x обученную на COCO dataset, а так-же модель обученную мною.</p>
<b>❗Желательно иметь Cuda</b>

<a href="#part1">Что нужно👍</a><p></p>
<a href="#part2">Создание датасета для обучения модели✊</a><p></p>
<a href="#part3">Обучение модели</a><p></p>
<a href="#part4">Запуск натренированной модели</a><p></p>
<a href="#part5">Запуск модели YoloV8x</a><p></p>
<a href="https://drive.google.com/drive/folders/1cw8_pHgt_QfCEEtuKlI_UNHO9PRXtYg0?usp=sharing">Ссылка на датасет</a>

<h2 id="part1"><b>Что нужно👍</b></h2>

<p>Установите <a href="https://www.python.org/ftp/python/3.11.6/python-3.11.6-amd64.exe">Python 3.11.6</a></p>
<!-- <p>Установите <a href="https://git-scm.com/downloads">Git</a>:octocat:</p> -->
<p>Скачайте и установите <a href="https://developer.nvidia.com/cuda-12-1-0-download-archive">CudaToolkit 12.1</a><img src="https://cdn.icon-icons.com/icons2/2107/PNG/512/file_type_cuda_icon_130656.png" style="width: 20px; height: 20px;"></p>
<p>Скачать и установить <a href="https://code.visualstudio.com/download">Visual Studio Code</a> (нужно для запуска модели через вашу камеру)</p>
<p><br></p>

<h2 id="part2">Создание датасета для обучения модели✊</h2>
<p>Я собрал 404 фотографии машин, после чего перешёл на сайт <a href="https://app.cvat.ai/tasks?page=1">CVAT </a>(нужен для разметки нашего датасета). Регистрируемся на данном сайте (можно через Гугл или ГитХаб) и заходим на наш аккаунт. <p></p>Переходим во вкладку <a href="https://app.cvat.ai/projects">Project.</a></p>
<p>Нажимаем на + в правом верхнем углу и создаём новый проект</p>
<img src="https://raw.githubusercontent.com/XiLiCe/Lab_Ilmir/cd00f8f5e5355e6352a6fe4b827060c9180282f6/+.png">
<p>Вводим название нашего проекта и добавляем лэйбл во вкладке Constructor (я назвал проекст YoloTest_detect, и добавил лэйбл с название Car_plate), после чего нажимаем Submit&Open</p>
<img src="https://github.com/XiLiCe/Lab_Ilmir/blob/XiLiCe-patch-1/newproject.png?raw=true">
<p>В открывшейся странице нажимает + и создаём новую задачу</p>
<img src="https://github.com/XiLiCe/Lab_Ilmir/blob/XiLiCe-patch-1/newtask.png?raw=true">
<p>В данном окне вводим название задачи, во вкладе Subset выбираем train и загружаем ранее собранные нами фотографии. Нажимает Submit&Open</p>
<img src="https://github.com/XiLiCe/Lab_Ilmir/blob/XiLiCe-patch-1/createnewtask.png?raw=true">
<p>В новом окне открываем нашу работу и начинаем разметку. После того как мы разметили всё что нам нужно, сохраняем работу и во вкладке Menu нажимаем Finish the job</p>
<img src="https://github.com/XiLiCe/Lab_Ilmir/blob/XiLiCe-patch-1/razmetka.png?raw=true">
<p>Теперь в нашем проекте данная задача будет отмеченна как выполенная</p>
<img src="https://github.com/XiLiCe/Lab_Ilmir/blob/XiLiCe-patch-1/finishjob.png?raw=true">
<p>Далее в разделе нашего проекта открываем Actions -> Export dataset. Здесь выбираем формат YOLO (в данном случае нам придётся в ручную загружать наши фото в папку с разметками, так как что бы скачивать файл с фото нам придётся улучшить аккаунт)</p>
<img src="https://github.com/XiLiCe/Lab_Ilmir/blob/XiLiCe-patch-1/export.png?raw=true">
<p>Нажимаем Ok и у нас происходит загрузка, нам нужно разархивировать данный файл, и в папку obj_train_data переместить все наши фотографии. У нас есть датасет, но он ещё не подходит для обучения модели Yolo</p><p></p>
<p>Для того что-бы конвертировать наш датасет в формат датасета для Yolo нам потребуется скачать репозиторий <a href="https://github.com/ankhafizov/CVAT2YOLO">CVAT2YOLO</a></p>
<p>Создаём новую папку и открываем в ней терминал. В терминале пишем (git clone https://github.com/ankhafizov/CVAT2YOLO.git). В эту папку у нас скачается репозиторий CVAT2YOLO. В папку CVAT2YOLO переносим наш датасет. После переноса датасета в папке CVAT2YOLO открываем терминал и пишем(python main_cvat2yolo.py --cvat Yolo_Vision --mode autosplit --output_folder out/my_dataset_yolov5 --img_format png), где Yolo_Vision название папки с нашим датасетом, out/my_dataset_yolov5 место сохранения нашего преобразованного датасета</p>
<img src="https://github.com/XiLiCe/Lab_Ilmir/blob/XiLiCe-patch-1/CVAT2YOLO.png?raw=true">
<img src="https://github.com/XiLiCe/Lab_Ilmir/blob/XiLiCe-patch-1/python.png?raw=true">
<p>Мы получили датасет который можно использовать для обучения нашей модели</p>

<h2 id="part3">Обучение модели</h2>

<p>Для обучения нашей модели желательно использовать Google Colab, и подключится к видеокарте</p><p></p>
<p>Открываем файл YoloTrain.ipynb и загружаем в сессию наш датасет (файл yolodataset). В файле YoloVisions.yaml в path указываем ссылку на наш датасет. </p>
<img src="https://github.com/XiLiCe/Lab_Ilmir/blob/XiLiCe-patch-1/yaml.png?raw=true">
<p>После всего этого запускаем каждый блок кода по порядку.</p>
<img src="https://github.com/XiLiCe/Lab_Ilmir/blob/XiLiCe-patch-1/train.png?raw=true">
<p>После окончания мы получим обученную модель, она будет называться best.pt</p>

<h2 id="part4">Запуск натренированной модели</h2>
<p>Для запуска нашей модели мы открывает в GoogleColab файл YoloTraffic.ipynb (подключаемся так же к видеокарте)</p><p></p>
<p>Запускаем каждый блок кода по очереди. После выполнения мы получим видео car_plate</p>
<img src="https://github.com/XiLiCe/Lab_Ilmir/blob/XiLiCe-patch-1/carplateitog.png?raw=true">

<h2 id="part5">Запуск модели YoloV8x</h2>
<p>Нам нужно открыть файл YoloVision.ipynb с помощью Visual Studio Code. Тут нам потребуется после загрузки библиотеки ultralytics, удалить библиотеки torch, torchvision и torchaudio. Мы удаляем их так как версии которые скачиваются вместе с ultralytics не работают с CudaToolkit 12.1</p><p></p>
<p>Нам нужно скачать нужные нам версии torch ,torchvision и torchaudio, которая будет поддерживать CudaToolkit 12.1</p>
<p>Запускаем по очереди блоки кода в файле YoloVision.ipynb</p><p></p>

<p>Так как не у всех есть веб камера на компьютере можно использовать приложение <a href="https://ivcam.softonic.ru/">iVCam</a> на телефон и компьютер. Подключаем их и получаем: </p>
<img src="https://github.com/XiLiCe/Lab_Ilmir/blob/XiLiCe-patch-1/itog.png?raw=true">
<img src="https://github.com/XiLiCe/Lab_Ilmir/blob/XiLiCe-patch-1/itog2.png?raw=true">


<p></p>
<p></p>
<p></p>


<h1 id="lab2" align="center"><b>Разработка программы для распознавания именованных сущностей в тексте: имена, даты, организации и др.</b></h1>
<p>Данный пункт содержит в себе объяснение кода по лабораторной работе по предмету "Компьютерная лингвистика и обработка естественного языка"</p>

<h2 id="download">Загружаем код</h2>
<p>Код можно склонировать с данного <a href="https://github.com/XiLiCe/Lab2.git">репозитория</a></p><p></p>
<p>Ссылка https://github.com/XiLiCe/Lab2.git</p>

<h2 id="start_code">Запуск кода</h2>
<p>Для запуска нужно запустить каждый блок кода по порядку</p><p></p>
<p>После запуска мы увидим поле в которое нужно ввести текст в котором мы хотим распознать сущности</p><p></p>
<img src="https://github.com/XiLiCe/Lab2/blob/XiLiCe-patch-1/pole.png">
<p>Как только мы введем туда текст мы получим определённые сущности на выводе</p>
<img scr="https://github.com/XiLiCe/Lab2/blob/XiLiCe-patch-1/output.png">
<p>Пример текста</p><p></p>
<p>Стерлитамак — город в России, второй (после Уфы) по численности населения город в Башкирии, административный центр Стерлитамакского района, в состав которого не входит. Город республиканского значения, образует муниципальное образование Городской округ город Стерлитамак как единственный населённый пункт в его составе. Крупный центр химической промышленности и машиностроения, один из центров Стерлитамакской агломерации. Основан в 1735 году как почтовая станция Ашкадарский Ям, статус города присвоен в 1781 году. В прошлом уездный город Уфимской области Уфимского наместничества (1781/82–1796)год; Оренбургской губернии (1796–1865)год, Уфимской губернии (1865–1920)год, столица Автономной Советской Башкирской Республики (1919—1922)год, а также административный центр Стерлитамакского кантона (1920—1930)год и Стерлитамакской области БАССР (1952—1953)год</p>

<h2 id="what">Как работает?</h2>
<p>


