Definitions:

* Record: A database record for a given content type, such as generic article, activty or photo.

* Content: The content of a field in the record. I will try to use "content" in the most specific way possible, as in the content of the headline field, not the content of the activity.

* Appropriate For: Audience(s) for which this record was written. For example, a lesson plan might be appropriate only for Educators.

* Variation: An alternate piece of content for a field that is written for one or more audiences. For example, while the instructions field might be written with Educators in mind, it may have a variation that is written for families and students.

* Version: If there are any variations on any field, there will be a version of this record for every audience indicated in the variation. A version must differ from the primary record by at least one field.


Basic Rules:

* A site may support the targeting of several audiences

* Models typically have an "appropriate_for" attribute indicating for which audience(s) this record is targeting. This does not mean that the content is inappropriate for the other audiences. It simply means that it was not written with them in mind, and they may not find it useful or interesting.

* Models may provide content variations on one or more fields. Each content variation indicate for which audience(s) this variation is targeting.

* The cumulation of content variations on a record provide an audience version. It is possible for two versions to be identical if they contain the same variations.

  For example, if a record contains one variation on the headline that targets both families and students, the family version and the student version are identical, but still differ from the primary version.

  This is done for convenience of retrieving the versions, since combinations of audiences can get complex.


URLs:

* If there is an audience-specific version, its URL will be the primary URL with the slugified audience name and a trailing slash appended. For example: if "/encyclopedia/rain-forest/" is the primary URL, "/encyclopedia/rain-forest/student/" would provide the student version **if it exists.**

* There can NEVER be an audience-specific version URL for the default audience. If the default audience is 'Educator', the URL "/encyclopedia/rain-forest/educator/" must not work, even if the variations indicate the default audience.

* If an attempt to reach a non-existant audience-specific version, the user should be redirected to the primary URL.


Display of available versions:

* If there are no other versions of a record, no audiences should be displayed.

* If there are versions of a record, the audiences that have versions should be displayed.

* If you are on a version page, the primary audience should be included in the other oversions.


Methodology:

* The URLs for the primary content and the audience versions are handled by the same view.

* The view doesn't worry about which version was requested.

* The actual altering of content is handled on the template using `django-audience` template tags to replace the individual field variations on the retrieved record.


Requested Audience version detection
    set info in the context via `site_ext.context_processors.audience`

Content substitution
    audience template tags in the template

Invalid audience version redirection
    The app MUST return a TemplateResponse. Since the content substitution is handled at render time, the only way to detect if the audience version requested exists, is after the view returns the requested object.

    The Template response allows us to do inspect the object and return a redirect if necessary.


