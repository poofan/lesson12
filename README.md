# Weather App
## Описание
Этот проект реализует клиент-серверную архитектуру для получения информации о погоде. Основные компоненты включают:

1. **WeatherProvider:** Загружает данные о погоде из API OpenWeatherMap.
2. **WeatherCache:** Потокобезопасный кеш, сохраняющий данные о погоде с периодом актуальности 5 минут.
3. **Юнит-тесты** для проверки работы WeatherCache с использованием Mockito.
## Требования
- **Java 17**
- **Maven**
- **Интернет-соединение для запросов к API OpenWeatherMap**
- **API-ключ OpenWeatherMap (зарегистрируйтесь на OpenWeatherMap для получения ключа)**
## Установка
Склонируйте репозиторий:

``
bash
git clone https://github.com/<ваш_профиль>/weather-app.git
cd weather-app
``
## Добавьте API-ключ в WeatherProvider:

**1.** Откройте файл WeatherProvider.java.
**2.** Вставьте ваш API-ключ в поле private final String apiKey = "YOUR_API_KEY";.
## Соберите проект:

``bash
mvn clean install
``
## Запуск
**1.** Запуск основного приложения
На данном этапе приложение не имеет интерфейса, но вы можете протестировать работу классов в отдельном main-методе. Пример:

``java
public class Main {
    public static void main(String[] args) {
        WeatherProvider provider = new WeatherProvider();
        WeatherCache cache = new WeatherCache(provider);
        WeatherInfo info = cache.getWeatherInfo(Moscow);
        if (info != null) {
            System.out.println(info);
        } else {
            System.out.println("Weather data not found.");
        }
    }
}
``
**2.** Скомпилируйте и запустите Main из вашей IDE.

## Запуск тестов
Выполните все тесты с помощью Maven:

``bash
mvn test
``
## Запуск тестов из IDE:

- Убедитесь, что тестовые классы находятся в папке src/test/java.
- Запустите тесты WeatherCacheTest из вашей IDE (например, в IntelliJ IDEA или Eclipse).
##Структура проекта
``css
weather-app
├── src
│   ├── main
│   │   ├── java
│   │   │   └── homework142
│   │   │       ├── WeatherCache.java
│   │   │       ├── WeatherInfo.java
│   │   │       ├── WeatherProvider.java
│   │   │       └── Main.java
│   └── test
│       ├── java
│       │   └── homework142
│       │       └── WeatherCacheTest.java
├── pom.xml
└── README.md
``
##Технологии
- **Java 17:** Основной язык разработки.
- **Spring RestTemplate:** Для выполнения HTTP-запросов.
- **JUnit 5:** Для модульного тестирования.
- **Mockito:** Для мокирования зависимостей.
## Примеры тестов
Тесты покрывают следующие сценарии:

**1.** Загрузка новой информации в кеш, когда данные отсутствуют.
**2.** Получение актуальной информации из кеша без запроса к интернету.
**3.** Обновление данных в кеше, если информация устарела.
**4.** бработка запроса для несуществующего города.
##Пример теста:

``java
@Test
void testLoadWeatherInfoFromProvider() {
    WeatherInfo mockInfo = new WeatherInfo.Builder()
            .setCity("Moscow")
            .setExpiryTime(LocalDateTime.now().plusMinutes(5))
            .build();
    when(mockProvider.get("Moscow")).thenReturn(mockInfo);
    WeatherInfo result = weatherCache.getWeatherInfo("Moscow");
    assertEquals(mockInfo, result);
    verify(mockProvider, times(1)).get("Moscow");
}
``
