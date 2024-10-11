# Miner software API
This API will be served from the controler of the hashblades on both windows and ubuntu. 
# Disclaimer
This API documentation is ongoing and could experience some changes in the near future. Our firmware is already designed for this functionality, but we might perform changes like adding more security features or additional endpoints.
# Authentication
This api does not have any private endpoint only a public endpoint, therfore no authentication is required

# Endpoints
## get_info

This method returns all information on the hashblade's current state

Method: GET
Private: NO

Example request in curl:
> curl -XGET 'http://localhost/get_info'


Example of result:
```
{
	error : none
	result: {
		"hashrate": 1978556352,
		"hashrate10": 1958556352
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
				"hashrate": 1978556352,
				"temperature": 62.8,
				"faulty_chips": 0,
				"status": "OK"
			},
			...
		]
	}
}
```
