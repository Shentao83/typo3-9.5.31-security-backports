# Test coverage

The same seven patch files passed local integration tests on the official classic TYPO3 9.5.31 release under Windows and PHP 7.4.33. Separate runs used SQLite 3.31.1 and MariaDB 10.3.39 (`mysqli`). The test environment used ImageMagick 6.9.13-61.

Both runs started with 026 and 027. The browser-based first-install wizard created the configuration, database tables, an administrator, and a sample page. The installer cleared its in-progress state on completion. The new administrator could sign in before and after installing the other five patches.

| Patch | Test performed |
| --- | --- |
| 026 | A local online-media test fixture produced a file and its FAL database record in configured storage. A fallback default destination was refused. |
| 027 | A clean first installation completed. A leftover setup marker did not reopen the setup wizard on the configured test site. |
| 028 | ImageMagick resized a PNG from 16 × 16 to 8 × 8 pixels. An additional PPM format required explicit configuration. |
| 029 | A multipart upload to an unavailable test storage returned a generic message. The exception was present in the TYPO3 log. |
| 030 | Local links and a same-origin absolute URL continued to work. An external URL was rejected. |
| 032 | With an empty MIME allowlist, both file and image upload fields refused a PNG. An explicit PNG allowlist accepted the upload and the file was stored through FAL. |
| 033 | Regular signed links returned the expected bytes for an original file and a processed file. An unsigned request returned HTTP 403. |

The combined patch set passed `git apply --check`, application, PHP 7.4 syntax checks for all twelve files, and reversal to the byte-for-byte original code. A SQLite setup attempt interrupted before completing `PackageStates.php` was repeated from an empty configuration and database. The completed SQLite and MariaDB installations both generated `PackageStates.php` normally.

These tests cover the stated Core paths. Use your own staging tests for extensions, storage drivers, Composer layouts, web-server rules, and deployment environments that differ from this setup.
