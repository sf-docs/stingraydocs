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

В системе iOS существует механизм автодополнения слов, которые вводит пользователь в текстовые поля. При этом, если система не знает вводимого пользователем слова, она может закэшировать (или предложить пользователю добавить слово в словарь). Эта функция может быть очень полезна, например, для приложений-мессенджеров. Однако, кэш клавиатуры может раскрывать конфиденциальную информацию, если они используются для ввода такой информации (данные кредитной карты, логин, пароль или персональные данные пользователя). 

## Рекомендации

За включение или выключение опции автодополнения отвечает параметр `autocorrectionType` в поле объекта класса `UITextField`.

**Пример кода:**

    UITextField *textField = [[UITextField alloc] initWithFrame:frame]; 
    textField.autocorrectionType = UITextAutocorrectionTypeNo;

Все поля ввода чувствительной информации должны помечаться параметром `secureTextEntry`

**Пример кода:**

    UITextField *textField = [[UITextField alloc] initWithFrame:frame]; 
    textField.secureTextEntry = YES;

Рекомендуется использовать реализацию custom-клавиатуры для ввода все чувствительных данных с отключенным кэшированием всех вводимых данных. Также необходимо позаботиться о запрещении возможности копирования в буфер обмена введенной информации для доступа к нему из других приложений

**Пример кода:**

    - (BOOL)canPerformAction:(SEL)action 
                withSender:(id)sender
    {
    UIMenuController *menuController = [UIMenuController sharedMenuController]; 
    if (menuController) {
        menuController.menuVisible = NO;
    }
    return NO;
    }

## Ссылки

1. [https://develoler.apple.com](https://developer.apple.com/library/archive/documentation/StringsTextFonts/Conceptual/TextAndWebiPhoneOS/KeyboardManagement/KeyboardManagement.html)
2. [OWASP Mobile Top 10](https://github.com/OWASP/owasp-mstg/blob/master/Document/0x05d-Testing-Data-Storage.md#determining-whether-the-keyboard-cache-is-disabled-for-text-input-fields-mstg-storage-5)
3. [owasp-mstg/0x05d-Testing-Data-Storage.md at master · OWASP/owasp-mstg](https://cwe.mitre.org/data/definitions/200.html)
4. [CWE - CWE-200: Exposure of Sensitive Information to an Unauthorized Actor (4.5)](https://cwe.mitre.org/data/definitions/200.html)
5. [Custom keyboard](https://developer.apple.com/documentation/uikit/keyboards_and_input/creating_a_custom_keyboard/configuring_a_custom_keyboard_interface)
6. [Cache keyboard review](https://www.programmersought.com/article/18395753289/)
 