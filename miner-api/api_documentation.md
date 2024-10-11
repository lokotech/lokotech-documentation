# Miner firmware API
This API will be served from the Full-stack (FS1), Rack-stack (RS1), Single-tube (LS1) and Double-tube (LD1) miners
# Disclaimer
This API documentation is ongoing and could experience some changes in the near future. Our firmware is already designed for this functionality, but we might perform changes like adding more security features or additional endpoints.
# Authentication
All private endpoints require the addition of an "apiKey" parameter into the request url in this fashion:
> https://\<ip\>/\<private endpoint\>?apiKey=\<apiKey\>

The api will be using a self signed certificate to allow for secure communication, any device that interacts with the api should check that that certificate is valid and signed by lokotech. We will provide our certificate in this repository on release. In python the verification and requests can be done like this:

> response = requests.get('https://\<ip\>/\<private endpoint\>?apiKey=\<apiKey\>' verify='lokotech.crt')

All endpoints use HTTPS, however, it is important to check the certififcate for any request that will carry your private key, to avoid a malicious actor from extracting it doing a MITM attack.

# Endpoints
## get_info

This method returns all information on the hashblade's current state

Method: GET
Private: NO

Example request in curl:
> curl -XGET 'https://\<ip\>/get_info'


Example of result:
```
{
	error : none
	result: {
		"miner_name": "Host123 user",
		"hashrate": 2397835382,
		"hashrate10": 23565835382
		"pools": [
			{
				"url": "scrypt.stratum.powerpool.io",
				"port": 3333,
				"user": "test",
				"password": "x",
				"difficulty": 1024,
				"accepted": 954,
				"rejected": 1,
				"invalid": 0,
				"status": "OK"
			},
			....
		],
		"hashblades": [
			{
				"id": 1,
				"hashrate": 1934932512,
				"temperature": 51.8,
				"faulty_chips": 0,
				"status": "OK"
			},
			...
		]
	}
}
```


## shutdown

This method stops mining and performs a gracefull shutdown, will require physical interaction to start mining again

Method: GET
Private: YES

Example request in curl:
> curl -XGET 'https://\<ip\>/shutdown?apiKey=\<apiKey\>'


Example of result:
```
{
	error : none,
	result: true
}
```

## reboot

This method stops mining and performs a gracefull reboot, the host will come online again in 3 minutes

Method: GET
Private: YES

Example request in curl:
> curl -XGET 'https://\<ip\>/reboot?apiKey=\<apiKey\>'


Example of result:

```
{
	error : none,
	result: true
}
```



## change_pool

This method changes the pool configuration, it is saved in a persistant storage space so it will not be reset on reboot. The changes take effect immediately, and stops mining for a few seconds.

Method: POST
Private: YES

Example request in curl:
> curl -XPOST -d '{"poolID":0, "url":"scrypt.stratum.powerpool.io", "port":3333, "user":"test", "password":"x"}' 'https://\<ip\>/change_pool?apiKey=\<apiKey\>'


Example of result:

```
{
	error : none,
	result: true
}
```



## stop_mining

This method stops mining in software but doesn't turn off the host, can be started again with the endpoint "start_mining".

Method: GET
Private: YES

Example request in curl:
> curl -XGET 'https://\<ip\>/stop_mining?apiKey=\<apiKey\>'


Example of result:

```
{
	error : none,
	result: true
}
```


## start_mining

This method reverts the stop_mining method and starts mining after a few seconds

Method: GET
Private: YES

Example request in curl:
> curl -XGET 'https://\<ip\>/start_mining?apiKey=\<apiKey\>'


Example of result:

```
{
	error : none,
	result: true
}
```


# Errors
If there is an error in the execution of a command, the result value of the repsonce will be none, and the error value will be equal to the error code number that was experiences
|Error Code| Error Explanation |
|--|--|
| 500 | The program experienced a runtime error  |
| 101| Api key is invalid  |
| 102| One of the introduced parameters is illegal  |
| 103| Could not perform action  |
| 104| Action is disallowed by configuration  |

Example of responce with error:

```
{
	error : 101,
	result: none
}
```
