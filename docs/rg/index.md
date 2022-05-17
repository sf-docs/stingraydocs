# Рекомендации по безопасной разработке приложений

<div style='float: left; width: 50%; margin-top: -20px; margin-right: 30px;' markdown>

## Рекомендации для Android

### Небезопасное хранение ключевой информации
      
<a href="ru/android/a_writable_keystore/">
        Доступное на запись хранилище ключей</a><br>
<a href="ru/android/a_keystore_with_write_permission_protected_by_a_weak_password/">
        Доступное на запись хранилище ключей со слабым паролем</a><br>
<a href="ru/android/readable_file_keystore/">
        Доступное на чтение файловое хранилище ключей</a><br>
<a href="ru/android/a_readable_keystore%2C_protected_by_a_weak_password%2C_with_private_keys/">
        Доступное на чтение хранилище ключей со слабым паролем, содержащее закрытые ключи</a><br>
<a href="ru/android/a_readable_keystore%2C_protected_by_a_weak_password%2C_with_public_keys/">
        Доступное на чтение хранилище ключей со слабым паролем, содержащее открытые ключи</a><br>
<a href="ru/android/a_readable_keystore_containing_private_keys_protected_by_a_weak_password/">
        Доступное на чтение хранилище ключей с приватными ключами, защищёнными слабым паролем</a><br>
<a href="ru/android/using_a_file_keystore/">
        Использование файлового хранилища ключей</a><br>
<a href="ru/android/a_keystore%2C_protected_by_weak_password%2C_containing_private_keys/">
        Хранилище ключей со слабым паролем, содержащее закрытые ключи</a><br>
<a href="ru/android/a_keystore%2C_protected_by_weak_password%2C_containing_public_keys/">
        Хранилище ключей со слабым паролем, содержащее открытые ключи</a><br>
<a href="ru/android/a_keystore_containing_private_keys_protected_by_a_weak_password/">
        Хранилище ключей с приватными ключами, защищёнными слабым паролем</a>

### Передача sensitive-информации в Activity

<a href="ru/android/insecure_transmission_of_sensitive_information_in_activity/">
        Небезопасная передача sensitive-информации в Activity</a><br>
<a href="ru/android/insecure_transmission_of_sensitive_information_in_external_activity/">
        Небезопасная передача sensitive-информации во внешнюю Activity</a><br>
<a href="ru/android/insecure_transmission_of_sensitive_information_in_private_activity/">
        Передача sensitive-информации во внутреннюю Activity</a>

### Передача sensitive-информации в Service

<a href="ru/android/insecure_transmission_of_sensitive_information_in_service/">
        Небезопасная передача sensitive-информации в Service</a><br>
<a href="ru/android/insecure_transmission_of_sensitive_information_in_external_service/">
        Небезопасная передача sensitive-информации во внешний Service</a><br>
<a href="ru/android/insecure_transmission_of_sensitive_information_in_internal_service/">
        Небезопасная передача sensitive-информации во внутренний Service</a>

### Передача sensitive-информации по сети

<a href="ru/android/inclusion_of_sensitive_information_into_the_get_request_parameters/">
        Включение sensitive-информации в параметры GET-запроса</a><br>
<a href="ru/android/inclusion_of_sensitive_information_into_an_https_request/">
        Включение чувствительной информации в HTTPS запрос</a><br>
<a href="ru/android/transmission_of_sensitive_information_in_an_http_request/">
        Передача sensitive информации в HTTP-запросе</a><br>
<a href="ru/android/transmission_of_sensitive_information_in_an_http_response/">
        Получение sensitive информации в HTTP-ответе</a><br>
<a href="ru/android/inclusion_of_sensitive_information_into_an_https_response/">
        Получение чувствительной информации в HTTPS-ответе</a>

### Хранение sensitive-информации

<a href="ru/android/storing_sensitive_information_in_memory/">
        Хранение sensitive-информации в памяти</a><br>
<a href="ru/android/storing_sensitive_information_in_a_public_file_outside_the_application_s_directory/">
        Хранение sensitive-информации в общедоступном файле вне директории приложения</a><br>
<a href="ru/android/storing_sensitive_information_in_a_public_file_inside_the_application_s_directory/">
        Хранение sensitive-информации в общедоступном файле внутри директории приложения</a><br>
<a href="ru/android/storing_sensitive_information_in_a_private_file_outside_the_application_s_directory/">
        Хранение sensitive-информации в приватном файле вне директории приложения</a><br>
<a href="ru/android/storing_sensitive_information_in_a_private_file_inside_the_application_s_directory/">
        Хранение sensitive-информации в приватном файле внутри директории приложения</a><br>
<a href="ru/android/storing_sensitive_information_in_a_public_protected_database/">
        Хранение sensitive-информации в общедоступной защищённой базе данных</a><br>
<a href="ru/android/storing_sensitive_information_in_a_protected_database/">
        Хранение sensitive-информации в защищённой базе данных</a><br>
