**Migrating from  Microsoft.Azure.Microsoft.Azure.ServiceBus Azure.Messaging.ServiceBus**
#
**Microsoft.Azure.ServiceBus** is no longer supported by microsoft.So,Recently we have migrated our system to Azure.Messaging.ServiceBus.

In this blog I will be showing how to Manage servicebus using **Microsoft.Azure.ServiceBus**

 Managing Service Bus Using [**Azure.Messaging.ServiceBus**](https://docs.microsoft.com/en-us/dotnet/api/azure.messaging.servicebus?view=azure-dotnet)
 --

Using Azure ServiceBus FunctionApp To Recive Message Message From ServiceBus For Topics And Queue
```
using Azure.Messaging.ServiceBus;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.ServiceBus;


[FunctionName("TestserviceBusTopic")]
    public async Task Run([ServiceBusTrigger("topicName", "subscriptionname", Connection = "ServiceBusNameSpace", AutoCompleteMessages = false)] ServiceBusReceivedMessage message, ServiceBusMessageActions messageActions)
    {
        try
        {
            var data=JsonConvert.DeserializeObject<ExpectedType>(Encoding.UTF8.GetString(message.Body)
            await messageActions.CompleteMessageAsync(message);
           
        }
        catch (Exception ex)
        {
            _logger.LogError(ex, "exception occured for AmsProcess request");
            await messageActions.DeadLetterMessageAsync(message);
        }
    }
```
Administration Of ServiebusTopicClient
```
Public void EnableOrDisableServiceBus(bool enableServicebus)
{


var serviceBusAdministrationClient = new ServiceBusAdministrationClient(_configuration["SerivceBusConnString"]);
AsyncPageable<TopicProperties> topics = serviceBusAdministrationClient.GetTopicsAsync();
            await foreach (TopicProperties secretProperties in topics)
            {
                if (enableServicebus)
                {
                    secretProperties.Status = EntityStatus.Active;
                }
                else
                {
                    secretProperties.Status = EntityStatus.Disabled;
                }
            }
}
```


