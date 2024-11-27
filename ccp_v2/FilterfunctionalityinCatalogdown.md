Filter functionality in Catalog down? I just noticed that the additional filters are not working, added Title, filtered for %LGN%, got everything. 
![Screenshot from 2024-05-30 20-06-23](https://github.com/FAIRiCUBE/catalog/assets/28819736/3ff30512-8030-452a-93fd-04c525b1dd6b)
I can see it's working
![grafik](https://github.com/FAIRiCUBE/catalog/assets/11915304/2ad2bb4e-49f3-4661-8419-f2a38f0ee013)

Request copied out from browser (response partially visible in screen shot above):

`curl 'https://stacapi.eoxhub.fairicube.eu/collections/index/items?limit=12&filter-lang=cql2-text&filter=title+ILIKE+%27%25LGN%25%27' --compressed -H 'User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:126.0) Gecko/20100101 Firefox/126.0' -H 'Accept: application/geo+json' -H 'Accept-Language: en,de;q=0.7,en-US;q=0.7,es;q=0.2,fr;q=0.2,it;q=0.2,ro;q=0.2' -H 'Accept-Encoding: gzip, deflate, br, zstd' -H 'Origin: https://catalog.eoxhub.fairicube.eu' -H 'Connection: keep-alive' -H 'Referer: https://catalog.eoxhub.fairicube.eu/' -H 'Sec-Fetch-Dest: empty' -H 'Sec-Fetch-Mode: cors' -H 'Sec-Fetch-Site: same-site' -H 'Priority: u=1' -H 'TE: trailers'`
aha, I see, this is a bit confusing I admit.
So you are using the collection filtering feature (button highlighted on the right in the picture below) in stac-browser, while the solution that was implemented (title search) is using search functionality (left ).
Both components apparently are using the same filters list (even though they shouldn't).
![Screenshot from 2024-05-30 22-35-50](https://github.com/FAIRiCUBE/catalog/assets/28819736/6d9145d8-cfc6-49dd-90ea-6c31bad990db)

Thanks!

Issues at Item Level in #21