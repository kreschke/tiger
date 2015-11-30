# TIGER import pipeline
A pipeline for importing TIGER data into Pelias.

## Install dependencies

```
npm install
```

## usage

### default
Make sure you have an elasticsearch server running at `localhost:9200` and the
[OpenVenues address deduplicator](https://github.com/openvenues/address_deduper) at `localhost:5000`, and then run
`node import.js DIR_NAME`, where `DIR_NAME` is the path to a directory filled with (unzipped) TIGER shapefiles.

### custom
[Configure](#configuration) importer, then run `node import.js`.

## configuration
This importer can be configured in [pelias-config](https://github.com/pelias/config), in the `imports.tiger` object:

```javascript
{
    "imports": {
        "tiger": {
            "datapath": "/path/to/tiger/data",
            "adminLookup": true,
            "addressDedup": true,
            "outputTarget": "elasticsearch"
        }
    }
}
```

The following properties are recognized:

  * `datapath` - The absolute path to a directory filled with (unzipped) TIGER shapefiles. Must be specified if no directory is given as a command-line argument. Command-line argument overrides this config.
  * `adminLookup` (default: `false`) - TIGER data doesn't have a full administrative hierarchy (ie, country, state, county, etc. names), but you can optionally create it via the [`pelias/admin-lookup`](https://github.com/pelias/admin-lookup) plugin; just set this property to `true`.  Consult the `admin-lookup` README for setup documentation (namely just downloading the Quattroshapes dataset).
  * `addressDedup` (default: `true`) - This makes it possible to import multiple datasets containing the same addresses, without getting duplicates in the pelias database. Please see [`pelias/address-deduplicator`](https://github.com/pelias/address-deduplicator) for more details. If `true`, an [OpenVenues address deduplicator](https://github.com/openvenues/address_deduper) must be running at `localhost:5000`.
  * `outputTarget` (default: `elasticsearch`)
    * `elasticsearch` - Write output to Elasticsearch server running at `localhost:9200`.
    * `jsonl` - Write output to [json-lines](http://jsonlines.org/) file.