# Недостаточная длина ключа подписи

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

Ключ, используемый для подписи APK файла, имеет недостаточную длину.

[Официальные рекомендации NIST](https://guardianproject.info/2015/12/29/how-to-migrate-your-android-apps-signing-key/) (PDF, стр. 64 и 67) признают 1024-битных ключи RSA небезопасными начиная с 2013 года. Это не означает, что 1024-битный RSA был скомпрометирован, скорее это превентивный шаг, для того чтобы быть на шаг впереди атакующих. Основная опасность использования слабого ключа заключается в том, что злоумышленник может взломать его, чтобы создать поддельные подписи для APK. Затем эти вредоносные APK, подписанные вашим ключом, могут быть установлены в качестве обновлений для приложения. Существуют и ряд других атак с использованием скомпрометированного ключа, которые зависят от использования ключа в приложении.

Также некоторые магазины приложений, например от [Huawei](https://developer.huawei.com/consumer/en/appgallery), не рекомендуют подписывать приложения при помощи ключа с длинной меньше, чем 2048 бит. И более того, не принимает такие приложения для загрузки.

## Рекомендации
Для исправления данной уязвимости необходимо подписывать приложение с использованием современных алгоритмов, к примеру таких как **SHA256withRSA** или **SHA512withRSA** с использованием ключа с длинной не менее 2048-бит (рекомендуемая длина — 4096 бит) Обратите внимание на то что ранние версии Android могут не поддерживать алгоритмы выше SHA1. Данная проблема и процесс замены подписи подробно описан в [данной статье](https://guardianproject.info/2015/12/29/how-to-migrate-your-android-apps-signing-key/).

**Пример генерации с использованием SHA512withRSA**

    keytool -genkey -v -keystore test.keystore -alias testkey -keyalg RSA -keysize 4096 -sigalg SHA512withRSA -dname "cn=Test,ou=Test,c=CA" -validity 10000

**Пример подписи с использованием сгенерированного ключа**

    jarsigner -verbose -sigalg SHA512withRSA -digestalg SHA512 -keystore test.keystore test.apk testkey

## Ссылки

1. [https://guardianproject.info/2015/12/29/how-to-migrate-your-android-apps-signing-key/](https://guardianproject.info/2015/12/29/how-to-migrate-your-android-apps-signing-key/)

2.  [https://developer.android.com/studio/publish/app-signing](https://developer.android.com/studio/publish/app-signing)

3. [https://github.com/OWASP/owasp-mstg/blob/master/Document/0x05i-Testing-Code-Quality-and-Build-Settings.md](https://github.com/OWASP/owasp-mstg/blob/master/Document/0x05i-Testing-Code-Quality-and-Build-Settings.md)

4. [https://sites.google.com/site/itstheshappening/](https://sites.google.com/site/itstheshappening/)

5. [https://guardianproject.info/2015/12/29/how-to-migrate-your-android-apps-signing-key/](https://guardianproject.info/2015/12/29/how-to-migrate-your-android-apps-signing-key/)