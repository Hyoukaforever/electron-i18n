# crashReporter

> Çökme raporlarını uzak sunucuya gönderin.

İşlem: [Ana](../glossary.md#main-process), [Renderer](../glossary.md#renderer-process)

Aşağıda bir çökme durumunun otomatik olarak uzak sunucuya gönderilmesinin bir örneği var:

```javascript
const {crashReporter} = require('electron')

crashReporter.start({
  productName: 'YourName',
  companyName: 'YourCompany',
  submitURL: 'https://web-adresiniz.com/gonderilecek-adres',
  uploadToServer: true
})
```

Gelen çökme raporlarını kabul edip işleyen bir sunucu kurmak için aşağıdaki projeleri kullanabilirsiniz:

* [socorro](https://github.com/mozilla/socorro)
* [mini-breakpad-server](https://github.com/electron/mini-breakpad-server)

Çökme raporları uygulamaya özel bir geçici dizinde kaydedilir. `isminizin` `ürünü` için çökme raporları `İsminiz Crashes` dizinimde /temp dizini altında tutulacaktır. Bu geçici dizinin yolunu `app.setPath('temp', '/my/custom/temp')` şeklinde kendinize göre ayarlayabilirsiniz.

## Yöntemler

`crashReporter` modülü aşağıdaki metodlara sahiptir:

### `crashReporter.start(options)`

* `seçenekler` Object 
  * `companyName` Katar (opsiyonel)
  * `submitURL` Katar - Çökme raporlarının POST olarak yollanacağı URL.
  * `productName` Katar (opsiyonel) - Varsayılan olarak `app.getName()`.
  * `uploadToServer` Boolean (opsiyonel) - Çökme raporları sunucuya yollansın mı? Varsayılan `true`.
  * `ignoreSystemCrashHandler` Boolean (opsiyonel) - Varsayılan değeri `false`.
  * `extra` Obje (opsiyonel) - Raporla beraber yollanabilir şekilde tanımlayabileceğiniz bir obje. Sadece katar tipinde özellikler düzgün şekilde yollanır. Iç içe objeler desteklenmez, özellik isimleri ve değerleri 64 karakterden küçük olmalıdır.
  * `crashesDirectory` String (optional) - Directory to store the crashreports temporarily (only used when the crash reporter is started via `process.crashReporter.start`)

`crashReporter` API'lerini kullanmak için ve süreçlerin çökme raporlarını almak için her süreçte (main/renderer) bu metodu çağırmalısınız. Farklı süreçlerden farklı opsiyonları `crashReporter.start`'a geçebilirsiniz.

**Not** `child_process` tarafından yaratılmış çocuk süreçlerin Electron modüllerine erişimi olmaz. Bu yüzden, çocuk süreçlere ait raporları toplamak için `process.crashReporter.start` kullanın. Çökme raporlarını geçici olarak tutan dizini işaret eden `crashesDirectory` ile birlikte aynı opsiyonları geçin. `process.crash()` ile çocuk süreci çökerterek bunu test edebilirsiniz.

**Not:** Çocuk süreçlerden çökme raporlarını toplamak için, bu ek kodu da eklemelisiniz. Bu çökmeleri dinleyen ve yollayan süreci başlatır. `submitURL`'i, `productName` ve `crashesDirectory`'i uygun değerlerle değiştirin.

**Not:** İlk çağrı `başlatma` sonrasında ek/güncellenmiş `ekstra` parametrelerini göndermeniz gerekiyorsa, macOS'ta `setExtraParameter`'i çağırabilirsiniz veya Linux ve Windows'ta yeni/güncellenmiş `ekstra` parametreleriyle tekrar `Başlat`'ı çağırın.

```js
 const args = [
   `--reporter-url=${gonderilecekURL}`,
   `--application-name=${urunIsmi}`,
   `--crashes-directory=${cokmeRaporuDizini}`
 ]
 const env = {
   ELECTRON_INTERNAL_CRASH_SERVICE: 1
 }
 spawn(process.execPath, args, {
   env: env,
   detached: true
 })
```

**Not:** macOS üzerinde Electron, çökme raporu toplama ve raporlama için `crashpad` istemcisi kullanır. Çökme raporlamayı aktif hale getirmek için, `crashpad<code>'i ana süreç içerisinden -hangi süreçten çökmeleri toplayacağınızdan bağımsız olarak-
 <0>crashReporter.start` ile başlatmanız gerekir. Bu şekilde başlatıldıktan sonra crashpad denetimcisi tüm süreçlerden çökmeleri toplar. Yine de `crashReporter.start`'ı renderer veya çoçuk süreçlerden çağırmanız gerekir, aksi halde çokmeler `companyName`, `productName` veya `ekstra` bilgiler olmadan toplanır.

### `crashReporter.getLastCrashReport()`

[`CrashReport`](structures/crash-report.md) döndürür:

Son çökme raporunun numarasını ve tarihini döndürür. Eğer hiçbir rapor gönderilmediyse veya çökme raporlayıcı başlamadıysa, `null` döner.

### `crashReporter.getUploadedReports()`

[`CrashReport[]`](structures/crash-report.md) döndürür:

Tüm yüklenmiş çökme raporlarını döndürür. Her rapor ilgili tarih ve numarayı da içerir.

### `crashReporter.getUploadToServer()` *Linux* *macOS*

`Boolean` döndürür - Raporların sunucuya gönderilip gönderilmesiyle ilgilidir. `start` metodu ile ya da `setUploadToServer` metodu ile değerini değiştirin.

**Not:** Bu API sadece ana süreç tarafından çağrılabilir.

### `crashReporter.setUploadToServer(uploadToServer)` *Linux* *macOS*

* `uploadToServer` Boolean *macOS* - Raporlar sunucuya gönderilsin mi

Normalda bu kullanıcı seçeneklerinden kontrol edilir. Eğer daha önce `start` çağrılmışsa herhangi bir etkisi yoktur.

**Not:** Bu API sadece ana süreç tarafından çağrılabilir.

### `crashReporter.addExtraParameter(key, value)` *macOS*

* `key` Katar - Parametre anahtarı, 64 karakterden az olmak zorundadır.
* `value` String - Parameter value, must be less than 64 characters long.

Çökme raporu ile birlikte gönderilmesi için ek bir parametre girin. The values specified here will be sent in addition to any values set via the `extra` option when `start` was called. This API is only available on macOS, if you need to add/update extra parameters on Linux and Windows after your first call to `start` you can call `start` again with the updated `extra` options.

### `crashReporter.removeExtraParameter(key)` *macOS*

* `key` Katar - Parametre anahtarı, 64 karakterden az olmak zorundadır.

Remove a extra parameter from the current set of parameters so that it will not be sent with the crash report.

### `crashReporter.getParameters()`

See all of the current parameters being passed to the crash reporter.

## Çökme Raporu verisi

Çökme raporlarlayıcısı aşağıdaki verileri `submitURL` adresine `multipart/form-data` `POST` olarak yollayacaktır:

* `ver` Katar - Electron versiyonu.
* `platform` Katar - örneğin. 'win32'.
* `process_type` Katar - örneğin. 'renderer'.
* `guid` Katar - örneğin. '5e1286fc-da97-479e-918b-6bfb0c3d1c72'
* `_version` Katar - `package.json` içerisindeki versiyon.
* `_productName` Katar - `crashReporter` `options` objesi içerisindeki ürün ismi.
* `prod` Katar - Arkadaki temel ürünün ismi. Bu durum için Electron.
* `_companyName` Katar - `crashReporter` `options` objesi içerisindeki şirket ismi.
* `upload_file_minidump` Dosya - `minidump` formatında çökme raporu.
* `crashReporter``options` objesi içerisindeki `extra`'nın tüm birinci seviye özellikleri.