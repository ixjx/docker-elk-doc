# 版本变化

以下是在升级到ELK镜像的后期版本时可能会产生副作用的破坏性变化列表：

- `path.repo`

    Applies to tags: after 623.

    Elasticsearch's path.repo parameter is predefined as /var/backups in elasticsearch.yml (see Snapshot and restore).

- Version 6

    Applies to tags: 600 and later.

    Breaking changes are introduced in version 6 of Elasticsearch, Logstash, and Kibana.

- `ES_HEAP_SIZE` and `LS_HEAP_SIZE`

    Applies to tags: 502 to 522.

    Overriding the `ES_HEAP_SIZE` and `LS_HEAP_SIZE` environment variables has no effect on the heap size used by Elasticsearch and Logstash (see issue #129).

- Elasticsearch home directory

    Applies to tags: 502 and later.

    Elasticsearch is no longer installed from the deb package (which attempts, in version 5.0.2, to modify system files that aren't accessible from a container); instead it is installed from the tar.gz package.

    As a consequence, Elasticsearch's home directory is now `/opt/elasticsearch` (was `/usr/share/elasticsearch`).

- Version 5

    Applies to tags: `es500_l500_k500` and later.

    Breaking changes are introduced in version 5 of Elasticsearch, Logstash, and Kibana.

- Private keys in PKCS#8 format

    Applies to tags: `es240_l240_k460` and `es241_l240_k461`.

    In Logstash version 2.4.x, the private keys used by Logstash with the Beats input are expected to be in PKCS#8 format. To convert the private key (`logstash-beats.key`) from its default PKCS#1 format to PKCS#8, use the following command:

    `$ openssl pkcs8 -in logstash-beats.key -topk8 -nocrypt -out logstash-beats.p8`

    and point to the logstash-beats.p8 file in the ssl_key option of Logstash's 02-beats-input.conf configuration file.

- Logstash forwarder

    Applies to tags: `es500_l500_k500` and later.

    The use of Logstash forwarder is deprecated, its Logstash input plugin configuration has been removed, and port 5000 is no longer exposed.

- UIDs and GIDs

    Applies to tags: es235_l234_k454 and later.

    Fixed UIDs and GIDs are now assigned to Elasticsearch (both the UID and GID are 991), Logstash (992), and Kibana (993).

- Java 8

    Applies to tags: `es234_l234_k452` and later.

    This image initially used Oracle JDK 7, which is no longer updated by Oracle, and no longer available as a Ubuntu package.

    As from tag `es234_l234_k452`, the image uses Oracle JDK 8. This may have unintended side effects on plugins that rely on Java.

- Logstash configuration auto-reload

    Applies to tags: `es231_l231_k450`, `es232_l232_k450`.

    Logstash's configuration auto-reload option was introduced in Logstash 2.3 and enabled in the images with tags `es231_l231_k450` and `es232_l232_k450`.

    As this feature created a resource leak prior to Logstash 2.3.3 (see https://github.com/elastic/logstash/issues/5235), the --auto-reload option was removed as from the `es233_l232_k451`-tagged image (see https://github.com/spujadas/elk-docker/issues/41).

    Users of images with tags `es231_l231_k450` and `es232_l232_k450` are strongly recommended to override Logstash's options to disable the auto-reload feature by setting the LS_OPTS environment variable to --no-auto-reload if this feature is not needed.

    To enable auto-reload in later versions of the image:

    - From `es500_l500_k500` onwards: add the --config.reload.automatic command-line option to LS_OPTS.
    - From `es234_l234_k452` to `es241_l240_k461`: add --auto-reload to LS_OPTS.