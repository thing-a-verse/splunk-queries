# splunk-queries

## Status of tailing processor on a Heavy Forwarder

'''
| rest /services/admin/inputstatus/TailingProcessor:FileStatus | spath | fields inputs./app/splunk/pplcsma02ui/log/today/* | transpose | rename column as input, "row 1" as data | table input, data
| search input=*.log.*
| rex field=input "inputs\.(?<filename>[^\s]+)\.(?<record_type>[a-z\s]+)$"
| eval {record_type}=data
| table filename, parent, "file size", "file position", percent
| stats values(parent) as parent, values("file size") as size, values("file position") as position, values(percent) as percent by filename
'''
