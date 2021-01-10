SortOrder: 1
# Use Cases

## ads.txt
Your app helps site owners keep control over the their inventory in the market, by adding an authorized digital seller to the ads.txt file.

In this example we will add a seller publisher ID (`pub-0000000000000001`) to an ads.txt file.

Notice in the response that our content field now has two lines, separeted by new line (`\n`)
```
curl PATCH https://www.wixapis.com/promote-seo-txt-file-server/v2/ads \
    -H 'Content-Type: application/json;charset=UTF-8' \
    -H 'Authorization: <AUTH>' \
    -d '{"adsTxt": {"content": "google.com, pub-0000000000000001, DIRECT, f08c47fec0942fa0", "subdomain": "www"}}'
```
```json
{
    "adsTxt": {
        "content": "google.com, pub-0000000000000000, DIRECT, f08c47fec0942fa0\ngoogle.com, pub-0000000000000001, DIRECT, f08c47fec0942fa0",
        "default": false,
        "subdomain": "www"
    }
}
```

### Result
Finaly our ads.txt will look like this: (`www.example.com/ads.txt`)
```
google.com, pub-0000000000000000, DIRECT, f08c47fec0942fa0
google.com, pub-0000000000000001, DIRECT, f08c47fec0942fa0
```

## robots.txt
In order to increase privacy and security, your app can disallow pages of websites from being crawled. To do so, critical pages like the `/back-office` can be added to the site's robots.txt file.


First we get the current content:
```
curl GET https://www.wixapis.com/promote-seo-txt-file-server/v2/robots \
    -H 'Content-Type: application/json;charset=UTF-8' \
    -H 'Authorization: <AUTH>' \
    -H 'subdomain: www'
```
```json
{
    "robotsTxt": {
        "content": "User-agent: *\nAllow: /\n\n# Optimization for Google Ads Bot\nUser-Agent: AdsBot-Google-Mobile\nUser-Agent: AdsBot-Google\nDisallow: /_api/*\nDisallow: /_partials*\nDisallow: /pro-gallery-webapp/v1/galleries/*'",
        "default": true,
        "subdomain": "www"
    }
}
```

Then we will add a new line: `Disallow: /back-office`
```
curl PUT https://www.wixapis.com/promote-seo-txt-file-server/v2/robots \
    -H 'Content-Type: application/json;charset=UTF-8' \
    -H 'Authorization: <AUTH>' \
    -d '{"robotsTxt": {"content": "User-agent: *\nAllow: /\n\n# Optimization for Google Ads Bot\nUser-Agent: AdsBot-Google-Mobile\nUser-Agent: AdsBot-Google\nDisallow: /_api/*\nDisallow: /_partials*\nDisallow: /pro-gallery-webapp/v1/galleries/*\nDisallow: /back-office", "subdomain": "www"}}'
```
```json
{
    "robotsTxt": {
        "content": "User-agent: *\nAllow: /\n\n# Optimization for Google Ads Bot\nUser-Agent: AdsBot-Google-Mobile\nUser-Agent: AdsBot-Google\nDisallow: /_api/*\nDisallow: /_partials*\nDisallow: /pro-gallery-webapp/v1/galleries/*\nDisallow: /back-office",
        "default": true,
        "subdomain": "www"
    }
}
```

### Result
Finaly our robots.txt will look like this: (`www.example.com/robots.txt`)
```
User-agent: *
Allow: /

# Optimization for Google Ads Bot
User-Agent: AdsBot-Google-Mobile
User-Agent: AdsBot-Google
Disallow: /_api/*
Disallow: /_partials*
Disallow: /pro-gallery-webapp/v1/galleries/*'
Disallow: /back-office
```