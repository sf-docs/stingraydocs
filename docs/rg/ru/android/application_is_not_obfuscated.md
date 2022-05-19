# Приложение не обфусцировано

<table class='noborder'>
    <colgroup>
      <col/>
      <col/>
    </colgroup>
    <tbody>
      <tr>
        <td rowspan="2"><img src="../../../img/defekt_nizkij.png"/></td>
        <td>Критичность:<strong> НИЗКИЙ</strong></td>
      </tr>
      <tr>
        <td>Способ обнаружения:<strong> SAST, APK</strong></td>
      </tr>
    </tbody>
</table>

## Описание

Исходный код приложения не обфусцирован или обфусцирован с недостаточным покрытием.

Плохая обфускация приложения или её отсутствие приводит к более легкому изучению кода после декомпиляции, что позволяет злоумышленнику без труда анализировать код приложения для поиска уязвимостей или способов обхода защиты.

## Рекомендации

Перед публикацией приложения необходимо убедиться, что в релизной версии приложения включены правила для обфускации и они корректно настроены.

Одним из распространенных решений является использований правил встроенного обфускатора ProGuard, для автоматического включения / выключения обфускации в зависимости от типа сборки (release / debug).

**Пример настроек ProGuard:**

    optimizationpasses 5
    -dontusemixedcaseclassnames
    -dontskipnonpubliclibraryclasses
    -dontskipnonpubliclibraryclassmembers
    -dontpreverify
    -verbose
    -dump class_files.txt
    -printseeds seeds.txt
    -printusage unused.txt
    -printmapping mapping.txt
    -optimizations !code/simplification/arithmetic,!field/*,!class/merging/*
    -allowaccessmodification
    -keepattributes *Annotation*
    -renamesourcefileattribute SourceFile
    -keepattributes SourceFile,LineNumberTable
    -repackageclasses ''


**Включение использования ProGuard для релизной сборки приложения:**

    buildTypes {
        ...
        release {
            minifyEnabled true
            proguardFiles 'proguard-rules.pro', getDefaultProguardFile('proguard-android.txt')
        }
    }

Также хорошей практикой может служит обфускация используемых в проекте библиотек с открытым исходным кодом. В последнее время все больше и больше библиотек распространяются уже с готовыми конфигурационными файлами для обфускации. ProGuard умеет заглядывать внутрь архива, находить конфигурационные файлы библиотеки и добавлять его к остальным опциям. Проверьте каждую библиотеку, которую вы используете на наличие такого файла.

Если авторы библиотеки не упаковывают конфиг в архив, возможно они позаботились и написали правила на своем сайте, страничке репозитория или в README файле. Попробуйте самостоятельно найти конфиг для той версии библиотеки, которую вы используете.

## Ссылки

1. [https://habr.com/ru/post/415499/](https://habr.com/ru/post/415499/)

2. [https://developer.android.com/studio/build/shrink-code](https://developer.android.com/studio/build/shrink-code)

3. [https://www.guardsquare.com/en/products/proguard/manual/examples](https://www.guardsquare.com/en/products/proguard/manual/examples)

4.  [https://github.com/OWASP/owasp-stg/blob/master/Document/0x05i-Testing-Code-Quality-and-Build-Settings.md#make-sure-that-free-security-features-are-activated-mstg-code-9](https://github.com/OWASP/owasp-stg/blob/master/Document/0x05i-Testing-Code-Quality-and-Build-Settings.md#make-sure-that-free-security-features-are-activated-mstg-code-9)