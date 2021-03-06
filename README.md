# Perfect - MariaDB Connector [简体中文](README.zh_CN.md)

<p align="center">
    <a href="http://perfect.org/get-involved.html" target="_blank">
        <img src="http://perfect.org/assets/github/perfect_github_2_0_0.jpg" alt="Get Involed with Perfect!" width="854" />
    </a>
</p>

<p align="center">
    <a href="https://github.com/PerfectlySoft/Perfect" target="_blank">
        <img src="http://www.perfect.org/github/Perfect_GH_button_1_Star.jpg" alt="Star Perfect On Github" />
    </a>  
    <a href="http://stackoverflow.com/questions/tagged/perfect" target="_blank">
        <img src="http://www.perfect.org/github/perfect_gh_button_2_SO.jpg" alt="Stack Overflow" />
    </a>  
    <a href="https://twitter.com/perfectlysoft" target="_blank">
        <img src="http://www.perfect.org/github/Perfect_GH_button_3_twit.jpg" alt="Follow Perfect on Twitter" />
    </a>  
    <a href="http://perfect.ly" target="_blank">
        <img src="http://www.perfect.org/github/Perfect_GH_button_4_slack.jpg" alt="Join the Perfect Slack" />
    </a>
</p>

<p align="center">
    <a href="https://developer.apple.com/swift/" target="_blank">
        <img src="https://img.shields.io/badge/Swift-4.0-orange.svg?style=flat" alt="Swift 4.0">
    </a>
    <a href="https://developer.apple.com/swift/" target="_blank">
        <img src="https://img.shields.io/badge/Platforms-OS%20X%20%7C%20Linux%20-lightgray.svg?style=flat" alt="Platforms OS X | Linux">
    </a>
    <a href="http://perfect.org/licensing.html" target="_blank">
        <img src="https://img.shields.io/badge/License-Apache-lightgrey.svg?style=flat" alt="License Apache">
    </a>
    <a href="http://twitter.com/PerfectlySoft" target="_blank">
        <img src="https://img.shields.io/badge/Twitter-@PerfectlySoft-blue.svg?style=flat" alt="PerfectlySoft Twitter">
    </a>
    <a href="http://perfect.ly" target="_blank">
        <img src="http://perfect.ly/badge.svg" alt="Slack Status">
    </a>
</p>



This project provides a Swift wrapper around the MariaDB client library, enabling access to MariaDB database servers.

This package builds with Swift Package Manager and is part of the [Perfect](https://github.com/PerfectlySoft/Perfect) project. It was written to be stand-alone and so does not require PerfectLib or any other components.

Ensure you have installed and activated the latest Swift 4.0 tool chain.


## OS X Build Notes

### To install MariaDB connector:

```bash
 brew install mariadb-connector-c
```

You will have to deal with pkg-config file, such as: /usr/local/lib/pkgconfig/mariadb.pc, similar with below:

```bash
libdir=/usr/local/lib/mariadb
includedir=/usr/local/include/mariadb

Name: mariadb
Description: MariaDB Connector/C
Version: 5.5.1
Requires:
Libs: -L${libdir} -lmariadb  -ldl -lm -lpthread
Cflags: -I${includedir}
Libs_r: -L${libdir} -lmariadb -ldl -lm -lpthread
```

Then please also edit your ~/.bash_profile with the following line:

```
export PKG_CONFIG_PATH="/usr/local/lib/pkgconfig:/usr/lib/pkgconfig"
```

once done, reload this profile as

```
source ~/.bash_profile
```

To test if pkg-config is working, try command:

```
$ pkg-config mariadb --cflags --libs
```

## Linux Build Notes


Tests performed on Ubuntu 16.04. Prior to build this library, please ensure:

```
sudo apt-get install pkg-config libmariadb2 libmariadb-client-lgpl-dev  
```

Please also make sure the pkg-config file /usr/lib/pkgconfig/mariadb.pc specified for MariaDB should be MANUALLY added and corrected before building, possibly looks like this:

```bash
libdir=/usr/lib/x86_64-linux-gnu
includedir=/usr/include/mariadb

Name: mariadb
Description: MariaDB Connector/C
Version: 5.5.0
Requires:
Libs: -L${libdir} -lmariadb  -ldl -lm -lpthread
Cflags: -I${includedir}
Libs_r: -L${libdir} -lmariadb -ldl -lm -lpthread
```

To test if pkg-config is working, try command:

```
$ pkg-config mariadb --cflags --libs
```

## Building

Add this project as a dependency in your Package.swift file.

```
.Package(url:"https://github.com/PerfectlySoft/Perfect-MariaDB.git", 
majorVersion: 3)
```

Import required libraries:
```
import MariaDB
```

Setup the credentials for your connection:

```
let testHost = "127.0.0.1"
let testUser = "test"

// PLEASE change to whatever your actual password is before running these tests
let testPassword = "password"
let testSchema = "schema"
```

Create an instance of the MySQL class

```
	let dataMysql = MySQL()

    // need to make sure something is available.
    guard dataMysql.connect(host: testHost, user: testUser, password: testPassword ) else {
    // something wrong here
        return
    }

    defer {
        dataMysql.close()  // defer ensures we close our db connection at the end of this request
    }

    //set database to be used, this example assumes presence of a users table and run a raw query, return failure message on a error
    guard dataMysql.selectDatabase(named: testSchema) && dataMysql.query(statement: "select * from users limit 1") else {
    // something wrong here
        return
    }

    //store complete result set
    let results = dataMysql.storeResults()

    //setup an array to store results
    var resultArray = [[String?]]()

    while let row = results?.next() {
        resultArray.append(row)

    }

	print(resultArray)
```

Additionally, there are more complex Statement constructors, and potential object designs which can further abstract the process of interacting with your data.


## Further Information
For more information on the Perfect project, please visit [perfect.org](http://perfect.org).
