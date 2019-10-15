# 行列ブロッククエリ

**行列ブロッククエリ**を使用して、テンプレートや PHP コード内で行列ブロックを取得できます。

::: code
```twig
{# Create a new Matrix block query #}
{% set myMatrixBlockQuery = craft.matrixBlocks() %}
```
```php
// Create a new Matrix block query
$myMatrixBlockQuery = \craft\elements\MatrixBlock::find();
```
:::

行列ブロッククエリを作成すると、結果を絞り込むための[パラメータ](#parameters)をセットできます。さらに、`.all()` を呼び出して[実行](README.md#executing-element-queries)できます。[MatrixBlock](api:craft\elements\MatrixBlock) オブジェクトの配列が返されます。

::: tip
エレメントクエリがどのように機能するかについては、[エレメントクエリについて](README.md)を参照してください。
:::

## 実例

次の操作を行うことで、エレメントのすべての行列ブロックのコンテンツを表示できます。

1. `craft.matrixBlocks()` で行列ブロッククエリを作成します。
2. [owner](#owner)、[fieldId](#fieldid)、および、[type](#type) パラメータをセットします。
3. `.all()` で行列ブロックを取得します。
4. [for](https://twig.symfony.com/doc/2.x/tags/for.html) タグを使用して行列ブロックをループ処理し、コンテンツを出力します。

```twig
{# Create a Matrix block query with the 'owner', 'fieldId', and 'type' parameters #}
{% set myMatrixBlockQuery = craft.matrixBlocks()
    .owner(myEntry)
    .fieldId(10)
    .type('text') %}

{# Fetch the Matrix blocks #}
{% set matrixBlocks = myMatrixBlockQuery.all() %}

{# Display their contents #}
{% for block in blocks %}
    <p>{{ block.text }}</p>
{% endfor %}
```

::: warning
返される行列ブロックにカスタムフィールドのコンテンツが代入されるよう、[fieldId](#fieldid) または [id](#id) パラメータを設定する必要があります。
:::

## パラメータ

行列ブロッククエリは、次のパラメータをサポートしています。

<!-- BEGIN PARAMS -->

### `allowOwnerDrafts`

Narrows the query results based on whether the Matrix blocks’ owners are drafts.

Possible values include:

| 値       | 取得する行列ブロック                      |
| ------- | ------------------------------- |
| `true`  | which can belong to a draft.    |
| `false` | which cannot belong to a draft. |




### `allowOwnerRevisions`

Narrows the query results based on whether the Matrix blocks’ owners are revisions.

Possible values include:

| 値       | 取得する行列ブロック                         |
| ------- | ---------------------------------- |
| `true`  | which can belong to a revision.    |
| `false` | which cannot belong to a revision. |




### `anyStatus`

Clears out the [status](#status) and [enabledForSite()](https://docs.craftcms.com/api/v3/craft-elements-db-elementquery.html#method-enabledforsite) parameters.





::: code
```twig
{# Fetch all Matrix blocks, regardless of status #}
{% set MatrixBlocks = craft.matrixBlocks()
    .anyStatus()
    .all() %}
```

```php
// Fetch all Matrix blocks, regardless of status
$MatrixBlocks = \craft\elements\MatrixBlock::find()
    ->anyStatus()
    ->all();
```
:::


### `asArray`

Causes the query to return matching Matrix blocks as arrays of data, rather than [MatrixBlock](api:craft\elements\MatrixBlock) objects.





::: code
```twig
{# Fetch Matrix blocks as arrays #}
{% set MatrixBlocks = craft.matrixBlocks()
    .asArray()
    .all() %}
```

```php
// Fetch Matrix blocks as arrays
$MatrixBlocks = \craft\elements\MatrixBlock::find()
    ->asArray()
    ->all();
```
:::


### `dateCreated`

Narrows the query results based on the Matrix blocks’ creation dates.



利用可能な値には、次のものが含まれます。

| 値                                                | Fetches Matrix blocks…                               |
| ------------------------------------------------ | ---------------------------------------------------- |
| `'>= 2018-04-01'`                             | that were created on or after 2018-04-01.            |
| `'< 2018-05-01'`                              | that were created before 2018-05-01                  |
| `['and', '>= 2018-04-04', '< 2018-05-01']` | that were created between 2018-04-01 and 2018-05-01. |



::: code
```twig
{# Fetch Matrix blocks created last month #}
{% set start = date('first day of last month')|atom %}
{% set end = date('first day of this month')|atom %}

{% set MatrixBlocks = craft.matrixBlocks()
    .dateCreated(['and', ">= #{start}", "< #{end}"])
    .all() %}
```

```php
// Fetch Matrix blocks created last month
$start = (new \DateTime('first day of last month'))->format(\DateTime::ATOM);
$end = (new \DateTime('first day of this month'))->format(\DateTime::ATOM);

$MatrixBlocks = \craft\elements\MatrixBlock::find()
    ->dateCreated(['and', ">= {$start}", "< {$end}"])
    ->all();
```
:::


### `dateUpdated`

Narrows the query results based on the Matrix blocks’ last-updated dates.



利用可能な値には、次のものが含まれます。

| 値                                                | Fetches Matrix blocks…                               |
| ------------------------------------------------ | ---------------------------------------------------- |
| `'>= 2018-04-01'`                             | that were updated on or after 2018-04-01.            |
| `'< 2018-05-01'`                              | that were updated before 2018-05-01                  |
| `['and', '>= 2018-04-04', '< 2018-05-01']` | that were updated between 2018-04-01 and 2018-05-01. |



::: code
```twig
{# Fetch Matrix blocks updated in the last week #}
{% set lastWeek = date('1 week ago')|atom %}

{% set MatrixBlocks = craft.matrixBlocks()
    .dateUpdated(">= #{lastWeek}")
    .all() %}
```

```php
// Fetch Matrix blocks updated in the last week
$lastWeek = (new \DateTime('1 week ago'))->format(\DateTime::ATOM);

$MatrixBlocks = \craft\elements\MatrixBlock::find()
    ->dateUpdated(">= {$lastWeek}")
    ->all();
```
:::


### `draftCreator`

Narrows the query results to only drafts created by a given user.



::: code

| 値                                      | 取得する行列ブロック                             |
| -------------------------------------- | -------------------------------------- |
| `1`                                    | created by the user with an ID of 1.   |
| a `\craft\elements\db\User` object | by the user represented by the object. |



:::
```twig
{# Fetch Matrix blocks in the field with an ID of 1 #}
{% set MatrixBlocks = craft.matrixBlocks()
    .fieldId(1)
    .all() %}
```

```php
// Fetch Matrix blocks in the field with an ID of 1
$MatrixBlocks = \craft\elements\MatrixBlock::find()
    ->fieldId(1)
    ->all();
```
:::


### `draftId`

Narrows the query results based on the Matrix blocks’ draft’s ID (from the `drafts` table).



::: code

| 値           | Fetches drafts…                |
| ----------- | ------------------------------ |
| `6463'foo'` | for the draft with an ID of 1. |



:::
```twig
{# Fetch Matrix blocks in a specific order #}
{% set MatrixBlocks = craft.matrixBlocks()
    .id([1, 2, 3, 4, 5])
    .fixedOrder()
    .all() %}
```

```php
// Fetch Matrix blocks in a specific order
$MatrixBlocks = \craft\elements\MatrixBlock::find()
    ->id([1, 2, 3, 4, 5])
    ->fixedOrder()
    ->all();
```
:::


### `draftOf`

Narrows the query results to only drafts of a given Matrix block.



Possible values include:

| 値                                                        | Fetches drafts…                                 |
| -------------------------------------------------------- | ----------------------------------------------- |
| `1`                                                      | for the Matrix block with an ID of 1.           |
| a [MatrixBlock](api:craft\elements\MatrixBlock) object | for the Matrix block represented by the object. |



::: code
```twig
{# Fetch the Matrix block by its ID #}
{% set MatrixBlock = craft.matrixBlocks()
    .id(1)
    .one() %}
```

```php
// Fetch the Matrix block by its ID
$MatrixBlock = \craft\elements\MatrixBlock::find()
    ->id(1)
    ->one();
```
:::


### `drafts`

Narrows the query results to only drafts Matrix blocks.





:::
```twig
{# Fetch Matrix blocks in reverse #}
{% set MatrixBlocks = craft.matrixBlocks()
    .inReverse()
    .all() %}
```

```php
// Fetch Matrix blocks in reverse
$MatrixBlocks = \craft\elements\MatrixBlock::find()
    ->inReverse()
    ->all();
```
:::


### `fieldId`

Narrows the query results based on the field the Matrix blocks belong to, per the fields’ IDs.

Possible values include:

| 値                       | 取得する行列ブロック                           |
| ----------------------- | ------------------------------------ |
| `4076'foo'`             | in a field with an ID of 1.          |
| `'not foo'`             | not in a field with an ID of 1.      |
| `['foo', 'bar']`        | in a field with an ID of 1 or 2.     |
| `['not', 'foo', 'bar']` | not in a field with an ID of 1 or 2. |



::: code
```twig
{# Fetch up to 10 Matrix blocks  #}
{% set MatrixBlocks = craft.matrixBlocks()
    .limit(10)
    .all() %}
```

```php
// Fetch up to 10 Matrix blocks
$MatrixBlocks = \craft\elements\MatrixBlock::find()
    ->limit(10)
    ->all();
```
:::


### `fixedOrder`

Causes the query results to be returned in the order specified by [id](#id).





指定されたエレメントに基づいて、[ownerId](#ownerid) および [siteId](#siteid) パラメータをセットします。
```twig
{# Fetch all Matrix blocks except for the first 3 #}
{% set MatrixBlocks = craft.matrixBlocks()
    .offset(3)
    .all() %}
```

```php
// Fetch all Matrix blocks except for the first 3
$MatrixBlocks = \craft\elements\MatrixBlock::find()
    ->offset(3)
    ->all();
```
:::


### `id`

Narrows the query results based on the Matrix blocks’ IDs.



Possible values include:

| 値               | Fetches Matrix blocks…    |
| --------------- | ------------------------- |
| `1`             | with an ID of 1.          |
| `'not 1'`       | not with an ID of 1.      |
| `[1, 2]`        | with an ID of 1 or 2.     |
| `['not', 1, 2]` | not with an ID of 1 or 2. |



::: code
```twig
{# Fetch all Matrix blocks in order of date created #}
{% set MatrixBlocks = craft.matrixBlocks()
    .orderBy('dateCreated asc')
    .all() %}
```

```php
// Fetch all Matrix blocks in order of date created
$MatrixBlocks = \craft\elements\MatrixBlock::find()
    ->orderBy('dateCreated asc')
    ->all();
```
:::



::: tip
This can be combined with [fixedOrder](#fixedorder) if you want the results to be returned in a specific order.
:::


### `ignorePlaceholders`

Causes the query to return matching Matrix blocks as they are stored in the database, ignoring matching placeholder elements that were set by [craft\services\Elements::setPlaceholderElement()](https://docs.craftcms.com/api/v3/craft-services-elements.html#method-setplaceholderelement).










### `inReverse`

Causes the query results to be returned in reverse order.





::: code
```twig
{# Fetch Matrix blocks created for this entry #}
{% set MatrixBlocks = craft.matrixBlocks()
    .owner(myEntry)
    .all() %}
```

```php
// Fetch Matrix blocks created for this entry
$MatrixBlocks = \craft\elements\MatrixBlock::find()
    ->owner($myEntry)
    ->all();
```
:::


### `limit`

Determines the number of Matrix blocks that should be returned.



::: code
```twig
{# Fetch Matrix blocks created for an element with an ID of 1 #}
{% set MatrixBlocks = craft.matrixBlocks()
    .ownerId(1)
    .all() %}
```

```php
// Fetch Matrix blocks created for an element with an ID of 1
$MatrixBlocks = \craft\elements\MatrixBlock::find()
    ->ownerId(1)
    ->all();
```
:::


### `offset`

Determines how many Matrix blocks should be skipped in the results.



::: code
```twig
{# Fetch all Matrix blocks that are related to myCategory #}
{% set MatrixBlocks = craft.matrixBlocks()
    .relatedTo(myCategory)
    .all() %}
```

```php
// Fetch all Matrix blocks that are related to $myCategory
$MatrixBlocks = \craft\elements\MatrixBlock::find()
    ->relatedTo($myCategory)
    ->all();
```
:::


### `orderBy`

Determines the order that the Matrix blocks should be returned in.



::: code
```twig
{# Get the search query from the 'q' query string param #}
{% set searchQuery = craft.app.request.getQueryParam('q') %}

{# Fetch all Matrix blocks that match the search query #}
{% set MatrixBlocks = craft.matrixBlocks()
    .search(searchQuery)
    .all() %}
```

```php
// Get the search query from the 'q' query string param
$searchQuery = \Craft::$app->request->getQueryParam('q');

// Fetch all Matrix blocks that match the search query
$MatrixBlocks = \craft\elements\MatrixBlock::find()
    ->search($searchQuery)
    ->all();
```
:::


### `owner`

Sets the [ownerId](#ownerid) and [siteId](#siteid) parameters based on a given element.



デフォルトでは、現在のサイトが使用されます。
```twig
{# Fetch Matrix blocks from the Foo site #}
{% set MatrixBlocks = craft.matrixBlocks()
    .site('foo')
    .all() %}
```

```php
// Fetch Matrix blocks from the Foo site
$MatrixBlocks = \craft\elements\MatrixBlock::find()
    ->site('foo')
    ->all();
```
:::


### `ownerId`

Narrows the query results based on the owner element of the Matrix blocks, per the owners’ IDs.

Possible values include:

| Value           | Fetches Matrix blocks…                           |
| --------------- | ------------------------------------------------ |
| `1`             | created for an element with an ID of 1.          |
| `'not 1'`       | not created for an element with an ID of 1.      |
| `[1, 2]`        | created for an element with an ID of 1 or 2.     |
| `['not', 1, 2]` | not created for an element with an ID of 1 or 2. |



::: code
```twig
{# Fetch Matrix blocks from the site with an ID of 1 #}
{% set MatrixBlocks = craft.matrixBlocks()
    .siteId(1)
    .all() %}
```

```php
// Fetch Matrix blocks from the site with an ID of 1
$MatrixBlocks = \craft\elements\MatrixBlock::find()
    ->siteId(1)
    ->all();
```
:::


### `preferSites`

If [unique](#unique) is set, this determines which site should be selected when querying multi-site elements.



For example, if element “Foo” exists in Site A and Site B, and element “Bar” exists in Site B and Site C, and this is set to `['c', 'b', 'a']`, then Foo will be returned for Site C, and Bar will be returned for Site B.

If this isn’t set, then preference goes to the current site.



:::
```twig
{# Fetch disabled Matrix blocks #}
{% set MatrixBlocks = craft.matrixBlocks()
    .status('disabled')
    .all() %}
```

```php
// Fetch disabled Matrix blocks
$MatrixBlocks = \craft\elements\MatrixBlock::find()
    ->status('disabled')
    ->all();
```
:::


### `relatedTo`

Narrows the query results to only Matrix blocks that are related to certain other elements.



See [Relations](https://docs.craftcms.com/v3/relations.html) for a full explanation of how to work with this parameter.



:::
```twig
{# Fetch trashed Matrix blocks #}
{% set MatrixBlocks = {twig-function}
    .trashed()
    .all() %}
```

```php
// Fetch trashed Matrix blocks
$MatrixBlocks = \craft\elements\MatrixBlock::find()
    ->trashed()
    ->all();
```
:::


### `revisionCreator`

Narrows the query results to only revisions created by a given user.



::: code

| Value                                  | Fetches revisions…                     |
| -------------------------------------- | -------------------------------------- |
| `1`                                    | created by the user with an ID of 1.   |
| a `\craft\elements\db\User` object | by the user represented by the object. |



:::
```twig
{# Fetch Matrix blocks with a Foo block type #}
{% set MatrixBlocks = myEntry.myMatrixField
    .type('foo')
    .all() %}
```

```php
// Fetch Matrix blocks with a Foo block type
$MatrixBlocks = $myEntry->myMatrixField
    ->type('foo')
    ->all();
```
:::


### `revisionId`

Narrows the query results based on the Matrix blocks’ revision’s ID (from the `revisions` table).



:::

| Value | Fetches revisions…                |
| ----- | --------------------------------- |
| `1`   | for the revision with an ID of 1. |



関連付けられたエレメントを eager-loaded した状態で、マッチした行列ブロックをクエリが返します。
```twig
{# Fetch Matrix blocks of the block type with an ID of 1 #}
{% set MatrixBlocks = myEntry.myMatrixField
    .typeId(1)
    .all() %}
```

```php
// Fetch Matrix blocks of the block type with an ID of 1
$MatrixBlocks = $myEntry->myMatrixField
    ->typeId(1)
    ->all();
```
:::


### `revisionOf`

Narrows the query results to only revisions of a given Matrix block.



Possible values include:

| Value                                                    | Fetches revisions…                              |
| -------------------------------------------------------- | ----------------------------------------------- |
| `1`                                                      | for the Matrix block with an ID of 1.           |
| a [MatrixBlock](api:craft\elements\MatrixBlock) object | for the Matrix block represented by the object. |



::: code
```twig
{# Fetch the Matrix block by its UID #}
{% set MatrixBlock = craft.matrixBlocks()
    .uid('xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx')
    .one() %}
```

```php
// Fetch the Matrix block by its UID
$MatrixBlock = \craft\elements\MatrixBlock::find()
    ->uid('xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx')
    ->one();
```
:::


### `revisions`

Narrows the query results to only revision Matrix blocks.





::: code
```twig
{# Fetch Matrix blocks eager-loaded with the "Related" field’s relations #}
{% set MatrixBlocks = craft.matrixBlocks()
    .with(['related'])
    .all() %}
```

```php
// Fetch Matrix blocks eager-loaded with the "Related" field’s relations
$MatrixBlocks = \craft\elements\MatrixBlock::find()
    ->with(['related'])
    ->all();
```
:::


### `search`

Narrows the query results to only Matrix blocks that match a search query.



See [Searching](https://docs.craftcms.com/v3/searching.html) for a full explanation of how to work with this parameter.



::: code
```twig
{# Get the search query from the 'q' query string param #}
{% set searchQuery = craft.app.request.getQueryParam('q') %}

{# Fetch all Matrix blocks that match the search query #}
{% set MatrixBlocks = craft.matrixBlocks()
    .search(searchQuery)
    .all() %}
```

```php
// Get the search query from the 'q' query string param
$searchQuery = \Craft::$app->request->getQueryParam('q');

// Fetch all Matrix blocks that match the search query
$MatrixBlocks = \craft\elements\MatrixBlock::find()
    ->search($searchQuery)
    ->all();
```
:::


### `site`

Determines which site(s) the Matrix blocks should be queried in.



The current site will be used by default.

Possible values include:

| Value                                  | Fetches Matrix blocks…                         |
| -------------------------------------- | ---------------------------------------------- |
| `'foo'`                                | from the site with a handle of `foo`.          |
| `['foo', 'bar']`                       | from a site with a handle of `foo` or `bar`.   |
| `['not', 'foo', 'bar']`                | not in a site with a handle of `foo` or `bar`. |
| a `\craft\elements\db\Site` object | from the site represented by the object.       |
| `'*'`                                  | from any site.                                 |

::: tip
If multiple sites are specified, elements that belong to multiple sites will be returned multiple times. If you only want unique elements to be returned, use [unique](#unique) in conjunction with this.
:::



::: code
```twig
{# Fetch Matrix blocks from the Foo site #}
{% set MatrixBlocks = craft.matrixBlocks()
    .site('foo')
    .all() %}
```

```php
// Fetch Matrix blocks from the Foo site
$MatrixBlocks = \craft\elements\MatrixBlock::find()
    ->site('foo')
    ->all();
```
:::


### `siteId`

Determines which site(s) the Matrix blocks should be queried in, per the site’s ID.



The current site will be used by default.



::: code
```twig
{# Fetch Matrix blocks from the site with an ID of 1 #}
{% set MatrixBlocks = craft.matrixBlocks()
    .siteId(1)
    .all() %}
```

```php
// Fetch Matrix blocks from the site with an ID of 1
$MatrixBlocks = \craft\elements\MatrixBlock::find()
    ->siteId(1)
    ->all();
```
:::


### `status`

Narrows the query results based on the Matrix blocks’ statuses.



Possible values include:

| Value                    | Fetches Matrix blocks… |
| ------------------------ | ---------------------- |
| `'enabled'`  _(default)_ | that are enabled.      |
| `'disabled'`             | that are disabled.     |



::: code
```twig
{# Fetch disabled Matrix blocks #}
{% set MatrixBlocks = craft.matrixBlocks()
    .status('disabled')
    .all() %}
```

```php
// Fetch disabled Matrix blocks
$MatrixBlocks = \craft\elements\MatrixBlock::find()
    ->status('disabled')
    ->all();
```
:::


### `trashed`

Narrows the query results to only Matrix blocks that have been soft-deleted.





::: code
```twig
{# Fetch trashed Matrix blocks #}
{% set MatrixBlocks = craft.matrixBlocks()
    .trashed()
    .all() %}
```

```php
// Fetch trashed Matrix blocks
$MatrixBlocks = \craft\elements\MatrixBlock::find()
    ->trashed()
    ->all();
```
:::


### `type`

Narrows the query results based on the Matrix blocks’ block types.

Possible values include:

| Value                                                           | Fetches Matrix blocks…                         |
| --------------------------------------------------------------- | ---------------------------------------------- |
| `'foo'`                                                         | of a type with a handle of `foo`.              |
| `'not foo'`                                                     | not of a type with a handle of `foo`.          |
| `['foo', 'bar']`                                                | of a type with a handle of `foo` or `bar`.     |
| `['not', 'foo', 'bar']`                                         | not of a type with a handle of `foo` or `bar`. |
| an [MatrixBlockType](api:craft\models\MatrixBlockType) object | of a type represented by the object.           |



::: code
```twig
{# Fetch Matrix blocks with a Foo block type #}
{% set MatrixBlocks = myEntry.myMatrixField
    .type('foo')
    .all() %}
```

```php
// Fetch Matrix blocks with a Foo block type
$MatrixBlocks = $myEntry->myMatrixField
    ->type('foo')
    ->all();
```
:::


### `typeId`

Narrows the query results based on the Matrix blocks’ block types, per the types’ IDs.

Possible values include:

| Value           | Fetches Matrix blocks…              |
| --------------- | ----------------------------------- |
| `1`             | of a type with an ID of 1.          |
| `'not 1'`       | not of a type with an ID of 1.      |
| `[1, 2]`        | of a type with an ID of 1 or 2.     |
| `['not', 1, 2]` | not of a type with an ID of 1 or 2. |



::: code
```twig
{# Fetch Matrix blocks of the block type with an ID of 1 #}
{% set MatrixBlocks = myEntry.myMatrixField
    .typeId(1)
    .all() %}
```

```php
// Fetch Matrix blocks of the block type with an ID of 1
$MatrixBlocks = $myEntry->myMatrixField
    ->typeId(1)
    ->all();
```
:::


### `uid`

Narrows the query results based on the Matrix blocks’ UIDs.





::: code
```twig
{# Fetch the Matrix block by its UID #}
{% set MatrixBlock = craft.matrixBlocks()
    .uid('xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx')
    .one() %}
```

```php
// Fetch the Matrix block by its UID
$MatrixBlock = \craft\elements\MatrixBlock::find()
    ->uid('xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx')
    ->one();
```
:::


### `unique`

Determines whether only elements with unique IDs should be returned by the query.



This should be used when querying elements from multiple sites at the same time, if “duplicate” results is not desired.



::: code
```twig
{# Fetch unique Matrix blocks across all sites #}
{% set MatrixBlocks = craft.matrixBlocks()
    .site('*')
    .unique()
    .all() %}
```

```php
// Fetch unique Matrix blocks across all sites
$MatrixBlocks = \craft\elements\MatrixBlock::find()
    ->site('*')
    ->unique()
    ->all();
```
:::


### `with`

Causes the query to return matching Matrix blocks eager-loaded with related elements.



See [Eager-Loading Elements](https://docs.craftcms.com/v3/dev/eager-loading-elements.html) for a full explanation of how to work with this parameter.



::: code
```twig
{# Fetch Matrix blocks eager-loaded with the "Related" field’s relations #}
{% set MatrixBlocks = craft.matrixBlocks()
    .with(['related'])
    .all() %}
```

```php
// Fetch Matrix blocks eager-loaded with the "Related" field’s relations
$MatrixBlocks = \craft\elements\MatrixBlock::find()
    ->with(['related'])
    ->all();
```
:::



<!-- END PARAMS -->
