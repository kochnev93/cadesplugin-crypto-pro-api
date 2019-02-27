# cadesplugin-api

Библиотека предоставляет API для работы c cadesplugin и Крипто Про

Форк библиотеки [cadesplugin-crypto-pro-api](https://github.com/smodean/cadesplugin-crypto-pro-api)

## Install

`npm i cadesplugin-api`

`yarn add cadesplugin-api`

## API

### about()

Выводит информацию о верисии плагина и так далее

### getCertsList()

Получает массив валидных сертификатов

### getFirstValidCertificate()

Получает первый валидный сертификат

### currentCadesCert(thumbprint)

Получает сертификат по thumbprint значению сертификата

### getCert(thumbprint)

Получает сертификат по thumbprint значению сертификата.
С помощью этой функции в сертификате доступны методы для парсинга информации о сертификате

### signBase64(thumbprint, base64, type)

Подписать строку в формате base64

### signXml(thumbprint, xml, cadescomXmlSignatureType)

Подписать строку в формате XML

### verifyBase64(signedMessage, base64)

Проверка подписи строки в формате base64

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

## Usage

```js
import getCadespluginAPI from 'cadesplugin-api';

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
