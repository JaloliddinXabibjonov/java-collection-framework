
![[java 21 collection fr full.png]]

<br>

Javada Collection framework 1.2 versiyadan boshlab  qo'shilgan. U bir nechta class va interfacelardan tashkil topgan. Javada Collection va Map interfacelari collection classlarini 2 ta asosiy interfacelaridir. 

Collection frameworkning afzalliklari:

- Consistent API - List, Set, Map kabi interfacelarning metodlari classlar bo'ylab mavjudligi.
- Developerlar data structureni design qilmasdan faqat foydalanishga e'tibor berishi
- Yaxshiroq performance - data structureni tez va ishonchli amalga oshirish, kod sifati va tezligini taklif qiladi.

#### Collection asosiy tarkibi va iyerarxiyasi
#### **1. Iterable**
`Iterable` interface - collection frameworkini asosini tashkil qiladi.Undan Collection interface `extend` qilgan, bu esa `Collection` ning barcha `implementation`larini `iterable` qiladi. `Iterable`da bitta `iterator()` metodi bor, bu esa elementlarni aylanib chiqishni taminlaydi. 
   
```java
   Iterator<T> iterator();
```
#### **2. Collection**
 `Collection` interface collection frameworkining asosini tashkil qiladi. U `Iterable` interfacedan extend olgan. U `add()`, `remove()`, `clear()` kabi common metodlarni o'zida jamlagan, bu esa barcha collectionlar implementationlar bo'ylab mustahkamlik va qayta foydalanishni ta'minlaydi.

```java
public interface Collection<E> extends Iterable<E> {}
```

#### **3. SequencedCollection**
SequencedCollection bu Collection interfacedan implement olgan interface. U collection elementlarini ikkala uchidagi elementlar bilan ishlash uchun bir nechta metodlarni taqdim qiladi:
  - `getFirst()`, `getLast()` - birinchi va oxirgi elementlarga access
  - `removeFirst()`, `removeLast()` - birinchi va oxirgi elementlarni o'chirish
  - `addFirst(E e)`, `addLast(E e)` - ro'yxatning boshiga va oxiriga element qo'shish
  - `reversed` - collectionning teskari tartibda olish uchun

```java
public interface SequencedCollection<E> extends Collection<E> {}
```
#### **4. List**
 `List` interface `Collection` ni extend qilgan hamda duplicate elementlarga ruxsat bergan tartibli collectionni taqdim qilgan. 
   Xususiyatlari:
   -  Qo'shish tartibida saqlanadi
   - `duplicate` elementlarga ruxsat bor
   - `null` elementlarga ruxsat bor(implementatsiyasiga qarab)
   - index bo'yicha amal bajarish mumkin

java 21 gacha:
```java
public interface List<E> extends Collection<E> {}
```

java 21 dan boshlab:
```java
public interface List<E> extends SequencedCollection<E> {}
```

##### **4.1. ArrayList**
`Arraylist` bu `List` dan implement olgan class bo'lib, elementlarni arrayda saqlaydi. U resizable. Oddiy arraydan farqli ravishda uni sizeni oldindan belgilash shart emas. U elementlar qo'shilishi yoki o'chirilishiga qarab dynamic ravishda o'zgaradi. `iterator()` hamda `listIterator()` metodlari qaytargan iteratorlar `fail-fast`, yani `iterator()` hamda `listIterator()` dan foydalanib iterator yaratilgandan keyin, list ustida iteratorning `remove` va `add` metodlaridan tashqari metodlar yordamida list o'zgartiriladigan bo'lsa, iterator `ConcurrentModificationException` tashlaydi.
```java
public class ArrayList<E> extends AbstractList<E> implements List<E>, RandomAccess, Cloneable, java.io.Serializable {}
```

