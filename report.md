# Log Audit

# System

Odoo 14 \
Postgres (Amazon RDS)
<br><br>

# Errors

There are two kind of errors in log file provided.

1. Related to reading file from filestore
2. Error related to mailing `SMTPDataError`

## File read error

```txt
2024-03-25 05:11:25,248 675571 INFO <database name> odoo.addons.base.models.ir_attachment: _read_file reading /opt/odoo/filestore/<database name>/2a/2a21947cd8e8260d10d6d6ca184b8d6923dfe42e 
Traceback (most recent call last):
  File "/home/ims/ODOO14/odoo/api.py", line 789, in get
    field_cache = field_cache[record.env.cache_key(field)]
KeyError: (None,)

During handling of the above exception, another exception occurred:

Traceback (most recent call last):
  File "/home/ims/ODOO14/odoo/fields.py", line 970, in __get__
    value = env.cache.get(record, self)
  File "/home/ims/ODOO14/odoo/api.py", line 793, in get
    raise CacheMiss(record, field)
odoo.exceptions.CacheMiss: 'res.partner(212917,).image_128'

During handling of the above exception, another exception occurred:

Traceback (most recent call last):
  File "/home/ims/ODOO14/odoo/api.py", line 789, in get
    field_cache = field_cache[record.env.cache_key(field)]
KeyError: (None, None)

During handling of the above exception, another exception occurred:

Traceback (most recent call last):
  File "/home/ims/ODOO14/odoo/fields.py", line 970, in __get__
    value = env.cache.get(record, self)
  File "/home/ims/ODOO14/odoo/api.py", line 793, in get
    raise CacheMiss(record, field)
odoo.exceptions.CacheMiss: 'ir.attachment(6349,).datas'

During handling of the above exception, another exception occurred:

Traceback (most recent call last):
  File "/home/ims/ODOO14/odoo/api.py", line 789, in get
    field_cache = field_cache[record.env.cache_key(field)]
KeyError: (None,)

During handling of the above exception, another exception occurred:

Traceback (most recent call last):
  File "/home/ims/ODOO14/odoo/fields.py", line 970, in __get__
    value = env.cache.get(record, self)
  File "/home/ims/ODOO14/odoo/api.py", line 793, in get
    raise CacheMiss(record, field)
odoo.exceptions.CacheMiss: 'ir.attachment(6349,).raw'

During handling of the above exception, another exception occurred:

Traceback (most recent call last):
  File "/home/ims/ODOO14/odoo/addons/base/models/ir_attachment.py", line 102, in _file_read
    with open(full_path, 'rb') as f:
FileNotFoundError: [Errno 2] No such file or directory: '/opt/odoo/filestore/<database name>/2a/2a21947cd8e8260d10d6d6ca184b8d6923dfe42e'

```

<br>

The error encountering, `KeyError: (None,)`, is related to Odoo's internal caching mechanism when trying to access an ```ir.attachment record's``` file data. This error typically occurs when Odoo attempts to read a file from the filestore but cannot find the file or when there's an issue with the cache. The subsequent `FileNotFoundError` indicates that the file Odoo is trying to access does not exist in the specified location.

> Feels like `filestore` has been modified or corrupted

## Possible solutions

* Clear Odoo's cache 
* Remove wrong metadata of missing file from `ir_attachement` table

## Related error documented online

* [No such file or directory" ODOO10](https://stackoverflow.com/questions/45794753/no-such-file-or-directory-odoo10)
* [ir_attachment: IOError: [Errno 2] No such file or directory](https://www.odoo.com/forum/help-1/ir-attachment-ioerror-errno-2-no-such-file-or-directory-94675)
* [Chat GPT response](https://chat.openai.com/share/9ee143c3-ae6f-4c52-bc8e-e109f499df09)



