# Типаж Clone

При работе с ресурсами, стандартным поведением является передача их (ресурсов) в ходе выполнения или вызов функции. Однако, иногда нам нужно также объявить копию ресурса.

Типаж [`Clone`] помогает нам сделать именно это. Чаще всего, мы можем использовать метод `.clone()` объявленный типажом `Clone`.

```rust,editable
// Единичная структура без ресурсов
#[derive(Debug, Clone, Copy)]
struct Unit;

// Кортежная структура с ресурсами, которая реализует типаж `Clone`
#[derive(Clone, Debug)]
struct Pair(Box<i32>, Box<i32>);

fn main() {
    // Объявим экземпляр `Unit`
    let unit = Unit;
    // Скопируем `Unit`, который не имеет ресурсов для перемещения
    let copied_unit = unit;

    // Оба `Unit` могут быть использованы независимо
    println!("оригинал: {:?}", unit);
    println!("копия: {:?}", copied_unit);

    // Объявим экземпляр `Pair`
    let pair = Pair(Box::new(1), Box::new(2));
    println!("оригинал: {:?}", pair);

    // Скопируем `pair` в `moved_pair`, перенаправляя ресурсы
    let moved_pair = pair;
    println!("копия: {:?}", moved_pair);

    // Ошибка! `pair` потеряла свои ресурсы
    //println!("оригинал: {:?}", pair);
    // ЗАДАНИЕ ^ Попробуйте раскомментировать эту строку

    // Скопируем `moved_pair` в `cloned_pair` (включая ресурсы)
    let cloned_pair = moved_pair.clone();
    // Сбросим оригинальную пару используя std::mem::drop
    drop(moved_pair);

    // Ошибка! `moved_pair` была сброшена
    //println!("копия: {:?}", moved_pair);
    // ЗАДАНИЕ ^ Попробуйте раскомментировать эту строку

    // Полученный результат из .clone() все ещё можно использовать!
    println!("клон: {:?}", cloned_pair);
}
```


[`Clone`]: https://doc.rust-lang.org/std/clone/trait.Clone.html