*Xususiyatlari:*
  - `resizable`
  - index mavjud, index bo'yicha murojaat qilish mumkin.
  - genericni support qiladi.
  - `synchronized` emas, `synchronized` qilish uchun ushbu usuldan foydalanish kk: ```
 ```java
 List list = Collections.synchronizedList(new ArrayList(...));
 ```
  - `null` va `duplicate` larga ruxsat beriladi.
  - elementlar qo'shish tartibida saqlanadi.

*Konstruktorlari:*
  - `new ArrayList<>()` - bo'sh ro'yxat yaratish uchun ishlatiladi
  - `new ArrayList<>(Collection c)` - berilgan collection elementlaridan iborat ro'yxat yaratish    uchun ishlatiladi
  - `new ArrayList<>(int initialCapacity)` - berilgan sizdagi array yaratish uchun ishlatiladi.

##### **4.2. LinkedList**
`LinkedList` bu `List` va `Deque` interfacelardan implement olgan class bo'lib, u o'zida `doubly-linked list` ni amalga oshiradi. Ya'ni har bir node 2 qismdan iborat bo'ladi: element va keyingi(va oldingi) elementga reference. `iterator()` hamda `listIterator()` metodlari qaytargan iteratorlar `fail-fast`, yani `iterator()` hamda `listIterator()` dan foydalanib iterator yaratilgandan keyin, list ustida iteratorning `remove` va `add` metodlaridan tashqari metodlar yordamida list o'zgartiriladigan bo'lsa, iterator `ConcurrentModificationException` tashlaydi.

*Xususiyatlari:*
  - `null` elementga ruxsat bor.
  - `resizable`
  - `synchronized` emas, `synchronized` qilish uchun ushbu usuldan foydalanish kk: ```
 ```java
 List list = Collections.synchronizedList(new LinkedList(...));
 ```

*Afzallik tomonlari:*
  - size dynamic ravishda o'sishi yoki kamayishi mumkin
  - o'rtadagi elementlarni o'chirish yoki qo'shish samarali, chunki faqat reference yangilanadi xolos
  - Doubly linked list oldinga va orqaga harakatlanish imkonini beradi
*Kamchiliklari:*
  - Elementga murojaat qilih sekin, chunki elementni topish uchun ro'yxatni boshidan boshlab aylanib chiqish kk
  - Har bir `node` referencelar uchun qo'shimcha xotira saqlaydi, bu esa ko'proq xotiradan foydalanishga olib keladi.
*Konstruktorlari:*
  - `new LinkedList<>()` - bo'sh ro'yxat yaratish uchun ishlatiladi
  - `new LinkedList<>(Collection c)` - berilgan collectionning iteratori qaytargan ro'yxat asosida tartibli ro'yxat yaratish uchun ishlatiladi

<br>
<br>

*`ArrayList` va` LinkedList`dagi amallar samaradorligi:*

| Amaliyot                        | ArrayList         | LinkedList                                    |
| ------------------------------- | ----------------- | --------------------------------------------- |
| Elementga murojaat qilish       | O(1) juda tez     | O(n)                                          |
| Boshiga qo'shish                | O(n)              | O(1)                                          |
| O'rtasiga qo'shish va o'chirish | O(n)              | O(1) agar `node` topilgan bo'lsa, topish O(n) |
| Oxiriga element qo'shish        | O(1)(samaraliroq) | O(1)                                          |
| Qidirish                        | O(n)              | O(n)                                          |
| Xotira sarfi                    | kam               | ko'p                                          |


##### **4.3. Vector**
Vector bu `List`dan implement olgan resizable array bo'lib, `ArrayList` kabi ishlaydi, faqat u `synchronized` hisoblanadi. Shuning uchun u `ArrayList`dan sekinroq. `iterator()` hamda `listIterator()` metodlari qaytargan iteratorlar `fail-fast`, yani `iterator()` hamda `listIterator()` dan foydalanib iterator yaratilgandan keyin, list ustida iteratorning `remove` va `add` metodlaridan tashqari metodlar yordamida list o'zgartiriladigan bo'lsa, iterator `ConcurrentModificationException` tashlaydi. `elements()` metodi orqali yaratilgan `java.util.Enumeration fail-fast` emas. `default` konstruktor yordamida yaratilganda `default capacity` ga teng bo'ladi. elementlar soni `current capacity`dan oshib ketsa, Vector `capacity`ni ushbu formula asosida oshiradi: 

> `newCapacity = oldCapacity * 2`

*Xususiyatlari:*
  - `resizable`
  - `synchronized`
  - elementlarni `ArrayList` kabi qo'shish tartibida saqlaydi
  - `null` va `duplicate` elementlar uchun ruxsat berilgan

