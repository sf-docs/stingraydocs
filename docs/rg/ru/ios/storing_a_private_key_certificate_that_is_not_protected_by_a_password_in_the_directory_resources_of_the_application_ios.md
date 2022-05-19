# Хранение приватного ключа/сертификата не защищенного паролем в директории/ресурсах приложения

<table class='noborder'>
    <colgroup>
      <col/>
      <col/>
    </colgroup>
    <tbody>
      <tr>
        <td rowspan="2"><img src="../../../img/defekt_kritichnyj.png"/></td>
        <td>Критичность:<strong> КРИТИЧЕСКИЙ</strong></td>
      </tr>
      <tr>
        <td>Способ обнаружения:<strong> DAST, SENSITIVE INFO</strong></td>
      </tr>
    </tbody>
</table>

## Описание

Системе не удалось идентифицировать тип ключа или сертификата, который хранится в директории/ресурсах приложения и определить его защищенность.

Тем не менее, рекомендуется дополнительно проверить, где и как используется данный ключ и удостовериться в безопасности данного процесса. Ниже представлены подходы к безопасному использованию и хранению ключей.

## Рекомендации

Для хранения ключей рекомендуемым способом является использование Keychain. 

Keychain предоставляет несколько основных функций, которые существенно упрощают работу с криптографическими ключами:

* Случайная генерация ключей. 
* Надежное хранение ключей.

Все, что вам нужно сделать, это:

* Сгенерировать случайный ключ при первом запуске приложения.
* Если вы хотите зашифровать данные, получите ключ из Keychain, зашифруйте с его помощью данные, а затем сохраните зашифрованные данные.
* Если вы хотите расшифровать данные, получите ключ из Keychain, а затем используйте его для расшифровки данных.

### Структура генерация ключа / шифрование данных / расшифровка данных

    import Foundation
    import CommonCrypto
    struct AES {
        // MARK: - Value
        // MARK: Private
        private let key: Data
        private let iv: Data
        // MARK: - Initialzier
        init?(key: String, iv: String) {
            guard key.count == kCCKeySizeAES128 || key.count == kCCKeySizeAES256, let keyData = key.data(using: .utf8) else {
                debugPrint("Error: Failed to set a key.")
                return nil
            }
            guard iv.count == kCCBlockSizeAES128, let ivData = iv.data(using: .utf8) else {
                debugPrint("Error: Failed to set an initial vector.")
                return nil
            }
            self.key = keyData
            self.iv  = ivData
        }
        // MARK: - Function
        // MARK: Public
        func encrypt(string: String) -> Data? {
            return crypt(data: string.data(using: .utf8), option: CCOperation(kCCEncrypt))
        }
        func decrypt(data: Data?) -> String? {
            guard let decryptedData = crypt(data: data, option: CCOperation(kCCDecrypt)) else { return nil }
            return String(bytes: decryptedData, encoding: .utf8)
        }
        func crypt(data: Data?, option: CCOperation) -> Data? {
            guard let data = data else { return nil }
            let cryptLength = data.count + kCCBlockSizeAES128
            var cryptData   = Data(count: cryptLength)
            let keyLength = key.count
            let options   = CCOptions(kCCOptionPKCS7Padding)
            var bytesLength = Int(0)
            let status = cryptData.withUnsafeMutableBytes { cryptBytes in
                data.withUnsafeBytes { dataBytes in
                    iv.withUnsafeBytes { ivBytes in
                        key.withUnsafeBytes { keyBytes in
                        CCCrypt(option, CCAlgorithm(kCCAlgorithmAES), options, keyBytes.baseAddress, keyLength,
                        ivBytes.baseAddress, dataBytes.baseAddress, data.count, cryptBytes.baseAddress, cryptLength,
                        &bytesLength)
                        }
                    }
                }
            }
            guard UInt32(status) == UInt32(kCCSuccess) else {
                debugPrint("Error: Failed to crypt data. Status \(status)")
                return nil
            }
            cryptData.removeSubrange(bytesLength..<cryptData.count)
            return cryptData
        }
    }

### Использование

    let password = "UserPassword1!"
    let key128   = "1234567890123456"                   // 16 bytes for AES128
    let key256   = "12345678901234561234567890123456"   // 32 bytes for AES256
    let iv       = "abcdefghijklmnop"                   // 16 bytes for AES128
    let aes128 = AES(key: key128, iv: iv)
    let aes256 = AES(key: key256, iv: iv)
    let encryptedPassword128 = aes128?.encrypt(string: password)
    aes128?.decrypt(data: encryptedPassword128)
    let encryptedPassword256 = aes256?.encrypt(string: password)
    aes256?.decrypt(data: encryptedPassword256)

## Ссылки

1. [https://developer.apple.com/](https://developer.apple.com/documentation/security/certificate_key_and_trust_services/certificates/storing_a_certificate_in_the_keychain)
2. [CertificateSDK](https://github.com/jamf/CertificateSDK/blob/main/Certificate%20SDK%20Sample%20App/KeychainHandler.swift)
3. [Certificates](https://developer.apple.com/documentation/security/certificate_key_and_trust_services/certificates)
4. [Keychain Services](https://www.raywenderlich.com/9240-keychain-services-api-tutorial-for-passwords-in-swift)
5. [Basic iOS Security: Keychain and Hashing](https://www.raywenderlich.com/129-basic-ios-security-keychain-and-hashing)