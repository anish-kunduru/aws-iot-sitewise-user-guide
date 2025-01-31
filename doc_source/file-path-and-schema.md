# File paths and schemas of data saved in the cold tier<a name="file-path-and-schema"></a>

AWS IoT SiteWise stores your data in the cold tier by replicating time series, including measurements, metrics, transforms and aggregates, and also asset and asset model definitions\. The following describes the file paths and schemas of data that is sent to the cold tier\.

**Topics**
+ [Equipment data \(measurements\)](#measurements-file-path-and-schema)
+ [Metrics, transforms, and aggregates](#metrics-file-path-and-schema)
+ [Asset metadata](#asset-metadata)
+ [Asset hierarchy metadata](#asset-hierarchy-metadata)

## Equipment data \(measurements\)<a name="measurements-file-path-and-schema"></a>

AWS IoT SiteWise exports equipment data \(measurements\) to the cold tier once every six hours\. Raw data is saved in the cold tier in the [Apache AVRO](https://avro.apache.org) \(`.avro`\) format\.

### File path<a name="measurements-file-path"></a>

To generate file paths of raw data in the cold tier, AWS IoT SiteWise uses the following template\.

```
{keyPrefix}/raw/startYear={startYear}/startMonth={startMonth}/startDay={startDay}/seriesBucket={seriesBucket}/raw_{timeseriesId}_{startTimestamp}_{quality}.avro
```

Every file path to raw data in Amazon S3 contains the following components\.

#### <a name="w492aac33c15b7b5b9b1b1"></a>


| Path component | Description | 
| --- | --- | 
|  `keyPrefix`  |  The Amazon S3 prefix that you specified in the AWS IoT SiteWise storage configuration\. Amazon S3 uses the prefix as a folder name in the bucket\.  | 
|  `raw`  |  The folder that stores time series data from equipment \(measurements\)\. The `raw` folder is saved in the prefix folder\.  | 
|  `seriesBucket`  |  A hexadecimal number between 00 and ff\. This number is derived from `timeSeriesId`\. This partition is used to increase throughput when AWS IoT SiteWise writes to the cold tier\. When you use Amazon Athena to run queries, you can use the partition for fine\-grain partitioning to improve query performance\. `seriesBucket` and `timeSeriesBucket` in the asset metadata are the same number\.  | 
|  `startYear`  |  The year of the exclusive start time associated with the time series data\.  | 
|  `startMonth`  |  The month of the exclusive start time associated with the time series data\.  | 
|  `startDay`  |  The day of the month of the exclusive start time associated with the time series data\.  | 
|  `fileName`  |  The file name uses the underscore \(\_\) character as a delimiter to separate the following: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/iot-sitewise/latest/userguide/file-path-and-schema.html) The file is saved in the `.avro` format by using the [Snappy](https://opensource.google/projects/snappy) compression\.  | 

**Example file path to raw data in the cold tier**  
`keyPrefix/raw/startYear=2021/startMonth=1/startDay=2/seriesBucket=a2/raw_7020c8e2-e6db-40fa-9845-ed0dddd4c77d_95e63da7-d34e-43e1-bc6f-1b490154b07a_1609577700_GOOD.avro`

### Fields<a name="measurements-fields"></a>

The schema of raw data that is exported to the cold tier contains the following fields\.

#### <a name="w492aac33c15b7b7b5b1"></a>


| Field name | Supported types | Default type | Description | 
| --- | --- | --- | --- | 
|  `seriesId`  |  `string`  |  N/A  |  The ID that identifies the time series data from equipment \(measurements\)\. You can use this field to join raw data and asset metadata in queries\.  | 
|  `timeInSeconds`  |  `long`  |  N/A  |  The timestamp date, in seconds, in the Unix epoch format\. Fractional nanosecond data is provided by `offsetInNanos`\.  | 
|  `offsetInNanos`  |  `long`  |  N/A  |  The nanosecond offset from `timeInSeconds`\.  | 
|  `quality`  |  `string`  |  N/A  |  The quality of the time series value\.  | 
|  `doubleValue`  |  `double` or `null`  |  `null`  |  Time series data of type double \(floating point number\)\.  | 
|  `stringValue`  |  `string` or `null`  |  `null`  |  Time series data of type string \(sequence of characters\)\.  | 
|  `integerValue`  |  `int` or `null`  |  `null`  |  Time series data of type integer \(whole number\)\.  | 
|  `booleanValue`  |  `boolean` or `null`  |  `null`  |  Time series data of type Boolean \(true or false\)\.  | 
|  `jsonValue`  |  `string` or `null`  |  `null`  |  Time series data of type JSON \(complex data types stored as a string\)\.  | 
|  `recordVersion`  |  `long` or `null`  |  `null`  |  The version number for the record\. You can use the version number to select the latest record\. Newer records have larger version numbers\.  | 

**Example raw data in the cold tier**  

```
{"seriesId":"e9687d2a-0dbe-4f65-9ed6-6f443cba41f7_95e63da7-d34e-43e1-bc6f-1b490154b07a","timeInSeconds":1625675887,"offsetInNanos":0,"quality":"GOOD","doubleValue":{"double":0.75},"stringValue":null,"integerValue":null,"booleanValue":null,"jsonValue":null,"recordVersion":null}
{"seriesId":"e9687d2a-0dbe-4f65-9ed6-6f443cba41f7_95e63da7-d34e-43e1-bc6f-1b490154b07a","timeInSeconds":1625675889,"offsetInNanos":0,"quality":"GOOD","doubleValue":{"double":0.69},"stringValue":null,"integerValue":null,"booleanValue":null,"jsonValue":null,"recordVersion":null}
{"seriesId":"e9687d2a-0dbe-4f65-9ed6-6f443cba41f7_95e63da7-d34e-43e1-bc6f-1b490154b07a","timeInSeconds":1625675890,"offsetInNanos":0,"quality":"GOOD","doubleValue":{"double":0.66},"stringValue":null,"integerValue":null,"booleanValue":null,"jsonValue":null,"recordVersion":null}
{"seriesId":"e9687d2a-0dbe-4f65-9ed6-6f443cba41f7_95e63da7-d34e-43e1-bc6f-1b490154b07a","timeInSeconds":1625675891,"offsetInNanos":0,"quality":"GOOD","doubleValue":{"double":0.92},"stringValue":null,"integerValue":null,"booleanValue":null,"jsonValue":null,"recordVersion":null}
{"seriesId":"e9687d2a-0dbe-4f65-9ed6-6f443cba41f7_95e63da7-d34e-43e1-bc6f-1b490154b07a","timeInSeconds":1625675892,"offsetInNanos":0,"quality":"GOOD","doubleValue":{"double":0.73},"stringValue":null,"integerValue":null,"booleanValue":null,"jsonValue":null,"recordVersion":null}
```

## Metrics, transforms, and aggregates<a name="metrics-file-path-and-schema"></a>

AWS IoT SiteWise exports metrics, transforms, and aggregates to the cold tier once every six hours\. Metrics, transforms, and aggregates are saved in the cold tier in the [Apache AVRO](https://avro.apache.org) \(`.avro`\) format\.

### File path<a name="metrics-file-path"></a>

To generate file paths of metrics, transforms, and aggregates in the cold tier, AWS IoT SiteWise uses the following template\.

```
{keyPrefix}/agg/startYear={startYear}/startMonth={startMonth}/startDay={startDay}/seriesBucket={seriesBucket}/agg_{timeseriesId}_{startTimestamp}_{quality}.avro
```

Every file path to metrics, transforms, and aggregates in Amazon S3 contains the following components\.

#### <a name="w492aac33c15b9b5b9b1b1"></a>


| Path component | Description | 
| --- | --- | 
|  `keyPrefix`  |  The Amazon S3 prefix that you specified in the AWS IoT SiteWise storage configuration\. Amazon S3 uses the prefix as a folder name in the bucket\.  | 
|  `agg`  |  The folder that stores time series data from metrics\. The `agg` folder is saved in the prefix folder\.  | 
|  `seriesBucket`  |  A hexadecimal number between 00 and ff\. This number is derived from `timeSeriesId`\. This partition is used to increase throughput when AWS IoT SiteWise writes to the cold tier\. When you use Amazon Athena to run queries, you can use the partition for fine\-grain partitioning to improve query performance\. `seriesBucket` and `timeSeriesBucket` in the asset metadata are the same number\.  | 
|  `startYear`  |  The year of the exclusive start time associated with the time series data\.  | 
|  `startMonth`  |  The month of the exclusive start time associated with the time series data\.  | 
|  `startDay`  |  The day of the month of the exclusive start time associated with the time series data\.  | 
|  `fileName`  |  The file name uses the underscore \(\_\) character as a delimiter to separate the following: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/iot-sitewise/latest/userguide/file-path-and-schema.html) The file is saved in the `.avro` format by using the [Snappy](https://opensource.google/projects/snappy) compression\.  | 

**Example file path to metrics in the cold tier**  
`keyPrefix/agg/startYear=2021/startMonth=1/startDay=2/seriesBucket=a2/agg_7020c8e2-e6db-40fa-9845-ed0dddd4c77d_95e63da7-d34e-43e1-bc6f-1b490154b07a_1609577700_GOOD.avro`

### Fields<a name="metrics-fields"></a>

The schema of metrics, transforms, and aggregates that are exported to the cold tier contains the following fields\.

#### <a name="w492aac33c15b9b7b5b1"></a>


| Field name | Supported types | Default type | Description | 
| --- | --- | --- | --- | 
|  `seriesId`  |  `string`  |  N/A  |  The ID that identifies the time series data from equipment, metrics, or transforms\. You can use this field to join raw data and asset metadata in queries\.  | 
|  `timeInSeconds`  |  `long`  |  N/A  |  The timestamp date, in seconds, in the Unix epoch format\. Fractional nanosecond data is provided by `offsetInNanos`\.  | 
|  `offsetInNanos`  |  `long`  |  N/A  |  The nanosecond offset from `timeInSeconds`\.  | 
|  `quality`  |  `string`  |  N/A  |  The quality by which to filter asset data\.  | 
|  `resolution`  |  `string`  |  N/A  |  The time interval over which to aggregate data\.  | 
|  `count`  |  `double` or `null`  |  `null`  |  The total number of data points for the given variables over the current time interval\.  | 
|  `average`  |  `double` or `null`  |  `null`  |  The mean of the given variables' values over the current time interval\.  | 
|  `min`  |  `double` or `null`  |  `null`  |  The minimum of the given variables' values over the current time interval\.  | 
|  `max`  |  `boolean` or `null`  |  `null`  |  The maximum of the given variables' values over the current time interval\.  | 
|  `sum`  |  `string` or `null`  |  `null`  |  The sum of the given variables' values over the current time interval\.  | 
|  `recordVersion`  |  `long` or `null`  |  `null`  |  The version number for the record\. You can use the version number to select the latest record\. Newer records have larger version numbers\.  | 

**Example Metric data in the cold tier**  

```
{"seriesId":"f74c2828-5317-4df3-ba16-6d41b5bcb531","timeInSeconds":1637334060,"offsetInNanos":0,"quality":"GOOD","resolution":"PT1M","count":31.0,"average":{"double":16.0},"min":{"double":1.0},"max":{"double":31.0},"sum":{"double":496.0},"recordVersion":null}
{"seriesId":"f74c2828-5317-4df3-ba16-6d41b5bcb531","timeInSeconds":1637334120,"offsetInNanos":0,"quality":"GOOD","resolution":"PT1M","count":29.0,"average":{"double":46.0},"min":{"double":32.0},"max":{"double":60.0},"sum":{"double":1334.0},"recordVersion":null}
{"seriesId":"f74c2828-5317-4df3-ba16-6d41b5bcb531","timeInSeconds":1637334540,"offsetInNanos":0,"quality":"GOOD","resolution":"PT1M","count":31.0,"average":{"double":16.0},"min":{"double":1.0},"max":{"double":31.0},"sum":{"double":496.0},"recordVersion":null}
{"seriesId":"f74c2828-5317-4df3-ba16-6d41b5bcb531","timeInSeconds":1637334600,"offsetInNanos":0,"quality":"GOOD","resolution":"PT1M","count":29.0,"average":{"double":46.0},"min":{"double":32.0},"max":{"double":60.0},"sum":{"double":1334.0},"recordVersion":null}
{"seriesId":"f74c2828-5317-4df3-ba16-6d41b5bcb531","timeInSeconds":1637335020,"offsetInNanos":0,"quality":"GOOD","resolution":"PT1M","count":31.0,"average":{"double":16.0},"min":{"double":1.0},"max":{"double":31.0},"sum":{"double":496.0},"recordVersion":null}
```

## Asset metadata<a name="asset-metadata"></a>

When you enable AWS IoT SiteWise to export data to the cold tier for the first time, asset metadata is exported to the cold tier\. After the initial configuration, AWS IoT SiteWise exports asset metadata to the tier only when you change asset model definitions or asset definitions\. Asset metadata is saved in the cold tier in the [Newline Delimited JSON](http://ndjson.org/) \(`.ndjson`\) format\.

### File path<a name="asset-metadata-file-path"></a>

To generate file paths of asset metadata in the cold tier, AWS IoT SiteWise uses the following template\.

```
{keyPrefix}/asset_metadata/asset_{assetId}.ndjson
```

Every file path to asset metadata in the cold tier contains the following components\.

#### <a name="w492aac33c15c11b5b9b1"></a>


| Path component | Description | 
| --- | --- | 
|  `keyPrefix`  |  The Amazon S3 prefix that you specified in the AWS IoT SiteWises storage configuration\. Amazon S3 uses the prefix as a folder name in the bucket\.  | 
|  `asset_metadata`  |  The folder that stores asset metadata\. The `asset_metadata` folder is saved in the prefix folder\.  | 
|  `fileName`  |  The file name uses the underscore \(\_\) character as a delimiter to separate the following: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/iot-sitewise/latest/userguide/file-path-and-schema.html) The file is saved in the `.ndjson` format\.  | 

**Example file path to asset metadata in the colder tier**  
`keyPrefix/asset_metadata/asset_35901915-d476-4dca-8637-d9ed4df939ed.ndjson`

### Fields<a name="asset-metadata-fields"></a>

The schema of asset metadata that is exported to the cold tier contains the following fields\.

#### <a name="w492aac33c15c11b7b5b1b1"></a>


| Field name | Description | 
| --- | --- | 
|  `assetId`  |  The ID of the asset\.  | 
|  `assetName`  |  The name of the asset\.  | 
|  `assetModelId`  |  The ID of the asset model used to create this asset\.  | 
|  `assetModelName`  |  The name of the asset model\.  | 
|  `assetPropertyId`  |  The ID of the asset property\.  | 
|  `assetPropertyName`  |  The name of the asset property\.  | 
|  `assetPropertyDataType`  |  The data type of the asset property\.  | 
|  `assetPropertyUnit`  |  The unit of the asset property \(for example, `Newtons` and `RPM`\)\.  | 
|  `assetPropertyAlias`  |  The alias that identifies the asset property, such as an OPC\-UA server data stream path \(for example, `/company/windfarm/3/turbine/7/temperature`\)\.  | 
|  `timeSeriesId`  |  The ID that identifies the time series data from equipment, metrics, or transforms\. You can use this field to join raw data and asset metadata in queries\.  | 
|  `timeSeriesBucket`  |  A hexadecimal number between 00 and ff\. This number is derived from `timeSeriesId`\. This partition is used to increase throughput when AWS IoT SiteWise writes to the cold tier\. When you use Amazon Athena to run queries, you can use the partition for fine\-grain partitioning to improve query performance\. `timeSeriesBucket` and `seriesBucket` in the file path to raw data are the same number\.  | 
|  `assetCompositeModelDescription`  |  The description of the composite model\.  | 
|  `assetCompositeModelName`  |  The name of the composite model\.  | 
|  `assetCompositeModelType`  |  The type of the composite model\. For alarm composite models, this type is `AWS/ALARM`\.  | 
|  `assetCreationDate`  |  The date the asset was created, in Unix epoch time\.  | 
|  `assetLastUpdateDate`  |  The date the asset was last updated, in Unix epoch time\.  | 
|  `assetStatusErrorCode`  |  The error code\.  | 
|  `assetStatusErrorMessage`  |  The error message\.  | 
|  `assetStatusState`  |  The current status of the asset\.  | 

**Example asset metadata in the cold tier**  

```
{"assetId":"7020c8e2-e6db-40fa-9845-ed0dddd4c77d","assetName":"Wind Turbine Asset 2","assetModelId":"ec1d924f-f07d-444f-b072-e2994c165d35","assetModelName":"Wind Turbine Asset Model","assetPropertyId":"95e63da7-d34e-43e1-bc6f-1b490154b07a","assetPropertyName":"Temperature","assetPropertyDataType":"DOUBLE","assetPropertyUnit":"Celsius","assetPropertyAlias":"USA/Washington/Seattle/WT2/temp","timeSeriesId":"7020c8e2-e6db-40fa-9845-ed0dddd4c77d_95e63da7-d34e-43e1-bc6f-1b490154b07a","timeSeriesBucket":"f6","assetArn":null,"assetCompositeModelDescription":null,"assetCompositeModelName":null,"assetCompositeModelType":null,"assetCreationDate":1619466323,"assetLastUpdateDate":1623859856,"assetStatusErrorCode":null,"assetStatusErrorMessage":null,"assetStatusState":"ACTIVE"}
{"assetId":"7020c8e2-e6db-40fa-9845-ed0dddd4c77d","assetName":"Wind Turbine Asset 2","assetModelId":"ec1d924f-f07d-444f-b072-e2994c165d35","assetModelName":"Wind Turbine Asset Model","assetPropertyId":"c706d54d-4c11-42dc-9a01-63662fc697b4","assetPropertyName":"Pressure","assetPropertyDataType":"DOUBLE","assetPropertyUnit":"KiloPascal","assetPropertyAlias":"USA/Washington/Seattle/WT2/pressure","timeSeriesId":"7020c8e2-e6db-40fa-9845-ed0dddd4c77d_c706d54d-4c11-42dc-9a01-63662fc697b4","timeSeriesBucket":"1e","assetArn":null,"assetCompositeModelDescription":null,"assetCompositeModelName":null,"assetCompositeModelType":null,"assetCreationDate":1619466323,"assetLastUpdateDate":1623859856,"assetStatusErrorCode":null,"assetStatusErrorMessage":null,"assetStatusState":"ACTIVE"}
{"assetId":"7020c8e2-e6db-40fa-9845-ed0dddd4c77d","assetName":"Wind Turbine Asset 2","assetModelId":"ec1d924f-f07d-444f-b072-e2994c165d35","assetModelName":"Wind Turbine Asset Model","assetPropertyId":"8cf1162f-dead-4fbe-b468-c8e24cde9f50","assetPropertyName":"Max Temperature","assetPropertyDataType":"DOUBLE","assetPropertyUnit":null,"assetPropertyAlias":null,"timeSeriesId":"7020c8e2-e6db-40fa-9845-ed0dddd4c77d_8cf1162f-dead-4fbe-b468-c8e24cde9f50","timeSeriesBucket":"d7","assetArn":null,"assetCompositeModelDescription":null,"assetCompositeModelName":null,"assetCompositeModelType":null,"assetCreationDate":1619466323,"assetLastUpdateDate":1623859856,"assetStatusErrorCode":null,"assetStatusErrorMessage":null,"assetStatusState":"ACTIVE"}
```

## Asset hierarchy metadata<a name="asset-hierarchy-metadata"></a>

When you enable AWS IoT SiteWise to save data the in cold tier for the first time, asset hierarchy metadata is exported to the cold tier\. After the initial configuration, AWS IoT SiteWise exports asset hierarchy metadata to the cold tier only when you make changes to asset model or asset definitions\. Asset hierarchy metadata is saved in the cold tier in the [Newline Delimited JSON](http://ndjson.org/) \(`.ndjson`\) format\.

### File path<a name="asset-hierarchy-metadata-file-path"></a>

To generate file paths of asset hierarchy metadata in the cold tier, AWS IoT SiteWise uses the following template\.

```
{keyPrefix}/asset_hierarchy_metadata/{parentAssetId}_{hierarchyId}.ndjson
```

Every file path to asset hierarchy metadata in the cold tier contains the following components\.

#### <a name="w492aac33c15c13b5b9b1b1"></a>


| Path component | Description | 
| --- | --- | 
|  `keyPrefix`  |  The Amazon S3 prefix that you specified in the AWS IoT SiteWise storage configuration\. Amazon S3 uses the prefix as a folder name in the bucket\.  | 
|  `asset_hierarchy_metadata`  |  The folder that stores asset hierarchy metadata\. The `asset_hierarchy_metadata` folder is saved in the prefix folder\.  | 
|  `fileName`  |  The file name uses the underscore \(\_\) character as a delimiter to separate the following: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/iot-sitewise/latest/userguide/file-path-and-schema.html) The file is saved in the `.ndjson` format\.  | 

**Example file path to asset hierarchy metadata in the cold tier**  
`keyPrefix/asset_hierarchy_metadata/35901915-d476-4dca-8637-d9ed4df939ed_c5b3ced8-589a-48c7-9998-cdccfc9747a0.ndjson`

### Fields<a name="asset-hierarchy-metadata-fields"></a>

The schema of asset hierarchy metadata that is exported to the cold tier contains the following fields\.

#### <a name="w492aac33c15c13b7b5b1b1"></a>


| Field name | Description | 
| --- | --- | 
|  `sourceAssetId`  |  The ID of the source asset in this asset relationship\.  | 
|  `targetAssetId`  |  The ID of the target asset in this asset relationship\.  | 
|  `hierarchyId`  |  The ID of the hierarchy\.  | 
|  `associationType`  |  The association type of this asset relationship\.  The value must be `CHILD`\. The target asset is a child asset of the source asset\.  | 

**Example asset hierarchy metadata in the cold tier**  

```
{"sourceAssetId":"80388e72-2284-44fb-9c89-bfbaf0dfedd2","targetAssetId":"2b866c25-0c74-4750-bdf5-b73683c8a2a2","hierarchyId":"bbed9f59-0412-4585-a61d-6044db526aee","associationType":"CHILD"}
{"sourceAssetId":"80388e72-2284-44fb-9c89-bfbaf0dfedd2","targetAssetId":"6b51246e-984d-460d-bc0b-470ea47d1e31","hierarchyId":"bbed9f59-0412-4585-a61d-6044db526aee","associationType":"CHILD"}
```

**To view your data in the cold tier**

1. Navigate to the [Amazon S3 console](https://console.aws.amazon.com/s3/)\.

1. In the navigation pane, choose **Buckets**, and then choose your Amazon S3 bucket\.

1. Navigate to the folder that contains the raw data, asset metadata, or asset hierarchy metadata\.

1. Select the files, and then from **Actions**, choose **Download**\.