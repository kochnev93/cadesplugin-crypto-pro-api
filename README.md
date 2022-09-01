# async-cadesplugin-extended

Библиотека предоставляет API для работы c cadesplugin и Крипто Про

Форк библиотеки [cadesplugin-crypto-pro-api](https://github.com/smodean/cadesplugin-crypto-pro-api)

## Install

`npm i async-cadesplugin-extended`

## API

### about()

Выводит информацию о верисии плагина и так далее

### getCertsList()

Получает массив активных сертификатов

### getValidCertificates()

Получает массив активных и валидных сертификатов

### getFirstValidCertificate()

Получает первый активный и валидный сертификат

### currentCadesCert(thumbprint)

Получает сертификат по thumbprint значению сертификата

### getHash()

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
 * @function sign
 * @description пример создания подписи
 */
async function sign() {
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
```

### License

MIT ©