*Afzalliklari:*
  - `synchronized`ligi uchun multi-threadingda foydalanish samarali
  - `resizable` ekanligi

*Kamchiliklari:*
  - `synchronized` bo'lganligi uchun `ArrayList`ga nisbatan sekin
  - `synchrozination` kerak bo'lmagan joyda ishlatish yuklamani orttiradi

*Konstruktorlari:*
  - `new Vector<E>()` - sig'imi 10 ta bo'lgan bo'sh vector yaratish uchun
  - `new Vector<E>(int size)` - berilgan sizega teng bo'lgan vector yaratish uchun
  - `new Vector<E>(int size, int incr)` - berilgan `size`ga teng bo'lgan va `capacity`ni oshirish kerak bo'lgan qiymat bilan vector yaratish uchun
  - `new Vector<E>(Collection c)` - berilgan collection asosida vector yaratish uchun
  
```java
public class Vector<E> extends AbstractList<E> implements List<E>, RandomAccess, Cloneable, java.io.Serializable {}
```

##### **4.4. Stack** 
Stack bu `Vector` classdan extend olgan va `LIFO(last-in-first-out)` prinsipida ishlovchi chiziqli malumot strukturasi. Agar `synchrozation` kerak bo'lmasa, `ArrayDeque` dan `Stack` amaliyotlarini bajarish ancha samarali va `null` elementga ruxsat etilmaydi:
```java
Deque<Integer> stack = new ArrayDeque<>();
stack.push(1);         // tepaga qo'shish
stack.push(2);
int top = stack.peek(); // 2
int popped = stack.pop(); // 2
```

*Xususiyatlari:*
  - kiritish tartibida saqlanadi
  - `null` va `duplicate` elementlarga ruxsat bor
  - `resizable`
  - `synchronized`

```java
public class Stack<E> extends Vector<E> {}
```
#### **5. Set**
`Set` interface faqat unique elementlarni salqaydigan tartibsiz collectionni taklif qiladi. 
 Xususiyatlari:
  - Bitta `null` elementga ruxsat bor
  - Tartib yo'q
  - `duplicate` elementlarni saqlamaydi

```java
public interface Set<E> extends Collection<E> {}
```
##### **5.1. SortedSet**

`SortedSet` - Set interfacedan extend olgan(java 21dan keyin `SequencedSet` dan ham) bo'lib, u o'zida tartiblangan elementlarni saqlash uchun mo'ljallangan.

Xususiyatlari: 
- Elementlar `default` holatda o'sib borish tartibida saqlanadi. `Comparator` dan foydalangan holda custom tartibga ruxsat etilgan.
- `duplicate` elementlar ruxsat etilmaydi.
- `null` elementlarga ruxsat etilmaydi, ayniqsa tabiiy tartib yoki custom comparator ishlatilgan         bo'lsa.
- `thread-safe` emas. `thread safe` uchun `ConcurrentSkipListSet` ni ishlatish kk

 Foydalanish joylari:
   - Eng kichik yoki eng katta qiymatlarni topishda
   - Malum intervaldagi qiymatlarni olishda
   - Natural yoki maxsus tartibda unique elementlarni saqlashda
##### **5.2. NavigableSet** 

NavigableSet - `SortedSet` dan extend olgan va o'zining bir nechta metolarini qo'shgan. U `SortedSet`dan farqli ravishda ascending va `descending iterator` tartib bo'ylab yurish imkoniyati bor. U bizga berilgan elementga yaqin qiymatlarni olishda qo'l keladi. 
   
*Metodlari:*
   - `lower(E e)` - berilgan elementdan kichigini qaytaradi, yo'q bo'lsa `null` qaytaradi
   - `higher(E e)` - berilgan elementdan kattasini qaytaradi, yo'q bo'lsa `null` qaytaradi
   - `floor(E e)` - berilgan elementdan kichik yoki teng bo'lganini qaytaradi, yo'q bo'lsa `null` qaytaradi
   - `ceiling(E e)` - berilgan elementdan katta yoki teng bo'lganini qaytaradi, yo'q bo'lsa `null` qaytaradi
   - `pollFirst()` - to'plamdagi eng kichik elementni o'chiradi va qaytaradi, agar set bo'sh bo'lsa `null` qaytaradi
   - `pollLast()` -  to'plamdagi eng katta elementni o'chiradi va qaytaradi, agar set bo'sh bo'lsa `null` qaytaradi

