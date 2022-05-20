# Хранение sensitive-информации в NSUserDefaults

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
        <td>Способ обнаружения:<strong> DAST, ФАЙЛЫ ПРИЛОЖЕНИЯ</strong></td>
      </tr>
    </tbody>
</table>

## Описание

Приложение хранит чувствительную информацию в приватном файле внутри директории приложения.

Для того, чтобы понять, какие именно данные необходимо защищать, прежде всего необходимо определить, какие данные обрабатывает и хранит приложение и какая часть этой информации считается конфиденциальной. Как правило, в таких случаях полагаются на законодательство и здравый смысл. Нет смысла защищать шифрованием абсолютно всю информацию, которую хранит приложение — это может повлиять на скорость и стабильность работы. Вместо этого следует однозначно определить, какие именно данные для приложения или компании являются конфиденциальными, и сосредоточить свое внимание именно на этих данных.

Принято считать, что необходимо хранить как можно меньше конфиденциальных данных в локальном хранилище (внутреннем или внешнем). Однако в большинстве случаев хранения такой информации избежать не удастся. Например, с точки зрения удобства использования не стоит заставлять пользователя вводить сложный пароль при каждом запуске приложения. Большинство приложений должны локально кэшировать какой-либо токен аутентификации. Персонально идентифицируемая информация (PII) и другие типы конфиденциальных данных также могут быть сохранены, если этого требует конкретный сценарий.

Приложение может хранить данные в различных форматах, одним из которых является NSUserDefaults.

NSUserDefaults предназначен для хранения относительно небольших объемов часто запрашиваемых и редко модифицируемых данных. Другие способы использования могут привести к медленной работе или большему потреблению памяти, чем более подходящие решения.

Хранение чувствительных данных при помощи механизма NSUserDefaults в открытом доступе не рекомендуется. Так как в физическом представлении это просто файл в файловой системе устройства, расположенный внутри директории приложения по относительному пути `/Library/Preferences/com.yourcompany.appName.plist`. Значение из этого файла может быть получено через локальный или облачный бекапы, а также при помощи эксплуатации различных уязвимостей.

!!! note "Внимание!"
    Очень часто ошибочно считается, что данные, которые хранятся во внутренней директории приложения, уже защищены при помощи механизма песочницы и злоумышленник до них не доберется. Существует большое количество способов, начиная от простого локального или облачного бекапа приложения и заканчивая физическим доступом к устройству и эксплуатации различных уязвимостей. **Информация, размещенная в открытом виде внутри директории приложения, не защищена!**

## Рекомендации

