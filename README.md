# Nginx patch to allow gzip filter to keep weak ETag

Nginx gzip filter module removes ETag every time gzip compression being applied whether the ETag is strong or weak. However, a weak ETag validator doesn't require that the resources to be compared are byte-for-byte identical. Hence a weak ETag header should be kept during gzip compression. Actually the latest version of nginx (1.7.x) has ETag downgrade functionality instead of clearing all ETag. ( see [here](http://hg.nginx.org/nginx/rev/e491b26fa5a1) )

Dislike the latest version's approach, this patch simply prevents gzip filter from removing ETag when the ETag is weak.

## Installation

```
tar zxfv nginx-1.6.0.tar.gz
cd nginx-1.6.0
patch -p1 < ../nginx-1.6.x-gzip-filter-allow-weak-etag.patch
./configure ...
```

## References

* http://hg.nginx.org/nginx/rev/e491b26fa5a1
* http://tools.ietf.org/html/rfc7232#section-2.1
* http://en.wikipedia.org/wiki/HTTP_ETag#Strong_and_weak_validation