*`SortedSet` va `NavigableSet` farqlari:*
   - `SortedSet` - faqat asosiy tartibni saqlashni ta'minlaydi
   - `NavigableSet` esa qo'shimcha metodlarga ega (`lower, floor, ceiling, higher`), ya'ni qidiruv osonroq
<br>
<br>
   
##### **5.3. TreeSet**

`TreeSet` - bu `TreeMap` ga asoslangan `NavigableSet` ning implementatsiyasi. U elementlarni saqlashda tartibni natural yoki uni yaratayotganda constructorga berilgan Comparator orqali ta'minlaydi. Setda elementlar `equals()` metodi orqali `unique` ligi aniqlanadi, lekin TreeSetda elementlarni taqqoslashda `Comparable` interfaceni `compareTo(T o)` yoki `Comparator` ning `compare(T o1, T o2)` metodi ishlatiladi. Agar `iterator` yaratilgandan keyin, set dagi malumotlar o'zgartirilishi `Iterator` ning `remove()` metodidan boshqa  yo'llar bilan bajarilsa, iterator `ConcurrentModificationException` tashlaydi.
```java
public class TreeSet<E> extends AbstractSet<E>  implements NavigableSet<E>,                               Cloneable, java.io.Serializable
```

Xususiyatlari:
  -  `duplicate` elementlarga ruxsat yo'q, `ignore` qilinadi
  - `null` valuelarga ruxsat yo'q, `null` qiymat qo'shishga harakat qilinganda `NullPointerException` tashlanadi(JDK 7dan boshlab).
  - `Thread safe` emas, concurrent access berish uchun u `Collections.synchronizedSet()` dan foydalanish orqali `synchronized` qilinishi kerak.
##### **5.4.  HashSet** 

HashSet - bu `Hashmap` ga asoslangan `Set` interfacening implementatsiyasi.  U `unique` elementlarni saqlashga mo'ljallangan. `iterator()` metodi qaytargan iterator `fail-fast`, yani `iterator()` hamda `listIterator()` dan foydalanib iterator yaratilgandan keyin, list ustida iteratorning `remove` va `add` metodlaridan tashqari metodlar yordamida list o'zgartiriladigan bo'lsa, iterator `ConcurrentModificationException` tashlaydi.

```java
 public class HashSet<E> extends AbstractSet<E> implements Set<E>, Cloneable, Serializable
 ```

Xususiyatlari:
 - U  orderga amal qilmaydi, lekin null elementga ruxsat bor. 
 - `Clonable` va `Serializable` interfacelardan implement olgan. 
 - `Thread safe` emas, thread safe qilish uchun uni tashqaridan sinxronlashtirish kerak:
```java
   Set s = Collections.synchronizedSet(new HashSet(...));
   ```
 - Elementni saqlashdan oldin elementning `unique` ligini aniqlash uchun u `hashcode` and `equals` metodlaridan foydalanadi.
 - default sig'imi 16 , load factor esa 0.75 ga teng. Elementlar soni chegaraga(`capacity * load factor`)  yetganda esa u old `newCapacity = oldCapacity * 2`   kabi capacityni oshiradi.

Konstruktorlari: 
   - `HashSet()`
   - `HashSet(Collection<? extends E> c)`
   - `HashSet(int initialCapacity)`
   - `HashSet(int initialCapacity, float loadFactor)`

`HashSet` va `HashMap` ning farqlari:

