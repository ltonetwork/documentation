# LegalThings Resource Identifier (LTRI) Scheme

The LTRI follows a RESTful path structure to identify a resource. The resource collection is always plural.

An HTTP URL identifies the system where the resource can be found. The LTRI identifies a resource within a system.

### Scheme syntax

LegalThings-URI = "lt:" [username ":" password "@"] [ service-name ":" ] "/" resource-collection "/" [ resource-id [ "/" sub-resource-collection "/" [ sub-resource-id ] ] ] [ "?" query ]

### Versioning

The query parament `v` SHOULD be used to differentiate between versions of a resource.

### Examples

* lt:/documents/
* lt:/documents/hzVZmgeE
* lt:/documents/hzVZmgeE.pdf
* lt:/documents/hzVZmgeE?v=1
* lt:some-user:secret-password@/documents/hzVZmgeE
* lt:/documents/hzVZmgeE/events/
* lt:/documents/hzVZmgeE/events/feNq1KBj


## Deprecation warning

Using the service name is deprecated. When using micro services, there SHOULD be mapping for the resource collection to
the specific service.

* lt:dms/documents/
* lt:dms/documents/hzVZmgeE
* lt:some-user:secret-password@dms/documents/hzVZmgeE
