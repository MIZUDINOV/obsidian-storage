#### Сигналы (Signals):

Сигналы - это новая возможность для оборачивания простых значений или сложных структур для, более, детального отслеживания изменений и последующего рендеринга фреймворком.

Сигналы есть двух видов: записываемые (writable signal) и сигналы только для чтения (readonly signals).


###### Записываемые сигналы:

Давайте создадим сигнал
```typescript
const count = signal(0);

// и здесь мы можем вызвать наш сигнал для получения его значения
console.log('The count is: ' + count());
```

Чтобы изменить значение в сигнале мы можем либо вызвать метод `.set()`
```typescript
count.set(1)
```

либо можем вызвать метод `.update()` для того, чтобы получить предыдущее значение и обновить его
```typescript
count.update((prevValue: number) => prevValue + 1)
```

Если у нас есть какой-нибудь объект, например массив, и мы не хотим заменить его на новый, а всего лишь обновить его внутренне свойство, то мы можем использовать метод `.mutate()`
```typescript
const todos = signal([{titile: 'coock', done: false}])

todos.mutate((prevValue) => {
	prevValue[0].done = true
})
```

Записываемые сигналы имеют тип [`WritableSignal<T>`](https://angular.io/api/core/WritableSignal), а сигналы только для чтения имеют тип [`Signal<T>`](https://angular.io/api/core/Signal) и он не может быть изменён или перезаписан новым значением.


###### Вычисляемые сигналы:

Вычисляемые сигналы получают данные на основе других сигналов
```typescript
const count: WritableSignal<number> = signal(0)

const doubleCount: Signal<numbdr> = computed((value) => count() * 2)
```

Здесь `doubleCount` вычисляется всякий раз, когда обновляется `count`.
Вычисляемые сигналы также имеют тип [`Signal<T>`](https://angular.io/api/core/Signal)

Вычисляемые сигналы ленивые и кэшируемые.

Связанные темы:
[[Angular]]