| Asos                                     | HashSet                                                                                                                     | HashMap                                                          |
| ---------------------------------------- | --------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------- |
| `implementation`                         | `Set` dan implement qilgan                                                                                                  | `Map` dan implement qilgan                                       |
| `duplicates`                             | ruxsat etilmaydi                                                                                                            | `key-value` juftligida saqlanadi, duplicate keylarga ruxsat yo'q |
| obyektlarni saqlashda obyektlarning soni | 1 ta obyekt                                                                                                                 | 2 ta obyekt(`key, value`)                                        |
| `dummy value`                            | Elementlarni saqlashda HashMapdan foydalanadi, har bir element key sifatida saqlanadi, valuega esa `new Obejct()` beriladi. | `dummy value` saqlamaydi                                         |
| Elementlarni saqlash mexanizmi           | HashMapdan foydalanadi                                                                                                      | Elementlarni saqlash uchun hashingdan foydalanadi                |
| Tezligi                                  | `HashMap`dan ko'ra sekinroq                                                                                                 | `HashSet`dan tezroq                                              |

##### **5.5 LinkedHashSet**
`LinkedHashSet` - `HashSet` dan (java 21 dan boshlab `SequencedSet` dan ham) implement qilgan class.  Elementlar qo'shilish tartibi bo'yicha saqlanadi. Double linked listdan foydalanadi. 

```java
public class LinkedHashSet<E>  extends HashSet<E> implements SequencedSet<E>, Cloneable, java.io.Serializable
```

Xususiyatlari:
  - Faqat `unique` elementlarni saqlaydi
  - insertion orderga amal qiladi
  - HashSetga qaraganda iteration qilish tezroq
  - `null` elementlarga ruxsat bor

##### **5.6 SequencedSet**
`SequencedSet` bu `SequencedCollection` va `Set`dan extend olgan interface bo'lib, java 21 da qo'shilgan. U `SequencedCollection`ni `reversed()` metodini override qilgan. Farqli tomoni return type `SequencedSet:`
```java
SequencedSet<E> reversed();
```

```java
public interface SequencedSet<E> extends SequencedCollection<E>, Set<E> {}
```

#### **6. Queue**
`Queue` interface FIFO(First-in, First-out) prinsipiga amal qiladi. Elementlar qo'shilgan tartibda process bajariladi.
   Xususiyatlari:
   - FIFO order
   - Random access yo'q
   - Metodlarining 2 xil to'plami:
   - Exception tashlaydigan(`add, remove, element`)
   - Exception tashlamaydigan(`offer, poll, peek`)

```java
public interface Queue<E> extends Collection<E> {}
```
##### **6.1 Deque**
Deque interface "Double Ended Queue" (ikki tomonlama navbat) ning qisqartmasi bo'lib, Java Collections Framework ning bir qismidir. U `Queue` dan extend olgan.  `List` interfacedan farqli ravishda `Deque` da elementlarga index bo'yicha murojaat qilib bo'lmaydi. `null` elementlarga ruxsat beruvchi uning implementatsiyalarida `null` insert qilish tavsiya etilmaydi. Chunki turli metodlar tomonidan qaytarilgan `null` deque empty ekanligini ko'rsatadi.  U elementlarni navbatning ikkala uchidan ham qo'shish va olib tashlash uchun metodlarni taqdim etadi. Bu metodlar ikki qismga bo'linadi:
  - operatsiya muvaffaqiyatsiz tugasa exception tashlaydigan
  - operatsiya muvaffaqiyatsiz tugasa false yoki null qaytaradigan

<table>
  <tr>
    <th rowspan="2">Operatsiya</th>
    <th colspan="2">1-element</th>
    <th colspan="2">oxirgi element</th>
  </tr>
  <tr>
    <th>throw tashlaydigan</th>
    <th>qiymat qaytaradigan</th>
    <th>throw tashlaydigan</th>
    <th>qiymat qaytaradigan</th>
  </tr>
  <tr>
    <td><code>insert</code></td>
    <td><code>addFirst(E e)</code></td>
    <td><code>offerFirst(E e)</code></td>
    <td><code>addLast(E e)</code></td>
    <td><code>offerLast(E e)</code></td>
  </tr>
  <tr>
    <td><code>remove</code></td>
    <td><code>removeFirst()</code></td>
    <td><code>pollFirst()</code></td>
    <td><code>removeLast()</code></td>
    <td><code>pollLast()</code></td>
  </tr>
  <tr>
    <td><code>get</code></td>
    <td><code>getFirst()</code></td>
    <td><code>peekFirst()</code></td>
    <td><code>getLast()</code></td>
    <td><code>peekLast()</code></td>
  </tr>
</table>


