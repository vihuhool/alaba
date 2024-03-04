```
import kotlinx.coroutines.*
import java.net.HttpURLConnection
import java.net.URL

fun main() {
    val websites = listOf(
        "https://www.google.com",
        "https://www.facebook.com",
        "https://www.github.com",
        "https://www.twitter.com",
        "https://www.instagram.com",
        "https://yandex.ru",
        "https://lk.etu.ru",
        "https://www.gosuslugi.ru",
        "https://digital.etu.ru",
        "https://pikabu.ru"
    )

    runBlocking {
        val results = mutableListOf<Pair<String, Boolean>>()

        val jobs = websites.map { url ->
            async {
                val result = checkWebsite(url)
                results.add(url to result)
            }
        }

        jobs.awaitAll()

        results.forEach { (url, isAvailable) ->
            if (isAvailable) {
                println("Сайт $url доступен")
            } else {
                println("Сайт $url недоступен")
            }
        }
    }
}

suspend fun checkWebsite(url: String): Boolean = withContext(Dispatchers.IO) {
    try {
        val connection = URL(url).openConnection() as HttpURLConnection
        connection.requestMethod = "HEAD"
        connection.connectTimeout = 5000
        connection.readTimeout = 5000
        connection.responseCode == HttpURLConnection.HTTP_OK
    } catch (e: Exception) {
        false
    }
}
```
![image](https://github.com/vihuhool/alaba/assets/69204363/13904a9d-bc68-4735-a9e4-5f8ac9457ad6)
