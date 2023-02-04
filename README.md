# Azure API Management with Azure Traffic Manager

[Azure API Management](https://learn.microsoft.com/en-us/azure/api-management/api-management-key-concepts) (APIM) is an API gateway that allows a team or company to control access to a series of APIs. It does not implement the APIs, but provides a means of discovering, curating, controlling access to APIs.

API Management in its Premium SKU is able to be scaled across availability zones in order to provide more scale and resilience from a single data centre failure. Please look [here](https://learn.microsoft.com/en-us/azure/reliability/availability-zones-overview) for more on what availability zones are.


## The need for a multi-region deployment of API Management
[Multi-region](https://learn.microsoft.com/en-us/azure/api-management/api-management-howto-deploy-multi-region) deployment of API Management is useful for exposing APIs to consumers of those APIs in different geographic regions with reduced latency. It also enables the APIs to still function in case of a region-wide outage.

It is important to note:

*With multi-region deployment, only the gateway component of your API Management instance is replicated to multiple regions. The instance's management plane and developer portal remain hosted only in the primary region, the region where you originally deployed the service.*

So, when multiple regions of an API management instance are created, only the *gateway* or proxy and its meta data is replicated. The portals remain in the primary region.

## Request Routing with multi-region API Management
By default, when setting up a multi-region implementation of API Management, an instance of [Traffic Manager](https://learn.microsoft.com/en-us/azure/traffic-manager/traffic-manager-overview) is embedded into API Management and so the main URL of traffic manager. This instance of traffic manager is set to *Performance Based* routing - which means that a client using APIM, will resolve a traffic manager instance, which will then send requests to the least APIM gateway in a region which has the lowest latency form the client. In many respects, this is ideal as traffic will land at the gateway in the region "nearest" to the client.

This, however is not ideal for all circumstances, so APIM has the ability to work alongside a [separate instance](https://learn.microsoft.com/en-us/azure/api-management/api-management-howto-deploy-multi-region#-use-custom-routing-to-api-management-regional-gateways) if Traffic Manager - where you can configure the policy and region targets of traffic manager to suit your requirements.

The rest of this article details an approach to this for one scenario.

## Scenario
In this scenario, there are a few requirements:
1. Multi-region, but control costs
2. Not rely on a single instance of the gateway in the primary region (preferably 2 instances in different availability zones
3. An instance in another region - primarily for DR
4. In normal circumstances direct all requests to the primary region - the one with more than one instance - for scale and resilience

In this scenario, there is no single point of failure in the primary region and this is the one with most scale, but there is also a smaller secondary region for times of a full region outage.

This will require a separate instance of traffic manager to be deployed and configured to work with API Management.

## API Management Configuration