`Queue` va `Deque` metodlarining farqlari

<table>
  <tr>
    <th>Queue metodi</th>
    <th>Dequening ekvivalent metodi</th>
  </tr>
  <tr>
    <td><code>add(e)</code></td>
    <td><code>addLast(e)</code></td>
  </tr>
  <tr>
    <td><code>offer(e)</code></td>
    <td><code>offerLast(e)</code></td>
  </tr>
  <tr>
    <td><code>remove()</code></td>
    <td><code>removeFirst()</code></td>
  </tr>
  <tr>
    <td><code>poll()</code></td>
    <td><code>pollFirst()</code></td>
  </tr>
  <tr>
    <td><code>element()</code></td>
    <td><code>getFirst()</code></td>
  </tr>
  <tr>
    <td><code>peek()</code></td>
    <td><code>peekFirst()</code></td>
  </tr>
</table>

Xususiyatlari:
  - elementlarni navbatning boshi va oxiridan qo'shish, o'chirish va ko'rish mumkin
  - u ham `Stack`(LIFO) ham Queue(FIFO) sifatida ishlashi mumkin.
  - hajm jihatdan cheklovlar yo'q
  - `synchronized` emas, sinxronizatsiya qilish uchu

##### **6.2 ArrayDeque**
`ArrayDeque` `Deque` dan implement olgan resizable array. U elementlarni navbatning ikkala uchidan samarali ravishda o'chirish va qo'shishga ruxsat beradi. Ushbu classni stack sifatida ishlatilganda `Stack`dan tezroq va queue sifatida ishlatilganda `LinkedList`dan tezroq. Classning `iterator` metodi tomonidan qaytarilgan iterator `fail-fast`, yani iterator yaratilgandan keyin iteratorning `remove` metodidan boshqa yo'llar bilan deque modify qilinsa `ConcurrentModificationException` tashlaydi.   U LIFO va FIFO sifatida ishlatilishi mumkin.

Xususiyatlari:
  - `dynamic` ravishda o'sadi
  - `addFirst`, `addLast()`, `removeFirst()` va `removeLast()` kabi metodlar `O(1)` vaqtda bajariladi
  - `thread-safe` emas
  - `null` elementlarga ruxsat yo'q
#### **7. Map** 
`Map` interface `key-value` juftligini to'plamini saqlaydi:
 - `key` `unique` bo'lishi kerak
 - 1 `key`ga 1 `value`
 - `value` duplicate bo'lishi mumkin
 - bitta `null` value ga `HashMap` va `LinkedHashMap` kabi implementatsiyalarda rusxat etiladi. Ko'p implementatsiyalarda bit nechta `null` valuelarga ruxsat etiladi.
 -  Thread safe amaliyotlarda ConcurrentHashMap dan foydalanish mumkin.
##### **7.1 SequencedMap**
SequencedMap - java 21 da bu interface qo'shilgan bo'lib, u xotirada `key-value` juftligini ma'lum bir tartibda saqlashni ta'minlaydi. Map interfacedan extend olgan. Qo'shimcha tarzda elementlar tartibi bilan ishlash imkonini beradi.

*Xususiyatlari*:
  - birinchi yoki oxirgi elementni olish
  - elementlarni tartib bilan ko'rib chiqish
  - boshi yoki oxiriga element qo'shish yoki olib tashlash

*Metodlari*:

  - `putFirst(K e, V v)`  - elementni boshiga qo'shadi

  - `putLast(K k, V v)` - elementni oxiriga qo'shadi

  - `firstEntry()` - birinchi `Map.Entry<K, V` ni qaytradi

  - `lastEntry()` - oxirgi `Map.Entry<K, V` ni qaytaradi

  - `pollFirstEntry()` - birinchi elementni olib tashlab qaytaradi

  - `pollLastEntry()` - oxirgi elementni olib tashlab qaytaradi

  - `reversed()` - teskari tartibda yangi `SequencedMap` yaratadi

*Implementatsiyalari:*

  - *interfacelar:*
    - `ConcurrentNavigableMap<K, V>`
    - `NavigableMap<K, V`
    - `SortedMap<K, V`

  - *classlar*:
    - `ConcurrentSkipListMap`
    - `LinkedHashMap`
    - `TreeMap`

