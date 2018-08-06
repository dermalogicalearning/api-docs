# DLA API Specifications

The following API endpoints will be used to integrate class registration information between DLA and our legacy education systems (V2, DES).

DLA will offer features for both online learning and conventional learning in the classroom. As such, the API will need to reflect class bookings from both concepts.


All calls are made using POST method. Using GET method will return 405 status. All endpoints that accept JSON data return the following in case JSON is invalid:

HTTP status 400

``` json
{
	"message": "Invalid json data."
}
```

# Classes

## Create class -- this should be called whenever a new Class is created in V2 or DES

`POST /api/create-class/`

#### JSON params:


``` json
{
	"external_class_id": "12345",
	"name": "cls name",
	"description": "cls description",
	"class_type": "classroom"
}
```

Notes:

Value for class_type can be either "online" or "classroom"

#### Response for success

HTTP status 200

``` json
{
	"message": "created"
}
```

#### Response for missing data

HTTP status 422

``` json
{
	"description": [
		{
			"message": "This field is required.",
			"code": "required"
		}
	],
	"name": [
		{
			"message": "This field is required.",
			"code": "required"
		}
	],
	"external_class_id": [
		{
			"message": "This field is required.",
			"code": "required"
		}
	],
	"class_type": [
		{
			"message": "This field is required.",
			"code": "required"
		}
	]
}
```

#### Response for duplicate class

HTTP status 422

``` json
{
	"message": "Duplicate external class ID."
}
```

## Update class -- this should be called whenever a Class is updated in V2 or DES.

`POST /api/update-class/EXTERNAL_CLASS_ID/`

#### JSON params:


``` json
{
	"name": "cls name updated",
	"description": "cls description updated",
	"class_type": "classroom"
}
```

Notes:

Value for class_type can be either "online" or "classroom"

#### Response for success

HTTP status 200

``` json
{
	"message": "updated"
}
```

#### Response for missing data

HTTP status 422

``` json
{
	"description": [
		{
			"message": "This field is required.",
			"code": "required"
		}
	],
	"name": [
		{
			"message": "This field is required.",
			"code": "required"
		}
	],
	"class_type": [
		{
			"message": "This field is required.",
			"code": "required"
		}
	]
}
```

#### Response for unknown class

HTTP status 404

``` json
{
	"message": "Class with given external ID not found."
}
```

## Deactivate class

`POST /api/deactivate-class/EXTERNAL_CLASS_ID/`

#### Response for success

HTTP status 200

``` json
{
	"message": "deactivated"
}
```

#### Response for unknown class

HTTP status 404

``` json
{
	"message": "Class with given external ID not found."
}
```

# Cohorts

Please note that the concept of Cohort is a one-to-one with the Class_Schedule table in V2 and the Workshop_Instance table in DES. The external_cohort_id should be the unique id for each of these tables in their respective systems.

## Create cohort

`POST /api/create-cohort/`

#### JSON params:


``` json
{
	"external_class_id": "12345",
	"external_cohort_id": "coh12345",
	"external_instructor_profile_id": "prof12345",
	"external_instructor_first_name": "fname",
	"external_instructor_last_name": "lname",
	"external_instructor_email": "fname.lname@example.com",
	"start_date": "11/11/2017 08:00:00",
	"end_date": "11/11/2017 17:30:00",
	"name": "cohort name"
}
```

Notes:

Dates start_date and end_date are in the current timezone of the application. For US version timezone is US/Pacific.

If there is already a user with the given external_instructor_profile_id it will be set as the cohort moderator.

If the user does not exist, it will be created using the given data.


#### Response for success

HTTP status 200

``` json
{
	"message": "created"
}
```

#### Response for missing data

HTTP status 422

``` json
{
	"external_class_id": [
		{
			"message": "This field is required.",
			"code": "required"
		}
	],
	"external_instructor_profile_id": [
		{
			"message": "This field is required.",
			"code": "required"
		}
	],
	"external_instructor_email": [
		{
			"message": "This field is required.",
			"code": "required"
		}
	],
	"end_date": [
		{
			"message": "This field is required.",
			"code": "required"
		}
	],
	"start_date": [
		{
			"message": "This field is required.",
			"code": "required"
		}
	],
	"external_instructor_first_name": [
		{
			"message": "This field is required.",
			"code": "required"
		}
	],
	"name": [
		{
			"message": "This field is required.",
			"code": "required"
		}
	],
	"external_cohort_id": [
		{
			"message": "This field is required.",
			"code": "required"
		}
	],
	"external_instructor_last_name": [
		{
			"message": "This field is required.",
			"code": "required"
		}
	]
}
```

