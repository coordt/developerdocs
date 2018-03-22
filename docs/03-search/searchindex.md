# Search Index


## Character Filters

### `ng-common`

**type:** mapping

**mappings:**

| From         | To        |
|--------------|-----------|
| `&`          | ` and `   |
| `\u0091`     | `'`       |
| `\u0092`     | `'`       |
| `‘` (\u2018) | `'`       |
| `’` (\u2019) | `'`       |
| `‛` (\u201B) | `'`       |
| `“` (\u201C) | `"`       |
| `”` (\u201D) | `"`       |
| `‟` (\u201F) | `"`       |
| `…` (\u2026) | `...`     |
 

**description:** Replaces characters with other characters to provide a standard representation.

### `english_stemmer`

**type:** stemmer

**language:** english

**description:** A basic stemmer for the English language.

### `english_possessive_stemmer`

**type:** stemmer

**language:** possessive_english

**description:** Removes possessives (’s) from words.


## Analyzers

### NGStandard

**character filter:** html_strip, ng-common

**tokenizer:** icu_tokenizer

**filters:** lowercase, icu_folding

**description:** Strips HTML tags and converts HTML entities to unicode, converts some characters into standard characters, breaks by words, converts to lowercase and normalizes unicode encoding.


### NGEnglish

**character filter:** html_strip, ng-common

**tokenizer:** icu_tokenizer

**filters:** english_possessive_stemmer, lowercase, icu_folding, english_stemmer

**description:** Strips HTML tags and converts HTML entities to unicode, converts some characters into standard characters, removes possessives, converts to lowercase, breaks by words, normalizes unicode encoding, and stems the words using the standard snowball stemmer.


### Keyword

**description:** Treats the entire text as one token


## Internal Identification

### `internal.contenttype`

**field type:** String

**indexed:** Not Analyzed

**multiple:** No

**analyzer:** Not Analyzed

**description:** The Django content type constructed as the application name, a period, and the model name.

### `internal.id`

**field type** String

**indexed:** Not Analyzed

**multiple:** No

**analyzer:** Not Analyzed

**description:** The Django primary key of the object.

### `internal.uuid`

**field type:** String

**indexed:** Not Analyzed

**multiple:** No

**analyzer:** Not Analyzed

**description:** The UUID of the object, if available.


## Content

### `url`

**field type:** String

**indexed:** Not Analyzed

**multiple:** No

**analyzer:** Not Analyzed

**description:** The URL without the domain. It should start with a "/".


### `title`

**field type:** String

**indexed:** Analyzed

**multiple:** No

**analyzer:** NGEnglish

**description:** The title or name of the object.


### `abstract`

**field type:** String

**indexed:** Analyzed

**multiple:** No

**analyzer:** NGEnglish

**description:** A short description or summary of the object or contents. We will use this field for teases of this object.

### `content`

**field type:** String

**indexed:** Analyzed

**multiple:** No

**analyzer:** NGEnglish

**description:** The full text of the object.

## Key Image

### `key_image.url`

**field type:** String

**indexed:** Not Analyzed

**multiple:** No

**analyzer:** Not Analyzed

**description:** The url to the object's key image

### `key_image.width`

**field type:** Integer

**indexed:** Not Analyzed

**multiple:** No

**analyzer:** Not Analyzed

**description:** The width in pixels of the object's key image

### `key_image.height`

**field type:** Integer

**indexed:** Not Analyzed

**multiple:** No

**analyzer:** Not Analyzed

**description:** The height in pixels of the object's key image

### `key_image.credit`

**field type:** String

**indexed:** Not Analyzed

**multiple:** No

**analyzer:** Not Analyzed

**description:** The credit or attribution, if available, of the key image's author

### `key_image.caption`

**field type:** String

**indexed:** Not Analyzed

**multiple:** No

**analyzer:** Not Analyzed

**description:** The caption of the key image if it is relevant to the object, or used in the object.


## Taxonomy Metadata

### `keywords`

**field type:** String

**indexed:** Analyzed

**multiple:** Yes

**analyzer:** Keyword

