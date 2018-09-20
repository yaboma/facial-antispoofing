# Facial-antispoofing

Спуфинг в биометрии (spoofing, атака с подменой личности) - ахиллесова пята многих биометрических систем идентификации и верификации. Подобным атакам подвержены практически все используемые модальности, но в первую очередь - голос и лицо. Спуфинг в системах лицевой биометрии, работающих с RGB-изображениями или видеорядом, может происходить по сценарию атаки повторного воспроизведения (video replay-attack): осуществляется съемка экрана, на котором проигрывается видеофайл с лицом взламываемой персоны. Эффект данного вид атаки усиливается, если биометрическая система работает в режиме frictionless, когда верификация пользователя выполняется параллельно его взаимодействию с приложением, а не прерывает его.

## Выборка
База IDRND_FASDB - это кадры из видеозаписей, снятых в различных условиях. 
Оригинальные видеозаписи получены с помощью веб-камер, камер мобильных телефонов или скачаны c Youtube.
Атаки повторного воспроизведения получены путем съемки экранов различных ноутбуков и мониторов (120 шт) камерами мобильных телефонов (111 шт) в момент проигравания оригинальных видеозаписей различных персон (115 чел), полученных с помощью различных устройств видеосъемки (12 шт).
Атаки были получены как в лабораторных условиях, так и с помощью исполнителей, зарегистрированных в краудсорсинговых интернет-сервисах Яндекс.Толока и Amazon Mechanical Turk.

![alt text](https://github.com/vicident/Facial-antispoofing/blob/master/crop.png "Crop-align scheme")

## Структура IDRND_FASDB
| Base        | Category           | Images  |
| ------------- |:-------------:| -----:|
| IDRND_FASDB_train | real | 1223 |
| IDRND_FASDB_train | spoof | 7076 |
| IDRND_FASDB_val | real | 373 |
| IDRND_FASDB_val | spoof | 632 |

Имена файлов из _train:

**real**: `real/<source code>_id<user id>_*.png` <br/>
**spoof**: `spoof/<source code>_<screen code>_<device code>_id<user id>_*.png`

## Baseline
Простой пример распознавания атаки по текстуре приведен в [train_baseline.ipynb](../master/train_baseline.ipynb). Для LBP (Local Binary Patterns) признаков и гистограмм HSV и YCbCr представлений исходного изображения формируется классификатор на основе метода градиентного бустинга деревьев решений ([CatBoost](https://catboost.yandex/)).

## Docker
Сборка образа для докер контейнера производится следующей командой:

    docker build -t danil328/antispoofing .
    
Или:

    docker pull danil328/antispoofing

Создание контейнера и запуск:

    nvidia-docker run -v 'test_dir':/test -v 'output_dir':/output -it danil328/antispoofing /bin/bash
    nvidia-docker run --mount src="/test",target=/test,type=bind --mount src="/output",target=/output,type=bind -it danil328/antispoofing /bin/bash 

    
Запуск predict:

    cd root
    python my_main.py


**Точки монтирования /test и /output - обязательны для оценки решений. Формат выходного решения представлен в примере.**

[Справочник по докеру](https://docs.docker.com/)
