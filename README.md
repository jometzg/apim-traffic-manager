# Azure API Management with Azure Traffic Manager

[Azure API Management](https://learn.microsoft.com/en-us/azure/api-management/api-management-key-concepts) (APIM) is an API gateway that allows a team or company to control access to a series of APIs. It does not implement the APIs, but provides a means of discovering, curating, controlling access to APIs.

API Management in its Premium SKU is able to be scaled across availability zones in order to provide more scale and resilience from a single data centre failure. Please look [here](https://learn.microsoft.com/en-us/azure/reliability/availability-zones-overview) for more on what availability zones are.


## The need for a multi-region deployment of API Management
[Multi-region](https://learn.microsoft.com/en-us/azure/api-management/api-management-howto-deploy-multi-region) deployment of API Management is useful for exposing APIs to consumers of those APIs in different geographic regions with reduced latency. It also enables the APIs to still function in case of a region-wide outage.

It is important to note:

*With multi-region deployment, only the gateway component of your API Management instance is replicated to multiple regions. The instance's management plane and developer portal remain hosted only in the primary region, the region where you originally deployed the service.*

So, when multiple regions of an API management instance are created, only the *gateway* or proxy and its meta data is replicated. The portals remain in the primary region.