<a href="ru/android/storing_sensitive_information_in_an_insecure_database/">
        Хранение чувствительной информации в незащищённой базе данных</a><br>
<a href="ru/android/storing_sensitive_information_in_a_public_unprotected_database/">
        Хранение sensitive-информации в общедоступной незащищённой базе данных</a><br>
<a href="ru/android/storing_sensitive_information_in_the_application_source_code/">
        Хранение sensitive-информации в исходном коде приложения</a><br>
<a href="ru/android/storage_or_use_of_previously_found_sensitive_information/">
        Хранение или использование ранее найденной sensitive-информации</a><br>
<a href="ru/android/storing_sensitive_information_in_the_keyboard_cache/">
        Хранение sensitive-информации в кэше клавиатуры</a>

### Хранение ключей/сертификатов

<a href="ru/android/storing_a_private_key_certificate_that_is_not_protected_by_a_password_in_the_directory_resources_of_the_application/">
        Хранение приватного ключа/сертификата не защищенного паролем в директории/ресурсах приложения</a><br>
<a href="ru/android/storing_a_public_key_certificate_in_the_directory_resources_of_the_application/">
        Хранение публичного ключа/сертификата в директории/ресурсах приложения</a><br>
<a href="ru/android/storing_a_private_key_certificate_protected_by_a_password_in_the_directory_resources_of_the_application/">
        Хранение приватного ключа/сертификата, защищенного паролем, в директории/ресурсах приложения</a><br>
<a href="ru/android/storing_a_key_certificate_in_the_directory_resources_of_the_application/">
        Хранение сертификата/ключа в директории/ресурсах приложения</a>

### Прочие

<a href="ru/android/output_of_sensitive_information_into_the_system_log/">
        Вывод sensitive-информации в системный лог</a><br>
<a href="ru/android/insecure_signature_algorithm/">
        Небезопасный алгоритм подписи</a><br>
<a href="ru/android/insufficient_length_of_a_signature_key/">
        Недостаточная длина ключа подписи</a><br>
<a href="ru/android/transmission_of_sensitive_information_in_broadcastreceiver/">
        Передача sensitive-информации в BroadcastReceiver</a><br>
<a href="ru/android/transmission_of_sensitive_information_to_a_private_broadcastreceiver/">
        Передача sensitive-информации в private BroadcastReceiver</a><br>
<a href="ru/android/transmission_of_sensitive_information_in_sql_query_parameters/">
        Передача sensitive-информации в параметрах SQL-запроса</a><br>
<a href="ru/android/possibility_to_create_a_backup_copy_of_the_application/">
        Возможность создания резервной копии приложения</a><br>
<a href="ru/android/application_is_not_obfuscated/">
        Приложение не обфусцировано</a><br>
<a href="ru/android/weak_database_encryption_password/">
        Слабый пароль шифрования базы данных</a><br>
<a href="ru/android/interception_of_the_database_encryption_password/">
        Перехват пароля шифрования базы данных</a><br>
<a href="ru/android/an_application_allows_network_connections_via_http/">
        Приложение разрешает сетевые соединения по протоколу HTTP</a><br>
<a href="ru/android/insecure_networking_configuration/">
        Небезопасная конфигурация сетевого взаимодействия</a><br>
<a href="ru/android/potential_execution_of_arbitrary_code_within_the_application/">
        Потенциальное выполнение произвольного кода в контексте приложения</a><br>
<a href="ru/android/storing_cookie_values_in_the_standard_webview_database/">
        Хранение значений Cookies в стандартной базе WebView</a><br>
<a href="ru/android/insecure_settings_in_androidmanifest.xml/">
        Небезопасные настройки в AndroidManifest.xml</a><br>
<a href="ru/android/insecure_settings_in_androidmanifest.xml._the_android_hasfragileuserdata_flag/">
        Небезопасные настройки в AndroidManifest.xml. Флаг android:hasFragileUserData</a><br>
<a href="ru/android/insecure_settings_in_androidmanifest.xml._the_android_requestlegacyexternalstorage_flag/">
        Небезопасные настройки в AndroidManifest.xml. Флаг android:requestLegacyExternalStorage</a><br>
<a href="ru/android/ssl-pinning_is_missing_or_incorrectly_realized/">
        Отсутствует или некорректно реализован SSL-pinning</a><br>
<a href="ru/android/ability_to_run_private_activity_indirectly/">
        Возможность опосредованного запуска приватных Activity</a><br>
<a href="ru/android/url_spoofing_possibility/">
        Возможность подмены URL</a><br>
<a href="ru/android/ability_to_open_an_arbitrary_url_in_webview/">
        Возможность открытия произвольного URL в WebView</a><br>
<a href="ru/android/access_to_an_arbitrary_file/">
        Возможность получения доступа к произвольному файлу</a><br>
<a href="ru/android/access_to_an_arbitrary_contentprovider/">
        Возможность получения доступа к произвольному ContentProvider</a><br>