##### **7.2 HashMap**
`HashMap` - bu hash tablega asoslangan `Map` interfacening implementatsiyasi. Bu class barcha kerakli map operatsiyalarni ta'minlaydi. `null` key va `null` valuelarga ruxsat beradi. `Hashtable` ga ekvivalent bo'lib, undan farqli ravishda unsynchronized va `null` valuelarga ruxsat beradi. Saqlaganda tartibga amal qilmaydi.

```java
public class HashMap<K,V> extends AbstractMap<K,V> implements Map<K,V>, Cloneable, Serializable
```

*Xususiyatlari:*

  - bitta `null` keyga ruxsat
  - ko'plab `null` valuelarga ruxsat
  - `key` unique bo'lishi kerak
  - `O(1)` tezlikda qo'shish/qidirish/o'chirish mumkin

*Ishlash prinsipi:*

  - `HashMap` har bir `key` ning `hashcode()` metodini chaqiradi
  - ushbu hashcode integer qiymatiga o'giriladi`
  - keyin bu qiymat `index(bucket)` ga aylantiriladi: odatda: `hash % table.lenth`

##### **7.3 LinkedHashMap**
`LinkedHashMap` - bu `HashMap` dan extend olgan (java 21 dan boshlab `SequencedMap` ni ham implement qilgan.). `key-value` juftligini kiritish tartibida saqlaydi.
```java
public class LinkedHashMap<K,V> extends HashMap<K,V> implements SequencedMap<K,V>
```

**Xususiyatlari**:
  - unique `key-value` juftligini saqlaydi
  - kiritish tartibi saqlanadi
  - bitta `null` keyga va bir nechta `null` valuelarga ruxsat bor
  - `doubly-linked list` implementatsiya bilan bir xil
  - `unsynchronized`, lekin uni ushbu usulda synchronized qilish mumkin:
 
```java
Map<K, V> synchronizedMap = Collections.synchronizedMap(new LinkedHashMap<>());
```

**Kamchiliklari**:
  - xotiradan ko'p foydalanish
  - kiritish sekinroq
  - katta malumotlar uchun samarali emas


##### **7.4 SortedMap**
`SortedMap` interface collection frameworkning bir qismi hisoblanadi. U `Map` interfacedan(java 21 dan boshlab `SequencedMap` dan)  extend olgan. U keylari saralangan(sorted) holdan saqlanadi. Bu tartib keylarning natural orderi yoki u yaratilayotgan paytda berilgan `Comparator` orqali tartiblanadi. Bu tartib `entrySet()`, `keySet()` va `values()` metodlaridan foydalanib iteratsiya qilinganda aks etadi. `SortedSet` ning mapdagi analogi hisoblanadi. `SortedMap` ga `Comparable`dan implement olgan `key`lar insert qilinishi kerak yoki sorted map yaratilganda comparator parametr sifatida berilishi kerak. Aks holda `ClassCastException` tashlaydi.    

java 21 gacha:

```java
public interface SortedMap<K,V> extends Map<K,V> {}
```

java 21 dan boshlab:

```java
public interface SortedMap<K,V> extends SequencedMap<K,V> {}
```
Xususiyatlari:
  - `key-value` juftligini saqlaydi
  - `key`lar tartib bilan saqlanadi
  - `key` larning saralash uchun comparator berish mumkin
  - `null` key yoki `null` valuega ruxsat yo'q. `null` key yoki value insert qilinganda throw tashlaydi.


##### **7.5 NavigableMap**
`NavigableMap` interface bu collection frameworkning bir qismi bo'lib, u `SortedMap`dan extend olgan. U `key`lari tartiblangan elementlar bilan ishlash uchun qulay navigatsiya metodlarini taqdim etadi. 

```java
public interface NavigableMap<K,V> extends SortedMap<K,V> {}
```
Xususiyatlari:
  - `SortedMap`dan extend olganligi sababli, `NavigableMap`dagi elementlar doimo o'z keylarining tabiiy tartibi bo'yicha yoki map yaratish vaqtida berilgan `Comparator` orqali tartiblanadi.
  - Navigatsiya metodlari: `key`larga asoslangan holda eng yaqin mos keladigan elementlarni topish uchun maxsus metodlarni taqdim etadi.

