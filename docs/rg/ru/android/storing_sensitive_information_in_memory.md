# Хранение sensitive-информации в памяти

<table class='noborder'>
    <colgroup>
      <col/>
      <col/>
    </colgroup>
    <tbody>
      <tr>
        <td rowspan="2"><img src="../../../img/defekt_srednij.png"/></td>
        <td>Критичность:<strong> СРЕДНИЙ</strong></td>
      </tr>
      <tr>
        <td>Способ обнаружения:<strong> DAST, HEAPDUMP</strong></td>
      </tr>
    </tbody>
</table>

## Описание

Анализ памяти может помочь разработчикам определить основные причины ряда проблем, возникающих во время работы приложения на устройстве. Однако, его также можно использовать для доступа к конфиденциальным данным. Когда приложение работает на устройстве, пользовательские или специфичные для приложения данные могут храниться в оперативной памяти и не очищаться должным образом, когда пользователь выходит из системы или приложения. Поскольку Android хранит приложение в памяти (даже после использования) до тех пор, пока память не будет восстановлена, различная конфиденциальная информация может оставаться в памяти неопределенное время. Злоумышленник, который обнаружит или похитит устройство, может подключить отладчик и выгрузить дамп памяти приложения.

## Рекомендации

Не храните конфиденциальные данные (например, ключи шифрования) в оперативной памяти дольше, чем требуется. Обнуляйте все переменные, которые содержат конфиденциальную информацию после её использования. Избегайте использования неизменяемых объектов для криптографических ключей или паролей, таких как `Android java.lang.String`.

Чтобы правильно очистить конфиденциальную информацию из памяти, храните ее в примитивных типах данных, таких как байтовые массивы (`byte []`) и char-массивы (`char []`).

Как один из вариантов обнуления информации в памяти — перезапись содержимого нулями.

**Пример на Java**

    byte[] secret = null;
    try{
        //get or generate the secret, do work with it, make sure you make no local copies
    } finally {
        if (null != secret) {
            Arrays.fill(secret, (byte) 0);
        }
    }

**Пример на Kotlin**

    val secret: ByteArray? = null
    try {
        //get or generate the secret, do work with it, make sure you make no local copies
    } finally {
        if (null != secret) {
            Arrays.fill(secret, 0.toByte())
        }
    }

Это, к сожалению, не гарантирует, что содержимое будет перезаписано во время исполнения приложения. Чтобы оптимизировать байт-код, компилятор проанализирует и решит не перезаписывать данные, поскольку впоследствии они не будут использоваться (то есть это ненужная операция с точки зрения компилятора).

Для этой проблемы нет идеального решения. Например, можно выполнить дополнительные вычисления (например, XOR данных в фиктивном буфере), но не будет возможности определить, удалил ли компилятор эти операции. С другой стороны, использование перезаписанных данных вне области компилятора (например, сериализация их во временном файле) гарантирует, что они будут перезаписаны, но, очевидно, такой подход может повлиять на производительность.

Кроме того, использование `Arrays.fill` для перезаписи данных может быть плохой идеей, поскольку этот метод можно перехватить и это делают достаточно часто в различных инструментах анализа. Последняя проблема с приведенным выше примером заключается в том, что содержимое перезаписывается только нулями. В идеальном варианте необходимо перезаписать объекты с конфиденциальной информацией случайными данными или содержимым других переменных.

**Пример на Java**

    byte[] nonSecret = somePublicString.getBytes("ISO-8859-1");
    byte[] secret = null;
    try{
        //get or generate the secret, do work with it, make sure you make no local copies
    } finally {
        if (null != secret) {
            for (int i = 0; i < secret.length; i++) {
                secret[i] = nonSecret[i % nonSecret.length];
            }
            FileOutputStream out = new FileOutputStream("/dev/null");
            out.write(secret);
            out.flush();
            out.close();
        }
    }

**Пример на Kotlin**

    val nonSecret: ByteArray = somePublicString.getBytes("ISO-8859-1")
    val secret: ByteArray? = null
    try {
        //get or generate the secret, do work with it, make sure you make no local copies
    } finally {
        if (null != secret) {
            for (i in secret.indices) {
                secret[i] = nonSecret[i % nonSecret.size]
            }
            val out = FileOutputStream("/dev/null")
            out.write(secret)
            out.flush()
            out.close()
            }
    }

## Ссылки

1. [https://mobile-security.gitbook.io/mobile-security-testing-guide/android-testing-guide/0x05d-testing-data-storage](https://mobile-security.gitbook.io/mobile-security-testing-guide/android-testing-guide/0x05d-testing-data-storage)

2. [https://cwe.mitre.org/data/definitions/316.html](https://cwe.mitre.org/data/definitions/316.html)

3. [https://www.pentestpartners.com/security-blog/how-to-extract-sensitive-plaintext-data-from-android-memory/](https://www.pentestpartners.com/security-blog/how-to-extract-sensitive-plaintext-data-from-android-memory/)

4. [https://securitygrind.com/dumping-and-analyzing-android-application-memory/](https://securitygrind.com/dumping-and-analyzing-android-application-memory/)

5. [https://developer.android.com/studio/profile/memory-profiler](https://developer.android.com/studio/profile/memory-profiler)