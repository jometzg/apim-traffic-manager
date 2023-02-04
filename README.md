# Azure API Management with Azure Traffic Manager

[Azure API Management](https://learn.microsoft.com/en-us/azure/api-management/api-management-key-concepts) (APIM) is an API gateway that allows a team or company to control access to a series of APIs. It does not implement the APIs, but provides a means of discovering, curating, controlling access to APIs.


## The need for a multi-region deployment of API Management
[Multi-region](https://learn.microsoft.com/en-us/azure/api-management/api-management-howto-deploy-multi-region) deployment of API

It is important to note:

*With multi-region deployment, only the gateway component of your API Management instance is replicated to multiple regions. The instance's management plane and developer portal remain hosted only in the primary region, the region where you originally deployed the service.*
