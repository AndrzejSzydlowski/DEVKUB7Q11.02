# Домашнее задание к занятию "11.02 Микросервисы: принципы"

## Задача 1: API Gateway

RPC | GraphQL | REST
------ | ------|----------
Легковесный payload      | Small payload     | Большой payload
Высокая производительность |  Один запрос     | Множественные HTTP-запросы.
 Практически нет стандартизации | Cтрого типизирован. Во время разработки проверка типов GraphQL помогает гарантировать, что запрос синтаксически верен и действителен  | Стандартное имя метода, формат аргументов и коды состояния

![image](https://user-images.githubusercontent.com/72221502/162594713-fc600e12-41ef-4e90-9ad1-b3bd8a87803f.png)

## Задача 2: Брокер сообщений
Kafka | RabbitMQ | AWS SNS/SQS
------ | ------|----------
 Акцент на потоковом контенте, работа с большими потоками данных | Механизм маршрутизации дополняют ключи маршрутизации (Routing Keys). Они позволяют создавать гибкие правила пересылки сообщений между источниками и потребителями  | Возможность передавать широковещательные сообщения и работать по модели «Публикации — подписки»
Для маршрутизации сообщений могут применяться Routing Keys, похожие на те, что используются в RabbitMQ. Но, в отличие от RabbitMQ, Apache Kafka гарантирует порядок доставки сообщений, обеспечивает сохранность сообщений и их многократную повторную обработку | Основная особенность — расширенный функционал маршрутизации | Быстрая настройка с помощью AWS
Параллелизм и распределенная архитектура хорошо сказываются на надежности: даже выход из строя части кластера не нарушит доставку сообщений | Реализует концепцию push-доставки: поставщик может направить новые события в сеть, но получатель не может их запросить у поставщика. При этом система не гарантирует порядок доставки сообщений.  | Отсутствие хостинга за пределами AWS

я бы выбрал RabbitMQ методом исключения: мы не знаем работает ли мы в кластере AWS или нет, соответственно AWS SNS/SQS по умолчанию отбрасываем. Kafka как и RabbitMQ имеют Routing Keys, но Kafka имеет порядок сообщений, а это может сказаться на скорости работы, а повышенная надежность обычно ведёт к сложности.
