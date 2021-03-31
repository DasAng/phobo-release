# AWS S3 actions

- [List objeccts](#list-objects)

---

## List objects

This action will list all the files and folders inside the specified S3 bucket.

`Regex`:

```shell
/aws s3 list objects: bucket=([a-zA-Z0-9_.-]{3,63})\s*(?=(?:.*prefix="(?<prefix>[^"]+)")?)(?=(?:.*maxkeys=(?<maxkeys>\d+))?)(?=(?:.*fetchowner=(?<fetchowner>true|false))?)(?=(?:.*delimiter="(?<delimiter>[^"]+)")?)(?=(?:.*fetchall=(?<fetchall>true|false))?)/i
```

`match signature`:

The matching is case insensitive and can appear anywhere within the sentence. The optional parameters can appear in any order.

```shell
aws s3 list objects: bucket=<bucket_name> [prefix="<prefix>"] [maxkeys=<max_keys>] [fetchowner=<fetchowner>] [delimiter="<delimiter>"] [fetchall=<fetchall>]
```

- **`bucket`**: this parameter is required and is the name of the S3 bucket to list
- `prefix`: specify only files and folders under a prefix. You can use an [Intrinsic expression](#intrinsic_expression.md) for this parameter. This parameter is optional
- `maxkeys`: maximum number of items included in the list. This parameter is optional
- `fetchowner`: if you want to return owner field with each key in the result then set the fetch owner field to true. This parameter is optional
- `delimiter`: a delimiter is a character you use to group keys. You can use an [Intrinsic expression](#intrinsic_expression.md) for this parameter. This parameter is optional
- `fetchall`: this value can either be *true* or *false*. If true then all objects will be fetched if not it will fetch only the specified objects up to the specified *maxkeys* or 1000. This parameter is optional.

The result of the action is a JSON response looking like the following:

```json
{
    "Contents": [
        {
            "ETag": "\"70ee1738b6b21e2c8a43f3a5ab0eee71\"", 
            "Key": "happyface.jpg", 
            "LastModified": <Date Representation>, 
            "Size": 11, 
            "StorageClass": "STANDARD"
        }, 
        {
            "ETag": "\"becf17f89c30367a9a44495d62ed521a-1\"", 
            "Key": "test.jpg", 
            "LastModified": <Date Representation>, 
            "Size": 4192256, 
            "StorageClass": "STANDARD"
        }
    ], 
    "IsTruncated": true, 
    "KeyCount": 2, 
    "MaxKeys": 2, 
    "Name": "examplebucket", 
    "NextContinuationToken": "1w41l63U0xa8q7smH50vCxyTQqdxo69O3EmK28Bi5PcROI4wI/EyIJg==", 
    "Prefix": ""
}
```

Example of usage:

- List all items for bucket *mybucket*

    > `aws s3 list objects: bucket=mybucket`

- List all items for bucket *mybucket* with prefix *mydata/*

    > `aws s3 list objects: bucket=mybucket prefix="mydata/"`

- List all items for bucket *mybucket* with prefix *mydata/* and max keys *10*

    > `aws s3 list objects: bucket=mybucket prefix="mydata/" maxkeys=10`

- List all items for bucket *mybucket* with prefix *mydata/* and max keys *10* and delimiter */*

    > `aws s3 list objects: bucket=mybucket prefix="mydata/" maxkeys=10 delimieter="/"`

- List all items for bucket *mybucket* with prefix *mydata/* and max keys *10* and delimiter */* and fetchowner to *true*

    > `aws s3 list objects: bucket=mybucket prefix="mydata/" maxkeys=10 delimieter="/" fetchowner=true`

- List all items for bucket *mybucket* with prefix *mydata/* and max keys *10* and delimiter */* and fetchowner to *true* and fetch all objects

    > `aws s3 list objects: bucket=mybucket prefix="mydata/" maxkeys=10 delimieter="/" fetchowner=true fetchall=true`

References:

[ListObjectsV2](https://docs.aws.amazon.com/AmazonS3/latest/API/API_ListObjectsV2.html)