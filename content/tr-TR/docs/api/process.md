# işlem

> İşlem nesnesine uzantılar.

İşlem: [Main](../glossary.md#main-process), [Renderer](../glossary.md#renderer-process)

Elektron'un `process` nesnesi [Node.js `process` object](https://nodejs.org/api/process.html)'ten genişletilir. Aşağıdaki etkinlikleri, özellikleri ve yöntemleri ekler:

## Etkinlikler

### Etkinlik: 'yüklenen'

Elektron dahili başlatma komut dosyasını yüklediğinde ve web sayfası ya da ana komut dosyası yüklenmeye başladığında yayılır.

Düğüm entegrasyonu kapatıldığında kaldırılan düğüm genel sembollerini evrensel alana geri eklemek için önyükleme komut dosyası tarafından kullanılabilir:

```javascript
// preload.js
const _setImmediate = setImmediate
const _clearImmediate = clearImmediate
process.once('loaded', () => {
  global.setImmediate = _setImmediate
  global.clearImmediate = _clearImmediate
})
```

## Özellikler

### `process.defaultApp`

Bir `Boolean`. Uygulama varsayılan uygulamaya parametre olarak geçirilip başlatıldığında, bu özellik ana işlemde `true` olur, aksi takdirde `undefined` olur.

### `process.mas`

Bir `Boolean`. Mac App Store kurmak için, bu özellik `true` olur, diğer kurulumlar için `undefined` olur.

### `process.noAsar`

Uygulamanızın içindeki ASAR desteğini kontrol eden bir `Boolean`. Bunu `true` olarak ayarlamak düğümün dahili modüllerindeki `asar` arşivleri için olan desteği devre dışı bırakacaktır.

### `process.noDeprecation`

İtiraz uyarılarının `stderr`'a yazdırılıp yazdırılmadığını kontrol eden bir `Boolean`.  
Bunu `true` olarak ayarlamak itiraz uyarılarını susturacaktır. Bu özellik `--no-deprecation` komut satırı etiketi yerine kullanılır.

### `process.resourcesPath`

Kaynaklar dizininin yolunu temsil eden bir `String`.

### `process.throwDeprecation`

İtiraz uyarılarının istisna olarak atılıp atılmayacağını kontrol eden bir `Boolean`. Bunu `true` olarak ayarlamak itirazlar için hatalar oluşturacak. Bu özellik `--throw-deprecation` komut satırı etiketi yerine kullanılır.

### `process.traceDeprecation`

İtirazların yığın izini içeren `stderr`'a yazdırılıp yazdırılmadığını kontrol eden bir `Boolean`. Bunu `true` olarak ayarlamak itirazların yığın izlerini yazdıracak. Bu özellik `--trace-deprecation` komut satırı etiketi yerine kullanılır.

### `process.traceProcessWarnings`

İşlem uyarılarının yığın izini içeren `stderr`'a yazdırılıp yazdırılmadığını kontrol eden bir `Boolean`. Bunu `true` olarak ayarlamak işlem uyarılarının yığın izlerini yazdıracak (itirazlar dahil). Bu özellik `--trace-warnings` komut satırı etiketinin yerine kullanılmalıdır.

### `process.type`

Geçerli işlemin türünü temsil eden bir `String`, `"browser"` (örneğin ana işlem) ya da `"renderer"` olabilir.

### `process.versions.chrome`

Chrome versiyonu dizesini temsil eden bir `String`.

### `process.versions.electron`

Elektron versiyonu dizesini temsil eden bir `String`.

### `process.windowsStore`

Bir `Boolean`. Eğer uygulama bir Windows Store uygulaması (appx) olarak çalışıyorsa, bu özellik `true` olur, aksi takdirde `undefined` olur.

## Yöntemler

`process` nesnesi aşağıdaki yöntemleri içerir:

### `process.crash()`

Geçerli işlemin ana iş parçacığının çökmesine neden olur.

### `process.getCPUUsage()`

[`CPUUsage`](structures/cpu-usage.md)'a döner

### `process.getIOCounters()` *Windows* *Linux*

[`IOCounters`](structures/io-counters.md)'a döner

### `process.getProcessMemoryInfo()`

`Object`'e döner:

* `workingSetSize` Tamsayı - O anda gerçek fiziksel RAM'e sabitlenmiş bellek miktarı.
* `peakWorkingSetSize` Tamsayı - Gerçek fiziksel RAM'e sabitlenmiş maksimum bellek miktarı.
* `privateBytes` Tamsayı - Diğer işlemlerle paylaşılmayan bellek miktarı, JS yığını ya da HTML içeriği gibi.
* `sharedBytes` Tamsayı - İşlemler arasında paylaşılan bellek miktarı, genel olarak Elektron kodunun kendisi tarafından tüketilen bellek

Geçerli işlem hakkında bellek kullanımı istatistiklerini veren bir nesneye döner. Tüm istatistiklerin Kilobayt olarak raporlandığına dikkat edin.

### `process.getSystemMemoryInfo()`

`Object`'e döner:

* `total` Tamsayı - Sistemde kullanılabilir durumda olan fiziksel belleğin Kilobayt olarak toplam miktarı.
* `free` Tamsayı - Uygulamalar ve disk önbelleği tarafından kullanılmayan belleğin toplam miktarı.
* `swapTotal` Tamsayı - Sistemde kullanılabilir durumda olan takas belleğinin Kilobayt olarak toplam miktarı. *Windows* *Linux*
* `swapFree` Tamsayı - Sistemde kullanılabilir durumda olan boş takas belleğinin toplam miktarı. *Windows* *Linux*

Tüm sistem hakkında bellek kullanımı istatistiklerini veren bir nesneye döner. Tüm nesnelerin Kilobayt olarak raporlandığına dikkat edin.

### `process.hang()`

Geçerli işlemin ana iş parçacığının askıda kalmasına neden olur.

### `process.setFdLimit(maxDescriptors)` *macOS* *Linux*

* `maxDescriptors` Tamsayı

Dosya tanımlayıcısının düşük limitini veya OS yüksek limitini `maxDescriptors` olarak ayarlar, geçerli işlem için hangisi daha düşükse.