# Возможность получения доступа к произвольному файлу

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

Уязвимость позволяет получить доступ к произвольным файлам приложения.

Уязвимость присутствует в приложениях, которые не проводят надлежащим образом проверку входного Uri-параметра и используют его для обращения к методам работы с файлами. Вредоносное приложение может специальным образом сформировать Uri, передать его тем или иным способом в уязвимое приложение и получить доступ к произвольному файлу.

Пример уязвимого кода:

    private void takeSomeSharedFile() {
    Intent interceptableIntent = new Intent("vuln.app.pkg.INTERCAPTABLE");
    startActivityForResult(interceptableIntent, 1001);
    }

    @Override
    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
    super.onActivityResult(requestCode, resultCode, data);
    if(resultCode == -1 && data != null) {
        if(requestCode == 1001) {
        copyToCache(data.getData());
        }
    }
    }

    public static File copyToCache(Uri uri) {
    try {
        File out = new File(getExternalCacheDir(), "" + System.currentTimeMillis());
        InputStream i = getContentResolver().openInputStream(uri);
        OutputStream o = new FileOutputStream(out);
        IOUtils.copy(i, o);
        i.close();
        o.close();
        return out;
    }
    catch (Exception e) {
        return null;
    }
    }

Вредоносное приложение может использовать такой код:

    Intent intent = new Intent();
    File privFile = new File("/data/data/vuln.app.pkg/databases/main.db");
    intent.setData(Uri.fromFile(privFile));
    setResult(RESULT_OK, intent);
    finish();

В результате уязвимое приложение скопирует файл ***main.db*** в файл `/storage/emulated/0/Android/data/vuln.app.pkg/1650464271`, доступ к которому может получить вредоносное приложение.

## Рекомендации

Необходимо проводить валидацию canonical пути файла непосредственно перед вызовом методов файловых операций над этим файлом:

    File file = new File(uri);
    if (!file.getCanonicalPath().startsWith(sdcardDir)) {
    throw new IllegalArgumentException();
    }

.