<a href="ru/android/ability_to_access_an_arbitrary_file_via_contentprovider/">
        Возможность доступа к произвольному файлу через ContentProvider</a>

</div>

<div markdown>

## Рекомендации для iOS

### Небезопасное хранение ключевой информации

<a href="ru/ios/a_writable_keystore_ios/">
        Доступное на запись хранилище ключей
      </a><br>
<a href="ru/ios/a_keystore_with_write_permission_protected_by_a_weak_password_ios/">
        Доступное на запись хранилище ключей со слабым паролем
      </a><br>
<a href="ru/ios/readable_file_keystore_ios/">
        Доступное на чтение файловое хранилище ключей
      </a><br>
<a href="ru/ios/a_readable_keystore%2C_protected_by_a_weak_password%2C_with_private_keys_ios/">
        Доступное на чтение хранилище ключей со слабым паролем, содержащее закрытые ключи
      </a><br>
<a href="ru/ios/a_readable_keystore%2C_protected_by_a_weak_password%2C_with_public_keys_ios/">
        Доступное на чтение хранилище ключей со слабым паролем, содержащее открытые ключи
      </a><br>
<a href="ru/ios/a_readable_keystore_containing_private_keys_protected_by_a_weak_password_ios/">
        Доступное на чтение хранилище ключей с приватными ключами, защищёнными слабым паролем
      </a><br>
<a href="ru/ios/using_a_file_keystore_ios/">
        Использование файлового хранилища ключей
      </a><br>
<a href="ru/ios/a_keystore%2C_protected_by_weak_password%2C_containing_private_keys_ios/">
        Хранилище ключей со слабым паролем, содержащее закрытые ключи
      </a><br>
<a href="ru/ios/a_keystore%2C_protected_by_weak_password%2C_containing_public_keys_ios/">
        Хранилище ключей со слабым паролем, содержащее открытые ключи
      </a><br>
<a href="ru/ios/a_keystore_containing_private_keys_protected_by_a_weak_password_ios/">
        Хранилище ключей с приватными ключами, защищёнными слабым паролем
      </a>

### Хранение ключей/сертификатов

<a href="ru/ios/storing_a_key_certificate_in_the_directory_resources_of_the_application_ios/">
        Хранение сертификата/ключа в директории/ресурсах приложения
      </a><br>
<a href="ru/ios/storing_a_private_key_certificate_protected_by_a_password_in_the_directory_resources_of_the_application_ios/">
        Хранение приватного ключа/сертификата защищенного паролем в директории/ресурсах приложения
      </a><br>
<a href="ru/ios/storing_a_public_key_certificate_in_the_directory_resources_of_the_application_ios/">
        Хранение публичного ключа/сертификата в директории/ресурсах приложения
      </a><br>
<a href="ru/ios/storing_a_private_key_certificate_that_is_not_protected_by_a_password_in_the_directory_resources_of_the_application_ios/">
        Хранение приватного ключа/сертификата не защищенного паролем в директории/ресурсах приложения
      </a><br>

### Прочие

<a href="ru/ios/enabled_caching_of_network_requests_ios/">
        Включенное кэширование сетевых запросов
      </a><br>
<a href="ru/ios/output_of_sensitive_information_into_the_system_log_ios/">
        Вывод sensitive-информации в системный лог
      </a><br>
<a href="ru/ios/insecure_app_transport_security_configuration_ios/">
        Небезопасная конфигурация App Transport Security
      </a><br>
<a href="ru/ios/application_does_not_use_overflow_protection_features_ios/">
        Приложение не использует функции защиты от переполнений
      </a><br>
<a href="ru/ios/presence_of_build_scripts_in_the_built_application_package_ios/">
        Наличие скриптов сборки в собранном пакете приложения
      </a><br>
<a href="ru/ios/presence_of_podfile_in_the_built_application_package_ios/">
        Наличие Podfile в собранном пакете приложения
      </a><br>
<a href="ru/ios/storing_sensitive_information_in_the_keyboard_cache_ios/">
        Хранение sensitive-информации в кэше клавиатуры
      </a><br>
<a href="ru/ios/storing_sensitive_information_in_nsuserdefaults_ios/">
        Хранение sensitive-информации в NSUserDefaults
      </a><br>
<a href="ru/ios/storing_sensitive_information_in_a_private_file_ios/">
        Хранение sensitive-информации в приватном файле
      </a><br>
<a href="ru/ios/storing_sensitive_information_in_the_application_source_code_ios/">
        Хранение sensitive-информации в исходном коде приложения
      </a><br>
<a href="ru/ios/storing_sensitive_information_in_binary_cookies_ios/">
        Хранение чувствительной информации в Binary Cookies
      </a><br>
<a href="ru/ios/storage_or_use_of_previously_found_sensitive_information_ios/">
        Хранение или использование ранее найденной чувствительной информации
      </a>

</div>