**description:** Keywords assigned to this object.

### `locations`

**field type:** String

**indexed:** Analyzed

**multiple:** Yes

**analyzer:** Keyword

**description:** Locations assigned to this object.

### `geometry`

**field type:** Geometry

**indexed:** Not Analyzed

**multiple:** No

**analyzer:** Not Analyzed

**description:** The combined geometry of the locations assigned to this object, for geographical search.

### `subjects`

**field type:** String

**indexed:** Analyzed

**multiple:** Yes

**analyzer:** Keyword

**description:** Subjects assigned to this object.

### `persons`

**field type:** String

**indexed:** Analyzed

**multiple:** Yes

**analyzer:** Keyword

**description:** People assigned to this object.

### `series`

**field type:** String

**indexed:** Analyzed

**multiple:** Yes

**analyzer:** Keyword

**description:** Series assigned to this object.

### `audiences`

**field type:** String

**indexed:** Analyzed

**multiple:** Yes

**analyzer:** Keyword

**description:** Audiences assigned to this object.

### `contributors`

**field type:** String

**indexed:** Analyzed

**multiple:** Yes

**analyzer:** Keyword

**description:** Contributors assigned to this object.

### `organizations`

**field type:** String

**indexed:** Analyzed

**multiple:** Yes

**analyzer:** Keyword

**description:** Organizations assigned to this object.

### `events`

**field type:** String

**indexed:** Analyzed

**multiple:** Yes

**analyzer:** Keyword

**description:** Events assigned to this object.

### `genres`

**field type:** String

**indexed:** Analyzed

**multiple:** Yes

**analyzer:** Keyword

**description:** Genres assigned to this object.

### `concepts`

**field type:** String

**indexed:** Analyzed

**multiple:** Yes

**analyzer:** Keyword

**description:** Concepts assigned to this object.

### `ages`

**field type:** Integer

**indexed:** Not Analyzed

**multiple:** Yes

**analyzer:** Not Analyzed

**description:** Ages assigned to this object.

### `grades`

**field type:** String

**indexed:** Not Analyzed

**multiple:** Yes

**analyzer:** Not Analyzed

**description:** Grades assigned to this object.

## Other Metadata

### `grades_string`

**field type:** String

**indexed:** Not Analyzed

**multiple:** No

**analyzer:** Not Analyzed

**description:** If the object has grades assigned to it, this is the string representation of those grades, e.g. 3-6, All grades, K-12


### `ages_string`

**field type:** String

**indexed:** Not Analyzed

**multiple:** No

**analyzer:** Not Analyzed

**description:** If the object has ages assigned to it, this is the string representation of those ages, e.g. 3-6, All ages, 6-12


### `alpha_sort`

**field type:** String

**indexed:** Not Analyzed

**multiple:** No

**analyzer:** Not Analyzed

**description:** The string to use for alphabetic sorting. This field may be the same as the title, or a slightly modified version of it.

### `last_modified`

**field type:** Date

**indexed:** Not Analyzed

**multiple:** No

**analyzer:** Not Analyzed

**description:** When the object was last modified.

### `created_at`

**field type:** Date

**indexed:** Not Analyzed

**multiple:** No

**analyzer:** Not Analyzed

**description:** When the object was created.

### `source`

**field type:** String

**indexed:** Not Analyzed

**multiple:** No

**analyzer:** Not Analyzed

**description:** The domain of the site indexed. The index may contain the objects of multiple sites.

### `downloadable`

**field type:** Boolean

**indexed:** Not Analyzed

**multiple:** No

**analyzer:** Not Analyzed

**description:** Is this object downloadable.


### `content_type`

**field type:** String

**indexed:** Not Analyzed

**multiple:** No

**analyzer:** Not Analyzed

**description:** The primary content type of this object, from a controlled vocabulary of content types. This is used for display purposes.

### `possible_content_types`

**field type:** String

**indexed:** Not Analyzed

**multiple:** Yes

**analyzer:** Not Analyzed

**description:** All possible content types valid for this object, including the primary content type referenced in the `content_type` field. This field is used for searching.
