# Handbook

## 1. Общее
#### 1.1. Чтобы смерджить pull request надо набрать минимум 2 святых и 3 обычных апрува.
#### 1.2. Для проекта GlassesUSA используем только цвета из стайл гайда в avocode и создаем переменные места использования.
#### 1.3. Используем idx для обращения к свойствам с глубокой вложенность. Удаляем после Babel 7.
#### 1.4. Создавая новый контейнер, стараемся дробить на реиспользуемые компоненты (кладем их в __helpers) компоненты которые пренадлежат только этому контейнеру кладем в папку с его именем в components (плоско, без вложенностей). Пример:
```
components/
  product/
    productItem/
    productList/
    productListTitle/
  category/
    categoryItem/
    categoryTitle/
containers/
    product/
    category/
```
#### 1.5. Истиной в код стайле является Prettier.
#### 1.6. Название файлов пишем через camelCase.
	
## 2. React
#### 2.1. Используем stateless компоненты когда надо и аккуратно.
#### 2.2. Не используем default экспорт в компонентах.

## 3. Redux
#### 3.1. В законнекченных компонентах используем mapStateToProps и mapDispatchToProps.
Если вы прокидываете dispatch внутрь компонанта только для того чтобы обернуть в него actionCreator, можно использовать bindActionCreators.

**Пример:**
```javascript
// actions.js
export const removeTodo = (id) => ({
    type: 'REMOVE_TODO',
    payload: id,
});

// TodoItem.jsx
import { connect } from 'react-redux';
import { bindActionCreators } from 'redux';
import { removeTodo } from './actions'

const TodoItem = ({remove, id, text}) => (
    <div>
        <p>{text}</p>
        <button onClick={() => { remove(id)} }>remove</button>
    </div>
);
const mapDispatchToProps = dispatch => 
    bindActionCreators({ remove: removeTodo }, dispatch);
    
export const TodoItemConnected = connect(null, mapDispatchToProps)(TodoItem);
```

#### 3.2. Рядом с редьюсерами создаем файл с типом.
#### 3.3. Рядом с редьюсерами создаем файл с фикстурой.
#### 3.4. Создавать InitialState в соответствии Flow type редюсера.
#### 3.5. Не сохранять полностью весь payload, а выбирать необходимые данные.
#### 3.6. Для connected компонентов используем имя ComponentNameConnected.
#### 3.7. Для типов экшенов создаем константы.
```javascript
export const PRODUCT = {
    GET_PRODUCT: 'PRODUCT@GET_PRODUCT'
}
```

## 4. Тесты
#### 4.1. Каждый новый компонент должен быть покрыт тестами на 100%.
#### 4.2. Больше не используем nock, sinon для тестов.
#### 4.3. Запрещается понижать проценты в coverageThreshold.

## 5. Селекторы
#### 5.1. В селекторах созданых с помощью createSelector, мы тестируем только результирующую функцию(последний аргумент). Для этого используем метод "resultFunc()" у созданного селектора. Пример:
```javascript
test('getCartItemOptions', () => {
  const quoteItem = {test: '100500'};
  const result = cartSelectors.getCartItemOptions.resultFunc(quoteItem);
  expect(result).toEqual(CustomOptionsFixture);
});
```
#### 5.2. Осторожно использовать selectors with props и внимательно читать документацию.

## 6. Flow
#### 6.1. Имена для типов даем через CamelCase + Type. Пример: CriteoEventType.
#### 6.2. Для глобального State в компонентах, селекторах и т.д. исползуем глобальный тип ApplicationStoreType.

## 7. Стили
#### 7.1. Используем react-responsive в связке с custom-media postCSS плагином для адаптивного дизайна.
#### 7.2. В стилях используем наши переменные: цвет, начертание шрифта (наши цифровые значения - 400, 700 и т.д.), название шрифта и т.д. Переменные для font-style удаляем (normal, italic).

## 8. Code review
#### 8.1. Уважай своих коллег. Не капси, не тролль, не используй супер огромный шрифт, не используй картинки. Ты оцениваешь код, а не человека.

