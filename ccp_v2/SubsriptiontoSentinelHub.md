Subsription to SentinelHub
Is there a plan for the subscription to the SentinelHub for the UC3 and UC5? Do we share it with the UC1 or we get new ones? 
In this way we can go on and access the ingested data that we need. 

Thank you. 
To my memory, the agreement was to share between UC, as no need to pay double. can you confirm?
yes, confirmed Thank you. 

Do we get credentials from the UC1 or is it all already set for the UC3 and 5? 
can you confirm that EOX has to manage the subscription for UC5 on their end or can you simply share credentials for the SentinelHub access?
Hi, yes. Credentials for SH were set as environment variables (by EOX Admin) in the EOX Hub. At least for UC4 it was done this way, if I remember right.  : can you please have someone from EOX do this as soon as possible? many thanks!
the credentials for SentinelHub have been added to UC5 and UC3 configurations
Pls. don't forget to also configure the local variables: 

```
config.instance_id = os.environ.get("SH_INSTANCE_ID")
config.sh_client_id = os.environ.get("SH_CLIENT_ID")
config.sh_client_secret = os.environ.get("SH_CLIENT_SECRET")
```
Thank you Christian 
Hi 
I do not have the environmental variables for SH configured (UC3)
hast du die variablen bei dir eh definiert?
```
config = SHConfig()
config.instance_id = os.environ.get("SH_INSTANCE_ID")
config.sh_client_id = os.environ.get("SH_CLIENT_ID")
config.sh_client_secret = os.environ.get("SH_CLIENT_SECRET")
```
Naja, die Funktion os.environ.get findet keine SH variablen und wenn ich im Terminal env aufrufe finde ich sie auch nicht
I know that this is "internal discussion" but the language in all GitHub issues should be english (^_^). otherwise the FAIR aspect might have difficulties...
Correct, I apologize, sometimes I am not aware of the language shift, here the translation of the above:
The function os.environ.get() does not find any SH associated variable, neither is it stored in env when I check via terminal
there was a [configuration](https://github.com/FAIRiCUBE/flux-config/commit/41311f640368e05d052796567c0c11a51b2e32f9) missing. it should be available now (tested in UC3).
Thank you, all set now!