# Using $aggregation with $lookup #
> $lookup performs a left outer join to another collection in the _same_ database

###Example 1: Use localField-foreignField matching to join###

```mongodb
/* 'Students' collection */
db.students.insertMany([
    { id: "STU1", firstName: "Tam", lastName: "Pham" },
    { id: "STU2", firstName: "Amber", lastName: "Stanley" },
    { id: "STU3", firstName: "Thomas", lastName: "Jenkins" },
    { id: "STU4", firstName: "Steven", lastName: "Rodriguez" },
    { id: "STU5", firstName: "Mildred", lastName: "Jimenez" }
])
```

```mongodb
/* 'Departments' collection */
db.departments.insertMany([
    { id: "DPCS", name: "Computer Science", student: "STU1" },
    { id: "DPCS", name: "Computer Science", student: "STU5" },
    { id: "DPLA", name: "Law", student: "STU2" },
    { id: "DPCH", name: "Chemistry", student: "STU4" },
    { id: "DPLA", name: "Chemistry", student: "STU3" }
])
```

```mongodb
/* Join those two collections */
db.students.aggregate([{
    $lookup: {
        from: "departments",
        as: "department_info",
        localField: "id",
        foreignField: "student"
    }
}
])
```
Result:
```json
/* 1 */
{
    "_id" : ObjectId("5d2df0465ccaf859729851ec"),
    "id" : "STU1",
    "firstName" : "Tam",
    "lastName" : "Pham",
    "department_info" : [ 
        {
            "_id" : ObjectId("5d2df0e75ccaf859729851f1"),
            "id" : "DPCS",
            "name" : "Computer Science",
            "student" : "STU1"
        }
    ]
}

/* 2 */
{
    "_id" : ObjectId("5d2df0465ccaf859729851ed"),
    "id" : "STU2",
    "firstName" : "Amber",
    "lastName" : "Stanley",
    "department_info" : [ 
        {
            "_id" : ObjectId("5d2df0e75ccaf859729851f3"),
            "id" : "DPLA",
            "name" : "Law",
            "student" : "STU2"
        }
    ]
}

/* 3 */
{
    "_id" : ObjectId("5d2df0465ccaf859729851ee"),
    "id" : "STU3",
    "firstName" : "Thomas",
    "lastName" : "Jenkins",
    "department_info" : [ 
        {
            "_id" : ObjectId("5d2df0e75ccaf859729851f5"),
            "id" : "DPLA",
            "name" : "Chemistry",
            "student" : "STU3"
        }
    ]
}

/* 4 */
{
    "_id" : ObjectId("5d2df0465ccaf859729851ef"),
    "id" : "STU4",
    "firstName" : "Steven",
    "lastName" : "Rodriguez",
    "department_info" : [ 
        {
            "_id" : ObjectId("5d2df0e75ccaf859729851f4"),
            "id" : "DPCH",
            "name" : "Chemistry",
            "student" : "STU4"
        }
    ]
}

/* 5 */
{
    "_id" : ObjectId("5d2df0465ccaf859729851f0"),
    "id" : "STU5",
    "firstName" : "Mildred",
    "lastName" : "Jimenez",
    "department_info" : [ 
        {
            "_id" : ObjectId("5d2df0e75ccaf859729851f2"),
            "id" : "DPCS",
            "name" : "Computer Science",
            "student" : "STU5"
        }
    ]
}
```