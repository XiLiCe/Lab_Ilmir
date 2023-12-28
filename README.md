 <h1 align="center">Использование нейронных сетей для создания системы обнаружения и классификации объектов на видеоизображениях</h1>
<p>Этот репозиторий включает в себя модель YoloV8x обученную на COCO dataset, а так-же модель обученную мною.</p>
<b>❗Желательно иметь Cuda</b>

<a href="#part1">Что нужно👍</a><p></p>
<a href="#part2">Создание датасета для обучения модели✊</a><p></p>
<a href="#part3">Обучение модели</a><p></p>
<a href="#part4">Запуск натренированной модели</a><p></p>
<a href="#part5">Запуск модели YoloV8x</a><p></p>
<a href="https://drive.google.com/drive/folders/1cw8_pHgt_QfCEEtuKlI_UNHO9PRXtYg0?usp=sharing">Ссылка на датасет</a>

<h2 id="part1"><b>Что нужно👍</b></h2>

<p>Установите <a href="https://www.python.org/ftp/python/3.11.6/python-3.11.6-amd64.exe">Python 3.11.6</a></p>
<p>Установите <a href="https://git-scm.com/downloads">Git</a>:octocat:</p>
<p>Скачайте и установите <a href="https://developer.nvidia.com/cuda-12-1-0-download-archive">CudaToolkit 12.1</a><img src="https://cdn.icon-icons.com/icons2/2107/PNG/512/file_type_cuda_icon_130656.png" style="width: 20px; height: 20px;"></p>
<p>Скачать и установить <a href="https://code.visualstudio.com/download">Visual Studio Code</a> (нужно для запуска модели через вашу камеру)</p>
<p><br></p>

<h3 id="part2">Создание датасета для обучения модели✊</h3>
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

<h3 id="part3">Обучение модели</h3>

<p>Для обучения нашей модели желательно использовать Google Colab, и подключится к видеокарте</p><p></p>
<p>Открываем файл YoloTrain.ipynb и загружаем в сессию наш датасет (файл yolodataset). В файле YoloVisions.yaml в path указываем ссылку на наш датасет. </p>
<img src="https://github.com/XiLiCe/Lab_Ilmir/blob/XiLiCe-patch-1/yaml.png?raw=true">
<p>После всего этого запускаем каждый блок кода по порядку.</p>
<img src="https://github.com/XiLiCe/Lab_Ilmir/blob/XiLiCe-patch-1/train.png?raw=true">
<p>После окончания мы получим обученную модель, она будет называться best.pt</p>

<h4 id="part4">Запуск натренированной модели</h4>
<p>Для запуска нашей модели мы открывает в GoogleColab файл YoloTraffic.ipynb (подключаемся так же к видеокарте)</p><p></p>
<p>Запускаем каждый блок кода по очереди. После выполнения мы получим видео car_plate</p>
<img src="https://github.com/XiLiCe/Lab_Ilmir/blob/XiLiCe-patch-1/carplateitog.png?raw=true">

<h5 id="part5">Запуск модели YoloV8x</h5>
<p>Нам нужно открыть файл YoloVision.ipynb с помощью Visual Studio Code. Тут нам потребуется после загрузки библиотеки ultralytics, удалить библиотеки torch, torchvision и torchaudio. Мы удаляем их так как версии которые скачиваются вместе с ultralytics не работают с CudaToolkit 12.1</p><p></p>
<p>Нам нужно скачать нужные нам версии torch ,torchvision и torchaudio, которая будет поддерживать CudaToolkit 12.1</p>
<p>Запускаем по очереди блоки кода в файле YoloVision.ipynb</p><p></p>

<p>Так как не у всех есть веб камера на компьютере можно использовать приложение <a href="https://ivcam.softonic.ru/">iVCam</a> на телефон и компьютер. Подключаем их и получаем: </p>
<img src="https://github.com/XiLiCe/Lab_Ilmir/blob/XiLiCe-patch-1/itog.png?raw=true">
<img src="https://github.com/XiLiCe/Lab_Ilmir/blob/XiLiCe-patch-1/itog2.png?raw=true">
