
 * 01-03 are generic - showcasing basic logstash usage.

 * >04 are service specific - requires pasting in some log or backfilling

The ones meant for backfilling are not to be used when running logstash as a continuous service, nor should it be run repeatedly without deleting old indices (unless you want duplicate entries in elasticsearch) or without making sure that the \_id field is consistent.
