GDPR
Cookies — небольшой фрагмент данных, отправленный веб-сервером и хранимый на компьютере пользователя.
Начиная с версии 2.1 в ASP.NET Core была добавленая функциональность для соответствия некоторым приниципам GDPR (General Data Protection Regulation) Европейского Союза. Применение встроенного в ASP.NET Core 2.1 не означает, что ваше приложение автоматически полностью соблюдает GDPR.

1) В методе ConfigureServices производится настройка объекта CookiePolicyOptions;
Для свойства CheckConsentNeeded указывается лямбда-выражение, которое возвращает true (запрос согласия на установку кук). При этом свойству передается не просто значение типа bool в виде true, 
а делегат - некоторое действие, которое в качестве параметра принимает контекст запроса HttpContext и возвращает значение bool. 
А это значит, что мы можем определить более сложную логику установки свойства в зависимости от деталей контекста запроса (например, проверять принадлежит ли ip-адрес Евросоюзу).

2) В методе Configure добавляется в конвейер обработки запроса компонент CookiePolicyMiddleware: pp.UseCookiePolicy();
В проекте сразу имеется частичное представление баннера о согласии: _CookieConsentPartial.cshtml (Views/Shared)
Согласие на отслеживание кук: _CookieConsentPartial(_Layout.cshtml)

Если не принять соглашение - В этом случае баннер вверху страницы будет отображаться постоянно. Если отключено отслеживание, то TempData, а также куки сессий также не смогут функционировать.

Essential Cookies:
Как выше было сказано, что если пользователь не дал согласие, то мы не сможем установить куки. Однако есть так называемые essential cookies или особо важные куки, которые можно устаноить в любом случае. Для этого при установке подобных кук указывается свойство IsEssential = true.
