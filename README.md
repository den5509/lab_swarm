# Исследование Swarm-mode Docker
## Выполнение работы
Для работы с роем контейнеров был использован docker-machine. Для создания хостов с установленным на них Docker Engine использовался драйвер virtualbox. Сначала был создан узел под названием manager в первом терминале:
```bash
docker-machine create manager
```
![alt]({{"/Снимок экрана от 2017-11-20 09-08-06.png" | absolute_url}})
Рисунок 1 — Создание узла manager

Во втором и третьем терменелах аналогично только имена другие 
```bash
docker-machine create agent1
docker-machine create agent2
```
![alt]({{"/Снимок экрана от 2017-11-20 09-10-46.png" | absolute_url}})
Рисунок 2 — Создание узла agent1

![alt]({{"/Снимок экрана от 2017-11-20 09-09-20.png" | absolute_url}})
Рисунок 3 — Создание узла agent2

Информацию о созданных узлах можно вывести при помощи команды:

```bash
docker-machine ls
```
![alt]({{"/Снимок экрана от 2017-11-20 00-16-03.png" | absolute_url}})
Рисунок 4 — Информация об узлах

Теперь необходимо произвести инициализацию роя. Она производится при помощи команды:
```bash
docker swarm init --advertise-addr 192.168.99.100:2377
```
Результат выполнения команды продемонстрирован на первом скриншоте, но его можно продублировать еще раз:
![alt]({{"/Снимок экрана от 2017-11-20 09-08-06.png" | absolute_url}})
Рисунок 5 — Инициализация роя
На данном этапе получен рой контейнеров, который состоит из 1 элемента. Рой состоящий из одного элемента-узла не может быть помехозащищенным и тем более масштабируемым. Добавим в рой ещё один узел и тогда сможем при минимальных требованиях получить масштабираемый рой.
![alt]({{"/Снимок экрана от 2017-11-20 00-49-10.png" | absolute_url}})
Рисунок 6 — Добавление в рой рабочего узла
Теперь мы получили масштабируемый рой контейнеров, состоящий из 2 узлов. Чтобы в рое появилась высокая доступность, добавим еще один узел - третий.
![alt]({{"/Снимок экрана от 2017-11-20 00-34-02.png" | absolute_url}})
Рисунок 7 — Добавление в рой еще одного рабочего узла
Вот теперь мы получили рой состоящий из трех узлов который обладает масштабируемостью, и высокой доступностью. Посмотрим информацию о нем при помощи команды:
```bash
docker node ls
```
![alt]({{"/Снимок экрана от 2017-11-20 00-33-00.png" | absolute_url}})
Рисунок 8 — Информация о созданном рое

---
## Вывод
Врамках выполнения лабораторной были выявленны следующие особенности:
- Если терминал привязан к виртуальной машине то новую виртуальную машину в нем создавать нельзя (каждая виртуальную машину запускать в отдельном терминале);
- Минимально рой контейнеров может состоять из 1 элемента, но мы не получаем помехозащищенности и масштабируемость;
- Масштабируемость достигается при двух элементах;
- Высокую доступность при трех и более элементах.
