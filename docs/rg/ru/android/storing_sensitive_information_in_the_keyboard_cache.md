# Хранение sensitive-информации в кэше клавиатуры

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
        <td>Способ обнаружения:<strong> DAST, SENSITIVE INFO</strong></td>
      </tr>
    </tbody>
</table>

## Описание

В системе Android существует механизм автодополнения слов, которые вводит пользователь в текстовые поля. При этом, если система не знает вводимого пользователем слова, она может закэшировать (или предложить пользователю добавить слово в словарь). Эта функция может быть очень полезна, например, для приложений-мессенджеров. Однако, кэш клавиатуры может раскрывать конфиденциальную информацию, если они используются для ввода такой информации (данные кредитной карты, логин, пароль или персональные данные пользователя).

За включение или выключение опции автодополнения отвечает параметр `android:inputType="textNoSuggestions"` в описании компонента ввода (`<EditText/>`).

**Пример уязвимого кода:**

    <EditText 
    android:id="@+id/KeyBoardCache"/>

## Рекомендации

Во всех полях ввода, которые запрашивают конфиденциальную информацию, должен быть выставлен следующий атрибут XML (для отключения автодополнений):

    <EditText 
    android:id="@+id/KeyBoardCache" 
    android:inputType="textNoSuggestions"/>

## Ссылки

1. [https://developer.android.com/reference/android/text/InputType.html#TYPE_TEXT_FLAG_NO_SUGGESTIONS](https://developer.android.com/reference/android/text/InputType.html#TYPE_TEXT_FLAG_NO_SUGGESTIONS)

2. [https://www.owasp.org/index.php/Mobile_Top_10_2016-M2-Insecure_Data_Storage](https://www.owasp.org/index.php/Mobile_Top_10_2016-M2-Insecure_Data_Storage)

3. [https://github.com/OWASP/owasp-mstg/blob/master/Document/0x05d-Testing-Data-Storage.md#determining-whether-the-keyboard-cache-is-disabled-for-text-input-fields-mstg-storage-5](https://github.com/OWASP/owasp-mstg/blob/master/Document/0x05d-Testing-Data-Storage.md#determining-whether-the-keyboard-cache-is-disabled-for-text-input-fields-mstg-storage-5)

4. [https://cwe.mitre.org/data/definitions/200.html](https://cwe.mitre.org/data/definitions/200.html)