Плохой пример:
Почему ты обозвал переменную myVar??? Мы же уже обсуждали это на прошлой неделе! Это ни в какие ворота!

Так лучше:
Почему переменная названа myVar, а не myAwesomeVar? Название myAwesomeVar подошло бы лучше, т.к. оно лучше отражает то, для чего заведена переменная. Предлагаю записать это правило в документацию.

#### 8.2. Замечания должны быть конструктивными.
Замечания типа “бред”, “трэш”, “плохо” — это не наша история. Считаешь, что плохо — объясни, что именно плохо и предложи вариант, как сделать лучше. Замечания всегда должны быть конструктивными.

#### 8.3. Не закрывай не глядя.
Ревью — это не формальная процедура, не еще одна задача, чтобы загрузить и так загруженного тебя. Это общий проект, и вам всем с этим кодом жить. Так что не ленись посмотреть внимательно.
Не дожидайся, когда коллега все исправит, чтобы задать новые вопросы. По возможности постарайся провести ревью сразу целиком.

#### 8.4. Пиши в описании, что сделано и зачем
Во-первых, то, что в итоге сделано, далеко не совпадает с первоначальной постановкой задачи (увы). Во-вторых, гораздо удобнее прочитать описание в ревью, чем искать задачу и вычитывать в ней.

#### 8.5. Отмечай уже решенные вопросы
Это нужно, чтобы быстрее ориентироваться в море комментариев. Многие системы позволяют пометить вопрос как resolved. Если нет — хотя бы условьтесь оставлять под комментарием слово “done”.

#### 8.6. Отвечай конструктивно
Самый плохой ответ на комментарий вот такой:

— Не лучше было бы сделать вот так: /*Пример решения*/? Это позволило бы решить проблему А и проблему Б.

— Нет

#### 8.7. Отвечай на вопросы
Если менее опытный коллега задал тебе вопрос, не надо отправлять его читать матчасть. Почему-то нам свойственно забывать, что мы тоже когда-то учились.

#### 8.8. Комментарии не соблюдающий выше изложенные правила может игнорироваться или даже удаляться. Вплоть до игнорирования needs work от этого человека.

## 9. Имена методов (функций)
Называть вещи своими именами, чтобы было понятно что происходит здесь и сейчас без ссылок на другие файлы, поиск вхождений и констант - пусть они останутся исключительно для помощи написания, а не чтения. **Не называть** функцию `setProduct` для получения данных с сервера, не сокращать до однобуквенных значений, не расписывать больше необходимого (`getWindowProductColorsSelector` → `getProductColors`).

> * Имена методов, представляют собой глаголы или глагольные словосочетания: fetchProduct, getProduct, setProduct и т.д.
> * Используйте имена из пространства задачи.
> * Используйте имена из пространства решения. Не стесняйтесь использовать термины из области информатики, названия алгоритмов и паттернов.
> * Описывайте все, что метод выполняет. Но без фанатизма: getWindowProductColorsSelector → getProductColors или getColors.

*Clean Code: A Handbook of Agile Software Craftsmanship. Robert Martin*

#### 9.1 Actions

* `fetchSomething` - fetch **remote** data (`fetchQuote()`), not **get**.
* `updateSomething` - update **remote** data (`updateCartItem(productId, cartItem)`).
* `removeSomething` - remove **remote** data (`removeCartItem(productId)`).
* `addSomething` - add/create something (`addToCart(productId)`).
* `setSomething` - update **local** store item to new value (`setPageNumber(page)`).
* `clearSomething` - clear (delete) **local** store item (`clearCategoryItems()`).

#### 9.2 Selectors

* `getSomething` - get from **local** store (`getQuote(store)`).

#### 9.3 Predictions

* `isSomethingActive` - checking the state of the local item.
* `hasSomething` - checking the existing of the local item.

---
## Разное
 - Телеграм канал с итогами после митапов https://t.me/joinchat/AAAAAEoHz9mmtzlylBkIIg
 - Телеграм канал с полезными ссылками https://t.me/joinchat/AAAAAEoHz9mmtzlylBkIIg
