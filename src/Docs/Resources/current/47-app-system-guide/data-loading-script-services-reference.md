# Data Loading scripting services reference

## services.repository (`Shopware\Core\Framework\DataAbstractionLayer\Facade\RepositoryFacade`)

The `repository` service allows you to query data, that is stored inside shopware.
Keep in mind that your app needs to have the correct permissions for the data it queries through this service.

### search()

The `search()` method allows you to search for Entities that match a given criteria.


#### Arguments

##### `entityName`: string


The name of the Entity you want to search for, e.g. `product` or `media`.

##### `criteria`: array


The criteria used for your search.


#### Return value

**Type**: `Shopware\Core\Framework\DataAbstractionLayer\Search\EntitySearchResult`

A `EntitySearchResult` including all entities that matched your criteria.

#### Examples

Load a single product.
```twig
{% set page = hook.page %}
{# @var page \Shopware\Storefront\Page\Page #}

{% set criteria = {
    'ids': [ hook.productId ]
} %}

{% set product = services.repository.search('product', criteria).first %}

{% do page.addExtension('myProduct', product) %}
```
Filter the search result.
```twig
{% set page = hook.page %}
{# @var page \Shopware\Storefront\Page\Page #}

{% set criteria = {
    'filter': [
        { 'field': 'productNumber', 'type': 'equals', 'value': 'p1' }
    ]
} %}

{% set product = services.repository.search('product', criteria).first %}

{% do page.addExtension('myProduct', product) %}
```
Add associations that should be included in the result.
```twig
{% set page = hook.page %}
{# @var page \Shopware\Storefront\Page\Page #}

{% set criteria = {
    'ids': [ hook.productId ],
    'associations': {
        'manufacturer': {}
    }
} %}

{% set product = services.repository.search('product', criteria).first %}

{% do page.addExtension('myProduct', product) %}
{% do page.addExtension('myManufacturer', product.manufacturer) %}
```
### ids()

The `ids()` method allows you to search for the Ids of Entities that match a given criteria.


#### Arguments

##### `entityName`: string


The name of the Entity you want to search for, e.g. `product` or `media`.

##### `criteria`: array


The criteria used for your search.


#### Return value

**Type**: `Shopware\Core\Framework\DataAbstractionLayer\Search\IdSearchResult`

A `IdSearchResult` including all entity-ids that matched your criteria.

#### Examples

Get the Ids of products with the given ProductNumber.
```twig
{% set page = hook.page %}
{# @var page \Shopware\Storefront\Page\Page #}

{% set criteria = {
    'filter': [
        { 'field': 'productNumber', 'type': 'equals', 'value': 'p1' }
    ]
} %}

{% set productIds = services.repository.ids('product', criteria).ids %}

{% do page.addArrayExtension('myProductIds', {
    'ids': productIds
}) %}
```
### aggregate()

The `aggregate()` method allows you to execute aggregations specified in the given criteria.


#### Arguments

##### `entityName`: string


The name of the Entity you want to aggregate data on, e.g. `product` or `media`.

##### `criteria`: array


The criteria that define your aggregations.


#### Return value

**Type**: `Shopware\Core\Framework\DataAbstractionLayer\Search\AggregationResult\AggregationResultCollection`

A `AggregationResultCollection` including the results of the aggregations you specified in the criteria.

#### Examples

Aggregate data for multiple entities, e.g. the sum of the gross price of all products.
```twig
{% set page = hook.page %}
{# @var page \Shopware\Storefront\Page\Page #}

{% set criteria = {
    'aggregations': [
        { 'name': 'sumOfPrices', 'type': 'sum', 'field': 'price.gross' }
    ]
} %}

{% set sumResult = services.repository.aggregate('product', criteria).get('sumOfPrices') %}

{% do page.addArrayExtension('myProductAggregations', {
    'sum': sumResult.getSum
}) %}
```


