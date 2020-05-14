# cache_manager_dio
Simple extension for [flutter_cache_manager](https://pub.dev/packages/flutter_cache_manager) that uses [dio](https://pub.dev/packages/dio) as http client.

Most things are done in the background, the extension itself adds, using Dio as the Http client the possibility of simpler cache manager configuration, which is not possible using the default implementation of CacheManager.
Due to the use of Dio, you have even more control over what happens with your requests.

### Example

With this extension you have 2 ways to add custom headers, to your every request.

1. Using settings:
```Dart
final settings = Settings(headers: {
  "Authorization": "Bearer MY_TOKEN",
  ...
})

DioCacheManager(Dio(), settings)
```

2. Using custom dio interceptor:
```Dart
final Dio = Dio();

dio.interceptors.add(InterceptorsWrapper(
  onRequest: (RequestOptions options) async {
    options.headers['Authorization'] = "Bearer MY_TOKEN";

    return options;
  },
));

DioCacheManager(Dio(), settings);
```

### Settings

```dart
class Settings {
  /// Cache duration, can be overriden by http headers when [allowHttpCacheHeaders] is true
  /// Default value: 7 days.
  Duration cacheDuration;

  /// Indicates if cache manager should respect http cache headers
  /// Default value: true.
  bool allowHttpCacheHeaders;

  /// Name of folder in temponary directory
  /// Default value: 'DioCacheManager'
  String cacheKey;

  /// Default request headers with lower priority than passed in [CacheManager] function
  Map<String, String> headers;

  /// Describes how http headers are treated
  /// [HeaderPolicy.Merge]: default headers have lower priority and are merged with those passed in [CacheManager] function
  /// [HeaderPolicy.Replace]: default headers are replaced with those passsed in [CacheManager] function
  /// Default value: HeaderPolicy.Merge
  HeaderPolicy headerPolicy;
}
```

### License

This extension uses MIT license
