vis.fairicube.eu subdomain
we soon have our raster data web visualization in place and therefore need a subdomain

vis.fairicube.eu
that links to 

https://github.com/FAIRiCUBE/webvisualization/settings/pages

from what I been reading, some configuration has to be in place ?
https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site
subdomain and redirection has been established
many thanks for the fast help, but I dont think that this exactly what we need. 
now the subdomain links to the [config/admin] part of the GitHub pages, not the content. I was trying to go through 
https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site
but I dont really know what to take from their, I was hoping, someone from EOX knows?
`vis.fairicube.eu`  links to  `https://github.com/FAIRiCUBE/webvisualization/settings/pages`  
exactly as you requested

So, please describe what you want us to do, or what do you expect as a result to be shown at `vis.fairicube.eu` 
did you read (and understand) : https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site
?

after reading a bit more, it looks like the subdomain has to point to

http://fairicube.github.io/

wheras the configuration has to be done under 
https://github.com/FAIRiCUBE/webvisualization/settings/pages
so that our GitHub project knows it owns GitHub pages....
Apologies for the misunderstanding, we thought you need a redirect but what is actually needed is a DNS entry. Anyways, the DNS entry is now in place and points to the deployed pages of the repository.
no worries, I did not exactly know how to phrase the issue correctly but was relying on technical expertise to translate it correctly. which just happenend, many thanks...