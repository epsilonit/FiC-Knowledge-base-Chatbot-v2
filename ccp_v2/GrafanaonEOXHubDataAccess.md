Grafana on EOX-Hub: Data Access
I startet investigating Grafana and tried to create dashboards to data stored on the EOX Hub.
A significant amount of data and results from our Use Case are stored as csv files and is not connected to or stored in a database, we are using the suggested PlugIn to source our data (Infinity data source) which can handle csv tables.
One can either upload from a local source (not Hub but machine) or provide an URL to a file, which in our case would be a link to the file in EoxHub e.g. "https://eoxhub.fairicube.eu/user/sonjastndl/.../.../s3/.../data/data.csv" that cannot be accessed because it requires a login.

Is there any other way to provide Hub-stored data?

Do I interpret your question correctly, that you want to publicly share your Grafana dashboard and thus use a method without authentication? Reading the Grafana documentation (https://grafana.com/docs/grafana/latest/dashboards/dashboard-public/) I believe you can use any of the available methods and still publicly share the resulting dashboard.

Sorry if I interpreted you question wrongly.
It's less about sharing Dashboards. If possible, we preferredly would like to use processed and stored on the EOX Hubto create these Dashbords. So far I understand that the EOX Hub Grafana instance is not directly connected to the Hub and one would have to provide a Web URL to the data (if not database). Connection then fails due to authentification. 

If this is not possible, we need to consider creating a database in order to create dashboards. 


Hi Sonja, i just found this in our FAIRiCUBE documentation

https://fairicube--8.org.readthedocs.build/en/8/guide/storage/

The currently available [data access] services are:

-- Direct access to the objects via S3 API
-- HTTPS file access provided via [AWS CloudFront CDN](https://aws.amazon.com/cloudfront/), which supports capabilities like custom domains, security, availability, and even adding own ones via lambda functions
-- Time-limited sharing via [presigned URLs](https://docs.aws.amazon.com/AmazonS3/latest/userguide/using-presigned-url.html)

maybe we have to learn more about that ourselves?