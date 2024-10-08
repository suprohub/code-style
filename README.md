# Типизация
Для маленьких проектов типизация особо не нужна, но для больших она очень поможет.
Используется она для трех целей:
+ Контракт
+ Проверка типов
+ Проектирования

Можно использовать и для других целей, но это основные.
Сейчас я расскажу про эти цели.

## Контракт
Есть у нас функция с объектом на входе.
```ts
const send_message = (message) => {
    this.send(message.text)
}
```
Какие там поля? непонятно. Другим разработчикам придется читать твой код чтобы понять что оно делает, а потом делать свою надстройку над твоим кодом. И чтобы потом друг под друга не подстраиваться удобно просто определить интерфейс и сказать что там будет тото тото.
```ts
interface Message {
  text: string;
}
const send_message = (message: Message) => {
  this.send(message.text)
}
```
Когда вы определились, то можно писать код.

## Проверка типов
Например есть какая-то структура и библиотека требует дополнительное поле в своей структуре.
```ts
import send_message from "superlib";

send_message({text: "i like this superlib"})
// <- Error: needed field "timestamp"
// now dont like superlib
```
И вроде легко поменять, но на самом деле ты у себя поменяешь, а понадобится поменять еще где-то там в 100 местах, там где используется структура. Даже когда код твой то это и то не простая задача, но твой код могут использовать еще и другие программисты, и у себя ты поправишь, а у других все сломается.И тут нам поможет тайпскрипт: при компиляции он покажет все места, где структура участвует и где это поле используется.

## Проектирование
Иногда что-то хочется сразу сделать, но иногда это приводит к тому, что мы что-то не учли и приходится все переделывать. Тут проектирование (абстракция) нам помогает.
```ts
abstract class Car {
    calculate_rotation(): Vec3;
    calculate_move(): Vec3;
    move(rotation: Vec3, move: Vec3);
    update() {
        move(calculate_rotation(), calculate_move())
    }
}
```
Мы делаем скелет, вместо того чтобы писать сразу. И потратив время казалось бы впустую, мы на самом деле можем сэкономить много времени.

# Code style
Стиль кода - это как ты пишешь свой код. Какой отступ перед инструкцией, ставить ли точку с запятой в конце, и т.д. Обычно каждый пишет по разному, и поэтому если ты работаешь в команде то это может быть проблемой, так как на чтение кода будет тратиться куда больше времени, когда на это, и так уходит много времени. Эффективность сильно зависит от чтения кода. Поэтому важно настроить линтер чтобы он сразу стандартизировал стиль кода между программистами. Линтер будет автоматически подсказывать как исправить свой код. Линтер не исправляет прям все все, важно так же самому следить и писать код для людей, а не для машин.
