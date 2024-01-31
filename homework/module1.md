### question 1:

```
docker run --help
```

### qustion 2:

```
$ docker run -it python:3.9 bash
                                                   /# pip list
Package    Version
---------- -------
pip        23.0.1
setuptools 58.1.0
wheel      0.42.0
```

### question 3:
```sql
select count(*)
from public.green_taxi_Data
where
    '2019-09-18' = lpep_pickup_datetime::date
    and '2019-09-18' = lpep_dropoff_datetime::date
```


### question4:
```sql
select
	lpep_pickup_datetime::date as trip_start_date,
	max(lpep_dropoff_datetime - lpep_pickup_datetime) as max_duration
from public.green_taxi_data
group by 1
order by max_duration desc;
```

### question 5:
```sql
select
    d.lpep_pickup_datetime::date  as trip_start_date,
    p."Borough", sum(d.total_amount) as total
from public.green_taxi_data d
inner join zones p on
    d."PULocationID" = p."LocationID"
where
    d.lpep_pickup_datetime::date = '2019-09-18'
    and p."Borough" != 'Unknown'
group by 1, 2
having sum(total_amount) > 50000;
```

### question 6:

```sql
select
    lpep_pickup_datetime::date,
    z."Borough",
    p."Zone" as p_zone,
    z."Zone" as d_zone,
    d.tip_amount
from public.green_taxi_Data d
inner join zones p on
    d."PULocationID" = p."LocationID"
inner join zones z on
    d."DOLocationID" = z."LocationID"
where
    lpep_pickup_datetime::date >= '2019-09-01'
    and lpep_pickup_datetime::date <=  '2019-09-30'
    and p."Zone" = 'Astoria'
order by tip_amount desc
limit 1;
```

### question 7:
```
# google_bigquery_dataset.demo_dataset will be created
  + resource "google_bigquery_dataset" "demo_dataset" {
      + creation_time              = (known after apply)
      + dataset_id                 = "bq_dataset"
      + default_collation          = (known after apply)
      + delete_contents_on_destroy = false
      + effective_labels           = (known after apply)
      + etag                       = (known after apply)
      + id                         = (known after apply)
      + is_case_insensitive        = (known after apply)
      + last_modified_time         = (known after apply)
      + location                   = "EUROPE-WEST1"
      + max_time_travel_hours      = (known after apply)
      + project                    = "cohesive-envoy-411420"
      + self_link                  = (known after apply)
      + storage_billing_model      = (known after apply)
      + terraform_labels           = (known after apply)
    }

  # google_storage_bucket.demo-bucket will be created
  + resource "google_storage_bucket" "demo-bucket" {
      + effective_labels            = (known after apply)
      + force_destroy               = true
      + id                          = (known after apply)
      + location                    = "EUROPE-WEST1"
      + name                        = "cohesive-envoy-411420"
      + project                     = (known after apply)
      + public_access_prevention    = (known after apply)
      + self_link                   = (known after apply)
      + storage_class               = "STANDARD"
      + terraform_labels            = (known after apply)
      + uniform_bucket_level_access = (known after apply)
      + url                         = (known after apply)

      + lifecycle_rule {
          + action {
              + type = "AbortIncompleteMultipartUpload"
            }
          + condition {
              + age                   = 1
              + matches_prefix        = []
              + matches_storage_class = []
              + matches_suffix        = []
              + with_state            = (known after apply)
            }
        }
    }

Plan: 2 to add, 0 to change, 0 to destroy.
```
