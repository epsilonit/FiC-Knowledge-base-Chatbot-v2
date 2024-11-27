Do not add username:password to the rasdaman dashboard URL
For example in https://catalog.eoxhub.fairicube.eu/collections/index/items/ERA5_Land_monthly there is an "Additional Resources" section with a "Link to the rasdaman coverage description in XML". The URL contains a username:password, which should not be added as it does not work with the Web page it points to. On chrome it redirects to the login page, but on firefox it doesn't work. All in all, it should contain no username:password. any plans to fix this? If not let me know where the code is so I can remove those credentials, as it's annoying to fix manually for every catalog request.
Hi, this issue is fixed in this [commit](https://github.com/FAIRiCUBE/stac-GUI-front/commit/c74aef18be40c72ae2e48524d9c00222dc4beb29), but a new version is not released yet, I will create a new version by  Monday.
Great, thanks! is it possible to configure rasdaman so that GetCapabilities and DescribeCoverage do not need a user? This would make it far easier to share what we have on rasdaman without having to worry about people downloading stuff at our cost. certainly, I configured this and now GetCapabilities and DescribeCoverage do not require credentials.
Great! Many thanks!