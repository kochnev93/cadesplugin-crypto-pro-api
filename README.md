# async-cadesplugin-extended

Библиотека предоставляет hасширенный функционал API для работы с cadesplugin

Форк библиотеки [cadesplugin-crypto-pro-api](https://github.com/smodean/cadesplugin-crypto-pro-api)

## Install

`npm i async-cadesplugin-extended`

## API

### about()

Выводит информацию о верисии плагина

### getCertsList()

Получает массив активных сертификатов

### getValidCertificates()

Получает массив активных и валидных сертификатов

### getFirstValidCertificate()

Получает первый активный и валидный сертификат

### currentCadesCert(thumbprint)

Получает сертификат по thumbprint значению сертификата

### getHash(base64)

Получает хэш по алгоритму GOST_3411_2012_512

### signHash512(thumbprint, hash, signOption)

Подписывает хэш полученный по алгоритму GOST_3411_2012_512

### coSignHash512(thumbprint, hash, signature, signOption)

Добавление параллельной подписи по алгоритму GOST_3411_2012_512

### signHash256(thumbprint, hash, signOption)

Подписывает хэш полученный по алгоритму GOST_3411_2012_256

### coSignHash256(thumbprint, hash, signature, signOption)

Добавление параллельной подписи по алгоритму GOST_3411_2012_256

### getCert(thumbprint)

Получает сертификат по thumbprint значению сертификата.
С помощью этой функции в сертификате доступны методы для парсинга информации о сертификате

### signBase64(thumbprint, base64, type)

Подписать строку в формате base64

### signXml(thumbprint, xml, cadescomXmlSignatureType)

Подписать строку в формате XML

### verifyBase64(signedMessage, base64)

Проверка подписи строки в формате base64

### signFile(thumbprint, base64, type, signOption)

Подпись файла в формате base64

## Custom certs format API

### friendlySubjectInfo()

Возвращает распаршенную информацию о строке subjectInfo

### friendlyIssuerInfo()

Возвращает распаршенную информацию о строке issuerInfo

### friendlyValidPeriod()

Возвращает распаршенную информацию об объекте validPeriod

### possibleInfo(subjectIssuer)

Функция формирует ключи и значения в зависимости от переданного параметра
Доступные параметры 'subjectInfo' и 'issuerInfo'

### friendlyDate(date)

Формирует дату от переданного параметра

### isValid()

Прозиводит проверку на валидность сертификата

## Custom signers format API

### parseSubject()

Возвращает распаршенную информацию о строке subject

### parseIssuer()

Возвращает распаршенную информацию о строке issuer

## Usage

```js
import getCadespluginAPI from 'async-cadesplugin';

/**
 * @async
 * @function signString
 * @description пример создания подписи строки
 */
async function signString() {
  try {
    const base64DataToSign = btoa('Hello world');
    const api = await getCadespluginAPI();
    const certificate = await api.getFirstValidCertificate();
    const signature = await api.signBase64(certificate.thumbprint, base64DataToSign);

    console.log(signature);
  } catch (error) {
    console.log(error.message);
  }
}

/**
 * @async
 * @function signFile
 * @description пример создания подписи файла
 */
async function signFile() {
  try {
    const fileToBase64 = 'SGVsbG8gd29ybGQh'; // файл в base64
    const api = await getCadespluginAPI();
    const certificate = await api.getFirstValidCertificate();
    const signature = await api.signFile(certificate.thumbprint, fileToBase64, true, 1);
    // true=откреплённая подпись false=прикреплённая подпись.
    // 0 CAPICOM_CERTIFICATE_INCLUDE_CHAIN_EXCEPT_ROOT Сохраняет все сертификаты цепочки за исключением корневого.
    // 1 CAPICOM_CERTIFICATE_INCLUDE_WHOLE_CHAIN Сохраняет полную цепочку.
    // 2 CAPICOM_CERTIFICATE_INCLUDE_END_ENTITY_ONLY Сертификат включает только конечное лицо
    console.log(signature);
  } catch (error) {
    console.log(error.message);
  }
}

/**
 * @async
 * @function signFileHash512
 * @description пример создания подписи хэша файла
 */
async function signFileHash512() {
  try {
    const fileHash = '2EB8457BFCCC3897D180CB3964996CF96F27DCC4E6B900CC35E4974E8AD8D7D9D1E70798B07879A7ABBFFFCA1B565D3F943E8DD0D1F1B3E525CDB5F2C2BBE4DF'; // хэш файла по алгоритму ГОСТ Р 34.11-2012 с длиной 512 бит
    const api = await getCadespluginAPI();
    const certificate = await api.getFirstValidCertificate(); // Для корректной работы необходим алгоритм хэширования ключа, аналогичный хэшу файла (в данном случае 512 бит)
    const signature = await api.signHash512(certificate.thumbprint, fileHash, 1);
    // 0 CAPICOM_CERTIFICATE_INCLUDE_CHAIN_EXCEPT_ROOT Сохраняет все сертификаты цепочки за исключением корневого.
    // 1 CAPICOM_CERTIFICATE_INCLUDE_WHOLE_CHAIN Сохраняет полную цепочку.
    // 2 CAPICOM_CERTIFICATE_INCLUDE_END_ENTITY_ONLY Сертификат включает только конечное лицо
    console.log(signature);
  } catch (error) {
    console.log(error.message);
  }
}

/**
 * @async
 * @function coSignFileHash512
 * @description пример создания параллельной подписи хэша файла
 */
async function coSignFileHash512() {
  try {
    const fileHash = '2EB8457BFCCC3897D180CB3964996CF96F27DCC4E6B900CC35E4974E8AD8D7D9D1E70798B07879A7ABBFFFCA1B565D3F943E8DD0D1F1B3E525CDB5F2C2BBE4DF'; // хэш файла по алгоритму ГОСТ Р 34.11-2012 с длиной 512 бит
    const api = await getCadespluginAPI();
    const prevSignature = '---'; // Значение предыдущей подписи
    const certificate = await api.getFirstValidCertificate();
    const signature = await api.coSignHash512(certificate.thumbprint, fileHash, prevSignature, 1);
    console.log(signature);
  } catch (error) {
    console.log(error.message);
  }
}

```

### License

MIT ©
