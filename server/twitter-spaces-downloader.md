# Twitter Spaces Downloader

## `Installation`

Installing from this GitHub repository

{% embed url="https://github.com/HitomaruKonpaku/twspace-crawler" %}
Follow the FULL INSTALLATION
{% endembed %}

After installation with&#x20;

```
npm install --global twspace-crawler
```

Create <mark style="color:blue;">.env</mark> and <mark style="color:blue;">config.json</mark> file in:

`/usr/local/lib/node_modules/twspace-crawler`

### Download a Space

1. Find the ID from the link to recorded Twitter Spaces such as this example:

https://twitter.com/i/spaces/<mark style="background-color:orange;">**XXXXXXXXXXXX**</mark>

From bash:

```
twspace-crawler --id XXXXXXXXXXXXX
```

Find the full link to playlist URL from the output in the bash, such as:

`twspace-crawler --url https://prod-fastly-ap-northeast-1.video.pscp.tv/Transcoding/v1/hls/1Nq1QFkYTQ4v1X4BTV_aJ_pFeQhYyuYXY7ykz5xB7v5NvGwFMJMKwnRBmxyi9twF4BZ90ZKks5wdGKqESVsjLw...cod`

Finally the file will be saved in \~/download

