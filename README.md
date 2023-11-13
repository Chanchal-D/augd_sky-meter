# augd_sky-meter

Sky-meter is a synthetic endpoint checker. You can deploy this on your infra run checks from your infra and set alerts. Here we are using the go httptrace library.  
Currenly I have added Database support. The endpoints and HTTP output are now being saved in the Database. We also have a sentry integration to catch the runtime errors.
 Development is in progress
 ### [Visit the Website](https://sky-meter.skywalks.in)   
### 

 ## Alerting
 I have integrated SMTP and Opsgenie, more integrations are in the pipeline
 Currently, the project is under development. You may have to experience some glitches at this moment.

## Tested Environments
GO version: 1.18  
Postgres: 15.0 
### Tested OS
- Ubuntu 22.10 
- alpine(docker)
- macOS

We highly recommend running the app as a docker container. 
See Docker Hub Image 

## Environment variables
| Variable       | Type    | Example         |
|----------------|---------|-----------------|
| DnsServer      | string  | 8.8.8.8         |
| Port           | string  | 8000            |
| EmailPass      | string  | youremailpass   |
| EmailFrom      | string  | from@gmail.com  |
| EmailPort      | string  | 583             |
| EmailServer    | string  | smtp.gmail.com  |
| OpsgenieSecret | string  | examplesecret   |
| SentryDsn      | string  | exapledsnvalue  |
| Mode           | string  | prod            |
| DbUrl          | string  | host=localhost user=postgres password=postgres dbname=postgres port=5433 sslmode=disable             |




## Add URLs to check
To add a URL to monitoring is pretty simple. Create **settings.yml** to add your endpoints to the monitor. See an example of **settings.yml** below  
```sh
opegenie:
  enabled: false
email:
  enabled: true
groups:
- name: prod
  emails:
     - reviceremail@gmail.com
     - reciver@yahoo.com
- name: dev
  emails:
     - reviceremail@gmail.com
     - reciver@yahoo.com
domains:
- name: https://skywalks.in
  enabled: true
  timeout: 10
  skip_ssl: false
  frequency: 10
  group: dev
- name: https://sky-meter.skywalks.in
  enabled: true
  timeout: 10
  skip_ssl: false
  frequency: 60
  group: dev
- name: https://github.com
  enabled: true
  timeout: 10
  skip_ssl: false
  frequency: 60
  group: prod

- name: https://githcccubs.com
  enabled: true
  timeout: 10
  skip_ssl: false
  frequency: 60
  group: prod
```
> _timeout_ : Timeout of request in Millisecond (int)  
> _skip_ssl_: set flase if you want to skip the ssl verification (bool)  
> _frequency_: frequency of health check in secont (int)  
> _group_ : Group settings

## Run the Code
Clone the code
```sh  
$ git clone https://github.com/sooraj-sky/sky-meter.git
$ cd sky-meter
```  
Run the Postgres docker container (skip this step if you already have a database)
```sh  
$ docker-compose up -d
```  
Export ENV variables (Sentry will work only in dev mode)    
If Email is disabled on **settings.yml** the following variables are not needed.
1. EmailPass
2. EmailFrom
3. EmailPort
4. EmailServer

**SentryDsn** is only needed when **Mode=dev**

```sh
$ export DnsServer="8.8.8.8" #requied  
$ export DbUrl="host=localhost user=postgres password=postgres dbname=postgres port=5433 sslmode=disable"  #requied          
$ export EmailPass="your-pass-here" #requied when Email is Enabled  
$ export EmailFrom="youremail@server.com" #requied when Email is Enabled     
$ export EmailPort="587" #requied when Email is Enabled     
$ export EmailServer="smtp.server-address-here.com" #requied when Email is Enabled   
$ export OpsgenieSecret="your-opsgenie-key-here" #requied when Opsgenie is Enabled on settings.yml
$ export Mode="dev"  
$ export SentryDsn="your-DSn-key-here" #requied when Mode="dev"           
```
Run the project
```sh    
$ go run cmd/main.go  
```

## CI

Here I am using concourse CI for  the Main Branch

For release branch, I have GitHub Actions





