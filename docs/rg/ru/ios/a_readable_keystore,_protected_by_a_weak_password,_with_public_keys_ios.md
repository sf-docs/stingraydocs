# Доступное на чтение хранилище ключей со слабым паролем, содержащее открытые ключи

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
        <td>Способ обнаружения:<strong> DAST, КЛЮЧЕВАЯ ИНФОРМАЦИЯ</strong></td>
      </tr>
    </tbody>
</table>

## Описание

Приложение использует доступное на чтение хранилище ключей со слабым паролем, содержащее открытые ключи. Это может привести к подмене ключевой информации. Ключ шифрования не должен храниться в общедоступном месте.

При использовании криптографических операций на устройстве необходимо обеспечить максимальную безопасность основного секрета в таких операциях - ключа шифрования. При использовании ассиметричного шифрования - необходимо сохранить в секрете приватный ключ, в то время как в случае использования симметричных алгоритмов следует защищать ключ, который используется и для шифрования, и для расшифрования sensitive-информации. 

Наиболее безопасным вариантом, безусловно является хранение ключей в Keychain.

Компрометация ключевой информации, которая используется в приложении, может привести к катастрофическим последствиям, в зависимости от использования данной информации в приложении, начиная от расшифровки файлов, трафика, заканчивая компрометацией закрытого ключа, использующегося для подписи приложения.

## Рекомендации

### Создание нового ключа в Keychain

    import Foundation
    import Security
    func computeSymmetricKey() -> String? {
        var keyData = Data(count: 32) // 32 bytes === 256 bits
        let result = keyData.withUnsafeMutableBytes {
            (mutableBytes: UnsafeMutablePointer) -> Int32 in
            SecRandomCopyBytes(kSecRandomDefault, keyData.count, mutableBytes)
        }
        if result == errSecSuccess {
            return keyData.base64EncodedString()
        } else {
            return nil
        }
    }
    let secretKey = computeSymmetricKey()

### Сохранение нового ключа в Keychain

    enum KeychainErrors:Error {
        case COULDNOTINSERT
        case COULDNOTREAD
    }
    func store (key: String, withTag: String) throws {
        let fromKey = key.data(using: .utf8)!
        
        let query: [String: Any] = [
            kSecClass as String: kSecClassKey,
            kSecAttrApplicationTag as String: withTag,
            kSecValueRef as String: fromKey
        ]
        
        let status = SecItemAdd(query as CFDictionary, nil)
        guard status == errSecSuccess else { throw KeychainErrors.COULDNOTINSERT }
    }
    do {
        try store(key: secretKey!, withTag: "com.myapp.keys.localStore")
    } catch {
        print(error)
    }

### Чтение ключа из Keychain

    func read (tag: String) throws -> CFTypeRef {
        let readQuery: [String: Any] = [
            kSecClass as String: kSecClassKey,
            kSecAttrApplicationTag as String: tag,
            kSecAttrKeyType as String: kSecAttrKeyTypeRSA,
            kSecReturnRef as String: true
        ]
        
        var item: CFTypeRef?
        let readStatus = SecItemCopyMatching(readQuery as CFDictionary, &item)
        guard readStatus == errSecSuccess else { throw KeychainErrors.COULDNOTREAD }
        
        return item!
    }
    do {
        let keyReadFromKeychain = try read(tag: "com.myapp.keys.localStore")
        print(keyReadFromKeychain)
    } catch {
        print(error)
    }

### Применение ключа для шифрования и расшифровки

    let algorithm: SecKeyAlgorithm = .rsaEncryptionOAEPSHA256
    let plainText = "this is our golden secret. Encrypt it!"
    var error: Unmanaged?
    guard let cipherText = SecKeyCreateEncryptedData(secretKey as! SecKey, algorithm,
        plainText as! CFData, &error) as Data? else {
            throw error!.takeRetainedValue() as Error
        }

### Отображение сертификатов

    func showCertificateInfo() -> String {
        var resultString = "--- Certificates in Keychain ---\n"
        var outputACert = false
        let query = [kSecMatchLimit: kSecMatchLimitAll,
                    kSecReturnRef: true,
                    kSecClass: kSecClassCertificate] as CFDictionary
        var result: CFTypeRef?
        let resultCode = SecItemCopyMatching(query, &result)
        if resultCode == errSecSuccess {
            if CFArrayGetTypeID() == CFGetTypeID(result) {
                let array = (result as? NSArray) as? [SecCertificate]
                array?.forEach { (item) in
                    resultString += self.displayCertificate(item)
                    outputACert = true
                }
            } else {
                // swiftlint:disable force_cast
                resultString += self.displayCertificate(result as! SecCertificate)
                // swiftlint:enable force_cast
                outputACert = true
            }
        }
        if !outputACert {
            resultString += "None\n"
        }
        resultString += "-------------------------------"
        return resultString
    }

## Ссылки

1. [https://developer.apple.com/](https://developer.apple.com/documentation/security/certificate_key_and_trust_services/certificates/storing_a_certificate_in_the_keychain)
2. [CertificateSDK](https://github.com/jamf/CertificateSDK/blob/main/Certificate%20SDK%20Sample%20App/KeychainHandler.swift)
3. [Certificates](https://developer.apple.com/documentation/security/certificate_key_and_trust_services/certificates)
4. [Keychain Services](https://www.raywenderlich.com/9240-keychain-services-api-tutorial-for-passwords-in-swift)
5. [Basic iOS Security: Keychain and Hashing](https://www.raywenderlich.com/129-basic-ios-security-keychain-and-hashing)