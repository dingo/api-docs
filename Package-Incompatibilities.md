The API will generally work with all other packages, however, some Laravel specific packages may conflict with the API as it integrates itself heavily with the Laravel router to provide the needed functionality. This page lists other packages that have some incompatibilities with the API and some required steps to ensure both packages work.

#### [Asset Pipeline](https://github.com/CodeSleeve/asset-pipeline)

The service provider of **Asset Pipeline** should be registered AFTER the API service provider. (Reference: [#127](https://github.com/dingo/api/issues/127))