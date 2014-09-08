# Django-boto

Is an implementation of django integration with **Amazon AWS**
(http://aws.amazon.com/) services through **boto**
(https://github.com/boto/boto) module.

Installation
============

To install:

    easy_install django-boto

or:

    pip install django-boto

Configuration
=============

DEFAULT_FILE_STORAGE
----------------------

Can't be used as default file storage system because of ``path`` not
implemented but you still can pass ``S3Storage`` object as storage
option to a ``FileField``:



    from django.db import models
    from django_boto.s3.storage import S3Storage

    s3 = S3Storage()

    class Car(models.Model):
        ...
        photo = models.ImageField(storage=s3)

Other settings.py options
-------------------------

``AWS_ACCESS_KEY_ID``
    *(required for default file storage using)* **Access Key ID** from
    **Security Credentials** settings on AWS service. Required for using
    as default storage.

``AWS_SECRET_ACCESS_KEY``
    *(required for default file storage using)* **Secret Access Key**
    from **Security Credentials** settings on AWS service. Required for
    using as default storage.

``AWS_ACL_POLICY``
    Default canned ACL for objects saved. Defaults to ``public-read``.

``BOTO_S3_BUCKET``
    Amazon S3 ``bucket`` name. Default set to ``AWS_ACCESS_KEY_ID``.

``BOTO_S3_HOST``
    Amazon S3 hostname. Default to ``s3.amazonaws.com``.

``BOTO_BUCKET_LOCATION``
    Amazon datacenter location. Default to ``US Classic Region``.

``AWS_S3_FORCE_HTTP_URL``
    Default to ``False``. This settings allow you forcing S3 to return http links to files (if you have problem with SSL).

# Using

Manual S3Storage using
----------------------

If need to using manually - you can direct set ``bucket_name`` (as
``BOTO_S3_BUCKET``), ``key`` (as ``AWS_ACCESS_KEY_ID``), ``secret`` (as
``AWS_SECRET_ACCESS_KEY``) and ``location`` (as
``BOTO_BUCKET_LOCATION``) on storage instatiation:


    from boto.s3.storage import S3Storage

    s3 = S3Storage(bucket='another-bucket', key='another-key',
        secret='another-secret', location='EU')

``S3Storage`` - it's tipical Djago storage system\_, only ``path`` is
not implemented and ``created_time`` and ``accessed_time`` return same
value as ``modified_time``.

Upload shortcut
---------------

For simple uploading you can use shortcut:



    from django_boto.s3 import upload

    upload(filename, name=None, prefix=False, bucket_name=False, key=None,
           secret=None, host=None, expires=0, query_auth=False, force_http=True,
           policy=None)

where:

``filename``
    ``string`` filesystem path to file or **django** ``File`` instance
    or **python** ``file`` object instance;

``name``
    ``string`` target django name for uploading file;

``prefix``
    ``string`` path prefix to filename in ``s3.amazonaws.com`` url. Like
    filename ``/images/image.jpg`` with ``avatars`` prefix convert to
    ``avatars/image.jpg`` in amazon url;

``bucket_name``
    name of bucket (if not exists - system try to create it) in amazon
    S3;

``key``
    ``AWS_ACCESS_KEY_ID`` replacement;

``secret``
    ``AWS_SECRET_ACCESS_KEY`` replacement.

``host``
    ``BOTO_S3_HOST`` replacement.

``expires``
    ``int`` How long should private links be valid for.

``query_auth``
    ``bool`` Should the url be generated with a valid signature?
    Required for private files.

``force_http``
    ``bool`` Should the generated url be http?

``policy``
    ``string`` Canned acl string for uploaded objects.

It's return URL for direct file download.

Last nine options are optional. If not set - it takes from settings.py
or used defaults.
