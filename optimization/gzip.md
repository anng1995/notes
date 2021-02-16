# Gzip

For gzip, we need:
-Accept-Encoding: gzip
-User-Agent: my program (gzip)

Partial Resource can import performance of your API calls by sending
and receiving only the portion of the data that you're interested in.

Two types of partial requestiosn:

- Partial Response: a requestwehre tou specify which fields to includ in
  the response - Patch: An update request where you send only the fields you want to
  change.

For Partial response:
use the `fields` request parameter to specify the fields you want returned.

examples: https://www.googleapis.com/demo/v1?fields=kind,items(title,character)

patch requests:

- Add: To add, specifify the new field and its value
- Modify: To change value, specify field and set it to new value
- Delete: To delete, specify field and set it to null
