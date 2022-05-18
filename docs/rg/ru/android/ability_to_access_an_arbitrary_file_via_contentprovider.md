# Возможность доступа к произвольному файлу через ContentProvider

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
        <td>Способ обнаружения:<strong> IAST</strong></td>
      </tr>
    </tbody>
</table>

## Описание

Уязвимость позволяет получить доступ к файлам приложения с помощью **экспортируемого ContentProvider**.

Уязвимость присутствует в приложениях, в которых реализация метода `openFile` класса производного от **ContentProvider** не проводит надлежащим образом проверки Uri-параметра. Вредоносное приложение может специальным образом сформировать Uri, передать его в этот **ContentProvider** и получить доступ к произвольному файлу.

Пример уязвимого кода:

    @Override
    public ParcelFileDescriptor openFile(Uri uri, String mode) throws FileNotFoundException {
    File file = new File(getContext().getFilesDir(), uri.getLastPathSegment());
    return ParcelFileDescriptor.open(file, ParcelFileDescriptor.MODE_READ_WRITE);
    }

Вредоносное приложение может использовать такой код:

    Uri uri = Uri.parse("content://vuln.app.pkg.some_authority/private_internal_file");
    try {
    Log.d("Evil", IOUtils.toString(getContentResolver().openInputStream(uri), Charset.defaultCharset()));
    } catch (Throwable th) {
    Log.e("Evil", "Error was occured during openInputStream call");
    throw new RuntimeException(th);
    }

В результате вредоносное приложение получит доступ к файлу `private_internal_file` в директории уязвимого приложения (`vuln.app.pkg`).

## Рекомендации

Для устранения подобных проблем в приложении необходимо убедиться в соответствии нескольким правилам.

1. Реализовать private/in-house видимость у **ContentProvider**. 

    Например, объявить **ContentProvider** внутренним:

        <provider
        android:name=".PrivateProvider"
        android:authorities="notvuln.app.pkg.some_authority"
        android:exported="false" />

    Чтобы оградить **ContentProvider** от его использования сторонними приложениями, необходимо определить `permission` с `protectionLevel="signature"` и прописать его в объявлении этого **ContentProvider**:

        <?xml version="1.0" encoding="utf-8"?>
        <manifest xmlns:android="http://schemas.android.com/apk/res/android"
            package="notvuln.app.pkg">
        
            <!-- *** 1 *** Определите in-house полномочие (permission) с protectionLevel="signature" -->
            <permission
                android:name="notvuln.app.pkg.inhouseprovider.MY_PERMISSION"
                android:protectionLevel="signature" />
        
            <application
                android:icon="@drawable/ic_launcher"
                android:label="@string/app_name" >
        
                <!-- *** 2 *** Ограничьте доступ к **ContentProvider** при его объявлении с помощью in-house полномочия -->
                <!-- *** 3 *** Явно указывайте атрибут exported="true" -->
                <provider
                    android:name=".InhouseProvider"
                    android:authorities="notvuln.app.pkg.inhouseprovider"
                    android:permission="notvuln.app.pkg.inhouseprovider.MY_PERMISSION"
                    android:exported="true" />
            </application>
        </manifest>

2. Если **ContentProvider** должен оставаться публичным для сторонних приложений, то необходимо проводить валидацию canonical пути файла непосредственно перед его возвратом запрашивающему приложению:

        @Override
        public ParcelFileDescriptor openFile (Uri uri, String mode) throws FileNotFoundException {
        File file = new File(sdcardDir, uri.getLastPathSegment());
        if (!file.getCanonicalPath().startsWith(sdcardDir)) {
            throw new IllegalArgumentException();
        }
        return ParcelFileDescriptor.open(file, ParcelFileDescriptor.MODE_READ_ONLY);
        }