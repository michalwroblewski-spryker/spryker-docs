

## Install feature core

### Prerequisites

To start feature integration, overview and install the necessary features:

| NAME | VERSION |
| --- | --- |
| Product Bundles | {{site.version}} |
| Return Management | {{site.version}} |
| Spryker Core | {{site.version}} |

### 1) Set up behavior

| PLUGIN | SPECIFICATION | PREREQUISITES | NAMESPACE |
| --- | --- | --- | --- |
| ProductBundleReturnCreateFormHandlerPlugin | Expands `ReturnCreateForm` data with product bundles subforms. Handles form submit. | None | Spryker\Zed\ProductBundle\Communication\Plugin\SalesReturnGui |

**src/Pyz/Zed/SalesReturnGui/SalesReturnGuiDependencyProvider.php**

```php
<?php

namespace Pyz\Zed\SalesReturnGui;

use Spryker\Zed\ProductBundle\Communication\Plugin\SalesReturnGui\ProductBundleReturnCreateFormHandlerPlugin;
use Spryker\Zed\SalesReturnGui\SalesReturnGuiDependencyProvider as SprykerSalesReturnGuiDependencyProvider;

class SalesReturnGuiDependencyProvider extends SprykerSalesReturnGuiDependencyProvider
{
    /**
     * @return \Spryker\Zed\SalesReturnGuiExtension\Dependency\Plugin\ReturnCreateFormHandlerPluginInterface[]
     */
    protected function getReturnCreateFormHandlerPlugins(): array
    {
        return [
            new ProductBundleReturnCreateFormHandlerPlugin(),
        ];
    }
}
```

{% info_block warningBox "Verification" %}

Make sure that on return creation page in the Back Office UI, you are able to create returns for orders which have product bundles.

{% endinfo_block %}