Любую чувствительную информацию, которая хранится на устройстве, необходимо шифровать. Это можно сделать самыми разными способами и один из таких способов — это шифрование на основе ключа, полученного из данных пользователя (пароля, пин-кода и т. д.), при помощи алгоритмов усиления ключа ([Key Stretching](https://en.wikipedia.org/wiki/Key_stretching)). Это позволяет получить ключ шифрования из достаточно простого пароля, применяя к нему несколько раз функцию хеширования вместе с солью. Соль — это некая последовательность случайных данных. Распространенной ошибкой является исключение соли из алгоритма. Соль дает ключу намного большую энтропию. Без неё намного проще получить/восстановить/подобрать ключ. Тем более, без использования соли два одинаковых пароля будут иметь одинаковое значение хеша и, соответственно, одинаковое окончательное значение ключа шифрования.

При этом, поскольку используется алгоритм усиления ключа, нет необходимости его где-то хранить. Каждый раз, когда возникнет необходимость в ключе, достаточно задействовать данные пользователя для его генерации.

Для шифрования и дешифрования используем функцию `CCCrypt` с помощью `kCCEncrypt` или `kCCDecrypt`. Поскольку применяется блочный шифр, необходимо дополнить сообщение, если оно не соответствует кратности размера блока. Используя параметр `KCCOptionPKCS7Padding`, определяем тип дополнения, как PKCS7:

### Шифрование

    class func encryptData(_ clearTextData : Data, withPassword password : String) -> Dictionary<String, Data>
        {
            var setupSuccess = true
            var outDictionary = Dictionary<String, Data>.init()
            var key = Data(repeating:0, count:kCCKeySizeAES256)
            var salt = Data(count: 8)
            salt.withUnsafeMutableBytes { (saltBytes: UnsafeMutablePointer<UInt8>) -> Void in
                let saltStatus = SecRandomCopyBytes(kSecRandomDefault, salt.count, saltBytes)
                if saltStatus == errSecSuccess
                {
                    let passwordData = password.data(using:String.Encoding.utf8)!
                    key.withUnsafeMutableBytes { (keyBytes : UnsafeMutablePointer<UInt8>) in
                        let derivationStatus = CCKeyDerivationPBKDF(CCPBKDFAlgorithm(kCCPBKDF2), password, passwordData.count, saltBytes, salt.count, CCPseudoRandomAlgorithm(kCCPRFHmacAlgSHA512), 14271, keyBytes, key.count)
                        if derivationStatus != Int32(kCCSuccess)
                        {
                            setupSuccess = false
                        }
                    }
                }
                else
                {
                    setupSuccess = false
                }
            }
            
            var iv = Data.init(count: kCCBlockSizeAES128)
            iv.withUnsafeMutableBytes { (ivBytes : UnsafeMutablePointer<UInt8>) in
                let ivStatus = SecRandomCopyBytes(kSecRandomDefault, kCCBlockSizeAES128, ivBytes)
                if ivStatus != errSecSuccess
                {
                    setupSuccess = false
                }
            }
            
            if (setupSuccess)
            {
                var numberOfBytesEncrypted : size_t = 0
                let size = clearTextData.count + kCCBlockSizeAES128
                var encrypted = Data.init(count: size)
                let cryptStatus = iv.withUnsafeBytes {ivBytes in
                    encrypted.withUnsafeMutableBytes {encryptedBytes in
                    clearTextData.withUnsafeBytes {clearTextBytes in
                        key.withUnsafeBytes {keyBytes in
                            CCCrypt(CCOperation(kCCEncrypt),
                                    CCAlgorithm(kCCAlgorithmAES),
                                    CCOptions(kCCOptionPKCS7Padding + kCCModeCBC),
                                    keyBytes,
                                    key.count,
                                    ivBytes,
                                    clearTextBytes,
                                    clearTextData.count,
                                    encryptedBytes,
                                    size,
                                    &numberOfBytesEncrypted)
                            }
                        }
                    }
                }
                if cryptStatus == Int32(kCCSuccess)
                {
                    encrypted.count = numberOfBytesEncrypted
                    outDictionary["EncryptionData"] = encrypted
                    outDictionary["EncryptionIV"] = iv
                    outDictionary["EncryptionSalt"] = salt
                }
            }
        
            return outDictionary;
        }
  
И, соответственно, функция расшифровки:

### Дешифрование

    class func decryp(fromDictionary dictionary : Dictionary<String, Data>, withPassword password : String) -> Data
        {
            var setupSuccess = true
            let encrypted = dictionary["EncryptionData"]
            let iv = dictionary["EncryptionIV"]
            let salt = dictionary["EncryptionSalt"]
            var key = Data(repeating:0, count:kCCKeySizeAES256)
            salt?.withUnsafeBytes { (saltBytes: UnsafePointer<UInt8>) -> Void in
                let passwordData = password.data(using:String.Encoding.utf8)!
                key.withUnsafeMutableBytes { (keyBytes : UnsafeMutablePointer<UInt8>) in
                    let derivationStatus = CCKeyDerivationPBKDF(CCPBKDFAlgorithm(kCCPBKDF2), password, passwordData.count, saltBytes, salt!.count, CCPseudoRandomAlgorithm(kCCPRFHmacAlgSHA512), 14271, keyBytes, key.count)
                    if derivationStatus != Int32(kCCSuccess)
                    {
                        setupSuccess = false
                    }
                }
            }
            
            var decryptSuccess = false
            let size = (encrypted?.count)! + kCCBlockSizeAES128
            var clearTextData = Data.init(count: size)
            if (setupSuccess)
            {
                var numberOfBytesDecrypted : size_t = 0
                let cryptStatus = iv?.withUnsafeBytes {ivBytes in
                    clearTextData.withUnsafeMutableBytes {clearTextBytes in
                    encrypted?.withUnsafeBytes {encryptedBytes in
                        key.withUnsafeBytes {keyBytes in
                            CCCrypt(CCOperation(kCCDecrypt),
                                    CCAlgorithm(kCCAlgorithmAES128),
                                    CCOptions(kCCOptionPKCS7Padding + kCCModeCBC),
                                    keyBytes,
                                    key.count,
                                    ivBytes,
                                    encryptedBytes,
                                    (encrypted?.count)!,
                                    clearTextBytes,
                                    size,
                                    &numberOfBytesDecrypted)
                            }
                        }
                    }
                }
                if cryptStatus! == Int32(kCCSuccess)
                {
                    clearTextData.count = numberOfBytesDecrypted
                    decryptSuccess = true
                }
            }
            
            return decryptSuccess ? clearTextData : Data.init(count: 0)
        }
  
Для проверки того, что эти функции работают и шифрование/расшифровка проходят корректно, можно воспользоваться простым примером:

### Пример

    class func encryptionTest()
        {
            let clearTextData = "some clear text to encrypt".data(using:String.Encoding.utf8)!
            let dictionary = encryptData(clearTextData, withPassword: "123456")
            let decrypted = decryp(fromDictionary: dictionary, withPassword: "123456")
            let decryptedString = String(data: decrypted, encoding: String.Encoding.utf8)
            print("decrypted cleartext result - ", decryptedString ?? "Error: Could not convert data to string")
        }

В этом примере мы упаковываем всю необходимую информацию и возвращаем ее в виде словаря, чтобы впоследствии все части могли использоваться для успешного дешифрования данных. Для этого необходимо хранить IV и соль либо в Keychain, либо на сервере.

## Ссылки

1. [https://developer.apple.com/](https://developer.apple.com/)
2. [https://en.wikipedia.org/wiki/Key_stretching](https://en.wikipedia.org/wiki/Key_stretching) 
3. [https://en.wikipedia.org/wiki/PBKDF2](https://en.wikipedia.org/wiki/PBKDF2) 
4. [https://en.wikipedia.org/wiki/Padding_(cryptography)#PKCS7](https://en.wikipedia.org/wiki/Padding_(cryptography)#PKCS7) 