## services.store (`Shopware\Core\Framework\DataAbstractionLayer\Facade\SalesChannelRepositoryFacade`)

The `store` service can be used to access publicly available `store-api` data.
As the data is publicly available your app does not need any additional permissions to use this service,
however querying data and also loading associations is restricted to the entities that are also available through the `store-api`.

Notice that the returned entities are already processed for the storefront,
this means that e.g. product prices are already calculated based on the current context.

### search()

The `search()` method allows you to search for Entities that match a given criteria.


#### Arguments

##### `entityName`: string


The name of the Entity you want to search for, e.g. `product` or `media`.

##### `criteria`: array


The criteria used for your search.


#### Return value

**Type**: `Shopware\Core\Framework\DataAbstractionLayer\Search\EntitySearchResult`

A `EntitySearchResult` including all entities that matched your criteria.

#### Examples

Load a single storefront product.
```twig
{% set page = hook.page %}
{# @var page \Shopware\Storefront\Page\Page #}

{% set criteria = {
    'ids': [ hook.productId ]
} %}

{% set product = services.store.search('product', criteria).first %}

{% do page.addExtension('myProduct', product) %}
```
Filter the search result.
```twig
{% set page = hook.page %}
{# @var page \Shopware\Storefront\Page\Page #}

{% set criteria = {
    'filter': [
        { 'field': 'productNumber', 'type': 'equals', 'value': 'p1' }
    ]
} %}

{% set product = services.store.search('product', criteria).first %}

{% do page.addExtension('myProduct', product) %}
```
Add associations that should be included in the result.
```twig
{% set page = hook.page %}
{# @var page \Shopware\Storefront\Page\Page #}

{% set criteria = {
    'ids': [ hook.productId ],
    'associations': {
        'manufacturer': {}
    }
} %}

{% set product = services.store.search('product', criteria).first %}

{% do page.addExtension('myProduct', product) %}
{% do page.addExtension('myManufacturer', product.manufacturer) %}
```
### ids()

The `ids()` method allows you to search for the Ids of Entities that match a given criteria.


#### Arguments

##### `entityName`: string


The name of the Entity you want to search for, e.g. `product` or `media`.

##### `criteria`: array


The criteria used for your search.


#### Return value

**Type**: `Shopware\Core\Framework\DataAbstractionLayer\Search\IdSearchResult`

A `IdSearchResult` including all entity-ids that matched your criteria.

#### Examples

Get the Ids of products with the given ProductNumber.
```twig
{% set page = hook.page %}
{# @var page \Shopware\Storefront\Page\Page #}

{% set criteria = {
    'filter': [
        { 'field': 'productNumber', 'type': 'equals', 'value': 'p1' }
    ]
} %}

{% set productIds = services.store.ids('product', criteria).ids %}

{% do page.addArrayExtension('myProductIds', {
    'ids': productIds
}) %}
```
### aggregate()

The `aggregate()` method allows you to execute aggregations specified in the given criteria.


#### Arguments

##### `entityName`: string


The name of the Entity you want to aggregate data on, e.g. `product` or `media`.

##### `criteria`: array


The criteria that define your aggregations.


#### Return value

**Type**: `Shopware\Core\Framework\DataAbstractionLayer\Search\AggregationResult\AggregationResultCollection`

A `AggregationResultCollection` including the results of the aggregations you specified in the criteria.

#### Examples

Aggregate data for multiple entities, e.g. the sum of the children of all products.
```twig
{% set page = hook.page %}
{# @var page \Shopware\Storefront\Page\Page #}

{% set criteria = {
    'aggregations': [
        { 'name': 'sumOfChildren', 'type': 'sum', 'field': 'childCount' }
    ]
} %}

{% set sumResult = services.store.aggregate('product', criteria).get('sumOfChildren') %}

{% do page.addArrayExtension('myProductAggregations', {
    'sum': sumResult.getSum
}) %}
```

