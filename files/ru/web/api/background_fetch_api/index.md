---
title: Background Fetch API
slug: Web/API/Background_Fetch_API
page-type: web-api-overview
tags:
  - API
  - Overview
  - Reference
  - Background Fetch API
  - Experimental
browser-compat: api.BackgroundFetchManager
---

{{DefaultAPISidebar("Background Fetch API")}} {{SeeCompatTable}}

**Background Fetch API** предоставляет способ управления загрузками, которые могут занимать продолжительное время, например фильмы, аудиофайлы и программное обеспечение.

## Концепт и использование

Довольно часто, когда веб-приложение требует от пользователя скачивания больших файлов, это создаёт проблему, поскольку пользователю необходимо оставаться подключённым к странице до завершения скачивания. Если же соединение пропадает, например пользователь закрывает вкладку или уходит со страницы, то скачивание прерывается.

{{domxref("Background Synchronization API")}} даёт сервис-воркерам возможность отложить обработку до тех пор, пока пользователь не подключится; однако такой подход не может быть использован для длительных задач, таких как скачивание больших файлов. Background Sync требует чтобы сервис-воркер оставался активным до завершения запроса, но для экономии заряда батареи и предотвращения возникновения нежелательных задач, выполняемых в фоновом режиме, браузер в какой-то момент завершит задачу.

Background Fetch API решает это проблему. Он позволяет веб-разработчику сообщать браузеру, что некоторые запросы нужно выполнять в фоновом режиме, например, когда пользователь нажимает на кнопку "скачать видеофайл". Затем браузер выполняет запрос "видимым" для пользователя способом, отображая прогресс и предоставляя способ отменить скачивание. После завершения загрузки браузер открывает сервис-воркер, и ваше приложение может, по необходимости, сделать что-то с ответом.

Background Fetch API позволит выполнить запрос, если пользователь начал процесс в автономном режиме. Как только соединение будет установлено, Background Fetch API начнёт скачивание. Если пользователь переходит в автономный режим, процесс приостанавливается до тех пор, пока пользователь не подключится к сети снова.

## Интерфейсы

- {{domxref("BackgroundFetchManager")}}
  - : Коллекция пар ключ-значение, где ключи это идентификаторы фоновых запросов, а значения - объекты {{domxref("BackgroundFetchRegistration")}}.
- {{domxref("BackgroundFetchRegistration")}}
  - : Представляет фоновый запрос.
- {{domxref("BackgroundFetchRecord")}}
  - : Представляет отдельный запрос и ответ.
- {{domxref("BackgroundFetchEvent")}}
  - : Тип события, который передаётся в `onbackgroundfetchabort` и `onbackgroundfetchclick`.
- {{domxref("BackgroundFetchUpdateUIEvent")}}
  - : Тип события, который передаётся в `onbackgroundfetchsuccess` и `onbackgroundfetchfail`.

## Примеры

Перед использованием Background Fetch проверьте совместимость с браузерами.

```css
if (!("BackgroundFetchManager" in self)) {
  // Обеспечьте резервный способ загрузки
}
```

Использование Background Fetch требует зарегистрированного сервис-воркера. Затем вызов `backgroundFetch.fetch()` инициирует запрос. Этот вызов возвращает промис, результатом обработки которого будет {{domxref("BackgroundFetchRegistration")}}.

Background Fetch может запрашивать несколько файлов. В нашем примере происходит запрос MP3 и JPEG файлов. Это позволяет скачать сразу пакет файлов, которые пользователь видит как один элемент (например, подкаст и обложку).

```js
navigator.serviceWorker.ready.then(async (swReg) => {
  const bgFetch = await swReg.backgroundFetch.fetch(
    "my-fetch",
    ["/ep-5.mp3", "ep-5-artwork.jpg"],
    {
      title: "Эпизод 5: Интересные дела.",
      icons: [
        {
          sizes: "300x300",
          src: "/ep-5-icon.png",
          type: "image/png",
        },
      ],
      downloadTotal: 60 * 1024 * 1024,
    }
  );
});
```

Вы можете найти демонстрационное приложение, которое реализует Background Fetch [здесь](https://glitch.com/edit/#!/bgfetch-http203?path=public%2Fclient.js%3A191%3A45).

## Спецификации

{{Specifications}}

## Совместимость с браузерами

{{Compat}}

## Смотрите также

- [Представляем Background Fetch (англ.)](https://developer.chrome.com/blog/background-fetch/)
- [Background Fetch - HTTP 203 (англ.)](https://www.youtube.com/watch?v=cElAoxhQz6w)