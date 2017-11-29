Simple Redis
============


[![Build Status](https://travis-ci.org/hadihabashi/sredis.svg?branch=master)](https://travis-ci.org/hadihabashi/sredis)
[![GoDoc](https://godoc.org/github.com/hadihabashi/sredis?status.svg)](http://godoc.org/github.com/hadihabashi/sredis)


A Few Change in Simple Redis Project By  xyproto
https://github.com/xyproto/simpleredis

Online API Documentation
------------------------

[godoc.org](http://godoc.org/github.com/hadihabashi/sredis)


Features and limitations
------------------------

* Supports simple use of lists, hashmaps, sets and key/values
* Deals mainly with strings
* Uses the [redigo](https://github.com/garyburd/redigo) package


Example usage
-------------

~~~go
package main

import (
	"log"

	redis "github.com/hadihabashi/sredis"
)

func main() {
	// Check if the redis service is up
	if err := redis.TestConnection(); err != nil {
		log.Fatalln("Could not connect to Redis. Is the service up and running?")
	}

	// Use instead for testing if a different host/port is up.
	// redis.TestConnectionHost("localhost:6379")

	// Create a connection pool, connect to the given redis server
	pool := redis.NewConnectionPool()

	// Use this for connecting to a different redis host/port
	// pool := redis.NewConnectionPoolHost("localhost:6379")

	// For connecting to a different redis host/port, with a password
	// pool := redis.NewConnectionPoolHost("password@redishost:6379")

	// Close the connection pool right after this function returns
	defer pool.Close()

	// Create a list named "greetings"
	list := redis.NewList(pool, "greetings")

	// Add "hello" to the list, check if there are errors
	if list.Add("hello") != nil {
		log.Fatalln("Could not add an item to list!")
	}

	// Get the last item of the list
	if item, err := list.GetLast(); err != nil {
		log.Fatalln("Could not fetch the last item from the list!")
	} else {
		log.Println("The value of the stored item is:", item)
	}

	// Remove the list
	if list.Remove() != nil {
		log.Fatalln("Could not remove the list!")
	}
}
~~~

Testing
-------

Redis must be up and running locally for the `go test` tests to work.


Timeout issues
--------------

If there are timeout issues when connecting to Redis, try consulting the Redis latency doctor on the server by running `redis-cli` and then `latency doctor`.