Navigatsiya metodlari:
  - `Map.Entry<K, V> lowerEntry(K key)`: Berilgan `key`dan qat'iy kichik bo'lgan eng katta `key-value` juftligini qaytaradi.
  - `Map.Entry<K, V> floorEntry(K key)`: Berilgan `key`dan kichik yoki unga teng bo'lgan eng katta `key-value` juftligini qaytaradi.
  - `Map.Entry<K, V> ceilingEntry(K key)`: Berilgan `key`dan katta yoki unga teng bo'lgan eng kichik `key-value`juftligini qaytaradi.
  - `Map.Entry<K, V> higherEntry(K key)`: Berilgan keydan qat'iy katta bo'lgan eng kichik `key-value` juftligini qaytaradi.
  - `Map.Entry<K, V> firstEntry()`: Mapdagi eng kichik `key`ga ega juftlikni qaytaradi.
  - `Map.Entry<K, V> lastEntry()`: Mapdagi eng katta `key`ga ega juftlikni qaytaradi.
  - `Map.Entry<K, V> pollFirstEntry()`: Eng kichik `key`ga ega juftlikni olib tashlaydi va qaytaradi.
  - `Map.Entry<K, V> pollLastEntry()`: Eng katta `key`ga ega juftlikni olib tashlaydi va qaytaradi.

##### **7.6 TreeMap**
TreeMap bu collection frameworkning bir qismi bo'lib, `NavigableMap` dan implement olgan. `key-value` juftliklarini tartiblangan holda saqlaydi. Uning ishlashi o'z-o'zidan muvozanatlashadigan **Red-Black Tree** malumotlar tuzilmasiga asoslangan. U `key`lari saralangan(sorted) holda saqlanadi. Bu tartib keylarning natural orderi yoki u yaratilayotgan paytda berilgan `Comparator` orqali tartiblanadi. `TreeMap` ga `Comparable`dan implement olgan `key`lar insert qilinishi kerak yoki sorted map yaratilganda comparator parametr sifatida berilishi kerak. Aks holda `ClassCastException` tashlaydi. Classning 'collection view methods' tomonidan qaytarilgan collectionlarning `iterator()` metodi tomonidan qaytarilgan iteratorlar `fail-fast`, ya'ni iterator yaratilgandan keyin ushbu collection iteratorning `remove()` metodidan boshqa metodlar bilan  `modify` qilinsa `ConcurrentModificationException` tashlaydi. 

```java
public class TreeMap<K,V> extends AbstractMap<K,V>  implements 
						NavigableMap<K,V>, Cloneable, java.io.Serializable
```

Xususiyatlari:
  - `null` keyga ruxsat berilmaydi, chunki `key` bo'yicha tartiblanadi
  - `null` valuega ruxsat bor
  - `synchronized` emas, `synchronized` qilish uchun ushbu usuldan foydalanish kerak:

  ```java
SortedMap m = Collections.synchronizedSortedMap(new TreeMap(...));
```

Ishlash Samaradorligi (Time Complexity):

| *Operation*                  | *Time complexity* |
| ---------------------------- | ----------------- |
| `put()`, `get()`, `remove()` | `O(logn)`         |
| `containsKey()`              | `O(logn)`         |
| iteratsiya                   | `O(n)`            |

`HashMap` bilan farqlari:

| *Xususiyat*         | *HashMap*                | *TreeMap*                          |
| ------------------- | ------------------------ | ---------------------------------- |
| Tartib              | Yo'q                     | Kalitlar bo'yicha tartiblangan.    |
| Ichki Tuzilma       | Hash Table               | Red-Black Tree (Qizil-Qora daraxt) |
| **Ishlash Tezligi** | `O(1)` (o'rtacha)        | `O(logn)` (kafolatlangan)          |
| `null` key          | faqat bitta uchun ruxsat | ruxsat yo'q                        |



Ushbu malumotnoma Xabibjonov Jaloliddin tomonidan tayyorlandi.
==**Muallif ruxsatisiz tarqatmang.**==  

Telegram: @JaloliddinXabibjonov

Tel: +998 99 363 62 24