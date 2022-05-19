# Хранение sensitive-информации в исходном коде приложения

<table class='noborder'>
    <colgroup>
      <col/>
      <col/>
    </colgroup>
    <tbody>
      <tr>
        <td rowspan="2"><img src="../../../img/defekt_kritichnyj.png"/></td>
        <td>Критичность:<strong> КРИТИЧНЫЙ</strong></td>
      </tr>
      <tr>
        <td>Способ обнаружения:<strong> DATA, FILES</strong></td>
      </tr>
    </tbody>
</table>

## Описание

Приложение хранит чувствительную информацию в исходном коде приложения. Очень часто ошибочно считается, что данные, которые зашиты в исходном коде приложений защищены и недоступны после компиляции и обфускации. Однако, в декомпилированном приложении все строковые ресурсы остаются в неизменном виде. Любая чувствительная информация, расположенная в исходном коде приложения, будет доступна злоумышленникам. Не рекомендуется хранить в исходном коде любую информацию, которая может помочь злоумышленнику, это касается как любых токенов, паролей, ключей шифрования, так и данных, которые используются для тестирования — адреса тестовых стендов, тестовые учетные данные и т. д. Такая информация раскрывает внутреннее устройство тестовых стендов и может быть использована в дальнейшем.

## Рекомендации

Если необходимо хранить конфиденциальную информацию, исходный код не самое лучшее место для этого. Оптимальным вариантом является получение такой информации с сервера и, при необходимости её хранения на устройстве, использование шифрования. Для обеспечения конфиденциальности данных Android оснащена множеством криптографических функций и методов, с помощью которых приложения Android могут безопасно осуществлять шифрование и дешифрование (для обеспечения конфиденциальности), а также аутентификацию сообщений (MAC) и цифровые подписи (для проверки целостности).

Для того, чтобы выбрать подходящий в заданных условиях метод шифрования и тип ключа, можно воспользоваться следующими схемами:

<figure markdown>
![](../../img/hranenie-ili-ispolzovanie-ranee-najdennoj-chuvstvitelnoj-informacii.png)
</figure>

**Шифрование/дешифрование c использованием библиотеки Pinkman**

В качестве примера, рассмотрим хранение PIN-кода пользователя с использованием библиотеки Pinkman. Данная библиотека демонстрирует один из примеров использования процедуры расширения ключа и использования механизма KeyStore для генерации и хранения ключа шифрования. Библиотека получает хэш из ПИН-кода пользователя с помощью хеш-функции Argon2 и сохраняет его в зашифрованном файле. Файл зашифрован алгоритмом AES-256 в режиме GCM, а ключи хранятся в AndroidKeystore. Эта библиотека не изобретает собственную криптографию, а использует проверенные временем алгоритмы.

Описание используемых технологий и их параметров.

**Получение хэша из PIN-кода**

Для получения хеша используется функция Argon2 со следующими параметрами:

* **Режим:** Argon2i.
* **Затраты времени на итерации:** 5.
* **Стоимость памяти в килобайтах:** 65 536.
* **Параллельность:** 2.
* **Полученная длина хэша:** 128 бит.

**Зашифрованные файлы**

Для безопасного хранения данных эта библиотека использует библиотеку безопасности Jetpack из набора библиотек Android Jetpack. Эта библиотека, в свою очередь, использует другую библиотеку — Tink.

**Пример использования**

Добавьте библиотеку в конфигурацию Gradle:

    implementation 'com.redmadrobot:pinkman:$pinkman_version'

Создайте экземпляр класса Pinkman и интегрируйте его в свою логику аутентификации.

    val pinkman = Pinkman(application.applicationContext)
    ...
    class CreatePinViewModel(private val pinkman: Pinkman) : ViewModel() {
        val pinIsCreated = MutableLiveData()
        fun createPin(pin: String) {
            pinkman.createPin(pin)
            pinIsCreated.postValue(true)
        }
    }
    ...
    class InputPinViewModel(private val pinkman: Pinkman) : ViewModel() {
        val pinIsValid = MutableLiveData()
        fun validatePin(pin: String) {
            pinIsValid.value = pinkman.isValidPin(pin)
        }
    }

На некоторых устройствах операции получения хэша могут занять значительное время. Чтобы избежать ANR в вашем приложении, не следует запускать методы `createPin()`, `changePin()`, `isValidPin()` в основном потоке.

В этой библиотеке уже есть два расширения для асинхронного запуска этих методов. Вы можете выбрать один в зависимости от ваших конкретных потребностей (или технического стека).

Вам нужно добавить эту зависимость, если вы предпочитаете RxJava:

    implementation 'com.redmadrobot:pinkman-rx3:$pinkman_version'

Но если вы используете новейшие технологии, вам следует использовать зависимость с поддержкой [Kotlin Coroutines](https://kotlinlang.org/docs/coroutines-overview.html):

    implementation 'com.redmadrobot:pinkman-coroutines:$pinkman_version'

В результате вы получите набор методов, специфичных для RxJava или Coroutines:

    // RxJava3
    fun createPinAsync(...): Completable
    fun changePinAsync(...): Completable
    fun isValidPinAsync(...): Single
    // Coroutines
    suspend fun createPinAsync(...)
    suspend fun changePinAsync(...)
    suspend fun isValidPinAsync(...): Boolean

!!! note "Примечание"
    Если же хранение подобной информации, по каким-то причинам, не может быть вынесено из исходного кода приложения, можно воспользоваться платными инструментами, которые позволяют шифровать строковые ресурсы при компиляции приложения. Примерами такого программного обеспечения могут служить [DexProtector](https://dexprotector.com/) и [DexGuard](https://www.guardsquare.com/en/products/dexguard).

## Ссылки

1. [https://mobile-security.gitbook.io/mobile-security-testing-guide/android-testing-guide/0x05d-testing-data-storage](https://mobile-security.gitbook.io/mobile-security-testing-guide/android-testing-guide/0x05d-testing-data-storage)

2. [https://cwe.mitre.org/data/definitions/200.html](https://cwe.mitre.org/data/definitions/200.html)

3. [https://cwe.mitre.org/data/definitions/311.html](https://cwe.mitre.org/data/definitions/311.html)

4. [https://cwe.mitre.org/data/definitions/312](https://cwe.mitre.org/data/definitions/312)

5. [https://www.guardsquare.com/en/products/dexguard](https://www.guardsquare.com/en/products/dexguard)

6. [https://dexprotector.com/](https://dexprotector.com/)

7. [https://github.com/RedMadRobot/PINkman](https://github.com/RedMadRobot/PINkman)