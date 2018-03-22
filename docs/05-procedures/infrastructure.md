NatGeo Society Web Infrastructure


Front-end caching and media server
----------------------------------

    - Varnish
    - nGinx
        - Handles SSL for varnish
        - Handles media
    - Transmogrify
        - 404 handler for nGinx that can crop/scale images dynamically


App server
----------

    - nGinx
    - Redis
    - Apps in Python and node.js


DB server
---------

    - PostgreSQL 9.5 with PostGIS extensions


Search and Geospatial server
----------------------------

    - Elasticsearch
    - GeoServer


Test server
-----------

    - nGinx
    - Redis
    - Apps in Python and node.js
    - Private PyPI index