#### Response for unknown class

HTTP status 404

``` json
{
	"message": "Class with given external ID not found."
}
```

#### Response for duplicate cohort

HTTP status 422

``` json
{
	"message": "Duplicate external cohort ID."
}
```

## Update cohort -- this should be called whenever a Class_Schedule or Workshop Instance is updated.

`POST /api/update-cohort/EXTERNAL_COHORT_ID/`

#### JSON params:

``` json
{
	"external_instructor_profile_id": "prof12345",
	"external_instructor_first_name": "fname",
	"external_instructor_last_name": "lname",
	"external_instructor_email": "fname.lname@example.com",
	"start_date": "11/11/2017 09:30:00",
	"end_date": "11/11/2017 18:45:00",
	"name": "cohort name updated"
}
```

Notes:

Dates start_date and end_date are in the current timezone of the application. For US version timezone is US/Pacific.

If there is already a user with the given external_instructor_profile_id it will be set as the cohort moderator.

If the user does not exist, it will be created using the given data.

#### Response for success

HTTP status 200

``` json
{
	"message": "updated"
}
```

#### Response for missing data

HTTP status 422

``` json
{
	"external_instructor_profile_id": [
		{
			"message": "This field is required.",
			"code": "required"
		}
	],
	"external_instructor_email": [
		{
			"message": "This field is required.",
			"code": "required"
		}
	],
	"end_date": [
		{
			"message": "This field is required.",
			"code": "required"
		}
	],
	"start_date": [
		{
			"message": "This field is required.",
			"code": "required"
		}
	],
	"external_instructor_first_name": [
		{
			"message": "This field is required.",
			"code": "required"
		}
	],
	"name": [
		{
			"message": "This field is required.",
			"code": "required"
		}
	],
	"external_instructor_last_name": [
		{
			"message": "This field is required.",
			"code": "required"
		}
	]
}
```

#### Response for unknown cohort

HTTP status 404

``` json
{
	"message": "Cohort not found"
}
```

## Delete cohort -- this should be called whenever a Class Instance is deleted.

`POST /api/delete-cohort/EXTERNAL_COHORT_ID/`

#### Response for success

HTTP status 200

``` json
{
	"message": "deleted"
}
```

#### Response for unknown cohort

HTTP status 404

``` json
{
	"message": "Cohort not found"
}
```


# Enrollments -- this should be called whever a student registers for a Class.

## Enroll student in class

`POST /api/enroll-student-in-class/`

#### JSON params:

``` json
{
	"external_class_id": "12345",
	"external_cohort_id": "coh12345",
	"external_profile_id": "prof12345678",
	"first_name": "fname",
	"last_name": "lname",
	"email": "fname.lname@example.com",
	"mobile": "1234567890",					 	// This field is now optional
	"external_registration_id": "11223344"
}
```

Notes:

Field external_cohort_id is optional.

If external_cohort_id is given, user will be enrolled into specified cohort.

If external_cohort_id is not given, first available cohort will be assigned to user (if available)

If there is already a user with the given external_profile_id that user will be enrolled.

If the user does not exist, it will be created using the given data.

#### Response for success

HTTP status 200

``` json
{
	"message": "enrolled"
}
```

#### Response for missing data

HTTP status 422


``` json
{
    "first_name":[
        {
            "message":"This field is required.",
            "code":"required"
        }
    ],
    "last_name":[
        {
            "message":"This field is required.",
            "code":"required"
        }
    ],
    "external_class_id":[
        {
            "message":"This field is required.",
            "code":"required"
        }
    ],
    "external_profile_id":[
        {
            "message":"This field is required.",
            "code":"required"
        }
    ],
    "external_registration_id":[
        {
            "message":"This field is required.",
            "code":"required"
        }
    ],    
    "email":[
        {
            "message":"This field is required.",
            "code":"required"
        }
    ]
}
```

#### Response for unknown/unavailable cohort

HTTP status 404

``` json
{
	"message": "Cohort not found."
}
```

#### Response for unknown class

HTTP status 404

``` json
{
	"message": "Class with given external ID not found."
}
```





## Remove student from a class

`POST /api/remove-student-from-class/`

#### JSON params:

``` json
{
	"external_registration_id": "11223344"
}
```


#### Response for success

HTTP status 200

``` json
{
	"message": "removed"
}
```

#### Response for missing data

HTTP status 422


``` json
{
	"external_registration_id": [
		{
			"message": "This field is required.",
			"code": "required"
		}
	]
}
```

#### Response for unknown/unavailable registration id

HTTP status 404

``` json
{
	"message": "Registration with given external ID not found."
}
```








