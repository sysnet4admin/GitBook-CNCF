# GCR Performance

## Sample image (Big-size! 16GB)

```bash
[root@m-k8s ~]# docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
test                latest              5eff03f2dd32        3 days ago          16GB
```

## Docker Push &#x20;

* `gcr.io` hosts images in data centers in the United States, but the location may change in the future
* `us.gcr.io` hosts images in data centers in the United States, in a separate storage bucket from images hosted by `gcr.io`
* `eu.gcr.io` hosts the images in the European Union
* `asia.gcr.io` hosts images in data centers in Asia

{% hint style="info" %}
&#x20;Check to`time` command from Seoul to each of location
{% endhint %}

| Location     | gcr.io         | us.gcr.io      | asia.gcr.io   | eu.gcr.io      |
| ------------ | -------------- | -------------- | ------------- | -------------- |
| _`local-VM`_ | **13m19.497s** | **12m19.291s** | **12m2.058s** | **16m16.204s** |



## Docker Pull

* `gcr.io` hosts images in data centers in the United States, but the location may change in the future
* `us.gcr.io` hosts images in data centers in the United States, in a separate storage bucket from images hosted by `gcr.io`
* `eu.gcr.io` hosts the images in the European Union
* `asia.gcr.io` hosts images in data centers in Asia

| Location                  | gcr.io         | us.gcr.io      | asia.gcr.io    | eu.gcr.io      |
| ------------------------- | -------------- | -------------- | -------------- | -------------- |
| _**`us-central1-a`**_     | **13m54.196s** | **14m3.473s**  | **16m1.591s**  | **14m2.601s**  |
| _**`asia-northeast3-a`**_ | **13m41.583s** | **13m57.774s** | **13m33.008s** | **14m6.000s**  |
| _**`europe-west4-a`**_    | **13m50.628s** | **15m14.861s** | **15m57.273s** | **13m58.923s** |

